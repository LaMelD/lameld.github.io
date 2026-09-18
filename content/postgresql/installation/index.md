---
title: "PostgreSQL 설치 전에 결정할 것: 로케일과 버전"
date: 2026-09-18
weight: 1
tags: [postgresql, database, ubuntu, locale, pgdg]
description: "Ubuntu에 PostgreSQL을 설치하기 전에 정해야 하는 두 가지 — 클러스터 로케일과 메이저 버전 — 을 정리하고, 기존 RDBMS가 돌고 있는 서버에 얹을 때의 충돌 검토와 설치 후 확인 절차를 덧붙였다."
---

PostgreSQL 설치 자체는 `apt install` 한 줄이다. 문제는 그 한 줄이 **나중에 되돌릴 수 없는 결정 두 개**를 대신 내려버린다는 데 있다.

1. 클러스터 로케일
2. 메이저 버전

둘 다 설치 시점에 정해지고, 특히 로케일은 클러스터를 재생성하지 않으면 바꿀 수 없다. 설치 명령을 치기 전에 읽어야 하는 이유다.

## 1. 로케일 — 되돌릴 수 없는 선택

### 무슨 일이 일어나는가

Ubuntu 패키지는 설치 시점에 `main` 클러스터를 **자동 생성**하며, 그때 셸의 `LANG` 값을 클러스터 로케일로 가져다 쓴다.

서버에 `ko_KR.UTF-8` 로케일이 생성돼 있지 않으면(대부분의 서버 기본 상태가 그렇다) 그대로 `C.UTF-8` 클러스터가 만들어진다. 아무 생각 없이 설치하면 이쪽이다.

### C.UTF-8도 대부분 문제없다

한국어 데이터를 다룬다고 해서 `ko_KR.UTF-8`이 반드시 필요한 것은 아니다. `C.UTF-8`이 오히려 유리한 점이 있다.

- **한글 완성형 음절은 유니코드 코드포인트 순서가 가나다 순과 같다.** 가·나·다… 정렬이 그냥 맞는다.
- 문자열 비교가 더 빠르다. 로케일 규칙을 타지 않고 바이트 순서로 비교한다.
- 인덱스에 `text_pattern_ops` 없이도 `LIKE 'abc%'` 최적화가 걸린다.

### ko_KR.UTF-8이 필요한 경우

언어 규칙에 맞는 정렬이 실제로 필요할 때다.

- 자모가 분리된 문자(`ㄱ` + `ㅏ` 형태)
- 호환 한글 영역 문자
- 대소문자·악센트가 섞인 정렬

이런 데이터를 정렬해야 한다면 설치 전에 로케일을 만들어야 한다.

```bash
sudo locale-gen ko_KR.UTF-8
```

### 권장: 기본은 C.UTF-8, 필요한 컬럼만 ICU

클러스터 로케일은 나중에 바꿀 수 없지만, **컬럼 단위 정렬은 ICU `COLLATION`으로 나중에 추가할 수 있다.**

그래서 실무에서는 이 조합이 안전하다.

- 클러스터: `C.UTF-8`
- 언어 정렬이 필요한 컬럼만: `ko-KR` ICU 콜레이션

```sql
CREATE COLLATION ko_kr_icu (provider = icu, locale = 'ko-KR');
ALTER TABLE member ALTER COLUMN name TYPE text COLLATE ko_kr_icu;
```

클러스터 생성 시점에 ICU를 기본 제공자로 쓰고 싶다면 `initdb` 옵션을 넘긴다.

```bash
sudo pg_createcluster 18 main -- --locale-provider=icu --icu-locale=ko-KR
```

## 2. 버전 — 기본 저장소냐 PGDG냐

### Ubuntu 기본 저장소

Ubuntu 24.04의 apt 기본 저장소가 주는 것은 **PostgreSQL 16**이다(커뮤니티 지원 종료 2028-11). 추가 설정이 필요 없고 OS 업데이트와 함께 관리된다는 것이 장점이다.

### PGDG 공식 저장소

