# #17 PostgreSQL JSON Columns

## #17.0 Introduction

JSON coulmn data type 으로 다양한 작업 가능



## #17.1 JSON and JSONB

```sql
CREATE TABLE users (
  user_id BIGINT PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
  profile JSONB
);

-- JSON 입력된 TEXT를 복사해 저장
-- JSONB 분해된 Binary 형식으로 저장 > 처리 속도 빠름

INSERT INTO users (profile) VALUES
('{"name": "Taco", "age": 30, "city": "Budapest"}'),
-- SELECT json_build_object('name', 'Taco', 'age', 30, 'city', 'Budapest')
('{"name": "Giga", "age": 25, "city": "Tbilisi", "hobbies": ["reading", "climbing"]}')
-- SELECT json_build_object('name', 'Giga', 'age', 25, 'city', 'Tbilisi', 'hobbies', json_build_array('reading', 'climbing'))
;
```



## #17.2 Querying JSON

```sql
SELECT
  profile -> 'name' AS name,
  profile -> 'city' AS city,
  profile -> 'age' AS age,
  profile -> 'hobbies' -> 0 AS first_hobby
FROM
  users;
  
  
-- 조건
SELECT
  profile -> 'name' AS name,
  profile -> 'city' AS city,
  profile -> 'age' AS age,
  profile -> 'hobbies' -> 0 AS first_hobby
FROM users
WHERE profile ? 'hobbies';

SELECT
  profile -> 'name' AS name,
  profile -> 'city' AS city,
  profile -> 'age' AS age,
  profile -> 'hobbies' -> 0 AS first_hobby
FROM
  users
WHERE profile -> 'hobbies' ? 'reading';


-- JSONB
SELECT
  profile -> 'name' AS name,
  profile -> 'city' AS city,
  jsonb_array_length(profile -> 'hobbies') AS first_hobby
FROM users;

SELECT
  profile -> 'name' AS name,
  profile -> 'city' AS city,
  jsonb_array_length(profile -> 'hobbies') AS first_hobby
FROM users
WHERE profile ->> 'name' = 'Taco';


SELECT
  profile -> 'name' AS name,
  profile -> 'city' AS city,
  jsonb_array_length(profile -> 'hobbies') AS first_hobby
FROM users
WHERE (profile ->> 'age')::integer < 30;


SELECT
  profile -> 'name' AS name,
  profile -> 'city' AS city,
  jsonb_array_length(profile -> 'hobbies') AS first_hobby
FROM users
WHERE profile ?& array['name', 'hobbies'];
WHERE profile -> 'hobbies' ?| array['reading', 'traveling'];
WHERE profile -> 'hobbies' ?& array['reading', 'traveling'];


SELECT
  profile -> 'name' AS name,
  profile -> 'city' AS city,
  jsonb_array_length(profile -> 'hobbies') AS first_hobby
FROM users
WHERE profile ->> 'city' LIKE 'B%';
```

[https://www.postgresql.org/docs/9.5/functions-json.html\
\
](https://www.postgresql.org/docs/9.5/functions-json.html)

## #17.3 Processing JSON

```sql
-- update
UPDATE users
SET profile = profile || jsonb_build_object('email', 'x@x.com')::jsonb;


UPDATE users
SET profile = profile - 'email'
WHERE profile ->> 'name' = 'Giga';

UPDATE users
SET profile = profile || jsonb_build_object('hobbies', jsonb_build_array('climbing', 'traveling'))
WHERE profile ->> 'name' = 'Taco';

-- select 에서 사용가능
SELECT (profile -> 'hobbies') - 'climbing'
FROM users;

UPDATE users
SET profile = profile || jsonb_set(profile, '{hobbies}', (profile -> 'hobbies') - 'climbing');
```

