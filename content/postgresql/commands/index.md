---
title: "PostgreSQL 사용법과 명령어: 설치, psql, 권한, 백업, 모니터링"
date: 2026-09-17
weight: 2
tags: [postgresql, database, psql, mariadb]
description: "Ubuntu 24.04와 PostgreSQL 16 기준 설치, 클러스터 관리, psql 메타 명령어, 역할과 권한, 설정, 백업·복구, 유지보수, 모니터링 명령을 정리하고 MariaDB 사용자를 위한 치트시트를 붙였다."
---

Ubuntu 24.04 + PostgreSQL 16 기준. 다른 버전은 경로의 `16`을 바꾼다.

## 1. 설치

### 1.1 Ubuntu 기본 저장소 (16)

```bash
sudo apt update
sudo apt install postgresql-16          # 서버 + contrib 모듈 포함
sudo apt install postgresql-client-16   # 클라이언트만 필요할 때
```

한국어 로케일 클러스터를 원하면 설치 전에 로케일을 만들고 `LANG`을 지정한다.

```bash
sudo locale-gen ko_KR.UTF-8
sudo LANG=ko_KR.UTF-8 apt install postgresql-16
```

### 1.2 PGDG 공식 저장소 (17·18 또는 최신 마이너)

```bash
sudo apt install -y postgresql-common
sudo /usr/share/postgresql-common/pgdg/apt.postgresql.org.sh
sudo apt update
sudo apt install postgresql-17          # 원하는 메이저 버전
```

### 1.3 설치 확인

```bash
pg_lsclusters
psql --version
sudo systemctl status postgresql@16-main
sudo -u postgres psql -c "SELECT version();"
```

## 2. 서비스와 클러스터 관리

### 2.1 systemd

```bash
sudo systemctl start|stop|restart|reload|status postgresql@16-main
sudo systemctl enable postgresql        # 부팅 시 모든 클러스터 시작
sudo systemctl status postgresql        # 전체 클러스터 상위 유닛
```

### 2.2 Debian/Ubuntu 클러스터 도구

여러 버전·여러 클러스터를 한 서버에서 운영할 수 있게 해 주는 래퍼다.

```bash
pg_lsclusters                              # 클러스터 목록, 포트, 상태
sudo pg_ctlcluster 16 main start|stop|restart|reload|status
sudo pg_createcluster 16 second -p 5433    # 새 클러스터 (포트 지정)
sudo pg_createcluster --locale ko_KR.UTF-8 16 main
sudo pg_createcluster 16 main -- --locale-provider=icu --icu-locale=ko-KR   # initdb 옵션 전달
sudo pg_dropcluster --stop 16 second       # 클러스터 삭제 (데이터 소멸)
sudo pg_upgradecluster 16 main             # 다음 메이저 버전으로 업그레이드
sudo pg_renamecluster 16 main primary
```

### 2.3 주요 경로

| 용도 | 경로 |
|---|---|
| 설정 | `/etc/postgresql/16/main/postgresql.conf` |
| 접속 인증 | `/etc/postgresql/16/main/pg_hba.conf` |
| 사용자 매핑 | `/etc/postgresql/16/main/pg_ident.conf` |
| 데이터 | `/var/lib/postgresql/16/main/` |
| 로그 | `/var/log/postgresql/postgresql-16-main.log` |
| 바이너리 | `/usr/lib/postgresql/16/bin/` |
| 유닉스 소켓 | `/var/run/postgresql/.s.PGSQL.5432` |
| 클러스터 생성 기본값 | `/etc/postgresql-common/createcluster.conf` |

## 3. 접속

### 3.1 psql

```bash
sudo -u postgres psql                       # 로컬 슈퍼유저 (peer 인증)
psql -U app -d appdb -h localhost -p 5432 -W  # 비밀번호 인증
psql "postgresql://app:secret@localhost:5432/appdb"   # URI
psql "host=localhost dbname=appdb user=app sslmode=require"
psql -d appdb -c "SELECT now();"            # 단일 명령
psql -d appdb -f script.sql                 # 파일 실행
psql -d appdb -At -c "SELECT id FROM t;"    # 정렬 없이 값만 (스크립트용)
psql -d appdb -X -q -v ON_ERROR_STOP=1 -f migrate.sql   # 오류 시 중단
```

### 3.2 비밀번호 저장

```bash
# ~/.pgpass  (권한 600 필수)
# hostname:port:database:username:password
localhost:5432:*:app:secret
chmod 600 ~/.pgpass
```

환경 변수: `PGHOST`, `PGPORT`, `PGUSER`, `PGDATABASE`, `PGPASSWORD`(권장하지 않음), `PGSSLMODE`

## 4. psql 메타 명령어

