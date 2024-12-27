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

[https://www.postgresql.org/docs/8.1/functions-datetime.html](https://www.postgresql.org/docs/8.1/functions-datetime.html)

```sql
INSERT INTO users (
	username,
  email,
  gender,
  interest,
  bio,
  age,
  is_admin,
  bitrh_date,
  bed_time,
  graduation_year,
  internship_period
) VALUES (
	'nico',
  'nico@n.com',
  'male',
  ARRAY['tect', 'music', 'travel'],
  'i like eating and traveling',
  18,
  TRUE,
  '1990-01-01',
  '21:00:00',
  1993,
  '2 years 6 months'
);

SELECT joined_at::date FROM users;
SELECT joined_at::time FROM users;

SELECT 
	joined_at::date as joined_date,
  EXTRACT(YEAR FROM joined_at) as joined_year,
  joined_at - INTERVAL '1 day' as day_before_joining,
  AGE(bitrh_date) as age
FROM users;
```



## #13.7 UNNEST

```sql
CREATE TABLE genres (
  genre_id BIGINT PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
  name VARCHAR(50) UNIQUE,
  created_at TIMESTAMPTZ DEFAULT CURRENT_TIMESTAMP NOT NULL,
  updated_at TIMESTAMPTZ DEFAULT CURRENT_TIMESTAMP NOT NULL
)


INSERT INTO
	genres (name)
  SELECT DISTINCT UNNEST(string_to_array(genres, ',')) FROM movies GROUP BY genres;
  
CREATE TABLE movies_genres (
  movie_id BIGINT NOT NULL,
  genre_id BIGINT NOT NULL,
	created_at TIMESTAMPTZ DEFAULT CURRENT_TIMESTAMP NOT NULL,
  updated_at TIMESTAMPTZ DEFAULT CURRENT_TIMESTAMP NOT NULL,
  
  PRIMARY KEY (movie_id, genre_id),
  FOREIGN KEY (movie_id) REFERENCES movies (movie_id),
  FOREIGN KEY (genre_id) REFERENCES genres (genre_id)
);

INSERT INTO movies_genres (movie_id, genre_id)
  SELECT movies.movie_id, genres.genre_id
  FROM movies
    JOIN genres ON movies.genres LIKE '%' || genres.name || '%';
    
ALTER TABLE movies DROP COLUMN genres;
```



## #13.8 FULL OUTER JOIN

```sql
ALTER TABLE movies ALTER COLUMN xxx TYPE TEXT;

ALTER TABLE movies RENAME COLUMN xxx TO ZZZ;

ALTER TABLE movies ADD PRIMARY KEY (movie_id);

ALTER TABLE movies
ADD CONSTRAINT fk
FOREIGN KEY (director_id) REFERENCES directors(director_id);

ALTER TABLE movies
ADD CONSTRAINT check_rating CHECK (rating BETWEEN 1 AND 10);

ALTER TABLE movies DROP CONSTRAINT check_rating;
ALTER TABLE movies ALTER COLUMN created_at SET DEFAULT CURRENT_TIMESTAMP;
ALTER TABLE movies RENAME TO movies_;
ALTER TABLE movies ADD CONSTRAINT unique_title UNIQUE (title);
ALTER TABLE movies ALTER COLUMN zzz SET NOT NULL;

ALTER TABLE movies
ADD COLUMN likes INT,
ADD COLUMN dislikes INT;

```

