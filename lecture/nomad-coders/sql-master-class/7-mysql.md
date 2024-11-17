# #7 MySQL

## #7.0 Introduction

MySQL\
MySQL은 세계에서 가장 많이 쓰이는 오픈 소스의 관계형 데이터베이스 관리 시스템.\
다중 스레드, 다중 사용자, 구조질의어 형식의 데이터베이스 관리 시스템으로 오라클이 관리 및 지원하고 있으며, Qt처럼 이중 라이선스가 적용됨\
https://www.mysql.com



MariaDB\
MariaDB는 오픈 소스의 관계형 데이터베이스 관리 시스템.\
MySQL과 동일한 소스 코드(MySQL에서 fork)를 기반으로 하며, GPL v2 라이선스를 따른다.\
오라클 소유인 MySQL의 불확실한라이센스 상태에 반발하여 만들어졌으며, 배포자는 몬티 프로그램 AB와 저작권을 공유해야 한다.\
https://mariadb.org



## #7.1 Installation

MySQL 설치



## #7.2 MySQLWorkbench

MySQLWorkbench 설치



## #7.3 Connecting

Beekeeper Stuidio와 연결



## #7.4 MySQL Data Types Part One

데이터 타입을 통해 정확히 어떤 종류의 데이터가 테이블에 들어올 수 있는지 명확히 할 수 있음

```sql
CREATE TABLE users (
  username CHAR(10),
  -- 'son' 저장시 => 'son       ' 공백 포함한 10자를 저장함, 최대 255글자까지 저장가능
  
  email VARCHAR(50),
  -- 가변적인 string 길이를 가진 데이터에 사용됨, 최대 50자 저장해라 !
  
  gender ENUM('Male', 'Female'),
  -- A OR B 와 같이 특정 옵션 값만 선택할 수 있도록
  
  interests SET('Technology', 'Sports', 'Music', 'Art', 'Travel', 'Food', 'Fashion', 'Science'),
  -- 리스트 내에서 선택
  
  bio TEXT,
  -- TINYTEXT 255자 / TEXT 65,535자 (64KB) / MEDIUMTEXT 16,775,215자 (16MB) / LONGTEXT 4,294,967,295자 (4GB)
  -- 다양하게 있지만 데이터베이스에 들어올 데이터를 고려해 가장 작은 타입을 선택하는게 좋음
  
  profile_picture TINYBLOB,
  -- TINYBLOB (255B), BLOB (64KB), MEDIUMBLOB (16MB), LONGBLOB (4GB)
  
  age TINYINT UNSIGNED,
  /* 
  1. TINYINT
  Signed: -128 to 127
  Unsigned: 0 to 255

  2. SMALLINT
  Signed: -32,768 to 32,767
  Unsigned: 0 to 65,535

  3. MEDIUMINT
  Signed: -8,388,608 to 8,388,607
  Unsigned: 0 to 16,777,215

  4. INT (INTEGER)
  Signed: -2,147,483,648 to 2,147,483,647
  Unsigned: 0 to 4,294,967,295

  5. BIGINT
  Signed: -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807
  Unsigned: 0 to 18,446,744,073,709,551,615
  */
  
  is_admin BOOLEAN -- TINYINT(1,0)
)










```



## #7.5 MySQL Data Types Part Two

```sql
CREATE TABLE users (
  user_id BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
	username CHAR(10) NOT NULL UNIQUE, 
  email VARCHAR(50) NOT NULL,
  gender ENUM('Male', 'Female') NOT NULL,
  interests SET('Technology', 'Sports', 'Music', 'Art', 'Travel', 'Food', 'Fashion', 'Science') NOT NULL,
  bio TEXT NOT NULL,
  profile_picture TINYBLOB,
  age TINYINT UNSIGNED NOT NULL,
  is_admin BOOLEAN DEFAULT FALSE NOT NULL,
  
  balance DECIMAL(5, 2) NOT NULL,
  -- DECIMAL(p,s) FLOAT 999.12
  
  joined_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP NOT NULL,
  -- DATETIME YYYY-MM-DD hh:mm:ss
  /*
  	TIMESTAMP -> '1970-01-01 00:00:01' UTC to '2038-01-19 03:14:07' UTC
  	DATETIME -> '1000-01-01 00:00:00' to '9999-12-31 23:59:59'
  */
  
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP NOT NULL,
  birth_date DATE NOT NULL,
  bed_time TIME NOT NULL,
  graduation_year YEAR NOT NULL,
  -- 1901 to 2155
  
  CONSTRAINT chk_age CHECK(age < 100),
  CONSTRAINT uq_email UNIQUE (email)
)

```



## #7.6 Insert Into Values

```sql
INSERT INTO users (
  username,
  email,
  gender,
  interests,
  bio,
  age,
  is_admin,
  birth_date,
  bed_time,
  graduation_year
) VALUES (
  'MR. NOBODY',
  'mr@nobody.com',
  'Male',
  'Travel,Food,Technology',
  'I like traveling',
  18,
  True,
  '1999-05-09', -- 19990509 1999/05/09
  '22:00', -- 223000 22:30 22
  1976
);
```



## #7.7 ALTER TABLE

```sql

-- drop column
ALTER TABLE users DROP COLUMN profile_picture;

-- rename column
ALTER TABLE users CHANGE COLUMN about_me about_me TEXT; -- 컬럼이름과 타입 함께 바꿀떄

-- change the column type
ALTER TABLE users MODIFY COLUMN about_me TINYTEXT;

-- rename database
ALTER TABLE users RENAME TO customers;
ALTER TABLE customers RENAME TO users;

-- drop constratints
ALTER TABLE users DROP CONSTRAINT uq_email;
ALTER TABLE users DROP CONSTRAINT username, DROP CONSTRAINT chk_age;

-- adding constraints
ALTER TABLE users ADD CONSTRAINT uq_email UNIQUE (email), ADD CONSTRAINT uq_username UNIQUE (username);

ALTER TABLE users ADD CONSTRAINT chk_age CHECK (age < 100);

-- add or remove a NULL constraint
ALTER TABLE users MODIFY COLUMN bed_time TIME NULL;
ALTER TABLE users MODIFY COLUMN bed_time TIME NOT NULL;

SHOW CREATE TABLE users;
```



## #7.8 ALTER TABLE Part Two

```sql
ALTER TABLE users MODIFY COLUMN graduation_year DATE; -- error 기존 값

ALTER TABLE users ADD COLUMN graduation_date DATE NOT NULL DEFAULT MAKEDATE(grdutaion_year, 1);

UPDATE users SET graduation_date = MAKEDATE(graduation_year, 1);

ALTER TABLE users DROP COLUMN graduation_year;

ALTER TABLE users MODIFY COLUMN graduation_date DATE NOT NULL;
```



## #7.9 Generated Columns

다른 컬럼을 사용하여 값을 도출하는 컬럼

```sql
CREATE TABLE users_v2 (
  user_id BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
  first_name VARCHAR(50),
  last_name VARCHAR(50),
  email VARCHAR(100),
  full_name VARCHAR(101) GENERATED ALWAYS AS (CONCAT(first_name, ' ', last_name)) STORED
  -- STORED 데이터를 디스크에 저장됨
);

ALTER TABLE users_v2 ADD COLUMN email_domain VARCHAR(50) GENERATED ALWAYS AS (SUBSTRING_INDEX(email, '@', -1)) virtual;
-- VIRTUAL 데이터를 디스크에 저장하지 않아도 되지만 조회시 불이익
```



## #7.10 Data Import



