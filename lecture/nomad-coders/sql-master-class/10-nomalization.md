# #10 Nomalization

## #10.1 Normalizing Status

```sql
-- status 빼기
CREATE TABLE statuses (
  status_id BIGINT UNSIGNED PRIMARY KEY auto_increment,
  status_name ENUM (
    'Canceled',
    'In Production',
    'Planned',
    'Post Production',
    'Released',
    'Rumored'
  ) NOT NULL,
  explanation TEXT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP NOT NULL,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP NOT NULL
)

INSERT INTO statuses (status_name) SELECT status from movies GROUP BY status;
-- value 대신 select 문 사용 가능

-- status_id 생성 후 연결
ALTER TABLE movies ADD COLUMN status_id BIGINT UNSIGNED;
ALTER TABLE movies ADD CONSTRAINT fk_status FOREIGN KEY (status_id) REFERENCES statuses (status_id) ON DELETE SET NULL;

-- 기존 데이터 기준으로 id 값 적용
UPDATE movies SET status_id = (SELECT status_id FROM statuses WHERE status_name = movies.status);

-- 기존 status 제거
ALTER TABLE movies DROP COLUMN status;
```



## #10.2 Normalizing Directors

```sql
-- director 테이블 생성
CREATE TABLE directors (
  director_id BIGINT UNSIGNED PRIMARY KEY auto_increment,
  name varchar(120),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP NOT NULL,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP NOT NULL
);

-- unique한감독명 추가
INSERT INTO
	directors (name)
SELECT director
FROM movies
GROUP BY director
HAVING director <> '';

-- director_id 생성 후 연결
ALTER TABLE movies ADD COLUMN director_id BIGINT UNSIGNED;
ALTER TABLE movies ADD CONSTRAINT fk_director FOREIGN KEY (director_id) REFERENCES directors (director_id) ON DELETE SET NULL;

-- index, 빠른 실행을 위한 작업. 테이블 생성에 unique 작업을 하면 안해도 됨
CREATE INDEX idx_director_name ON directors (name);
UPDATE movies SET director_id = (SELECT director_id FROM directors WHERE name = movies.director);

-- 기존 director 제거
ALTER TABLE movies DROP COLUMN director;
```



## #10.3 Normalizing Original Language

```sql
CREATE TABLE langs (
  lang_id BIGINT UNSIGNED PRIMARY KEY auto_increment,
  name varchar(120),
  code char(2),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP NOT NULL,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP NOT NULL
);


INSERT INTO
  langs (code)
SELECT original_language
FROM movies
GROUP BY original_language;

ALTER TABLE movies ADD COLUMN original_lang_id BIGINT UNSIGNED;
ALTER TABLE movies ADD CONSTRAINT fk_org_lang FOREIGN KEY (original_lang_id) REFERENCES langs (lang_id) ON DELETE SET NULL;

UPDATE movies SET original_lang_id = (SELECT lang_id FROM langs WHERE code = movies.original_language);

-- 기존 original_language 제거
ALTER TABLE movies DROP COLUMN original_language;
```



## #10.4 Normalizing Countries

```sql
CREATE TABLE countries (
  country_id BIGINT UNSIGNED PRIMARY KEY auto_increment,
  country_code char(2) UNIQUE NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP NOT NULL,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP NOT NULL
);


INSERT INTO countries (country_code)
SELECT country
FROM movies
WHERE country NOT LIKE '%,%'
GROUP BY country;


SELECT country, SUBSTRING_INDEX(country, ',', 1),SUBSTRING_INDEX(country, ',', -1)
FROM movies
WHERE country LIKE '__,__'
GROUP BY country;
```



## #10.5 Unions

```sql
-- join ----
/*
    union 
    -
    -
    -
    -
*/ 

INSERT IGNORE INTO countries (country_code)
SELECT SUBSTRING_INDEX(country, ',', 1)
FROM movies
WHERE country LIKE '__,__'
GROUP BY country
UNION
SELECT SUBSTRING_INDEX(country, ',', -1)
FROM movies
WHERE country LIKE '__,__'
GROUP BY country;
```



## #10.6 Conclusions

```sql
INSERT IGNORE INTO countries (country_code)
SELECT SUBSTRING_INDEX(country, ',', 1)
FROM movies
WHERE country LIKE '__,__,__'
GROUP BY country
UNION
SELECT SUBSTRING_INDEX(country, ',', -1)
FROM movies
WHERE country LIKE '__,__,__'
GROUP BY country
UNION
SELECT SUBSTRING_INDEX(SUBSTRING_INDEX(country, ',', 2), ',', -1)
FROM movies
WHERE country LIKE '__,__,__'
GROUP BY country;



CREATE TABLE movies_countries (
  movie_id BIGINT unsigned,
  country_id BIGINT unsigned,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP NOT NULL,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP NOT NULL,
  PRIMARY KEY (movie_id, country_id),
  FOREIGN KEY (movie_id) REFERENCES movies (movie_id) ON DELETE CASCADE,
  FOREIGN KEY (country_id) REFERENCES countries (country_id) ON DELETE CASCADE
);


INSERT INTO movies_countries (movie_id, country_id)
SELECT movies.movie_id, countries.country_id
FROM movies JOIN countries ON movies.country LIKE CONCAT('%', countries.country_code, '%')
WHERE movies.country <> '' AND countries.country_code <> '';

ALTER TABLE movies DROP COLUMN country;
ALTER TABLE movies DROP COLUMN country;
```



