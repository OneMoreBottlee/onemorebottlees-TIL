# #14 Functions And Procedures

## #14.0 Introduction

Functions & Procedure - database object로 특정 작업을 수행하는 SQL 구문을 세트로 캡슐화 가능





## #14.1 Functions

```sql
CREATE OR REPLACE FUNCTION hello_world() -- funcion은 value를 return함
RETURNS text AS 
$$ -- BODY
	SELECT 'hello_world';
$$
LANGUAGE SQL; -- 언어

SELECT title, hello_world() FROM movies;

CREATE OR REPLACE FUNCTION hello_world(user_name text)
RETURNS text AS 
$$ -- BODY
	SELECT 'hello ' || user_name;
$$
LANGUAGE SQL; 

SELECT title, hello_world('nico') FROM movies;
SELECT title, hello_world() FROM movies; -- 실행시 위의 hello_world()가 실행된다

DROP FUNCTION hello_world(); -- 삭제

CREATE OR REPLACE FUNCTION hello_world(text, text) -- parameter
RETURNS text AS 
$$ -- BODY
	SELECT 'hello ' || $1 || ' and ' || $2;
$$
LANGUAGE SQL;

SELECT hello_world('kim', 'son');
```

## #14.2 Return Types

```sql
CREATE OR REPLACE FUNCTION is_hit_or_flop(movie movies)
RETURNS TEXT AS
$$
    SELECT CASE
    WHEN movie.revenue > movie.budget THEN 'Hit'
    WHEN movie.revenue < movie.budget THEN 'Flop'
    ELSE 'N/A'
    END
$$
LANGUAGE SQL;

SELECT 
    title,
    is_hit_or_flop(movies.*) -- movies의 모든 column
FROM movies;


CREATE OR REPLACE FUNCTION is_hit_or_flop(movie movies)
RETURNS TABLE (hit_or_flop text, other_thing numeric) AS
$$
    SELECT CASE
    WHEN movie.revenue > movie.budget THEN 'Hit'
    WHEN movie.revenue < movie.budget THEN 'Flop'
    ELSE 'N/A'
    END, 11111;
$$
LANGUAGE SQL;

DROP FUNCTION is_hit_or_flop(movies);

SELECT 
    title,
    (is_hit_or_flop(movies.*)).*
FROM movies;

```



## #14.3 Trigger Functions

```sql
-- VOLATILE RECORD 수정 삭제 등 가능, 같은 인자로 들어온 성공적인 요청에 대해 다른 결과 RETURN
-- STABLE 단일 구문 내 모든 ROW에서 동일한 ARGUMENT에 대해 같은 결과를 RETURN
-- IMMUTABLE 동일한 ARGUMENT가 주어질 경우 영원히 같은 값 RETURN

CREATE OR REPLACE FUNCTION set_updated_at()
RETURNS TRIGGER AS
$$
	BEGIN
  	NEW.updated_at = CURRENT_TIMESTAMP;
    RETURN NEW;
  END;
$$
LANGUAGE plpgsql;

CREATE TRIGGER updated_at
BEFORE UPDATE -- OF title , title이 업데이트 되기 전
ON movies
FOR EACH ROW EXECUTE PROCEDURE set_updated_at();
```



## #14.4 Procedures

```sql
-- procedure return할 필요 없음  DML command에서 호출하지 않음

CREATE PROCEDURE set_zero_revenue() AS
$$
	UPDATE movies SET revenue = NULL WHERE revenue = 0;
$$
LANGUAGE SQL;

CALL set_zero_revenue();


-- RETURN 도 가능함, 귀찮음
CREATE PROCEDURE hello_world(IN name text, OUT greeting text) AS
$$
	BEGIN
  	greeting = 'Hello ' || name;
  END;
$$
LANGUAGE plpgsql;

CALL hello_world('SON', NULL);
```



## #14.5 Python Extension

```sql
-- PostgreSQL이 가장 발전된 평가를 받는 이유는 EXTENSION 때문
-- APP STACK BUILDER 활용

CREATE OR REPLACE FUNCTION hello_world_py(name text)
RETURNS TEXT AS
$$
    def hello(name);
        return f'hello {name}'
    output = hello(name)
    return output
$$
LANGUAGE plpython3u;

SELECT hello_world_py('SON');


CREATE OR REPLACE FUNCTION log_updated_at_py(name text)
RETURNS TRIGGER AS
$$
    import json, requests
    
    request.post('http://localhost:3000', data=json.dumps(TD))
$$
LANGUAGE plpython3u;

CREATE TRIGGER updated_at_py
BEFORE UPDATE
ON movies
FOR EACH ROW EXECUTE PROCEDURE log_updated_at_py();
```



