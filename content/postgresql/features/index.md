---
title: "PostgreSQL 특징과 기능: 아키텍처, 타입, 인덱스, MariaDB와 비교"
date: 2026-09-17
weight: 1
tags: [postgresql, database, mariadb]
description: "PostgreSQL의 프로세스·메모리·저장 구조, MVCC와 VACUUM, 데이터 타입, 인덱스, SQL 기능, 확장, 복제, 버전별 변화를 정리하고 MariaDB와 객체 계층·문법을 비교한다."
---

## 1. 개요

- 1986년 UC 버클리 POSTGRES 프로젝트에서 출발한 객체-관계형 데이터베이스(ORDBMS). 1996년 SQL을 지원하며 PostgreSQL로 이름을 바꿨다.
- 라이선스는 PostgreSQL License(BSD/MIT 계열). 상용 이용·재배포에 제한이 없고 단일 회사가 소유하지 않는다.
- 매년 9~10월에 메이저 버전이 나오고, 각 메이저 버전은 5년간 마이너 업데이트를 받는다. 마이너 업데이트는 분기마다 나온다.
- 2026-09 기준 최신 메이저는 18이며, Ubuntu 24.04 기본 저장소는 16을 제공한다.
- SQL 표준 준수도가 높고, 트랜잭션 DDL·MVCC·확장 시스템이 강점이다.

## 2. 아키텍처

### 2.1 프로세스 구조

- `postmaster`(postgres 메인 프로세스)가 접속마다 백엔드 프로세스를 하나씩 fork한다. 스레드가 아니라 프로세스 모델이다.
- 접속 수가 많으면 프로세스 비용이 커지므로 수백 개 이상 동시 접속에는 PgBouncer 같은 커넥션 풀러를 앞에 둔다.
- 백그라운드 프로세스:
  - checkpointer: 더티 페이지를 주기적으로 디스크에 기록
  - background writer: 버퍼를 미리 비워 백엔드의 대기를 줄임
  - WAL writer: WAL 버퍼를 디스크로 flush
  - autovacuum launcher/worker: 죽은 튜플 정리와 통계 갱신
  - stats collector(15 이전) / 공유 메모리 통계(15 이후)
  - archiver, logical replication launcher, walsender/walreceiver

### 2.2 메모리

| 파라미터 | 역할 | 기본값 |
|---|---|---|
| `shared_buffers` | 디스크 페이지 캐시. 보통 RAM의 25% | 128MB |
| `work_mem` | 정렬·해시 작업 하나당 메모리. 쿼리·노드마다 별도 할당 | 4MB |
| `maintenance_work_mem` | VACUUM, CREATE INDEX 등 유지보수 작업 메모리 | 64MB |
| `effective_cache_size` | 플래너가 가정하는 OS 캐시 포함 총 캐시 크기(할당 아님) | 4GB |
| `wal_buffers` | WAL 쓰기 버퍼 | shared_buffers의 1/32 |

### 2.3 저장 구조와 WAL

- 논리 계층: 클러스터(인스턴스) > 데이터베이스 > 스키마 > 테이블/인덱스/뷰/함수.
- 하나의 클러스터에 여러 데이터베이스가 있지만, 한 접속은 한 데이터베이스에만 붙는다. 데이터베이스 간 조인은 `dblink`나 `postgres_fdw`가 필요하다.
- 테이블은 8KB 페이지(블록) 단위로 힙 파일에 저장된다. 큰 값은 TOAST로 압축·분리 저장한다.
- WAL(Write-Ahead Log): 데이터 파일을 바꾸기 전에 변경 로그를 먼저 기록해 크래시 복구, 스트리밍 복제, PITR의 기반이 된다.

### 2.4 MVCC와 VACUUM

- 다중 버전 동시성 제어(MVCC): UPDATE·DELETE는 기존 행을 덮어쓰지 않고 새 버전을 만들며, 읽기는 쓰기를 막지 않고 쓰기도 읽기를 막지 않는다.
- 오래된 행 버전(dead tuple)은 VACUUM이 회수한다. autovacuum이 자동으로 돌지만, 대량 갱신 테이블은 임계값 튜닝이 필요하다.
- 트랜잭션 ID는 32비트라서 약 20억 트랜잭션마다 wraparound를 막기 위한 freeze VACUUM이 반드시 필요하다. autovacuum을 끄면 안 되는 이유다.
- 격리 수준: Read Committed(기본), Repeatable Read, Serializable(SSI, 진짜 직렬화 보장). Read Uncommitted는 Read Committed로 동작한다.