| 명령 | 설명 |
|---|---|
| `\?` | 메타 명령어 도움말 |
| `\h CREATE INDEX` | SQL 문법 도움말 |
| `\q` | 종료 |
| `\l` `\l+` | 데이터베이스 목록 |
| `\c dbname [user]` | 데이터베이스 전환 |
| `\conninfo` | 현재 접속 정보 |
| `\dn` | 스키마 목록 |
| `\dt` `\dt+` `\dt schema.*` | 테이블 목록 |
| `\d tbl` `\d+ tbl` | 테이블 구조 (MariaDB `DESCRIBE`) |
| `\di` `\dv` `\ds` `\dm` | 인덱스·뷰·시퀀스·구체화 뷰 |
| `\df` `\df+ fn` `\sf fn` | 함수 목록·정의 |
| `\du` `\du+` | 역할 목록 |
| `\dp tbl` `\z` | 권한 |
| `\dx` | 설치된 확장 |
| `\x` `\x auto` | 확장(세로) 출력 토글 |
| `\gx` | 이번 쿼리만 세로 출력 |
| `\timing` | 실행 시간 표시 |
| `\e` | 마지막 쿼리를 에디터로 편집 |
| `\i file.sql` | 파일 실행 |
| `\o out.txt` | 출력을 파일로 |
| `\copy tbl TO 'f.csv' CSV HEADER` | 클라이언트 측 CSV 내보내기 |
| `\copy tbl FROM 'f.csv' CSV HEADER` | CSV 가져오기 |
| `\watch 2` | 마지막 쿼리를 2초마다 반복 |
| `\set AUTOCOMMIT off` | 자동 커밋 해제 |
| `\password [user]` | 비밀번호 변경 (평문 로그 남지 않음) |
| `\! cmd` | 셸 명령 |
| `\encoding` | 클라이언트 인코딩 |
| `\pset null '(null)'` | NULL 표시 방식 |

## 5. 역할(사용자)과 데이터베이스

### 5.1 생성·변경·삭제

```sql
-- 역할
CREATE ROLE app WITH LOGIN PASSWORD 'secret';
CREATE ROLE readonly NOLOGIN;
CREATE USER admin WITH SUPERUSER PASSWORD 'x';   -- CREATE USER = CREATE ROLE ... LOGIN
ALTER ROLE app WITH PASSWORD 'new';
ALTER ROLE app CONNECTION LIMIT 20;
ALTER ROLE app SET search_path = app, public;
GRANT readonly TO app;
DROP ROLE app;

-- 데이터베이스
CREATE DATABASE appdb OWNER app ENCODING 'UTF8';
CREATE DATABASE appdb2 TEMPLATE template0 LC_COLLATE 'C.UTF-8' LC_CTYPE 'C.UTF-8';
ALTER DATABASE appdb OWNER TO app;
DROP DATABASE appdb;                              -- 접속 중이면 실패
DROP DATABASE appdb WITH (FORCE);                 -- 접속 강제 종료 후 삭제

-- 스키마
CREATE SCHEMA app AUTHORIZATION app;

-- 권한
GRANT CONNECT ON DATABASE appdb TO readonly;
GRANT USAGE ON SCHEMA public TO readonly;
GRANT SELECT ON ALL TABLES IN SCHEMA public TO readonly;
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT SELECT ON TABLES TO readonly;  -- 앞으로 만들 테이블에도
GRANT ALL PRIVILEGES ON DATABASE appdb TO app;
REVOKE ALL ON SCHEMA public FROM PUBLIC;
```

셸 도구:

```bash
sudo -u postgres createuser --pwprompt app
sudo -u postgres createdb -O app appdb
sudo -u postgres dropdb appdb
sudo -u postgres dropuser app
```

### 5.2 권한의 두 층

PostgreSQL은 역할(ROLE) 하나로 사용자와 그룹을 모두 표현한다. 권한은 역할 자체에 붙는 **속성**과 객체마다 `GRANT`로 주는 **객체 권한** 두 층으로 나뉜다.

역할 속성은 `CREATE ROLE` / `ALTER ROLE`로 정하고 클러스터 전역에 적용된다.

| 속성 | 의미 |
|---|---|
| `LOGIN` | 접속 가능 여부. 없으면 그룹 역할 |
| `SUPERUSER` | 모든 권한 검사 우회 |
| `CREATEDB`, `CREATEROLE` | DB·역할 생성 권한 |
| `REPLICATION`, `BYPASSRLS` | 복제 접속, 행 수준 보안 우회 |
| `PASSWORD`, `VALID UNTIL`, `CONNECTION LIMIT` | 비밀번호, 만료, 동시 접속 수 |

