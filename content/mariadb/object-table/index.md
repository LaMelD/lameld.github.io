---
title: "테이블"
date: 2023-04-19
weight: 4
---

## 개념

1. 테이블의 컬럼과 행

  - 컬럼(Column) : 테이블에 저장될 데이터의 특성을 지정
  - 행(Row) : 컬럼에 정의된 형식으로 저장된 데이터를 의미

2. 데이터 타입

  - Number Data Type

    |데이터 타입|의미|크기|설명|
    |---|---|---|---|
    |BIT[(M)]|비트 수<br>M은 값당 비트 수를 의미|1|- 1에서 64 사이의 값<br>- M생략 시 기본값 1|
    |BOOL|TRUE 또는 FALSE||- TINYINT(1)의 동의어로 0은 False<br>- 0이 아닌 값은 True로 간주|
    |TINYINT[(M)]|매우 작은 정수|1 Byte|- (signed) -(2^7) ~ (2^7 - 1)<br>- (unsigned) 0 ~ (2^8 - 1)|
    |SMALLINT[(M)]|작은 정수|2 Byte|- (signed) -(2^15) ~ (2^15 - 1)<br>- (unsigned) 0 ~ (2^16 - 1)|
    |INT[(M)]|표준 정수|4 Byte|- (signed) -(2^31) ~ (2^31 - 1)<br>- (unsigned) 0 ~ (2^32 - 1)|
    |BIGINT[(M)]|큰 정수|8 Byte|- (signed) -(2^63) ~ (2^63 - 1)<br>- (unsigned) 0 ~ (2^64 - 1)|
    |FLOAT[(M,D)]|단정밀도 부동 소수|4 Byte|- (signed) -3.40E+38 ~ -1.17E-38 (유효숫자 7자리)<br>- 0<br>- (unsigned) 1.17E-38 ~ 3.40E+38 (유효숫자 8자리)|
    |DOUBLE[(M,D)]<br>DOUBLE PERCISION[(M,D)]<br>REAL[(M,D)]|배정밀도 부동 소수|8 Byte|- (signed) -1.22E-308 ~ 1.79E+308 (유효숫자 15자리)<br>- 0<br>- (unsigned) 2.22E-308 ~ 1.79E+308 (유효숫자 16자리)|
    |DECIMAL[(M[,D])]|고정 소수|길이 + 1|- 묶음 고정 소수점 숫자<br>- M은 전체 자리수 : 정밀도<br>- D는 소수점 뒷자리수 : 배율<br>- M의 최대 자리수는 65<br>- D의 최대수 10.2.1 기준 이전 30, 이후 38<br>- D 생략 시 기본값 : 0<br>- M 생략 시 기본값 : 10|
    |DEC[(M[,D])]<br>NUMERIC[(M[,D])]<br>FIXED[(M[,D])]|고정 소수||- DECIMAL과 동의어<br>- FIXED 동의어는 다른 데이터베이스 시스템과 호환을 위해서 사용 가능

  - Character String Data Type

    |데이터 타입|설명|크기|범위|
    |---|---|---|---|
    |CHAR[(M)]|고정 길이 문자열|M Byte|1 ~ 255|
    |VARCHAR[(M)]|가변 길이 문자열|M + 1 Byte|1 ~ 65535|
    |TINYTEXT[(M)]|매우 작은 문자열|M + 1 Byte|최대 255|
    |TEXT[(M)]|작은 문자열|M + 2 Byte|최대 65535|
    |MEDIUMTEXT[(M)]|중간 크기 문자열|M + 3 Byte|최대 16777215|
    |LONGTEXT[(M)]|큰 문자열|M + 4 Byte|최대 4294967295|
    |ENUM('v1','v2',...)|열거형<br>정해진 몇가지의 값 중 하나만 지정||- 최대 65535개의 개별 값 제한<br>- 내부적으로 정수값 표현|
    |SET('v1','v2',...)|집합형<br>정해진 몇가지의 값 중 여러 개를 지정||- 최대 64개의 요소로 구성 제한<br>- 내부적으로 정수값 표현|

    > CHAR 와 VARCHAR 데이터 타입의 차이점
    > - CHAR : 컬럼의 지정 값으로 저장
    > - VARCHAR : 실제 문자열 크기만큼 저장
    >> SET SESSION sql_mode='PAD_CHAR_TO_FULL_LENGTH' : 공백까지 읽을 수 있도록 SQL 모드를 설정
    >>
    >>  ```sql
    >>  DESC test_tbl;
    >>  +---------------+------------------+------+-----+---------+----------------+
    >>  | Field         | Type             | Null | Key | Default | Extra          |
    >>  +---------------+------------------+------+-----+---------+----------------+
    >>  | key           | int(11) unsigned | NO   | PRI | NULL    | auto_increment |
    >>  | value_float   | double           | YES  |     | NULL    |                |
    >>  | value_char    | char(3)          | YES  |     | NULL    |                |
    >>  | value_varchar | varchar(3)       | YES  |     | NULL    |                |
    >>  +---------------+------------------+------+-----+---------+----------------+
    >>  SELECT LENGTH(value_char), LENGTH(value_varchar) FROM test_tbl;
    >>  +--------------------+-----------------------+
    >>  | LENGTH(value_char) | LENGTH(value_varchar) |
    >>  +--------------------+-----------------------+
    >>  |                  1 |                     1 |
    >>  +--------------------+-----------------------+
    >>  SET SESSION sql_mode='PAD_CHAR_TO_FULL_LENGTH';
    >>  SELECT LENGTH(value_char), LENGTH(value_varchar) FROM test_tbl;
    >>  +--------------------+-----------------------+
    >>  | LENGTH(value_char) | LENGTH(value_varchar) |
    >>  +--------------------+-----------------------+
    >>  |                  3 |                     1 |
    >>  +--------------------+-----------------------+
    >>  ```

  - Binary String Data Type

    |데이터 타입|설명|크기|범위|
    |---|---|---|---|
    |BINARY[(M)]|고정 길이 바이너리 문자열|M Byte|
    |VARBINARY[(M)]|가변 길이 바이너리 문자열|M + 1 Byte<br>또는<br>M + 2 Byte||
    |TINYBLOB[(M)]|매우 작은 BLOB(Binary Large Object)|M + 1 Byte|최대 255|
    |BLOB[(M)]|작은 BLOB|M + 2 Byte|최대 65535|
    |MEDIUMBLOB[(M)]|중간 크기 BLOB|M + 3 Byte|최대 16777215|
    |LONGBLOB[(M)]|큰 BLOB|M + 4 Byte|최대 4294967295|

    > - Binary 데이터를 저장하므로 이미지 파일도 저장이 가능하다.
    > - ***처리 성능이 저하되므로 사용 시 주의가 필요하다.***

  - DATE Data Type

    |데이터 타입|설명|크기|범위|
    |---|---|---|---|
    |DATE|YYYY-MM-DD|3 Byte|1000-01-01 ~ 9999-12-31|
    |DATETIME|YYYY-MM-DD hh:mm:ss|8 Byte|1000-01-01 00:00:00 ~ 9999-12-31 23:59:59|
    |TIMESTAMP|YYYY-MM-DD hh:mm:ss.ffffff|4 Byte|1970-01-01 00:00:00 ~ 2038-01-19 03:14:07|
    |TIME|hh:mm:ss|3 Byte|-839:59:59 ~ 839:59:59|
    |YEAR[(4)]|YYYY|1 Byte|1901 ~ 2155|

    > TIMESTAMP 와 DATETIME 데이터 타입의 차이점
    > - DATETIME : 날짜와 시작은 저장하는 데이터 타입
    > - TIMESTAMP
    >   - TIME_ZONE '시스템 변수'의 값을 기본으로 날짜와 시간을 저장하는 데이터 타입
    >   - UTC 기반으로 저장
    >> 각각 저장 이후에 SET SESSION time_zone = '+03:00' 변경 이후 조회시 이격 확인