### 2.5 클러스터 구성: 단일 vs 다중

서버 한 대에 클러스터 하나를 두고 그 안에서 database와 schema로 나누는 것이 표준 구성이다. 다중 클러스터는 특정한 이유가 있을 때만 쓴다.

단일 클러스터가 기본인 이유:

- 클러스터마다 `shared_buffers`와 백그라운드 프로세스(checkpointer, autovacuum, WAL writer)가 따로 뜬다. 한 서버에 여러 개를 돌리면 메모리를 나눠 써야 하고 캐시가 공유되지 않는다.
- 포트, 설정 파일, 백업, 모니터링, 업그레이드 작업이 클러스터 수만큼 늘어난다.
- 격리는 대부분 database·schema 수준에서 해결된다. 역할 권한, RLS, 스키마 분리로 접근 통제가 가능하다.
- 더 강한 격리가 필요하면 같은 호스트에 클러스터를 여럿 두기보다 VM이나 컨테이너를 나누어 각각 클러스터 하나씩 두는 쪽을 택한다.

다중 클러스터를 쓰는 경우:

| 상황 | 이유 | 지속성 |
|---|---|---|
| 메이저 버전 업그레이드 | 16/main 옆에 17/main을 만들어 옮긴 뒤 옛 것을 삭제. `pg_upgradecluster`가 이 방식 | 일시적 |
| 개발·테스트 환경 | 한 서버에서 버전별 동작 확인, 실험용 인스턴스 | 개발 서버 한정 |
| 클러스터 수준 설정이 충돌 | 한쪽은 `wal_level = logical`과 큰 `work_mem`, 다른 쪽은 OLTP 튜닝처럼 `postgresql.conf`가 근본적으로 달라야 할 때 | 드묾 |
| 독립적인 재시작·장애 범위 | 한 서비스의 설정 변경이나 장애가 다른 서비스를 건드리면 안 될 때 | 호스팅·멀티테넌트 |
| 복구 시점이 달라야 할 때 | PITR은 클러스터 단위라, 한 서비스만 특정 시점으로 되돌리려면 클러스터가 분리돼 있어야 함 | 설계 단계에서 결정 |
| 복제 토폴로지가 다를 때 | 물리 복제는 클러스터 전체를 복제하므로, 일부 데이터만 다른 곳으로 보내야 하면 분리하거나 논리 복제 사용 | 설계 단계에서 결정 |

Debian 계열의 `pg_createcluster`, `pg_lsclusters` 도구는 다중 클러스터 운영을 권장해서가 아니라, 메이저 업그레이드를 옆에 새 클러스터를 세워 안전하게 진행하기 위해 만들어진 것이다. 평소에는 `pg_lsclusters`에 한 줄만 있는 것이 정상이다.

이 서버(RAM 15GB, MariaDB와 공유)에는 클러스터 하나만 두고, 서비스 격리는 database로, 모듈 구분은 schema로 하는 구성을 권장한다. 다중 클러스터는 메이저 버전을 올릴 때 잠깐 생겼다가 사라지는 상태로만 본다.

## 3. 데이터 타입

