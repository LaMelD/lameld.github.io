---
title: "PostgreSQL 고가용성 구성: Galera·MaxScale·ProxySQL에 대응하는 요소"
date: 2026-09-17
weight: 4
tags: [postgresql, database, high-availability, patroni]
description: "Galera·MaxScale·ProxySQL에 대응하는 PostgreSQL HA 구성 요소를 정리한다. 스트리밍 복제, Patroni·repmgr, Pgpool-II·HAProxy·PgBouncer, 멀티마스터 확장, 쿠버네티스 오퍼레이터와 대표 아키텍처를 다룬다."
---

- 조사일: 2026-09-11
- 목적: MariaDB/MySQL의 Galera Cluster, MaxScale, ProxySQL에 대응하는 PostgreSQL HA 구성 요소를 정리한다.

## 1. 결론 요약

- PostgreSQL 코어에는 Galera 같은 **동기 멀티마스터가 없다.** 표준 HA는 "스트리밍 복제(단일 writer) + 자동 장애 조치 관리자 + 프록시" 조합이다.
- MaxScale 하나가 하던 일(R/W 분리, 자동 failover, 커넥션 라우팅)은 PostgreSQL에서 보통 **Patroni + HAProxy (+ PgBouncer)** 조합으로 나누어 하거나, **Pgpool-II** 하나로 한다.
- 멀티마스터가 꼭 필요하면 논리 복제 기반 확장(pgactive, pgEdge Spock, EDB PGD)을 쓴다. 대부분 **비동기 + 충돌 해결** 방식이라 Galera의 동기 인증(certification) 방식과는 성격이 다르다.
- 쿠버네티스에서는 CloudNativePG가 표준이다.

## 2. MariaDB 구성 요소와의 대응

| MariaDB / MySQL | 역할 | PostgreSQL 대응 |
|---|---|---|
| Galera Cluster | 동기 멀티마스터, 가상 동기 복제, 자동 멤버십 | 직접 대응 없음. 가용성 면에서는 동기 스트리밍 복제 + Patroni. 다중 writer는 pgactive / pgEdge Spock / EDB PGD |
| MaxScale | R/W 분리, 자동 failover, 커넥션 라우팅, 필터 | Pgpool-II (단일 도구) 또는 Patroni + HAProxy + PgBouncer (조합) |
| ProxySQL | SQL 인식 프록시, 풀링, 쿼리 라우팅 | PgBouncer, pgcat, PgDog, ProxySQL 3.0 (PostgreSQL 지원 추가) |
| 비동기 binlog 복제 | 복제 기본 | 스트리밍 물리 복제(비동기), 논리 복제 |
| 반동기(semi-sync) 복제 | 커밋 내구성 보장 | `synchronous_commit`  • `synchronous_standby_names` (쿼럼 커밋 포함) |
| GTID | 복제 위치 식별 | LSN, 타임라인, replication slot |
| MHA, Orchestrator | 자동 failover 관리자 | Patroni, repmgr, pg_auto_failover |
| garbd (Galera arbitrator) | 쿼럼 유지 | etcd / Consul / ZooKeeper (Patroni DCS), witness 노드 (repmgr) |
| mariabackup, xtrabackup | 물리 백업 | pg_basebackup, pgBackRest, Barman, WAL-G |
| MariaDB Operator, Percona Operator | 쿠버네티스 | CloudNativePG, Zalando postgres-operator, Crunchy PGO, Percona Operator for PostgreSQL, StackGres |

## 3. 코어가 제공하는 기반

코어는 복제와 승격의 기본 동작만 제공하고, 자동 장애 조치·VIP·클라이언트 라우팅·멀티마스터는 외부 도구에 맡긴다.

### 3.1 스트리밍 물리 복제

- primary의 WAL을 standby로 실시간 전송해 바이트 단위로 동일한 복제본을 유지한다. standby는 읽기 전용 쿼리를 받는다(hot standby).
- `pg_basebackup -R`로 standby를 만들면 접속 설정이 자동 기록된다. 승격은 `pg_ctl promote` 또는 `SELECT pg_promote()`.
- replication slot을 쓰면 standby가 뒤처져도 primary가 WAL을 지우지 않는다. 대신 standby가 죽으면 디스크가 찰 수 있어 `max_slot_wal_keep_size`로 상한을 둔다.
- `pg_rewind`로 옛 primary를 데이터 재복사 없이 새 primary의 standby로 재합류시킨다. Patroni가 자동으로 쓴다.

### 3.2 동기 복제 수준

`synchronous_standby_names`에 standby를 지정하고 `synchronous_commit`으로 커밋이 어디까지 도달해야 반환할지 정한다.

