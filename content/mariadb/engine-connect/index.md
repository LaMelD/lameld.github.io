---
title: "컨넥트 스토리지 엔진"
date: 2023-04-09
weight: 3
---

## 특징

- 기능을 제공하는 스토리지 엔진

- 다른 서버의 mariaDB 테이블과 연결

- 이기종 RDBMS의 테이블과 연결

- 통신이 가능해야한다는 전제조건

- 파일 또는 디렉토리의 연결 또한 가능

- 스토리지 엔진이 설치되어 있지 않은 경우 추가 설치를 통해 사용 가능

---

## 설치

>확인
>```sql
>SHOW ENGINES;
>SHOW PLUGINS;
>```

>설치
>- 버전이 일치해야 한다.
>```
>yum install MariaDB-connect-engine-10.9.4
>```

>리눅스
>```sql
>INSTALL PLUGIN CONNECT SONAME 'ha_connect.so';
>```

>윈도우
>```sql
>INSTALL PLUGIN CONNECT SONAME 'ha_connect.dll';
>```

---

## 테이블 생성

```sql
    CREATE TABLE tbl_conn
    ENGINE=CONNECT
    TABLE_TYPE=mysql
    TABNAME=tbl_out
    CONNECTION='mysql://user:password@10.0.0.1';
```

```sql
    CREATE TABLE tbl_ora
    ENGINE=CONNECT
    TABLE_TYPE=odbc
    TABNAME=tbl_out
    CONNECTION='DSN=10.0.0.1;UID=user;PWD=password';
```

- TABNAME : 외부 서버에 존재하는 테이블 이름

- CONNECTION : 외부 서버 정보 및 접속 유저 정보

---

## 추가

- 테이블 생성 시 TABLE_TYPE

    - ini

    ```sql
        CREATE TABLE mysql_config (
            section VARCHAR(64) flag = 1,
            keyname VARCHAR(64) flag = 2,
            val VARCHAR(256)
        )
        ENGINE=CONNECT
        TABLE_TYPE=ini
        FILE_NAME = '/etc/my.cnf'
        OPTION_LIST='Layout=Row;seclen=90000';
    ```

    - dir
    ```sql
        create table temp_dir (
            path_name VARCHAR(@56) NOT NULL flag = 1,
            fname VARCHAR(256) NOT NULL,
            ftype CHAR(4) NOT NULL,
            size DOUBLE(12,0) NOT NULL flag = 5
        )
        ENGINE=CONNECT
        TABLE_TYPE=dir
        FILE_NAME='/data/*'
        OPTION_LIST='subdir=1';
    ```