## 테이블의 종류

> 테이블 : 실제 데이터가 저장되는 구조
> - 일반 테이블 : 데이터를 저장하는 기본 구조의 테이블 : MyISAM
> - 파티션 테이블 : 데이터를 분할하여 저장하는 테이블 : InnoDB/Xtradb/MyRocks/MyISAM
> - 클러스터 테이블 : 기본적으로 Primary Key 순서로 정렬되어 데이터를 저장하는 테이블 : InnoDB/Xtradb/MyRocks

1. 일반 테이블

  - 스토리지 엔진 : MyISAM

  - 생성

    ```sql
    CREATE TABLE test_general_tbl(
      id INT(11) DEFAULT NULL,
      name VARCHAR(32) DEFAULT NULL
    )
    ENGINE=MYISAM
    DEFAULT CHARSET=utf8
    ;
    ```

  - **단일 파일에서 모든 데이터를 처리하기 때문에 대용량의 데이터를 다룰 때 성능 이슈가 발생할 수 있다.** 

2. 파티션 테이블

  - 특징

    - 특정 컬럼에 의해 물리적으로 구분하여 저장하는 형태의 테이블

    - 대용량 데이터를 효과적으로 저장하고 관리할 수 있는 아키텍처

    - 파티션은 별도로 데이터가 저장되므로 효율적인 관리가 가능

    - 대용량 데이터가 한 테이블에 저장되어 검색 속도가 저하되는 현상 해소

    - 대용량 테이블에 대해 보관 주기가 필요한 경우 분리 저장 후 제거

  - 장점

    - 하나의 테이블에 저장하던 데이터를 범위로 구분하는 여러 개의 테이블로 분산 저장함으로 특정 범위 내의 데이터 검색을 빠르게 수행 가능

    - 파티션 단위의 데이터를 Truncate 문으로 삭제할 경우 테이블이 사용중인 물리 디스크의 크기를 감소시킬 수 있다.
      (데이터를 DELETE로 수행하여 보관 주기에 따른 관리가 가능하나 테이블의 크기는 감소하지 않는다.)
  
  - 제약 사항

    - 파티션 컬럼은 Primary Key 중 하나여야 한다.
    
    - 파티션 테이블은 Foreign Key를 포함할 수 없다.
    
    - 각 파티션은 동인한 스토리지 엔진을 사용해야 한다.

  - 생성 예시
  
    ```sql
    CREATE TABLE test_part_tbl(
      id VARCHAR(120) NOT NULL,
      email VARCHAR(32) NOT NULL,
      reg_date DATETIME DEFAULT CURRENT_TIMESTAMP,
      PRIMARY KEY(id, reg_date)
    )
    ENGINE=INNODB
    CHARSET=UTF8
    PARTITION BY RANGE (TO_DAYS(reg_date))
    (
      PARTITION P20230401 VALUES LESS THAN (TO_DAYS('2023-04-02 00:00:00')),
      PARTITION P20230402 VALUES LESS THAN (TO_DAYS('2023-04-03 00:00:00')),
      PARTITION P20230403 VALUES LESS THAN (TO_DAYS('2023-04-04 00:00:00')),
      PARTITION PMAX VALUES LESS THAN MAXVALUE
    )
    ;
    ```

    - PARTITION BY RANGE

      - 파티션 Key 컬럼

      - reg_date 컬럼값에 의해 분리되어 저장

    - PARTITION P20230401 VALUES LESS THAN (TO_DAYS('2023-04-02 00:00:00'))

      - 파티션에 저장될 수 있는 데이터에 대해 정의

      - 2023-04-02 00:00:00 미만의 데이터만 저장

  - 파티션의 종류

    - RANGE

      - Key 컬럼 값의 범위로 구분되어 저장되는 형태의 테이블

    - LIST

      - 동일한 값을 가지는 데이터들로 각각의 파티션을 구성하는 형태의 테이블

      - 예시
        
        ```sql
        CREATE TABLE test_part_tbl(
          id VARCHAR(120) NOT NULL,
          name VARCHAR(32) NOT NULL,
          stress INT NOT NULL,
          PRIMARY KEY(id, stress)
        )
        ENGINE=INNODB
        CHARSET=UTF8
        PARTITION BY LIST (stress)
        (
          PARTITION P_LIST VALUES IN (0),
          PARTITION P_LIST_0 VALUES IN (1,2,3),
          PARTITION P_LIST_1 VALUES IN (4,5,6,7,8)
        )
        ;
        ```
      
    - HASH

      - 데이터베이스에서 제공하는 Hash 함수를 적용한 결과 값이 동일한 데이터를 같이 저장하는 형태의 테이블

      - 예시
        
        ```sql
        CREATE TABLE test_part_tbl(
          id VARCHAR(120) NOT NULL,
          name VARCHAR(32) NOT NULL,
          stress INT NOT NULL,
          PRIMARY KEY(id, stress)
        )
        ENGINE=INNODB
        CHARSET=UTF8
        PARTITION BY HASH (stress)
        PARTITION 8
        ;
        ```

        - 8 개의 파티션을 생성
        - `stress`를 Hash 값으로 변환하여 생성된 8개의 Hash 파티션에 저장
        - `stress`값이 같은 경우 동일한 Hash를 갖으므로 같은 파티션에 저장