| synchronous_commit | 커밋 반환 시점 | 비고 |
|---|---|---|
| `off` | 로컬 WAL 기록 전 | 크래시 시 최근 트랜잭션 유실 가능 |
| `local` | 로컬 WAL flush | 비동기 복제 |
| `remote_write` | standby OS 버퍼 도달 | standby 프로세스 크래시에는 안전, OS 크래시에는 아님 |
| `on` (기본) | standby WAL flush | 동기 복제. Galera의 내구성에 해당 |
| `remote_apply` | standby에 적용 완료 | standby에서 즉시 읽기 일관성 |

- `synchronous_standby_names = 'ANY 1 (s1, s2, s3)'`처럼 쓰면 쿼럼 커밋이 된다. `FIRST 2 (s1, s2)`는 우선순위 방식이다.
- 동기 standby가 모두 죽으면 커밋이 멈춘다. Patroni는 이 상황을 감지해 동기 모드를 자동 조정한다(`synchronous_mode`, `synchronous_mode_strict`).

### 3.3 논리 복제

`PUBLICATION` / `SUBSCRIPTION`으로 테이블 단위, 버전 간, 양방향 복제가 가능하다. 멀티마스터 확장들의 기반이다. 16부터 standby에서도 발행할 수 있고, 17부터 `pg_createsubscriber`로 물리 standby를 논리 구독자로 바꿀 수 있다.

### 3.4 클라이언트 측 failover (libpq)

프록시 없이도 접속 문자열에 호스트를 여러 개 쓰면 primary를 찾아간다. 드라이버가 libpq 기반이면(psycopg, Go pgx는 자체 구현) 대부분 지원한다.

```text
postgresql://app@node1:5432,node2:5432,node3:5432/appdb?target_session_attrs=read-write
postgresql://app@node1,node2,node3/appdb?target_session_attrs=read-only&load_balance_hosts=random   # 16 이상
```

## 4. 자동 장애 조치 관리자

Galera의 클러스터 멤버십 관리와 MaxScale의 auto-failover가 하던 역할이다.

### 4.1 Patroni (사실상 표준)

- 최신 4.1.5 (2026-08-12), PostgreSQL 14~18 지원. Zalando가 시작해 Percona, Crunchy Data, EDB 등이 배포판에 내장한다.
- 각 PG 노드에 Patroni 데몬을 두고, **DCS**(etcd, Consul, ZooKeeper, 또는 쿠버네티스 API)에 리더 락을 유지한다. 리더가 락을 갱신하지 못하면 다른 노드가 락을 잡고 승격한다. DCS 쿼럼이 split-brain을 막는다.
- REST API(`/primary`, `/replica`, `/health`)를 제공해 HAProxy 헬스체크로 R/W 라우팅을 만든다.
- `patronictl switchover`(계획 전환), `patronictl failover`, `patronictl edit-config`(전 노드 설정 동기화), `pg_rewind` 자동 실행, 동기 모드 관리, 카스케이드 복제, standby cluster(원격 DR).
- 요구: etcd 3노드 이상(PG 노드와 같은 서버에 두어도 됨), Python.

### 4.2 repmgr

- 최신 5.5.0, PostgreSQL 12~18. EDB가 관리한다.
- DCS 없이 repmgr 자체 메타데이터 DB와 `repmgrd` 데몬으로 자동 failover를 한다. witness 노드로 쿼럼을 보강한다.
- 설정과 운영이 단순하지만 split-brain 방어는 Patroni보다 약하다. 프록시나 VIP 전환은 `promote_command`, `follow_command` 훅으로 직접 붙여야 한다.

### 4.3 pg_auto_failover

- 최신 2.2. Citus Data/Microsoft가 만들었고, 현재는 재정 지원 없이 자원봉사로 유지보수된다.
- monitor 노드 하나 + primary + secondary의 단순 구성. 소규모에 편하지만 신규 도입에는 Patroni를 권장한다.

### 4.4 그 외

- Stolon: 쿠버네티스 초기 도구, 사실상 개발 중단.
- EDB Failover Manager(EFM): EDB 상용.

## 5. 프록시, 라우터, 커넥션 풀러

MaxScale과 ProxySQL이 하던 역할이다. PostgreSQL은 접속당 프로세스를 만들기 때문에 풀러가 HA와 별개로도 거의 필수다.

