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

```sql
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

```sql
SELECT title, rating FROM movies;
-- FROM - 먼저 실행되고 SELECT - 나중에 실행됨, 결과를 재구성
```



## #4.4 SELECT Expressions

```sql
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

```sql
BETWEEN 2024 AND 2020; -- X 작은 수가 앞으로 와야함
BETWEEN 2020 AND 2024;

IN (); -- () 안에 있는 데이터만 확인 OR 개념
```



패턴 매칭

```sql
SELECT *
FROM movies
WHERE title LIKE 'The%' -- The 로 시작하는 단어
WHERE title LIKE 'The ___ %';
```



## #4.8 SELECT Case

특정 기준과 조건에 맞춘 컬럼 생성

CASE 표현식은 조건 목록을 평가하고 평가 결과에 따라 표현식을 반환한다.

```sql
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

```sql
SELECT *,
FROM movies
ORDER BY release_date DESC, revenue DESC
```



## #4.10 LIMIT and OFFSET Clauses

쿼리 제한해야 하는 이유: 성능적으로직면하는 한계를 해결하기 위해\
ex\_ 백만개의 데이터를 가져오면 어떻게 보여줄 것인가?



```sql
SELECT * -- 3rd 그중 *열을 보여주는데
FROM moives -- 1st movies 테이블에서
WHERE XX -- 2nd XX 조건에 맞는 데이터를 가져옴
ORDER BY YY -- 4th YY 순서에따라
LIMIT 5; -- 6th 5개의 row만
OFFSET 5; -- 5th 5개를 스킵하고
```



## #4.11 GROUP BY Clause

GROUP BY - 즉각적으로 분명히 나타나지 않는 인사이트 추출 가능

ㄴ 동일한 데이터를 그룹으로 병합

집계 함수 - SUM / AVG 등 여러 row 값을 집계



```sql
SELECT director, SUM(revenue) as TOTAL_revenue-- 4rd
FROM movies -- 1st
WHERE -- 2nd
    director IS NOT NULL
    AND revenue IS NOT NULL
GROUP BY director -- 3th
ORDER BY total_revenue DESC; -- 5th
```



```sql
SELECT release_date, ROUND(AVG(rating), 2) AS avg_rating -- 4rd
FROM movies -- 1st
WHERE -- 2nd
    rating IS NOT NULL AND release_date IS NOT NULL
GROUP BY release_date -- 3th
ORDER BY avg_rating DESC; -- 5th
```



## 4.12 GROUP BY Gothas

* SQLite
* 집계 함수를 사용하지 않아도 병합된다.
* GROUP BY 하지 않은 column을 SELECT 하면 결과값은 얻지만 각 그룹의 마지막 행을 가져온다.
* GROUP BY를 사용하지 않으면 그룹 자체가 존재하지 않고 데이터베이스 전체가 그룹이 된다.



## #4.13 HAVING Clause

HAVING - WHERE와 비슷하게 row를 필터링하는 기능, 실행하는 순간이 다름

ㄴ WHERE 다음, GROUP BY 다음에 실행됨

ㄴ GROUP BY와 함께 사용

```sql
SELECT release_date, ROUND(AVG(rating), 2) AS avg_rating -- 5th
FROM movies -- 1st
WHERE -- 2nd
    rating IS NOT NULL AND release_date IS NOT NULL
GROUP BY release_date -- 3th
HAVING avg_rating > 6 -- 4rd
ORDER BY avg_rating DESC; -- 6th
```



## #4.14 Super Practice Part 1

[https://www.sqlite.org/lang\_aggfunc.html\
](https://www.sqlite.org/lang\_aggfunc.html)\
ㄴ SQLite 집계함수 확인



```sql
-- Q1. What is the average rating of each director??
SELECT director, AVG(rating) AS avg_rating
FROM movies
WHERE director IS NOT NULL AND rating IS NOT NULL
GROUP BY director
ORDER BY avg_rating DESC;

-- * that has more than 5 movies
SELECT director, AVG(rating) AS avg_rating
FROM movies
WHERE director IS NOT NULL AND rating IS NOT NULL 
GROUP BY director
HAVING COUNT(*) > 5
ORDER BY avg_rating DESC;


-- Q2. How many movies are in each genre?
SELECT genres, COUNT(genres) AS cnt_genres
FROM movies
WHERE genres IS NOT NULL
GROUP BY genres
ORDER BY cnt_genres DESC;

