# #3 Data Definition Language

## #3.0 Intoduction

주석 추가

```
-- < 한줄 주석

/*
    여러줄 주석
*/
```



SQL 명령어

* 대/소문자 구분하지 않음
* SQL 쿼리 끝 세미콜론 ; 붙이기



DDL (Data Definition Language) : 데이터 정의 언어



## #3.1 Tables

테이블 : 데이터베이스의 모든 데이터를 포함하는 테이터베이스 객체\
스프레드시트와 유사한 행-열 형식으로 구성되며 각 행은 고유한 레코드, 각 열은 레코드의 필드를 나타낸다.



## #3.2  Create Table

```
CREATE TABLE movies (
	title,
  released,
  overview,
  rating,
  director
);

-- CREATE TABLE - SQLite 에서 새 테이블 생성에 사용됨
-- sqlite는 이 정도로 작동함
```



## #3.3 Insert Into Values

```
DROP TABLE movies;

-- DROP TABLE 테이블 제거, 확인 메시지 없기에 확신을 갖고 사용


INSERT INTO movies VALUES (
    'The Godfather',
    1980,
    'The Best Movie In The World',
    10,
    'F.F.C'
);

-- 테이블에 데이터 추가
```



## #3.4 Insert Into Values (part Two)

위와 같은 방식으로 INSERT INTO 를 활용해 데이터를 추가할 때 반드시 열의 순서를 알아야 함, null 값으로 둘 수 있지만 이는 근본적인 해결이 아님

```
INSERT INTO movies (title) VALUES ('TLOTR');

INSERT INTO movies (title, rating) VALUES ('TLOTR 2', 10);

INSERT INTO movies (title, rating,released) VALUES ('TLOTR 3', 10, 2003);

-- 과 같은 방식으로 특정 값만 추가할 수 있음, 값을 지정하지 않으면 null

INSERT INTO movies (title, rating) VALUES ('ET', 10), ('ET 2', 9);

-- 와 같은 방식으로 여러 데이터 삽입 가능
```



## #3.5 Data Types

하지만 이렇게 데이터를 추가할 때, 각 데이터의 타입이 뒤섞일 수 있음. 예를 들어 title은 text고 rating은 number지만 위와 같이 데이터를 추가하면 title에 숫자, rating에 text가 추가되는걸 방지할 수 없음.

따라서 아래와 같은 방식으로 타입을 설정 (Daya Types은 DB 마다 다름)

```
CREATE TABLE movies (
    title TEXT,
    released INTEGER, -- 1, 2, 3 ...
    overview TEXT,
    rating REAL, -- 1.2 9.5 ... 음수 x
    director TEXT,
    for_kids INTEGER -- 0 or 1 ... sql에서는 T/F 설정 x
    -- poster BLOB -- image but DB에 이미지를 넣는건 안좋음 이미지 주소를 넣는 방법
) STRICT; -- 위 타입이 아닐 시 오류
```

하지만 이렇게 해도 조금 부족함. rating을 10점까지만 주고 싶지만 그 이상부터 음수까지 모두 가능...



## #3.6 Constraints

column 에 추가될 데이터에 제약 조건을 줄 수 있음

```
/*
    UNIQUE 유일값
    NOT NULL null 금지
    DEFAULT 기본값
*/

CREATE TABLE movies (
    title TEXT UNIQUE NOT NULL,
    released INTEGER NOT NULL,
    overview TEXT NOT NULL,
    rating REAL NOT NULL,
    director TEXT,
    for_kids INTEGER NOT NULL DEFAULT 0
) STRICT;

/*
    순서에 영향을 받는 조건들도 있기에 시도해보기
*/
```



## #3.7 CHECK Constraint

디테일한 제약 조건 추가