| 도구 | 성격 | R/W 분리 | 자동 failover | 풀링 | 비고 |
|---|---|---|---|---|---|
| Pgpool-II 4.7.2 | 올인원 미들웨어 | SQL 파싱으로 자동 | 자체 (watchdog + VIP) | 세션 풀 | MaxScale과 가장 유사. 설정 복잡, 파싱 오버헤드 |
| HAProxy | L4 TCP 프록시 | 포트 분리 (5000 write / 5001 read) | Patroni 헬스체크로 | 없음 | 가장 흔한 조합의 라우터 |
| PgBouncer | 경량 풀러 | 없음 | 없음 | 세션/트랜잭션/문장 | 단일 스레드. 여러 인스턴스를 앞단 LB로 |
| pgcat | Rust 풀러 | 있음 | replica failover | 트랜잭션 | 샤딩, 멀티스레드 |
| PgDog | Rust 풀러·프록시 | 있음 | 있음 | 트랜잭션 (SET, RLS, LISTEN 유지) | 샤딩, 상용 지원사 있음 |
| ProxySQL 3.0 | SQL 인식 프록시 | hostgroup 기반 | 승격은 외부 관리자, 상태 반영 | 있음 | 3.0(2024-09 알파)부터 PostgreSQL 지원. MariaDB에서 쓰던 팀에 익숙 |
| Odyssey | Yandex 풀러 | 없음 | 없음 | 있음 | 멀티스레드 |
| Supavisor | Elixir 풀러 | 없음 | 없음 | 있음 | 멀티테넌트, Supabase |

권장 조합:

- **가장 흔한 표준**: Patroni + etcd + HAProxy + (각 노드 앞 또는 앱 쪽) PgBouncer
- **MaxScale처럼 한 덩어리**: Pgpool-II (watchdog로 Pgpool 자체도 이중화)
- **ProxySQL 경험이 있는 팀**: ProxySQL 3.0 또는 pgcat/PgDog로 R/W 분리, 승격은 Patroni에 맡김

Pgpool-II와 Patroni는 역할이 겹치므로 둘 다 쓰면 승격 주체를 하나(보통 Patroni)로 정하고 Pgpool-II는 failover 기능을 끄고 라우팅만 맡긴다.

## 6. 멀티마스터 (Galera 대응)

### 6.1 Galera와의 근본적 차이

Galera는 커밋 시점에 모든 노드에 쓰기 집합을 전파하고 인증(certification)을 통과해야 커밋되는 **동기 방식**이다. 충돌은 커밋 시 거부되므로 데이터가 갈라지지 않는다.

PostgreSQL 생태계의 멀티마스터는 대부분 **논리 복제 기반 비동기**다. 각 노드가 먼저 로컬 커밋하고 나중에 다른 노드에 전파하며, 충돌은 사후에 정책(last-write-wins 등)으로 해결한다. 즉 같은 행을 두 노드가 동시에 바꾸면 한쪽 변경이 버려질 수 있다. 애플리케이션이 지역별로 쓰기 대상을 나누는 등 충돌을 설계로 피해야 한다. EDB PGD의 Quorum Commit만이 동기 쿼럼 커밋을 제공한다.

### 6.2 도구

| 도구 | 방식 | 라이선스 | 상태 (2026-09) |
|---|---|---|---|
| pgactive (AWS) | 논리 복제 기반 비동기 active-active. 충돌 감지·LWW 해결, 지연·충돌 모니터링. 16 이상 | Apache 2.0 (2025-06 오픈소스) | RDS에서 먼저 제공, 셀프호스팅 가능. 활발 |
| pgEdge Spock | 논리 복제 비동기 멀티마스터. Snowflake(전역 유일 시퀀스), LOLOR(large object 복제) 동반. 15~18 | PostgreSQL License (2025-09 재라이선스) | 활발. pgEdge Distributed/Enterprise Postgres에 포함 |
| EDB Postgres Distributed (PGD) | BDR 후속. 6.5. 6.4에서 Quorum Commit(동기 쿼럼) 도입, Connection Manager(풀링·라우팅) 내장, 충돌 정책 세분화 | 상용 | Galera에 기능적으로 가장 가까움 |
| Bucardo | 트리거 기반 비동기 멀티마스터 | BSD | 구식. 신규 도입 비권장 |

멀티마스터가 아니지만 함께 거론되는 것:

| 도구 | 성격 |
|---|---|
| Citus | 코디네이터 + 워커 샤딩. 수평 확장용. 노드 HA는 스트리밍 복제로 별도 |
| YugabyteDB, CockroachDB | PostgreSQL 와이어 프로토콜 호환 분산 SQL. Raft 동기 복제로 모든 노드 쓰기 가능. PostgreSQL 자체가 아니라 확장·기능 호환에 제약 있음 |

