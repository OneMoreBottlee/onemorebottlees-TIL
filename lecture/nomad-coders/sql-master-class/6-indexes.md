# #6 Indexes

## #6.0 Introduction

느린 쿼리 작업 속도를 빠르게 해주는 인덱스



## #6.1 Table Scans

데이터베이스가 무언가 찾기 위해 테이블의 모든 행을 하나씩 찾아보는 것을 의미



query plan - 데이터베이스가 어떻게 가장 효율적으로 SQL 문을 처리할 것인지 결정하는 계획

```sql
EXPLAIN QUERY PLAN ~~
```

를 활용해 INDEX 효과 확인. 스캔하는 대신 INDEX 사



## #6.2 Create INDEX

데이터 - 스프레드 시트와 비슷한 구조로 저장되어 있음

인덱스는 테이블에서 데이터 위치를 가리키는 자료구조, 인덱스가 없다면 원하는 데이터를 찾기 위해 모든 테이블을 뒤져야 함. 따라서 인덱스는 테이블의 검색 속도를 향상시키기 위한 자료구조이기도 함

하지만 모든 경우에서 인덱스를 사용하는 것은 데이터베이스의 성능을 저하시킬 수 있음. 필요에 따라 인덱스를 생성하는 것이 좋다.

```sql
SELECT *
FROM movies
WHERE director = 'Guy Ritchie';

CREATE INDEX idx_director ON movies (director);
DROP INDEX idx_director 
```



## #6.2 Disclaimer



## #6.3 B+ Trees