3. 클러스터 테이블

  - 하나의 거대한 인덱스 구조로 관리된다.
  
  - Primary Key 컬럼 순서로 데이터가 저장되어 있는 테이블을 클러스터 테이블이라고 부른다.

  - 클러스터 테이블 : Primary Key의 컬럼으로 정렬되어 테이블의 데이터를 저장한다.
  
  - 클러스터 Key

    - 클러스터 테이블에 데이터를 저장할 때 기준이 되는 Key 컬럼

    - Primary Key
  
  - InnoDB에서 클러스터 Key 컬럼 선택 기준

    - 1순위 : Primary Key가 존재하면 Primary Key 선택

    - 2순위 : NOT NULL 옵션의 Unique 인덱스 중에서 첫 번째 인덱스를 선택

    - 3순위 : 자동으로 Unique한 값을 가지도록 증가되는 보이지 않는 내부컬럼을 추가하여 클러스터 Key로 선택

4. 차이 확인

  - 일반 테이블

    - 삽입 순서로 저장

    - 데이터가 패턴을 갖지 않고 랜덤하게 저장

    - 저장 이후 랜덤 엑세스 발생에 대해 Key의 데이터를 찾기위한 I/O 발생

  - 클러스터 테이블

    - Primary Key 순서로 정렬되어 데이터 저장

    - Range 검색 시 처리 성능 매우 빠름 (인덱스 구조)

    - 정렬되어 있기 때문에 삽입 시 저장 위치가 결정되어 성능의 저하 발생

    - Primary Key를 업데이트 시 해당 Primary Key를 삭제 후 정렬 순서에 따라 삽입하는 방식으로 처리 성능이 저하

