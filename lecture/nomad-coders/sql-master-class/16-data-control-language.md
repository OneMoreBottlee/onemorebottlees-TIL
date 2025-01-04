# #16 Data Control Language

## #16.0 Introduction

Data Control Language

사용자가 데이터베이스를 조회하고 수정할 권한을 설정할 수 있게 해주는 언어



## #16.1 Users

```sql
CREATE ROLE marketer WITH login password 'marketer4ever';

-- 권한 부여
GRANT SELECT, UPDATE, INSERT ON movies TO marketer;

GRANT SELECT, INSERT ON statuses, directors TO marketer;

GRANT SELECT ON ALL TABLES IN SCHEMA PUBLIC TO marketer;

-- 권한 취소
REVOKE INSERT ON statuses, directors FROM marketer;

-- ALL
GRANT INSERT ON ALL TABLES IN SCHEMA PUBLIC TO marketer;
REVOKE INSERT ON ALL TABLES IN SCHEMA PUBLIC FROM marketer;
```



## #16.2 Roles

```sql
CREATE ROLE editor;
GRANT SELECT, INSERT, UPDATE ON ALL TABLES IN SCHEMA PUBLIC TO editor;

-- USER 생성 및 권한 부여
CREATE USER editor_one WITH PASSWORD 'word4ever';
GRANT editor TO editor_one;

----------------

REVOKE ALL ON movies FROM editor;

-- 특정 부분만 권한 부여
GRANT SELECT (title) ON movies TO editor;
GRANT UPDATE (budget) ON movies TO editor;

-- 동시 접속 제한
ALTER ROLE editor_one WITH CONNECTION LIMIT 1;
```

[https://www.postgresql.org/docs/current/sql-createrole.html](https://www.postgresql.org/docs/current/sql-createrole.html)