| 분류 | 타입 | 비고 |
|---|---|---|
| 정수 | `smallint`, `integer`, `bigint` | unsigned 없음 |
| 자동 증가 | `GENERATED ALWAYS AS IDENTITY`, `serial`/`bigserial` | IDENTITY가 표준이고 권장 |
| 실수 | `numeric(p,s)`, `real`, `double precision` | 금액은 `numeric` |
| 문자 | `text`, `varchar(n)`, `char(n)` | `text`와 `varchar`는 성능 차이 없음 |
| 불리언 | `boolean` | 진짜 불리언, `TINYINT(1)` 아님 |
| 날짜/시간 | `timestamp`, `timestamptz`, `date`, `time`, `interval` | 대부분 `timestamptz` 권장 |
| 바이너리 | `bytea` |  |
| UUID | `uuid` | 16바이트 네이티브 저장. 18부터 `uuidv7()` 내장 |
| JSON | `json`, `jsonb` | `jsonb`는 바이너리 저장·인덱싱 가능. 거의 항상 `jsonb` |
| 배열 | `integer[]`, `text[]` 등 모든 타입의 배열 | GIN 인덱스 가능 |
| 범위 | `int4range`, `tstzrange`, `daterange` 등 | 겹침 연산자 `&&`, 배타 제약과 결합 |
| 열거 | `CREATE TYPE ... AS ENUM` |  |
| 복합 | `CREATE TYPE ... AS (...)` | 행 타입 |
| 네트워크 | `inet`, `cidr`, `macaddr` | 서브넷 연산 지원 |
| 기하 | `point`, `line`, `box`, `polygon`, `circle` | 실무는 PostGIS 사용 |
| 전문 검색 | `tsvector`, `tsquery` |  |
| 기타 | `xml`, `money`, `hstore`(확장), `vector`(pgvector 확장) |  |

## 4. 인덱스

| 종류 | 용도 |
|---|---|
| B-tree | 기본. 등호·범위·정렬·`LIKE 'abc%'` |
| Hash | 등호만. 10 이후 WAL 지원으로 안전 |
| GIN | 배열, `jsonb`, 전문 검색, `pg_trgm`. 다중 값 포함 검색 |
| GiST | 기하, 범위, 전문 검색, 최근접 검색(KNN). 확장 가능한 트리 |
| SP-GiST | 분할 공간 트리. 전화번호·IP 접두어 등 |
| BRIN | 블록 범위 요약. 시계열처럼 물리 순서와 상관있는 거대 테이블에 매우 작은 인덱스 |

- 부분 인덱스: `CREATE INDEX ... WHERE status = 'active'`
- 표현식 인덱스: `CREATE INDEX ... ON t (lower(email))`
- 커버링 인덱스: `INCLUDE (col)`로 인덱스 전용 스캔
- 동시 생성: `CREATE INDEX CONCURRENTLY`로 쓰기를 막지 않고 생성
- 다중 컬럼, 유니크, 배타(EXCLUDE) 제약

## 5. SQL 기능

- CTE(`WITH`), 재귀 CTE, `MATERIALIZED` 힌트
- 윈도우 함수: `row_number()`, `rank()`, `lag()`, `lead()`, `sum() OVER (...)`
- `LATERAL` 조인, `DISTINCT ON`, `FILTER (WHERE ...)` 집계
- UPSERT: `INSERT ... ON CONFLICT (...) DO UPDATE / DO NOTHING`
- `MERGE`(15 이상), 17부터 `RETURNING` 지원
- `RETURNING`: INSERT/UPDATE/DELETE 결과 행을 바로 반환. 18부터 `RETURNING OLD.col, NEW.col`
- 생성 컬럼: `GENERATED ALWAYS AS (expr) STORED`, 18부터 `VIRTUAL`이 기본
- 선언적 파티셔닝: RANGE, LIST, HASH. 파티션 프루닝, 파티션별 인덱스, `ATTACH/DETACH PARTITION CONCURRENTLY`
- 트랜잭션 DDL: `CREATE TABLE`, `ALTER TABLE`도 롤백된다. 마이그레이션 실패 시 절반만 적용되는 일이 없다.
- 세이브포인트, 2단계 커밋(`PREPARE TRANSACTION`), 어드바이저리 락
- `LISTEN` / `NOTIFY`: DB 내장 pub/sub
- 전문 검색: `to_tsvector`, `to_tsquery`, 사전·랭킹 내장. 한국어는 형태소 분석기가 없어 `pg_trgm`이나 외부 검색엔진과 병행
- JSON: `->`, `->>`, `@>`, `?`, `jsonb_path_query`(SQL/JSON path), 16부터 `JSON_ARRAY`·`JSON_OBJECT` 표준 생성자, 17부터 `JSON_TABLE`
- 행 수준 보안(RLS): `CREATE POLICY`로 사용자별 행 필터링
- 테이블 상속, 규칙(RULE), 트리거(행/문장, BEFORE/AFTER/INSTEAD OF), 이벤트 트리거
- `COPY`: 대량 적재/추출. 17부터 `ON_ERROR ignore`
- `EXPLAIN (ANALYZE, BUFFERS)`로 실행 계획과 실제 비용 확인