## 테이블 관리

1. 명령 모음

  ```sql
  SHOW TABLES;
  DESC `table_name`;
  SHOW COLUMNS FROM `table_name`;
  SHOW FULL COLUMNS FROM `table_name`;
  ALTER TABLE `table_name` ADD (add_column VARCHAR(10) DEFAULT NULL);
  ALTER TABLE `table_name` MODIFY modify_column VARCHAR(15); # table_lock 발생
  ALTER TABLE `table_name` DROP COLUMN drop_column; # table_lock 발생
  ALTER TABLE `table_name` RENAME `renamed_table_name`;
  DROP TABLE `table_name`; # 테이블 DROP 시 인덱스 DROP, 인덱스 DROP 시 테이블은 DROP 되지 않는다.
  TRUNCATE TABLE `table_name`;
    # 데이터베이스 공간 반납
    # ROLLBACK 수행 불가능
    # 기존 사용 데이터 공간 반납
    # 테이블의 인덱스도 TRUNCATE 된다
    # AUTO_INCREMENT 초기화
    # ** DELETE 시행 시 데이터 공간은 남아 있다 **
  ```

2. 테이블 압축

  - 테이블 생성 시 압축 옵션을 적용하여 데이터 저장 공간을 적게 사용할 수 있다.

  ```sql
  # 옵션 미적용
  CREATE TABLE test_normal(
    idx INT(11),
    my_date DATETIME
  )
  ;
  # 옵션 적용
  CREATE TABLE test_compress(
    idx INT(11),
    my_date DATETIME
  )
  ROW_FORMAT=COMPRESSED
  KEY_BLOCK_SIZE=2 # 기본값 16 이며 2,4,8,16 설정 가능, 블록 사이즈에 따라 압축률이 달라짐, ROW 데이터 크기에 따른 설정
  ;
  # 기본 옵션
  CREATE TABLE test_compress_default(
    idx INT(11),
    my_date DATETIME
  )
  ROW_FORMAT=COMPRESSED
  ;

  # 압축된 테이블 데이터 삽입
  INSERT INTO test_normal SELECT seq, NOW() FROM seq_1_to_10000;
  INSERT INTO test_compress SELECT seq, NOW() FROM seq_1_to_10000;
  INSERT INTO test_compress_default SELECT seq, NOW() FROM seq_1_to_10000;

  # 데이터 확인 : information_schema 에서 확인
  SELECT table_name, data_length/1024/1024 AS mb
  FROM information_schema.tables
  WHERE table_name IN ('test_normal','test_compress','test_compress_default');
  +-----------------------+------------+
  | table_name            | mb         |
  +-----------------------+------------+
  | test_normal           | 0.39062500 |
  | test_compress         | 0.25195313 |
  | test_compress_default | 0.20312500 |
  +-----------------------+------------+

  ```

  - [테스트 사례]("https://estenpark.tistory.com/377") : [Admin] MariaDB/MySQL InnoDB 테이블 압축(Compression)