17·18이 필요하면 [apt.postgresql.org](https://apt.postgresql.org) 저장소를 등록한다.

```bash
sudo apt install -y postgresql-common
sudo /usr/share/postgresql-common/pgdg/apt.postgresql.org.sh
sudo apt update
sudo apt install postgresql-18
```

PGDG를 쓰면 얻는 것:

- **여러 메이저 버전을 한 서버에서 동시 운영**할 수 있다 (`pg_lsclusters`로 확인)
- 마이너 업데이트가 더 빨리 나온다
- 메이저 업그레이드를 `pg_upgradecluster`로 나란히 놓고 옮길 수 있다

### 설치 순서에 따른 함정

**기본 저장소로 16을 먼저 깔고 나중에 PGDG로 18을 설치하면, 18 클러스터가 자동 생성되지 않는다.**

`postgresql-common`은 이미 다른 클러스터가 있으면 `main` 클러스터 생성을 건너뛴다. 이때는 수동으로 만들어야 한다.

```bash
sudo pg_createcluster --locale ko_KR.UTF-8 18 main --start
```

또 하나. 16이 이미 5432를 점유한 상태에서 18 클러스터를 만들면 **5433으로 배정된다.** 16을 제거한 뒤 18을 5432로 옮기려면 설정을 직접 고쳐야 한다.

```bash
sudo pg_dropcluster --stop 16 main
sudo apt purge postgresql-16
# /etc/postgresql/18/main/postgresql.conf 의 port 를 5432 로 수정
sudo pg_ctlcluster 18 main restart
```

처음부터 PGDG로 원하는 버전 하나만 까는 편이 깔끔하다.

## 3. 기존 RDBMS가 돌고 있는 서버라면

MySQL·MariaDB가 이미 운영 중인 서버에 PostgreSQL을 얹는 경우를 검토해 보면, 막는 제약은 사실상 없다.

### 패키지

MariaDB 계열(`mariadb-server`, `mariadb-common`, `mysql-common`)과 PostgreSQL 계열(`postgresql-*`, `postgresql-common`, `postgresql-client-common`)은 **서로 의존·충돌 관계가 없다.** 클라이언트 라이브러리 `libmariadb3`·`libmysqlclient21`과 `libpq5`도 독립적으로 공존한다.

### 포트

MySQL·MariaDB 3306, PostgreSQL 5432. 겹치지 않는다. 다만 설치 전에 5432·5433이 비어 있는지는 확인한다.

```bash
ss -tlnp | grep -E ':(3306|5432|5433)\b'
```

### 자원

- PostgreSQL 패키지와 초기 클러스터는 100MB 미만이다.
- 기본 `shared_buffers`는 128MB라 메모리 부담이 거의 없다.
- **다만 두 DB를 함께 돌린다면 각 DB의 메모리 상한을 명시적으로 잡아 두는 편이 낫다.** 기본값으로 두면 둘 다 필요할 때 가져가려 하고, 스왑이 없는 서버에서는 OOM 킬러가 둘 중 하나를 죽인다.

### 외부 접속 기본값 차이

이 차이는 알고 있어야 한다.

| | 기본 바인딩 |
|---|---|
| MySQL·MariaDB | 배포판에 따라 `0.0.0.0` (모든 인터페이스) |
| PostgreSQL | `listen_addresses = 'localhost'` |

PostgreSQL은 기본적으로 외부 접속이 막혀 있다. 열려면 `postgresql.conf`의 `listen_addresses`와 `pg_hba.conf`를 **함께** 고쳐야 하고, 방화벽도 같이 봐야 한다.

반대로 말하면, 옆에서 돌던 DB가 모든 인터페이스에 열려 있었다고 해서 PostgreSQL도 그럴 거라고 가정하면 안 된다는 뜻이다.

### 컨테이너와 같이 쓸 때

- 나중에 PostgreSQL 컨테이너를 띄우면서 호스트 5432를 바인딩하면 네이티브 설치본과 충돌한다. 둘 중 하나만 쓰거나, 컨테이너는 다른 호스트 포트(예: 5433)를 쓴다.
- 컨테이너에서 호스트의 DB에 붙어야 한다면 호스트는 `172.17.0.1` 또는 `host.docker.internal`(`extra_hosts` 설정 필요)로 보인다. 이때 PostgreSQL의 `listen_addresses`와 `pg_hba.conf`에 도커 네트워크 대역을 허용해야 한다.

## 4. 설치

```bash
sudo apt update

# 기본 저장소 (16)
sudo apt install postgresql-16

# 또는 PGDG (18)
sudo apt install -y postgresql-common
sudo /usr/share/postgresql-common/pgdg/apt.postgresql.org.sh
sudo apt update
sudo apt install postgresql-18
```

한국어 로케일 클러스터가 필요하면 `LANG`을 지정해서 설치한다.

```bash
sudo locale-gen ko_KR.UTF-8
sudo LANG=ko_KR.UTF-8 apt install postgresql-18
```

이미 설치한 뒤 로케일을 바꾸고 싶다면 클러스터를 재생성한다. **데이터가 지워지므로 빈 상태에서만 쓴다.**

```bash
sudo pg_dropcluster --stop 18 main
sudo pg_createcluster --locale ko_KR.UTF-8 18 main
sudo pg_ctlcluster 18 main start
```

## 5. 설치 후 확인

```bash
pg_lsclusters                                     # 버전, 클러스터, 포트, 상태
sudo systemctl status postgresql@18-main
sudo -u postgres psql -c "SELECT version();"
sudo -u postgres psql -c "SHOW lc_collate;"       # C.UTF-8 또는 ko_KR.UTF-8
sudo -u postgres psql -c "SHOW server_encoding;"  # UTF8
ss -tlnp | grep -E ':(3306|5432)\b'               # 각자 포트에서 수신하는지
```

확인해 둘 기본값 몇 가지.

| 항목 | 기본값 |
|---|---|
| `listen_addresses` | `localhost` (외부 접속 차단) |
| `shared_buffers` | 128MB |
| `max_connections` | 100 |
| 데이터 체크섬 | 18부터 on |
| `io_method` | 18부터 `worker` |
| 인증 | `postgres` 슈퍼유저, peer 인증, 비밀번호 미설정 |

## 6. 생성되는 경로

| 용도 | 경로 |
|---|---|
| 설정 | `/etc/postgresql/18/main/postgresql.conf`, `pg_hba.conf`, `pg_ident.conf` |
| 데이터 | `/var/lib/postgresql/18/main/` |
| 로그 | `/var/log/postgresql/postgresql-18-main.log` |
| 바이너리 | `/usr/lib/postgresql/18/bin/` |
| 소켓 | `/var/run/postgresql/.s.PGSQL.5432` |

`postgres` 시스템 계정은 패키지 설치 시 자동 생성된다.

## 7. 설치 전 점검 명령어

새 서버에 얹기 전에 한 번 돌려 보면 좋은 것들.

```bash
# OS·자원
cat /etc/os-release; uname -m; nproc; free -h; df -h /

# 기존 DB
dpkg -l | grep -iE 'mariadb|mysql|postgres|libpq'
systemctl is-active mariadb mysql

# PostgreSQL 흔적
id postgres; ls -ld /var/lib/postgresql /etc/postgresql /usr/lib/postgresql
apt-cache policy postgresql postgresql-16
grep -rhs pgdg /etc/apt/sources.list /etc/apt/sources.list.d/

# 포트·로케일·공유메모리
ss -tlnp
locale -a | grep -i utf
sysctl kernel.shmmax kernel.shmall
```

요즘 커널은 `kernel.shmmax`·`kernel.shmall`이 사실상 무제한이라 예전처럼 손댈 일은 거의 없다.

## 요약

설치 전에 답해 둘 것은 두 줄이다.

1. **로케일** — 특별한 이유가 없으면 `C.UTF-8`. 언어 정렬이 필요한 컬럼만 ICU 콜레이션으로 따로 처리한다. 클러스터 로케일은 나중에 못 바꾼다.
2. **버전** — 기본 저장소의 16으로 충분한지, PGDG로 18을 쓸지. 섞어서 깔면 클러스터 자동 생성과 포트 배정에서 손이 간다.
