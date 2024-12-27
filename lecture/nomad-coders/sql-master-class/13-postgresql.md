# #13 PostgreSQL

## #13.3 PostgreSQL Data Types

```sql
CREATE TYPE gender_type AS ENUM ('male', 'femalle');

CREATE TABLE users (
	
  -- 0 < char(n) varchar(n) < 10,485,760
  username CHAR(10) NOT NULL UNIQUE,
  email VARCHAR(50) NOT NULL UNIQUE,
  
  gender gender_type NOT NULL,
  
  interest TEXT[] NOT NULL, -- TEXT로 된 리스트 > 좋은 방법은 아님
  
  -- TEXT 1GB가 최대, > 2KB 다른 테이블로 옮겨지고 주소만 저장 TOAST (the oversized-attribute storage technique)
  bio TEXT,
  
  profile_photo BYTEA
)
```



## #13.4 PostgreSQL Data Types part Two

```sql
CREATE TYPE gender_type AS ENUM ('male', 'femalle');

CREATE TABLE users (
  -- 0 < char(n) varchar(n) < 10,485,760
  username CHAR(10) NOT NULL UNIQUE,
  email VARCHAR(50) NOT NULL UNIQUE,
  
  gender gender_type NOT NULL,
  
  interest TEXT[] NOT NULL, -- TEXT로 된 리스트 > 좋은 방법은 아님
  
  -- TEXT 1GB가 최대, > 2KB 다른 테이블로 옮겨지고 주소만 저장 TOAST (the oversized-attribute storage technique)
  bio TEXT,
  
  profile_photo BYTEA,
  
 

  -- SMALLINT -32,768 to 32,767
  -- INTEGER -2,147,483,648 to 2,147,483,647
  -- BIGINT -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807
  
  -- SMALLSERIAL 1 to 32,767
  -- SERIAL 1 to 2,147,483,647
  -- BIGSERIAL 1 to 9,223,372,036,854,775,807
  
  -- DECIMAL & NUMERIC (precision, scale) 10.53 4p 2s
  -- REAL (6 decimal digits) & DOUBLE PRECISION (15 decimal digits)
  
  age SMALLINT IS NOT NULL CHECK (age >= 0),
  
  is_admin BOOLEAN NOT NULL DEFAULT FALSE,
  
  -- 4,713 BE to 294,276 AD
  joined_at TIMESTAMPTZ DEFAULT CURRENT_TIMESTAMP NOT NULL,
  updated_at TIMESTAMPTZ DEFAULT CURRENT_TIMESTAMP NOT NULL,
  bitrh_date DATE NOT NULL,
  bed_time TIME NOT NULL,
  graduation_year INTEGER NOT NULL CHECK (graduation_year BETWEEN 1901 AND 2115),
  internship_period INTERVAL
)
```

[https://www.postgresql.org/docs/16/datatype-geometric.html\
](https://www.postgresql.org/docs/16/datatype-geometric.html)

## #13.5 Type Casting