3. 파티션

  ```sql
  # 조회 : information_schema 존재
  # 파티션 정보 확인 가능 p341
  SELECT *
  FROM information_schema.partitions
  WHERE TABLE_NAME = 'test_part_tbl';
  *************************** 1. row ***************************
                  TABLE_CATALOG: def
                  TABLE_SCHEMA: test
                    TABLE_NAME: test_part_tbl
                PARTITION_NAME: P20230401             # 파티션 이름
              SUBPARTITION_NAME: NULL
    PARTITION_ORDINAL_POSITION: 1
  SUBPARTITION_ORDINAL_POSITION: NULL
              PARTITION_METHOD: RANGE                 # 유형
            SUBPARTITION_METHOD: NULL
          PARTITION_EXPRESSION: to_days(`reg_date`)   # 파티션 조건
        SUBPARTITION_EXPRESSION: NULL
          PARTITION_DESCRIPTION: 738977
                    TABLE_ROWS: 2
                AVG_ROW_LENGTH: 8192
                    DATA_LENGTH: 16384
                MAX_DATA_LENGTH: NULL
                  INDEX_LENGTH: 0
                      DATA_FREE: 0
                    CREATE_TIME: 2023-04-09 22:36:15
                    UPDATE_TIME: 2023-04-12 04:10:45
                    CHECK_TIME: NULL
                      CHECKSUM: NULL
              PARTITION_COMMENT:                      # 파티션 설명
                      NODEGROUP: default
                TABLESPACE_NAME: NULL
  *************************** 2. row ***************************
  ...

  # 추가
  ALTER TABLE test_part_tbl ADD PARTITION
  (PARTITION P20230405 VALUES LESS THAN (TO_DAYS('2023-04-05 00:00:00')));

  # 분할
  ALTER TABLE test_part_tbl REORGANIZE
  PARTITION PMAX INTO (
    PARTITION P20230404 VALUES LESS THAN (TO_DAYS('2023-04-05 00:00:00')),
    PARTITION PMAX VALUES LESS THAN MAXVALUE
  );

  # 중간 변경은 잘 동작하지 않는다.
  ALTER TABLE test_part_tbl REORGANIZE
  PARTITION P20230404 INTO (
    PARTITION P20230404A VALUES LESS THAN (TO_DAYS('2023-04-04 12:00:00')),
    PARTITION P20230404B VALUES LESS THAN (TO_DAYS('2023-04-05 00:00:00'))
	);

  # 삭제
  ALTER TABLE test_part_tbl DROP PARTITION PMAX;
  ```

  - `MAXVALUE` 파티션에 데이터가 존재한다면 실제로 데이터를 조건에 맞게 분할한 파티션으로 이동시켜야 하므로 작업 수행 동안 성능 저하가 발생할 수 있다.
