# #4 Data Manipulation Language

## #4.0 Introduction

DML Data Manipulation Language

데이터 조작 언어, 데이터베이스에 데이터 추가(삽입), 삭제, 수정(업데이트) 하는데 사용되는 컴퓨터 프로그래밍 언어



Update / Query 로 종류가 나뉨



## #4.1 Update Commands

Update 명령

* INSERT INTO
* UPDATE
* DELETE

```
UPDATE movies SET rating = 10; -- 모든 rating 값이 10으로 변경됨

DELETE movies; -- 모든 movies 를 삭제함

-- WHERE 를 사용해 위치를 좁힘 & AND 등 다양한 조건을 추가할 수 있

UPDATE movies SET rating = 10 WHERE title = 'The Lord of The Rings';
UPDATE movies SET rating = rating + 2 WHERE title = 'The Lord of The Rings';
-- ㄴ업데이트할 데이터를 참조할 수 있음

DELETE movies WHERE movie_id = 2;
```



## #4.2 SELECT Command

SELECT - Table을 결과물로 제공하는 명령



## #4.3  FROM Clause

FROM table의 데이터를 가져올 수 있게 해줌

```
SELECT title, rating FROM movies;
-- FROM - 먼저 실행되고 SELECT - 나중에 실행됨, 결과를 재구성
```



## #4.4 SELECT Expressions

```
SELECT
     REPLACE(title, ': Part One', ' I') AS title,
    rating * 2 AS double_rating,
    UPPER(overview) AS overview_upp
FROM movies;

-- 과 같이 custom table 생성 가능
```



## #4.5  Movies Database

## #4.6  WHERE Clause

WHERE 선택될 데이터 좁히기

* NULL 값 확인은 IS NULL
* AND \_ 모든 조건이 참이어야 함
* OR \_ 조건 중 하나라도 참이포함



## #4.7 WHERE Predicates

```
BETWEEN 2024 AND 2020; -- X 작은 수가 앞으로 와야함
BETWEEN 2020 AND 2024;

IN (); -- () 안에 있는 데이터만 확인 OR 개념
```



패턴 매칭

```
SELECT *
FROM movies
WHERE title LIKE 'The%' -- The 로 시작하는 단어
WHERE title LIKE 'The ___ %';
```



## #4.8 SELECT Case

특정 기준과 조건에 맞춘 컬럼 생성

CASE 표현식은 조건 목록을 평가하고 평가 결과에 따라 표현식을 반환한다.

```
SELECT title,
 CASE WHEN rating >= 8 THEN 'GOOD'
       WHEN rating <= THEN 'BAD'
       ELSE '00'
       END AS good_or_not 
FROM 
```



## #4.9 ORDER BY Clause

하나 이상의 열을 기준으로 데이터를 오름차순 또는 내림차순으로 정렬

ASC | DESC

둘 이상의 열을 정렬할 때는 첫 번째 조건으로 정렬 후 나중 조건 정렬

```
SELECT *,
FROM movies
ORDER BY release_date DESC, revenue DESC
```



## #4.10 LIMIT and OFFSET Clauses