객체 권한은 계층마다 따로 있고, **위 계층 권한이 없으면 아래를 줘도 소용없다.** 테이블 `SELECT`를 줬는데 스키마 `USAGE`가 없어서 막히는 것이 가장 흔한 실수다.

| 계층 | 권한 |
|---|---|
| 접속 경로 | `pg_hba.conf` (역할 밖의 관문) |
| 데이터베이스 | `CONNECT`, `CREATE`, `TEMP` |
| 스키마 | `USAGE` (조회), `CREATE` (객체 생성) |
| 테이블·뷰 | `SELECT`, `INSERT`, `UPDATE`, `DELETE`, `TRUNCATE`, `REFERENCES`, `TRIGGER` |
| 컬럼 | `SELECT (col1, col2)`, `UPDATE (col)` 처럼 컬럼 단위 지정 |
| 시퀀스 | `USAGE`, `SELECT`, `UPDATE` (IDENTITY 컬럼 INSERT에 필요) |
| 함수 | `EXECUTE` |

객체의 소유자는 그 객체에 대해 모든 권한을 가지며 `GRANT`로 나눠 줄 수 있다. 소유권은 `ALTER TABLE t OWNER TO role`로 넘긴다.

### 5.3 그룹 역할 + 기본 권한 패턴

역할에 직접 권한을 주지 말고, `NOLOGIN` 그룹 역할에 권한을 모은 뒤 사용자를 멤버로 넣는 방식이 표준이다.

```sql
-- 그룹 역할
CREATE ROLE app_rw NOLOGIN;
CREATE ROLE app_ro NOLOGIN;

-- 계층별 권한
GRANT CONNECT ON DATABASE appdb TO app_rw, app_ro;
GRANT USAGE ON SCHEMA public TO app_rw, app_ro;
GRANT SELECT ON ALL TABLES IN SCHEMA public TO app_ro;
GRANT SELECT, INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA public TO app_rw;
GRANT USAGE, SELECT ON ALL SEQUENCES IN SCHEMA public TO app_rw;
GRANT CREATE ON SCHEMA public TO app_rw;          -- 15 이후 앱이 테이블을 만들려면 필요

-- 앞으로 만들어질 객체에도 자동 적용 (객체를 만드는 역할 기준)
ALTER DEFAULT PRIVILEGES FOR ROLE postgres IN SCHEMA public
    GRANT SELECT ON TABLES TO app_ro;
ALTER DEFAULT PRIVILEGES FOR ROLE postgres IN SCHEMA public
    GRANT SELECT, INSERT, UPDATE, DELETE ON TABLES TO app_rw;
ALTER DEFAULT PRIVILEGES FOR ROLE postgres IN SCHEMA public
    GRANT USAGE, SELECT ON SEQUENCES TO app_rw;

-- 실제 로그인 사용자
CREATE ROLE app_user LOGIN PASSWORD 'secret';
CREATE ROLE analyst LOGIN PASSWORD 'secret';
GRANT app_rw TO app_user;
GRANT app_ro TO analyst;
```

- `GRANT ... ON ALL TABLES`는 **지금 있는** 객체에만, `ALTER DEFAULT PRIVILEGES`는 **앞으로 생길** 객체에만 적용된다. 둘 다 해야 빠짐이 없다.
- `ALTER DEFAULT PRIVILEGES`는 `FOR ROLE`에 지정한 역할이 만드는 객체에만 적용된다. 테이블을 만드는 역할이 여럿이면 각각 설정한다.
- 멤버십은 기본이 `INHERIT`라 그룹 권한을 바로 쓴다. 필요할 때만 쓰게 하려면 `NOINHERIT`로 만들고 `SET ROLE app_rw`로 전환한다.
- 16부터 `GRANT app_rw TO app_user WITH INHERIT FALSE, SET TRUE`처럼 멤버십 단위로 상속·전환 여부를 지정할 수 있다.

14 이후에는 자주 쓰는 묶음이 사전 정의 역할로 있다.

```sql
GRANT pg_read_all_data TO analyst;    -- 모든 DB의 모든 테이블 읽기
GRANT pg_write_all_data TO etl;       -- 쓰기
GRANT pg_monitor TO grafana;          -- 통계 뷰·설정 조회
GRANT pg_read_server_files TO loader; -- 서버 파일 COPY FROM
GRANT pg_checkpoint TO ops;           -- CHECKPOINT 실행 (15 이후)
```

### 5.4 권한 확인·회수·정리