-- Q3. How many movies have a rating greater than 6? what is the most common?
SELECT rating
FROM movies
WHERE rating > 6
GROUP BY rating
ORDER BY count(*) DESC;
```



## #4.15 Super Practice Part 2

```sql
-- Q1. Find the number of movies released each year.
SELECT release_date, COUNT(*)
FROM movies
WHERE release_date IS NOT NULL
GROUP BY release_date
ORDER BY release_date;

-- Q2. List the top 10 years with the highest average movie runtime.
SELECT release_date, AVG(runtime) AS avg_runtime
FROM movies
GROUP BY release_date
ORDER BY avg_runtime DESC
LIMIT 10;

-- Q3. Calculate the average rating for movies released in the 21st century.
SELECT AVG(rating)
FROM movies
WHERE release_date > 2000;

-- Q4. Find the director with the highest average movie runtime.
SELECT director
FROM movies
WHERE director IS NOT NULL AND RUNTIME IS NOT NULL
GROUP BY director
ORDER BY avg(runtime) DESC
LIMIT 1;

-- Q5. List the top 5 most prolific directors.
-- (those who have directed the most movies)
SELECT director
FROM movies
WHERE director IS NOT NULL
GROUP BY director
ORDER BY COUNT(*) DESC
LIMIT 5;

-- Q6. Find the highest and lowest rating of each director.
SELECT director, MAX(rating) AS high_rating, MIN(rating) AS low_rating
FROM movies
WHERE director IS NOT NULL AND rating IS NOT NULL
GROUP BY director;

-- Q7. Find the director that has made the most money (revenue - budget)
SELECT director, SUM(revenue)-SUM(budget) AS earn
FROM movies
WHERE director IS NOT NULL AND revenue IS NOT NULL AND budget IS NOT NULL
GROUP BY director
ORDER BY earn DESC;

-- Q8. Calculate the average rating for movies longer than 2 hours.
SELECT AVG(rating)
FROM movies
WHERE runtime >= 120 AND rating IS NOT NULL

-- Q9. Find the year with the most movies released.
SELECT release_date
FROM movies
WHERE release_date IS NOT NULL
GROUP BY release_date
ORDER BY COUNT(*) DESC
LIMIT 1;

-- Q10. Find the average runtime of movies for each decade.
SELECT (release_date/10)*10 AS decade, avg(runtime)
FROM movies
WHERE release_date IS NOT NULL AND runtime IS NOT NULL
GROUP BY decade
ORDER BY COUNT(*) DESC;

-- Q11. List the top 5 years
-- where the difference between the highest and lowest rated movie
-- was the greatest.
SELECT release_date, MAX(rating) - MIN(rating) AS difference
FROM movies
WHERE release_date IS NOT NULL AND RATING IS NOT NULL
GROUP BY release_date
ORDER BY difference DESC
LIMIT 5;


-- Q12. List directors who have never made a movie shorter than 2 hours.
SELECT director
FROM movies
GROUP BY director
HAVING MIN(runtime) >= 120;



```



## #4.16 Super Practice Part 3

```sql
-- Q13. Calculate the percentage of movies with a rating above 8.0.
SELECT
	COUNT(CASE WHEN rating > 8 THEN 1 END) * 100 / COUNT(*) AS percentage
FROM movies;

-- Q14. Find the director with the highest ratio of movies rated above 7.0.
SELECT director,
	COUNT(CASE WHEN rating > 7 THEN 1 END) * 100 / COUNT(*) AS ratio
FROM movies
WHERE director IS NOT NULL
GROUP BY director
HAVING COUNT(*) >= 5
ORDER BY ratio DESC;

-- Q15. Categorize and group movies by length.

SELECT 
	CASE WHEN runtime < 90 THEN 'Short' 
	WHEN runtime BETWEEN 90 AND 120 THEN 'Normal'
  WHEN runtime > 120 THEN 'LONG'
  END AS runtime_category,
  COUNT(*) AS cnt_movies
FROM movies
WHERE runtime IS NOT NULL
GROUP BY runtime_category
ORDER BY cnt_movies DESC;


-- Q16. Categorize and group movies by flop or not
-- flop is more cost then income
SELECT
  CASE WHEN revenue < budget THEN 'flop' ELSE 'Sucess'
  END AS flop_category,
  count(*) as cnt_movies
FROM movies
WHERE budget IS NOT NULL AND revenue IS NOT NULL
GROUP BY flop_category;
```



## #4.17 Views

View [https://www.tutorialspoint.com/sqlite/sqlite\_views.htm\
](https://www.tutorialspoint.com/sqlite/sqlite\_views.htm)



```sql
-- View 저장
CREATE VIEW v_something AS SELECT ~~~~;

-- View 삭제
DROP VIEW v_something
```

