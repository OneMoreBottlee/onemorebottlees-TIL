# #18 PostgreSQL Extensions

## #18.0 Introduction

Extention을 활용해 기존 database가 하지 못하던 것들을 가능하게 해줌 ⇒ postgreSQL 의 가장 큰 장점

```sql
select * from pg_available_extensions;

-- 활성화 / 비활성화
CREATE EXTENSION hstore;
DROP EXTENSION hstore;

-- 사용한 extension 확인
SELECT * FROM pg_extension;
```



## #18.1 HSTORE

```sql
CREATE TABLE users (
  user_id BIGINT PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
  prefs HSTORE
);

INSERT INTO users (prefs) VALUES
('theme => dark, lang => kr, notifications => off'),
('theme => light, lang => es, notifications => on, push_notifications => on, email_notifications => off'),
('theme => dark, lang => it, start_page => dashboard, font_size => large');

SELECT * FROM USERS;
-- HSSTORE, JSON 타입을 사용하면 중첩된 정보를 가질 수 있는데 HSTORE는 이를 방지함

SELECT
  user_id,
  prefs -> 'theme',
  prefs -> ARRAY['lang', 'notifications'],
  prefs ? 'font_size' AS has_font_size,
  prefs ?| ARRAY['push_notifications', 'start_page']
FROM users;

SELECT
  user_id,
  akeys(prefs),
  avals(prefs)
FROM users;

SELECT
  user_id,
  each(prefs)
FROM users;

-- UPDATE
UPDATE users
SET 
  prefs['theme'] = 'light'
WHERE user_id = 1;

UPDATE users
SET
  prefs = prefs || hstore(
  ARRAY['currency', 'cookies_ok'],
  ARRAY['krw', 'yes']
)
  
-- DELETE
UPDATE users
SET prefs = delete(prefs, 'cookies_ok');

SELECT * from users;

DROP TABLE users
```



## #18.2 PGCrypto

```sql
CREATE TABLE users (
  user_id BIGINT PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
  username VARCHAR(100),
  password VARCHAR(100)
)

-- password 해싱
INSERT INTO users (username, password)
VALUES ('son', crypt('user_password', gen_salt('bf')));

SELECT username
FROM users
WHERE 
  username = 'son' AND
  password = crypt('user_password', password);
```



## #18.3 UUID

```sql
CREATE EXTENSION "uuid-ossp";

-- BIGINT의 한계 보완 가능
CREATE TABLE users (
  user_id UUID PRIMARY KEY DEFAULT(uuid_generate_v4()),
  username VARCHAR(100),
  password VARCHAR(100)
)

INSERT INTO users (username, password) VALUES ('SON', '1234');

SELECT * FROM users;
```