```sql
\du                          -- 역할과 속성, 소속 그룹
\l                           -- DB 권한 (Access privileges 열)
\dn+                         -- 스키마 권한
\dp users                    -- 테이블·컬럼 권한
\ddp                         -- 기본 권한 설정
SELECT has_table_privilege('analyst', 'users', 'SELECT');
SELECT has_schema_privilege('analyst', 'public', 'USAGE');
SELECT grantee, table_name, privilege_type
FROM information_schema.role_table_grants WHERE grantee = 'analyst';
SELECT rolname FROM pg_roles WHERE pg_has_role('app_user', oid, 'member');   -- 소속 그룹

REVOKE INSERT ON users FROM app_rw;
REVOKE app_rw FROM app_user;
REVOKE ALL ON SCHEMA public FROM PUBLIC;     -- 모든 역할에 암묵 부여된 권한 회수
REVOKE CONNECT ON DATABASE appdb FROM PUBLIC; -- 기본적으로 모든 역할이 CONNECT 가능하므로
```

`\dp` 출력의 권한 문자: `r`=SELECT, `w`=UPDATE, `a`=INSERT, `d`=DELETE, `D`=TRUNCATE, `x`=REFERENCES, `t`=TRIGGER, `U`=USAGE, `C`=CREATE, `c`=CONNECT, `T`=TEMP, `X`=EXECUTE, `*`=GRANT OPTION 포함. `=r/postgres`는 PUBLIC에 postgres가 SELECT를 준 것.

역할을 삭제할 때는 소유 객체와 받은 권한을 먼저 정리해야 한다. 그렇지 않으면 `role "x" cannot be dropped because some objects depend on it` 오류가 난다.

```sql
REASSIGN OWNED BY old_user TO postgres;   -- 소유 객체를 넘기고 (접속한 DB 기준)
DROP OWNED BY old_user;                   -- 남은 권한을 지운 뒤
DROP ROLE old_user;                       -- 여러 DB에 객체가 있으면 각 DB에서 반복
```

### 5.5 MariaDB와 다른 점

| MariaDB | PostgreSQL |
|---|---|
| `'user'@'host'` 로 접속 출처 제어 | 역할에는 호스트가 없음. 출처 제어는 `pg_hba.conf` |
| `GRANT ALL ON db.* TO u` 한 줄 | DB `CONNECT`  • 스키마 `USAGE`  • 테이블 권한 + 기본 권한을 각각 |
| `FLUSH PRIVILEGES` | 없음. `GRANT` 즉시 반영 |
| 사용자와 역할(10.0.5+)이 별개 | 역할 하나로 통합. `LOGIN` 여부만 다름 |
| `SHOW GRANTS FOR u` | `\du u`, `\dp`, `information_schema.role_table_grants` |
| `mysql.user` 테이블 | `pg_roles` (비밀번호 제외), `pg_authid` (슈퍼유저만) |
| `DROP USER`로 바로 삭제 | 소유 객체·권한 정리 후 `DROP ROLE` |
| 기본 DB에 누구나 CREATE | 15부터 `public` 스키마에 일반 역할의 `CREATE` 없음 |

## 6. 설정

### 6.1 postgresql.conf 주요 항목

```text
listen_addresses = 'localhost'      # '*' 또는 '0.0.0.0,::' 로 외부 허용
port = 5432
max_connections = 100
shared_buffers = 128MB              # RAM 25% 권장
work_mem = 4MB
maintenance_work_mem = 64MB
effective_cache_size = 4GB
wal_level = replica                 # 논리 복제는 logical
max_wal_size = 1GB
checkpoint_completion_target = 0.9
random_page_cost = 1.1              # SSD
log_min_duration_statement = 1000   # 1초 넘는 쿼리 로깅 (ms)
log_line_prefix = '%m [%p] %u@%d '
timezone = 'Asia/Seoul'
shared_preload_libraries = 'pg_stat_statements'   # 변경 시 재시작 필요
```

이 서버(15GB RAM, MariaDB와 공유) 예시:

```text
shared_buffers = 2GB
effective_cache_size = 6GB
work_mem = 16MB
maintenance_work_mem = 512MB
max_connections = 100
```

### 6.2 설정 변경 방법

```sql
SHOW shared_buffers;
SHOW ALL;
SELECT name, setting, unit, context FROM pg_settings WHERE name LIKE 'log%';
-- context가 postmaster면 재시작, sighup이면 reload, user면 세션 단위

ALTER SYSTEM SET work_mem = '16MB';      -- postgresql.auto.conf에 기록
ALTER SYSTEM RESET work_mem;
SELECT pg_reload_conf();                 -- reload
SET work_mem = '64MB';                   -- 현재 세션만
```

```bash
sudo systemctl reload postgresql@16-main    # sighup 항목
sudo systemctl restart postgresql@16-main   # postmaster 항목
```

### 6.3 pg_hba.conf