## 6. 확장성

- `CREATE EXTENSION`으로 설치하는 확장 시스템. 타입·연산자·인덱스 방식·함수를 추가할 수 있다.
- 자주 쓰는 확장:
  - `pg_stat_statements`: 쿼리별 실행 통계 (성능 분석 필수)
  - `pg_trgm`: 트라이그램 유사도, `LIKE '%abc%'` 인덱싱
  - `pgcrypto`: 해시·암호화 함수
  - `uuid-ossp`: UUID 생성 (13부터 `gen_random_uuid()` 내장이라 거의 불필요)
  - `hstore`: 키-값 타입
  - `citext`: 대소문자 무시 텍스트
  - `postgres_fdw`, `mysql_fdw`, `file_fdw`: 외부 데이터 래퍼. 다른 DB 테이블을 로컬 테이블처럼 조회
  - `PostGIS`: 공간 데이터 (사실상 표준 GIS 엔진)
  - `pgvector`: 벡터 유사도 검색 (임베딩·RAG)
  - `TimescaleDB`: 시계열 하이퍼테이블
  - `pg_partman`: 파티션 자동 관리
  - `pg_cron`: DB 안에서 크론 작업
- 프로시저 언어: PL/pgSQL(기본), PL/Python, PL/Perl, PL/Tcl, 서드파티 PL/V8, PL/Rust
- 사용자 정의 타입, 연산자, 집계 함수, 인덱스 연산자 클래스

## 7. 복제와 고가용성

| 방식 | 설명 |
|---|---|
| 스트리밍 물리 복제 | WAL을 그대로 전송해 바이트 단위 동일한 standby 유지. hot standby로 읽기 분산 |
| 동기 복제 | `synchronous_standby_names`로 커밋 시 standby 기록 보장 |
| 논리 복제 | `PUBLICATION` / `SUBSCRIPTION`. 테이블 단위, 다른 메이저 버전·다른 DB 간 복제, 16부터 standby에서도 발행 가능 |
| PITR | 베이스 백업 + WAL 아카이브로 특정 시점 복구 |
| 증분 백업 | 17부터 `pg_basebackup --incremental`, `pg_combinebackup` |

- 자동 failover는 내장되지 않았다. Patroni(etcd/Consul 기반), repmgr, pg_auto_failover 같은 도구를 쓴다.
- 커넥션 풀·라우팅: PgBouncer, Pgpool-II, HAProxy
- 관리형: Amazon RDS/Aurora, Google Cloud SQL/AlloyDB, Azure Database, Supabase, Neon

## 8. 보안

- 역할(ROLE) 기반. 사용자와 그룹이 모두 역할이며, 상속과 멤버십으로 권한을 구성한다.
- 인증은 `pg_hba.conf`에서 접속 경로별로 지정: `peer`(OS 사용자 매핑), `scram-sha-256`(비밀번호, 권장), `md5`(구식), `cert`, `ldap`, `gss`, 18부터 `oauth`
- 객체 권한: 데이터베이스·스키마·테이블·컬럼 단위 `GRANT`. 기본 권한은 `ALTER DEFAULT PRIVILEGES`
- 행 수준 보안(RLS), 보안 정의자 함수(`SECURITY DEFINER`)
- SSL/TLS 접속, 클라이언트 인증서, `pgcrypto`로 컬럼 암호화
- 15부터 `public` 스키마에 일반 사용자의 `CREATE` 권한이 기본적으로 없다.
- 감사: `pgaudit` 확장, `log_statement`, `log_connections`

## 9. 버전별 주요 변화

### 16 (2023-09, Ubuntu 24.04 기본)

- 논리 복제를 standby에서 발행 가능, 병렬 적용
- `pg_stat_io` 뷰로 I/O 통계
- SQL/JSON 표준 생성자·조건자(`JSON_ARRAY`, `IS JSON`)
- FULL/RIGHT OUTER JOIN 병렬 해시 조인
- `pg_hba.conf`에 정규식과 include 지원
- libpq `load_balance_hosts` 접속 부하 분산
- psql `\bind`

