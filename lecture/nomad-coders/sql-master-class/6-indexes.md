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



## #6.4 B+ Trees

트리 자료 구조 중 하나로 키에 의해 식별되는 레코드의 효율적 삽입, 검색, 삭제를 통해 정렬된 데이터를 표현하기 위한 자료 구조



root node : 부모가 없는 노드

leaf node : 자식이 없는 노드

sibling : 같은 부모를 가진 노드 (형제 노드)



## #6.5 Leaf Nodes

실제 데이터를 저장하는 노드, 트리 구조의 가장 하단에 위치하며, 해당 노트에 데이터가 저장된다.



## #6.7 Indexes and Keys

SQLite, MySQL 등의 DB는 Primary Key가 데이터 구조를 정렬하는데 사용하고 있음

따라서 Primary Key로 검색하는 것이 빠름 > Unique 값을 Primary key로 설정하는 이유

SQLite의 경우 자동적으로 rowid를 추가하지만 MySQL은 특정한 경우를 제외하고 자동으로 추가하지 않는다.



## #6.8  Multi Column Indexes

다중 Column에 index 생성, 다중 조건

```sql
SELECT title
FROM movies
WHERE release_date = 2022 AND rating = 7 AND revenue > 100;

CREATE INDEX idx ON movies (release_date, rating, revenue);
-- 가장 많이 검색하는 항목, = 활용 조건을 앞에 두고
-- 범위 조건을 뒤에 두어야 함

DROP INDEX idx;

```



## #6.9 Covering Indexes

데이터베이스에서 특정 쿼리를 처리할 때, 쿼리의 모든 데이터를 인덱스 자체에서 가져올 수 있도록 하는 인덱스. 본 데이터에 접근하지 않고 인덱스만으로 쿼리를 해결할 수 있기에 성능 향상됨

모든 데이터를 가져오기에 업데이트나 삽입이 느려질 수 있으므로 항상 사용하는건 지양해야 함

```sql
SELECT title
FROM movies
WHERE rating > 7;

CREATE INDEX idx ON movies (rating);
CREATE INDEX idx ON movies (rating, title);

DROP INDEX idx
```



## #6.10  When To Use Indexes

* WHERE, ORDER BY, JOIN에서 자주 사용하는 열이 있을 경우
* 고유한 값을 가진 열이 있을 경우 (높은 카디널리티)
* 테이블이 클 경우
* 외래키가 있는 경우
* 과도한 인덱스 사용 금지 (삽입, 수정, 삭제 작업이 지연될 수 있음) > 모든 단일 열마다 인덱스를 만들 필요 없음 (인덱스는 공짜가 아님)
* 작업 완료 후 쿼리 최적화 하기
* 여러 열을 함께 필터링하거나 정렬하는 쿼리의 경우 멀티 열이나 복합 인덱스 사용
* 가능하면 커버링 인덱스 사용 (비용 저렴) > 커버링 인덱스는 성능을 향상 시켜줄 수 있지만 거대하지 않음
* 작은 테이블에 인덱스를 만들지 않기
* 업데이트 빈도 고려 > 변경이 잦은 열은 인덱스 대상이 아님
* 큰 텍스트 열은 B-트리 대신 전체 텍스트 인덱스를 사용하기