형식: `TYPE  DATABASE  USER  ADDRESS  METHOD`. 위에서부터 첫 일치 규칙이 적용된다.

```text
# TYPE  DATABASE        USER            ADDRESS                 METHOD
local   all             postgres                                peer
local   all             all                                     peer
host    all             all             127.0.0.1/32            scram-sha-256
host    all             all             ::1/128                 scram-sha-256
# 내부망 허용 예시
host    appdb           app             192.168.0.0/24          scram-sha-256
# 도커 브리지 네트워크 허용 예시
host    appdb           app             172.16.0.0/12           scram-sha-256
# SSL 강제
hostssl all             all             0.0.0.0/0               scram-sha-256
```

- `peer`: OS 사용자명과 DB 역할명이 같으면 비밀번호 없이 접속(로컬 소켓 전용)
- `scram-sha-256`: 비밀번호. `md5`는 구식이므로 쓰지 않는다.
- `trust`: 인증 없음. 로컬 테스트 외 금지.
- 수정 후 `SELECT pg_reload_conf();` 또는 `systemctl reload`

### 6.4 외부 접속 열기 절차

1. `postgresql.conf`: `listen_addresses = '*'`
2. `pg_hba.conf`: 허용할 대역과 `scram-sha-256` 규칙 추가
3. `sudo systemctl restart postgresql@16-main`
4. 방화벽: `sudo ufw allow from 192.168.0.0/24 to any port 5432`
5. 확인: `ss -tlnp | grep 5432`, 원격에서 `psql -h server -U app -d appdb`

## 7. 기본 SQL 예제

```sql
CREATE TABLE users (
    id          bigint GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    email       text NOT NULL UNIQUE,
    name        text NOT NULL,
    profile     jsonb DEFAULT '{}'::jsonb,
    tags        text[] DEFAULT '{}',
    is_active   boolean NOT NULL DEFAULT true,
    created_at  timestamptz NOT NULL DEFAULT now()
);

CREATE INDEX users_created_at_idx ON users (created_at DESC);
CREATE INDEX users_profile_gin ON users USING gin (profile);
CREATE INDEX users_lower_email ON users (lower(email));
CREATE INDEX CONCURRENTLY users_active_idx ON users (id) WHERE is_active;

INSERT INTO users (email, name, profile) VALUES ('a@x.com', 'Kim', '{"age": 30}') RETURNING id;

-- UPSERT
INSERT INTO users (email, name) VALUES ('a@x.com', 'Kim2')
ON CONFLICT (email) DO UPDATE SET name = EXCLUDED.name;

-- JSONB
SELECT profile->>'age' FROM users WHERE profile @> '{"age": 30}';
UPDATE users SET profile = profile || '{"city": "Seoul"}' WHERE id = 1;

-- 배열
SELECT * FROM users WHERE 'vip' = ANY(tags);
UPDATE users SET tags = array_append(tags, 'vip') WHERE id = 1;

-- 윈도우, CTE
WITH ranked AS (
    SELECT id, name, row_number() OVER (ORDER BY created_at DESC) AS rn FROM users
)
SELECT * FROM ranked WHERE rn <= 10;

-- 트랜잭션
BEGIN;
UPDATE users SET is_active = false WHERE id = 1;
SAVEPOINT sp1;
DELETE FROM users WHERE id = 2;
ROLLBACK TO SAVEPOINT sp1;
COMMIT;

-- 실행 계획
EXPLAIN (ANALYZE, BUFFERS) SELECT * FROM users WHERE email = 'a@x.com';

-- 컬럼 단위 한국어 정렬 (C.UTF-8 클러스터에서)
CREATE COLLATION ko (provider = icu, locale = 'ko-KR');
SELECT name FROM users ORDER BY name COLLATE "ko";
```

## 8. 백업과 복구

### 8.1 논리 백업

```bash
# 커스텀 포맷 (압축, 선택 복원 가능, 권장)
sudo -u postgres pg_dump -Fc appdb -f appdb.dump
# 평문 SQL
sudo -u postgres pg_dump appdb > appdb.sql
# 스키마만 / 데이터만 / 특정 테이블
sudo -u postgres pg_dump -s appdb > schema.sql
sudo -u postgres pg_dump -a -t users appdb > users_data.sql
# 병렬 디렉터리 포맷
sudo -u postgres pg_dump -Fd -j 4 appdb -f appdb_dir/
# 전체 클러스터 (역할, 테이블스페이스 포함)
sudo -u postgres pg_dumpall > all.sql
sudo -u postgres pg_dumpall --globals-only > globals.sql   # 역할만
```

### 8.2 복원