### 17 (2024-09)

- VACUUM 메모리 구조 개선(TID store), 최대 20배 적은 메모리
- 증분 백업 `pg_basebackup --incremental`
- `JSON_TABLE`, `MERGE ... RETURNING`, `MERGE`에 `WHEN NOT MATCHED BY SOURCE`
- `COPY ... ON_ERROR ignore`, `LOG_VERBOSITY`
- `pg_createsubscriber`: 물리 standby를 논리 구독자로 전환
- 논리 복제 슬롯이 업그레이드 후 유지
- 스트리밍 I/O(순차 스캔·ANALYZE 읽기 성능 향상)

### 18 (2025-09)

- 비동기 I/O 서브시스템(`io_method = worker | io_uring`)
- B-tree 스킵 스캔(복합 인덱스 선두 컬럼 없이도 활용)
- `uuidv7()` 내장, 가상 생성 컬럼(`VIRTUAL`) 기본
- `RETURNING OLD/NEW`
- 시간적 제약(`PRIMARY KEY ... WITHOUT OVERLAPS`, `PERIOD`)
- OAuth 2.0 인증, `md5` 인증 폐기 예고
- 데이터 체크섬 기본 활성화
- `pg_upgrade` 후 통계 유지

## 10. MariaDB와 비교

| 항목 | MariaDB 10.11 | PostgreSQL 16 |
|---|---|---|
| 객체 계층 | 인스턴스 > database(=schema) > 테이블 (3단계) | 클러스터 > database > schema > 테이블 (4단계) |
| database와 schema | 같은 것의 두 이름. `CREATE DATABASE` = `CREATE SCHEMA` | 다른 계층. database 안에 schema가 있음 |
| 조인 가능 범위 | 같은 인스턴스의 모든 database 간 (`db1.t JOIN db2.t`) | 같은 database의 schema 간만. 다른 database는 `postgres_fdw`·`dblink` 필요 |
| 접속 단위 | 인스턴스 (`USE db`로 자유 전환) | database (`\c db`는 재접속, 스키마 전환은 `search_path`) |
| 계정 범위 | 인스턴스 전역 | 클러스터 전역 (모든 database 공유) |
| 스토리지 엔진 | InnoDB, Aria, MyRocks 등 선택 | 단일 힙 엔진 (테이블 접근 방식 API로 확장 가능) |
| 동시성 | InnoDB MVCC + undo log | MVCC + VACUUM |
| DDL 트랜잭션 | 대부분 암묵 커밋 | 완전한 트랜잭션 DDL |
| 자동 증가 | `AUTO_INCREMENT` | `GENERATED ... AS IDENTITY`, 시퀀스 |
| 식별자 인용 | 백틱, 대소문자는 파일시스템 의존 | 큰따옴표, 인용 없으면 소문자로 접힘 |
| 문자열 비교 | 콜레이션에 따라 대소문자 무시(`_ci`)가 기본 | 대소문자 구분. `ILIKE`, `citext`, 비결정적 ICU 콜레이션 사용 |
| UPSERT | `ON DUPLICATE KEY UPDATE`, `REPLACE` | `ON CONFLICT DO UPDATE`, `MERGE` |
| JSON | `JSON`(longtext 별칭) + 함수 | `jsonb` 네이티브, 인덱싱, JSON path |
| 배열·범위 타입 | 없음 | 있음 |
| 복제 | 바이너리 로그(statement/row), Galera | WAL 스트리밍, 논리 복제 |
| 확장 | 플러그인 | `CREATE EXTENSION` 생태계 (PostGIS, pgvector 등) |
| 파티셔닝 | 있음 | 선언적 파티셔닝, 외부 테이블 파티션 |
| 프로시저 언어 | SQL/PSM | PL/pgSQL, PL/Python 등 다수 |
| 기본 포트 | 3306 | 5432 |
| 로컬 인증 | `unix_socket` | `peer` |
| 클라이언트 | `mariadb`, `mysql` | `psql` |
| 덤프 | `mariadb-dump`, `mysqldump` | `pg_dump`, `pg_dumpall` |
| 설정 파일 | `/etc/mysql/mariadb.conf.d/*.cnf` | `/etc/postgresql/16/main/postgresql.conf`, `pg_hba.conf` |
| 데이터 경로 | `/var/lib/mysql` | `/var/lib/postgresql/16/main` |
| 라이선스 | GPLv2 | PostgreSQL License |

