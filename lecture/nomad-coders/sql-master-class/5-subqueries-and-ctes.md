# #5 Subqueries and CTEs

## #5.1 Introduction

subquery와 CTE를 활용해 더 나은 활용이 가능

```sql
-- List movies with a (rating|revenue) higher than the average (rating|revenue) of all movies.
-- List movies with a rating higher than the average rating of movies in their genre.
-- Find the directors with a career revenue higher than the average revenue of all directors.
-- Find the movies with a rating higher than the average rating of movies released in the same year.
-- Make a super slow query and optimize it as much as we can with what we know so far.
```



## #5.1 Independent Subqueries

Subquery - 다른 쿼리 내부에 있는 쿼리로 괄호로 감싸 사용

Independence Subquery - 서브 쿼리의 일종으로 쿼리의 결과가 바뀌지 않기에 한번만 실행시킨 후 값을 기억해 나머지에 활용

```sql
-- List movies with a (rating|revenue) higher
-- than the average (rating|revenue) of all movies.

SELECT title, rating, revenue
FROM movies
WHERE 
  rating > (SELECT avg(rating) FROM movies) AND
  revenue > (SELECT avg(revenue) FROM movies)
```



## #5.2 CTEs

Common Table Expressions - Independent Subqueries를 재사용할 수 있게 한다.

SQL 쿼리를 더 작은 논리적 단위로 나눠 모듈식 SQL 쿼리를 구현한다.

View와 비슷하지만 영구적이지 않으며 해당 쿼리의 context에서만 생성되고 버려진다.

```sql
-- CTE 생성하기
WITH cte_something AS (SELECT AVG(rating) FROM movies)


-- CTE EX
WITH avg_revenue_cte AS (SELECT AVG(revenue) FROM movies),
    avg_rating_cte AS (SELECT AVG(rating) FROM movies)

SELECT
    title, director, revenue, rating,
    round((SELECT * FROM avg_revenue_cte), 0) AS avg_revenue,
    round((SELECT * FROM avg_rating_cte), 0) AS avg_rating
FROM movies
WHERE
    revenue > (SELECT avg_revenue FROM avg_revenue_cte) AND
    rating > (SELECT avg_rating FROM avg_rating_cte);
```



## #5.3 Correlated Subqueries