```bash
sudo -u postgres createdb appdb_restored
sudo -u postgres pg_restore -d appdb_restored appdb.dump
sudo -u postgres pg_restore -d appdb --clean --if-exists -j 4 appdb.dump
sudo -u postgres pg_restore -l appdb.dump              # 내용 목록
sudo -u postgres pg_restore -d appdb -t users appdb.dump  # 특정 테이블만
sudo -u postgres psql -d appdb -f appdb.sql            # 평문 SQL
sudo -u postgres psql -f all.sql postgres              # pg_dumpall 복원
```

### 8.3 물리 백업

```bash
sudo -u postgres pg_basebackup -D /backup/base -Fp -Xs -P -c fast
sudo -u postgres pg_basebackup -D /backup/base.tar -Ft -z -Xs -P   # tar.gz
# 17 이상 증분
sudo -u postgres pg_basebackup -D /backup/incr1 --incremental=/backup/base/backup_manifest
```

PITR은 `archive_mode = on`, `archive_command`로 WAL을 보관한 뒤 `recovery_target_time`으로 복구한다.

전문 도구: pgBackRest, Barman

### 8.4 크론 예시

```bash
# /etc/cron.d/pg-backup
0 3 * * * postgres pg_dump -Fc appdb -f /backup/pg/appdb_$(date +\%F).dump && find /backup/pg -name '*.dump' -mtime +14 -delete
```

## 9. 유지보수

```sql
VACUUM;                          -- 전체 DB 죽은 튜플 회수
VACUUM (VERBOSE, ANALYZE) users; -- 특정 테이블 + 통계
VACUUM FULL users;               -- 파일 재작성으로 공간 반환. 배타 락, 오래 걸림
ANALYZE;                         -- 플래너 통계 갱신
REINDEX INDEX CONCURRENTLY users_email_key;
REINDEX TABLE CONCURRENTLY users;
CLUSTER users USING users_created_at_idx;   -- 인덱스 순서로 물리 재정렬
```

autovacuum 확인·튜닝:

```sql
SELECT relname, n_dead_tup, n_live_tup, last_autovacuum, last_autoanalyze
FROM pg_stat_user_tables ORDER BY n_dead_tup DESC LIMIT 10;

ALTER TABLE events SET (autovacuum_vacuum_scale_factor = 0.02, autovacuum_vacuum_threshold = 1000);
```

```bash
sudo -u postgres vacuumdb --all --analyze-in-stages   # 업그레이드 후 통계 재생성
sudo -u postgres reindexdb appdb
```

## 10. 모니터링과 문제 해결

### 10.1 접속과 실행 중 쿼리

```sql
SELECT pid, usename, datname, state, wait_event_type, wait_event,
       now() - query_start AS runtime, left(query, 80) AS query
FROM pg_stat_activity
WHERE state <> 'idle' AND pid <> pg_backend_pid()
ORDER BY runtime DESC;

SELECT count(*), state FROM pg_stat_activity GROUP BY state;

SELECT pg_cancel_backend(12345);     -- 쿼리만 취소
SELECT pg_terminate_backend(12345);  -- 접속 종료
```

### 10.2 락 대기

```sql
SELECT pid, pg_blocking_pids(pid) AS blocked_by, state, left(query, 60)
FROM pg_stat_activity
WHERE cardinality(pg_blocking_pids(pid)) > 0;

SELECT locktype, relation::regclass, mode, granted, pid
FROM pg_locks WHERE NOT granted;
```

### 10.3 크기

```sql
SELECT datname, pg_size_pretty(pg_database_size(datname)) FROM pg_database ORDER BY 2 DESC;

SELECT relname, pg_size_pretty(pg_total_relation_size(c.oid)) AS total,
       pg_size_pretty(pg_relation_size(c.oid)) AS table_only
FROM pg_class c JOIN pg_namespace n ON n.oid = c.relnamespace
WHERE relkind = 'r' AND n.nspname NOT IN ('pg_catalog', 'information_schema')
ORDER BY pg_total_relation_size(c.oid) DESC LIMIT 10;
```

### 10.4 느린 쿼리 (pg_stat_statements)

```bash
# postgresql.conf 에 shared_preload_libraries = 'pg_stat_statements' 추가 후 재시작
```

```sql
CREATE EXTENSION IF NOT EXISTS pg_stat_statements;

SELECT calls, round(total_exec_time::numeric, 1) AS total_ms,
       round(mean_exec_time::numeric, 2) AS mean_ms, rows, left(query, 80)
FROM pg_stat_statements ORDER BY total_exec_time DESC LIMIT 10;

SELECT pg_stat_statements_reset();
```

### 10.5 인덱스 사용률·캐시 적중률