Galera처럼 "모든 노드 쓰기 + 강한 일관성"이 진짜 요구라면, PostgreSQL 자체로는 PGD Quorum Commit 외에는 답이 없고, YugabyteDB/CockroachDB 같은 분산 SQL로 아키텍처를 바꾸는 편이 가깝다. 대부분의 경우는 "단일 writer + 수 초 내 자동 failover + 읽기 분산"으로 재설계하는 것이 정석이다.

## 7. 백업과 PITR (HA의 보완)

복제는 논리적 실수(DROP TABLE)를 그대로 전파하므로 백업은 별개로 필요하다.

| 도구 | 특징 |
|---|---|
| pgBackRest | 병렬·증분·차등 백업, 압축·암호화, S3/GCS/Azure, Patroni·CloudNativePG 통합. 현재 가장 널리 권장 |
| Barman | EDB. 스트리밍 백업 서버 방식, 원격 관리에 강함 |
| WAL-G | Go. 클라우드 스토리지 중심, 경량 |
| pg_basebackup | 코어 내장. 17부터 증분 백업 지원. 소규모에 충분 |

## 8. 쿠버네티스

| 오퍼레이터 | 장애 조치 방식 | 비고 |
|---|---|---|
| CloudNativePG 1.29.x | 자체 구현 (Patroni 없음). 쿠버네티스 API를 DCS처럼 사용 | CNCF Sandbox (2025-01). `-rw`, `-ro`, `-r` 서비스 자동 생성, pgBackRest 대신 자체 Barman 기반 백업. 1.29.1이 CVE-2026-44477(메트릭 익스포터, CVSS 9.4) 수정 |
| Zalando postgres-operator | Patroni (Spilo 이미지) | 가장 오래됨, 성숙 |
| Crunchy PGO | Patroni | pgBackRest, pgBouncer, pgMonitor 통합 |
| Percona Operator for PostgreSQL | Patroni | Crunchy PGO 기반 |
| StackGres | Patroni | 웹 UI, Envoy 프록시 |

신규 구축이면 CloudNativePG가 기본 선택이다.

## 9. 대표 아키텍처

### 9.1 온프레미스 표준 (3노드)

```text
                app
                 │
           HAProxy (x2, keepalived VIP)
      5000 write ─┤─ 5001 read
                 │  헬스체크: Patroni REST /primary, /replica
    ┌────────────┼────────────┐
  node1        node2        node3
PG primary   PG standby   PG standby
Patroni      Patroni      Patroni
PgBouncer    PgBouncer    PgBouncer
etcd         etcd         etcd
                 │
           pgBackRest 저장소 (별도 노드 또는 S3)
```

- etcd는 PG 노드에 같이 두어도 되고, 더 안정적으로 하려면 별도 3노드로 뺀다.
- HAProxy는 `option httpchk GET /primary` (write 백엔드), `GET /replica` (read 백엔드)로 Patroni에 묻는다.
- 동기 복제가 필요하면 Patroni 설정에서 `synchronous_mode: true`.

### 9.2 Pgpool-II 올인원 (MaxScale 스타일)

```text
app ──► VIP (Pgpool-II watchdog)
          ├─ pgpool-node1 ─┐
          └─ pgpool-node2 ─┤ 헬스체크, R/W 분리, failover
                           ├─ PG primary
                           └─ PG standby
```

- Pgpool-II가 승격 스크립트(`failover_command`)로 standby를 승격하고 `follow_primary_command`로 나머지를 재연결한다.
- 구성 요소가 적은 대신 Pgpool-II 설정이 방대하다.

### 9.3 쿠버네티스 (CloudNativePG)

```yaml
apiVersion: postgresql.cnpg.io/v1
kind: Cluster
metadata:
  name: appdb
spec:
  instances: 3
  imageName: ghcr.io/cloudnative-pg/postgresql:18
  storage:
    size: 20Gi
  postgresql:
    parameters:
      max_connections: "200"
  backup:
    barmanObjectStore:
      destinationPath: s3://backups/appdb
      s3Credentials: { ... }
```

`appdb-rw`(primary), `appdb-ro`(standby만), `appdb-r`(전체) 서비스가 생긴다.

### 9.4 다지역 active-active

```text
region A                      region B
PG + pgactive/Spock  ◄──논리 복제──►  PG + pgactive/Spock
(A 지역 사용자 쓰기)                 (B 지역 사용자 쓰기)
```

- 각 지역 안에서는 9.1 구성으로 HA를 잡고, 지역 간에만 멀티마스터를 쓴다.
- 전역 유일 키(UUID, Snowflake 시퀀스), 지역별 쓰기 분리, 충돌 모니터링이 필수다.