### 10.1 계층 대응

MariaDB에서 database와 schema는 동의어라서 3단계 구조이고, PostgreSQL은 "database"라는 단어를 한 단계 위에 써서 4단계 구조다. 그래서 같은 단어가 서로 다른 계층을 가리킨다.

| 계층 | MariaDB 용어 | PostgreSQL 용어 | 경계의 성격 |
|---|---|---|---|
| 서버 프로세스 | 인스턴스 (mysqld, datadir 하나) | 클러스터 (postmaster, 데이터 디렉터리 하나) | 프로세스·포트·계정·설정·WAL의 경계 |
| 접속 단위 | (없음) | database | 접속이 붙는 단위. 다른 database는 일반 조인 불가 |
| 이름 공간 | database = schema | schema | 한 접속 안에서 넘나들며 조인 가능 |
| 테이블 | 테이블 | 테이블 |  |

```text
MariaDB 인스턴스                      PostgreSQL 클러스터
├─ shop   (database=schema)           ├─ shopdb (database)
│  ├─ orders                          │  ├─ public.orders   ─┐ 조인 가능
│  └─ customers                       │  └─ crm.customers   ─┘
└─ crm    (database=schema)           └─ analyticsdb (database)
   └─ leads                              └─ public.events   ← shopdb에서 조인 불가
   ↑ shop.orders JOIN crm.leads 가능
```

대응 관계 요약:

- MariaDB 인스턴스 = PostgreSQL 클러스터
- MariaDB database(=schema) = PostgreSQL schema
- PostgreSQL database = MariaDB에 없는 격리 계층
- 조인의 벽: MariaDB는 인스턴스, PostgreSQL은 database (같은 클러스터여도 database가 다르면 불가)

이전 구성 방식에 따른 영향:

- MariaDB에서 서비스마다 database를 나누고 서로 조인하던 구성은 PostgreSQL에서 **database 하나 + schema 여러 개**로 옮겨야 같은 방식으로 동작한다. database 여러 개로 나누면 조인이 막힌다.
- pgloader도 기본적으로 MariaDB database 하나를 PostgreSQL schema 하나로 옮긴다.
- 완전히 격리할 대상(다른 팀, 다른 고객)은 database로, 같은 서비스 안의 모듈 구분은 schema로 나누는 것이 일반적이다.

| MariaDB 명령 | PostgreSQL 대응 |
|---|---|
| `SHOW DATABASES` / `SHOW SCHEMAS` | `\dn` (schema 목록). `\l`은 database 목록이라 의미가 다름 |
| `USE shop` | `SET search_path TO shop` (schema 전환, 접속 유지) |
| 다른 인스턴스 접속 | `\c otherdb` (같은 클러스터의 다른 database로 재접속) |
| `CREATE DATABASE shop` | `CREATE SCHEMA shop` (기존 database 안에) |

## 11. 적합한 경우와 주의할 경우

### 적합

- 복잡한 쿼리, 분석, 리포팅 (윈도우 함수·CTE·플래너 성능)
- 반정형 데이터를 관계형과 함께 다룰 때 (`jsonb`)
- 공간 데이터 (PostGIS), 벡터 검색 (pgvector), 시계열 (TimescaleDB)
- 엄격한 데이터 무결성과 트랜잭션 DDL이 필요한 마이그레이션 중심 개발
- 표준 SQL 준수가 중요한 경우

### 주의

- 접속당 프로세스 모델이라 수천 개 짧은 접속에는 반드시 커넥션 풀러가 필요하다.
- 대량 UPDATE 테이블은 VACUUM과 테이블 팽창(bloat)을 관리해야 한다.
- 메이저 버전 업그레이드는 `pg_upgrade` 또는 논리 복제로 해야 하며, 데이터 디렉터리를 그대로 새 버전에서 열 수 없다.
- `count(*)`가 MyISAM처럼 즉시 반환되지 않는다(MVCC 특성상 스캔 필요).
- 한국어 형태소 기반 전문 검색은 내장되어 있지 않다.