```sql
SELECT relname, idx_scan, seq_scan, n_live_tup
FROM pg_stat_user_tables ORDER BY seq_scan DESC LIMIT 10;   -- seq_scan 많으면 인덱스 검토

SELECT indexrelname, idx_scan FROM pg_stat_user_indexes WHERE idx_scan = 0;  -- 안 쓰는 인덱스

SELECT round(sum(blks_hit) * 100.0 / nullif(sum(blks_hit + blks_read), 0), 2) AS cache_hit_pct
FROM pg_stat_database;
```

### 10.6 복제 상태

```sql
SELECT client_addr, state, sent_lsn, replay_lsn, replay_lag FROM pg_stat_replication;  -- primary
SELECT pg_is_in_recovery(), pg_last_wal_replay_lsn();                                  -- standby
SELECT slot_name, active, wal_status FROM pg_replication_slots;
```

### 10.7 로그

```bash
sudo tail -f /var/log/postgresql/postgresql-16-main.log
sudo journalctl -u postgresql@16-main -n 100
```

## 11. 확장 관리

```sql
SELECT name, default_version, installed_version FROM pg_available_extensions ORDER BY name;
CREATE EXTENSION IF NOT EXISTS pg_trgm;
CREATE EXTENSION IF NOT EXISTS pgcrypto;
CREATE EXTENSION IF NOT EXISTS pg_stat_statements;
ALTER EXTENSION pg_trgm UPDATE;
DROP EXTENSION pg_trgm;
\dx
```

apt로 추가 설치하는 확장 예시:

```bash
sudo apt install postgresql-16-postgis-3 postgresql-16-pgvector postgresql-16-cron
```

## 12. Docker로 실행할 때

호스트에 네이티브 PostgreSQL이 있으면 호스트 5432를 피해서 바인딩한다.

```yaml
services:
  db:
    image: postgres:16
    environment:
      POSTGRES_USER: app
      POSTGRES_PASSWORD: change_me
      POSTGRES_DB: appdb
      POSTGRES_INITDB_ARGS: "--locale-provider=icu --icu-locale=ko-KR"
    ports:
      - "5433:5432"          # 호스트 5432는 네이티브용
    volumes:
      - pgdata:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U app -d appdb"]
      interval: 10s
volumes:
  pgdata:
```

```bash
docker compose exec db psql -U app -d appdb
docker compose exec db pg_dump -U app -Fc appdb > appdb.dump
```

## 13. MariaDB 사용자를 위한 치트시트

### 13.1 명령 대응

| MariaDB | PostgreSQL |
|---|---|
| `mariadb -u root -p` | `psql -U postgres -W` 또는 `sudo -u postgres psql` |
| `SHOW DATABASES;` | `\l` |
| `USE db;` | `\c db` |
| `SHOW TABLES;` | `\dt` |
| `DESCRIBE t;` / `SHOW CREATE TABLE t;` | `\d t` / `\d+ t` |
| `SHOW INDEX FROM t;` | `\di t*` 또는 `\d t` |
| `SHOW PROCESSLIST;` | `SELECT * FROM pg_stat_activity;` |
| `KILL 123;` | `SELECT pg_terminate_backend(123);` |
| `SHOW VARIABLES LIKE 'x';` | `SHOW x;` / `SELECT * FROM pg_settings WHERE name = 'x';` |
| `SHOW GRANTS FOR u;` | `\du u`, `\dp` |
| `SELECT USER();` | `SELECT current_user, session_user;` |
| `SELECT DATABASE();` | `SELECT current_database();` |
| `mysqldump db > f.sql` | `pg_dump db > f.sql` |
| `mysql db < f.sql` | `psql -d db -f f.sql` |
| `FLUSH PRIVILEGES;` | 불필요 (즉시 적용) |
| `SET GLOBAL x = y;` | `ALTER SYSTEM SET x = y; SELECT pg_reload_conf();` |
| `OPTIMIZE TABLE t;` | `VACUUM (FULL, ANALYZE) t;` |
| `ANALYZE TABLE t;` | `ANALYZE t;` |
| `EXPLAIN SELECT ...` | `EXPLAIN (ANALYZE, BUFFERS) SELECT ...` |

### 13.2 SQL 차이