```
/*
    CHECK () - CHECK 뒤 내용이 참인지 확인
    BETWEEN A AND B
*/

CREATE TABLE movies (
    title TEXT UNIQUE NOT NULL,
    released INTEGER NOT NULL CHECK (released > 0),
    overview TEXT NOT NULL,
    rating REAL NOT NULL CHECK (rating BETWEEN 0 AND 10),
    director TEXT,
    for_kids INTEGER NOT NULL DEFAULT 0 CHECK (for_kids BETWEEN 0 AND 1)
) STRICT;
```



## #3.8 Recap

\+ 내장 FUNCTION [참고](https://sqlite.org/lang\_corefunc.html)

```
CREATE TABLE movies (
    title TEXT UNIQUE NOT NULL,
    released INTEGER NOT NULL CHECK (released > 0),
    overview TEXT NOT NULL CHECK (LENGTH(overview) <= 100),
    rating REAL NOT NULL CHECK (rating BETWEEN 0 AND 10),
    director TEXT,
    for_kids INTEGER NOT NULL DEFAULT 0 CHECK (for_kids BETWEEN 0 AND 1)
) STRICT;
```



## #3.9 Primary Keys

기본 키 - 데이터베이스 테이블의 각 행/레코드를 고유하게 식별하는 테이블의 필드.\
고유한 값이 포함되어야 하며, 기본 키 열은 NULL 값을 가질 수 없다. 여러 필드가 키본 키로 사용되는 경우 복합 키라고 한다.



기본 키는 **불변**하고 **고유**해야 함



NATURAL PRIMARY KEY - 자연 기본키, 테이블의 데이터에서 파생되어 고유 식별자로 사용하는 column을 의미



SURROGATE PRIMARY KEY - 대체 기본키, 의미 없지만 고유 식별자를 위해 사용하는 인위적인 열



자연 기본키는 유지가 어렵기에 대체 기본키 사용이 권장됨



```
-- 자연 기본키 title
CREATE TABLE movies (
    title TEXT PRIMARY KEY UNIQUE NOT NULL,
    released INTEGER NOT NULL CHECK (released > 0),
    overview TEXT NOT NULL CHECK (LENGTH(overview) <= 100),
    rating REAL NOT NULL CHECK (rating BETWEEN 0 AND 10),
    director TEXT,
    for_kids INTEGER NOT NULL DEFAULT 0 CHECK (for_kids BETWEEN 0 AND 1)
) STRICT;

-- 대체 기본키 movieId
CREATE TABLE movies (
    movieId INTEGER NOT NULL PRIMARY KEY,
    title TEXT UNIQUE NOT NULL,
    released INTEGER NOT NULL CHECK (released > 0),
    overview TEXT NOT NULL CHECK (LENGTH(overview) <= 100),
    rating REAL NOT NULL CHECK (rating BETWEEN 0 AND 10),
    director TEXT,
    for_kids INTEGER NOT NULL DEFAULT 0 CHECK (for_kids BETWEEN 0 AND 1)
) STRICT;


-- SQLite_ AUTO INCREMENT 항상 새롭고 고유한 기본키 보장
```



***



### PRIMARY KEY & UNIQUE

|            | PRIMARY KEY               | UNIQUE                               |
| ---------- | ------------------------- | ------------------------------------ |
| NULL 허용 여부 | X                         | O                                    |
| 테이블당 개수    | 1                         | MANY                                 |
| INDEX 생성   | 자동으로 클러스터드 인덱스 생성         | NON-클러스터드 인덱스 생성                     |
| 레코드 식별     | 테이블이 각 레코드를 고유하게 식별하는데 사용 | 특정 열의 값이 고유함을 보장하지만, 반드시 레코드 식별용은 아님 |
| 외래 키 참조    | 다른 테이블의 외래 키가 참조 가능       | 외래 키의 참조 대상이 될 수 있지만, 일반적이지 않음       |
| 변경 가능성     | 일반적으로 변경 권장 X             | 상대적으로 변경 용이, 주의 필요                   |