## 10. 선택 가이드

| 요구 | 권장 |
|---|---|
| 온프레미스 단순 HA | Patroni + etcd + HAProxy + PgBouncer + pgBackRest |
| MaxScale처럼 도구 하나로 | Pgpool-II (watchdog로 이중화) |
| MariaDB에서 ProxySQL을 쓰던 팀 | ProxySQL 3.0 검토, 또는 pgcat / PgDog. 승격은 Patroni |
| 쿠버네티스 | CloudNativePG |
| 다지역 쓰기, 충돌 허용 가능 | pgactive 또는 pgEdge Spock (오픈소스), EDB PGD (상용) |
| 동기 멀티마스터, 강한 일관성 | EDB PGD Quorum Commit, 또는 YugabyteDB/CockroachDB로 전환 |
| 읽기 확장만 | 스트리밍 복제 standby + HAProxy 또는 pgcat R/W 분리 |
| 2노드만 가능 | Patroni + 외부 etcd(3노드, 작은 VM) 또는 repmgr + witness. 2노드 자체만으로는 쿼럼 불가 |

## 11. 이 서버에서의 적용

단일 노드(18/main 하나)라 HA 구성 대상은 아니다. 실습은 다음 두 가지가 가능하다.

### 11.1 같은 서버에 standby 클러스터 만들어 스트리밍 복제 실습

```bash
# primary(18/main) 준비: 복제 역할과 pg_hba
sudo -u postgres psql -c "CREATE ROLE replicator WITH REPLICATION LOGIN PASSWORD 'repl';"
echo "host replication replicator 127.0.0.1/32 scram-sha-256" | sudo tee -a /etc/postgresql/18/main/pg_hba.conf
sudo systemctl reload postgresql@18-main

# standby 클러스터 껍데기 생성 후 데이터 디렉터리를 베이스 백업으로 교체
sudo pg_createcluster 18 standby -p 5433
sudo rm -rf /var/lib/postgresql/18/standby
sudo -u postgres pg_basebackup -h 127.0.0.1 -p 5432 -U replicator -D /var/lib/postgresql/18/standby -R -X stream -C -S standby_slot
sudo pg_ctlcluster 18 standby start

# 확인
sudo -u postgres psql -p 5432 -c "SELECT client_addr, state, sync_state FROM pg_stat_replication;"
sudo -u postgres psql -p 5433 -c "SELECT pg_is_in_recovery();"    # t
```

실습이 끝나면 `sudo pg_dropcluster --stop 18 standby`로 제거한다.

### 11.2 Docker Compose로 Patroni 3노드 실습

Patroni 저장소의 `docker-compose.yml`(etcd 3 + Patroni 3 + HAProxy)을 그대로 쓰면 된다. 호스트 5432는 네이티브 18/main이 쓰고 있으므로 HAProxy 포트를 5000/5001이 아닌 다른 값으로 매핑한다. 메모리 15GB에서 MariaDB, 18/main과 함께 돌리기에 충분하다.

## 12. 참고 자료

- Patroni 릴리스 노트: [https://patroni.readthedocs.io/en/latest/releases.html](https://patroni.readthedocs.io/en/latest/releases.html)
- Pgpool-II 4.7.2 릴리스: [https://www.pgpool.net/news/2026-06-04/](https://www.pgpool.net/news/2026-06-04/)
- pg_auto_failover 문서: [https://pg-auto-failover.readthedocs.io/en/main/](https://pg-auto-failover.readthedocs.io/en/main/)
- pgactive (AWS): [https://github.com/aws/pgactive](https://github.com/aws/pgactive)
- pgEdge Spock 오픈소스 전환: [https://www.pgedge.com/blog/pgedge-goes-open-source](https://www.pgedge.com/blog/pgedge-goes-open-source)
- EDB PGD 문서: [https://www.enterprisedb.com/docs/pgd/latest/](https://www.enterprisedb.com/docs/pgd/latest/)
- ProxySQL PostgreSQL 지원: [https://proxysql.com/blog/a-perfect-complement-to-postgresql/](https://proxysql.com/blog/a-perfect-complement-to-postgresql/)
- CloudNativePG: [https://cloudnative-pg.io/](https://cloudnative-pg.io/) , CNCF: [https://www.cncf.io/projects/cloudnativepg/](https://www.cncf.io/projects/cloudnativepg/)
- repmgr: [https://www.repmgr.org/](https://www.repmgr.org/)
- 커넥션 풀러 비교: [https://www.bytebase.com/blog/open-source-postgres-connection-pooler/](https://www.bytebase.com/blog/open-source-postgres-connection-pooler/)