| MariaDB | PostgreSQL |
|---|---|
| `id INT AUTO_INCREMENT PRIMARY KEY` | `id bigint GENERATED ALWAYS AS IDENTITY PRIMARY KEY` |
| 백틱 인용 (`column`) | `"column"` (큰따옴표). 인용 안 하면 소문자로 접힘 |
| `TINYINT(1)` | `boolean` |
| `INT UNSIGNED` | 없음. `bigint`  • `CHECK (x >= 0)` |
| `DATETIME` | `timestamp` 또는 `timestamptz` |
| `TEXT`, `VARCHAR(255)` | `text` (길이 제한 불필요) |
| `ENUM('a','b')` 인라인 | `CREATE TYPE t AS ENUM ('a','b')` 후 사용 |
| `JSON` | `jsonb` |
| `INSERT ... ON DUPLICATE KEY UPDATE c = VALUES(c)` | `INSERT ... ON CONFLICT (key) DO UPDATE SET c = EXCLUDED.c` |
| `REPLACE INTO` | `ON CONFLICT DO UPDATE` |
| `INSERT IGNORE` | `ON CONFLICT DO NOTHING` |
| `LIMIT 10, 20` | `LIMIT 20 OFFSET 10` |
| `GROUP_CONCAT(x SEPARATOR ',')` | `string_agg(x, ',')` |
| `IFNULL(a, b)` | `COALESCE(a, b)` |
| `IF(cond, a, b)` | `CASE WHEN cond THEN a ELSE b END` |
| `CONCAT(a, b)` | `a \|\| b` 또는 `concat(a, b)` |
| `DATE_FORMAT(d, '%Y-%m')` | `to_char(d, 'YYYY-MM')` |
| `DATE_ADD(d, INTERVAL 1 DAY)` | `d + interval '1 day'` |
| `UNIX_TIMESTAMP()` | `extract(epoch from now())` |
| `RAND()` | `random()` |
| `SUBSTRING_INDEX(s, ',', 1)` | `split_part(s, ',', 1)` |
| `LIKE` (대소문자 무시) | `LIKE` 구분, `ILIKE` 무시 |
| `x REGEXP 'p'` | `x ~ 'p'` (`~*` 대소문자 무시) |
| `SELECT ... FOR UPDATE` | 동일 (`SKIP LOCKED`, `NOWAIT` 추가 지원) |
| `TRUNCATE t` | 동일 (트랜잭션 안에서 롤백 가능) |
| `ALTER TABLE t MODIFY c INT` | `ALTER TABLE t ALTER COLUMN c TYPE integer` |
| `ALTER TABLE t CHANGE a b INT` | `ALTER TABLE t RENAME COLUMN a TO b` |
| 스토리지 엔진 선택 | 없음 |
| 암묵 형변환 관대함 | 엄격. `'1' + 1`은 동작하지만 `'abc'::int`는 오류 |
| `0000-00-00` 날짜 허용 | 오류 |
| 문자열 큰따옴표 허용 | 문자열은 작은따옴표만 |

### 13.3 데이터 이전

```bash
sudo apt install pgloader
pgloader mysql://user:pass@localhost/srcdb postgresql://app:secret@localhost/appdb
```

pgloader가 타입 변환·인덱스·외래키를 자동으로 옮긴다. 저장 프로시저·트리거·뷰는 수동으로 옮겨야 한다.

## 14. 자주 겪는 문제

| 증상 | 원인·해결 |
|---|---|
| `Peer authentication failed for user "app"` | 로컬 소켓으로 접속하며 OS 사용자명이 다름. `-h localhost`를 붙여 TCP로 접속하거나 `pg_hba.conf`의 `local` 규칙을 `scram-sha-256`으로 변경 |
| `password authentication failed` | 비밀번호 오류 또는 역할에 `LOGIN` 없음. `\du`로 확인 |
| `no pg_hba.conf entry for host ...` | 해당 IP·DB·사용자 조합의 규칙 없음. `pg_hba.conf`에 추가 후 reload |
| `connection refused` | `listen_addresses`가 `localhost`이거나 방화벽. `ss -tlnp`로 확인 |
| `database "x" is being accessed by other users` | `DROP DATABASE x WITH (FORCE)` 또는 접속 종료 후 삭제 |
| `relation "Users" does not exist` | 인용 없는 식별자는 소문자로 접힘. `"Users"`로 만들었으면 항상 따옴표 필요 |
| `too many connections` | `max_connections` 초과. 풀러 도입 또는 idle 접속 정리 |
| `could not resize shared memory segment` | 도커에서 `shm_size` 부족. `shm_size: 256m` 지정 |
| `FATAL: lock file "postmaster.pid" already exists` | 비정상 종료 잔여 파일. 프로세스 없음 확인 후 삭제 |
| 디스크 사용량이 줄지 않음 | 일반 VACUUM은 공간을 OS에 반환하지 않음. `VACUUM FULL` 또는 `pg_repack` |
| 쿼리가 갑자기 느려짐 | 통계 오래됨. `ANALYZE` 실행, `pg_stat_statements`로 계획 변화 확인 |
| 한글 정렬이 기대와 다름 | 클러스터 로케일이 `C`. 컬럼에 ICU `COLLATE "ko"` 지정 |
