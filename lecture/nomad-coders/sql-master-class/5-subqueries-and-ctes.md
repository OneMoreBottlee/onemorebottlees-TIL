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

상관관계 서브쿼리 - 외부 쿼리의 값을 사용하는 서브쿼리\
외부 쿼리에서 참조되는 테이블에서 데이터를 검색함\
서브쿼리의 실행이 외부 쿼리의 행에 영향을 받기 때문에 "상관됨"이라고 함\
상관관계가 없는 서브쿼리에서는 서브쿼리가 먼저 독립적으로 실행된 다음 해당 결과가 외부 쿼리의 값으로 사용되지만, 상관 관계가 있는 서브쿼리는 열을 참조하여 외부 쿼리에 연결되고 외부 쿼리에서 처리하는 각 행에 대해 한 번씩 반복적으로 실행된다.

```sql
-- Find the movies with a rating higher than
-- the average rating of movies released in the same year.

-- Correlated Subquery
SELECT 
    main_movies.title,
    main_movies.director,
    main_movies.rating,
    main_movies.release_date,
    (
        SELECT avg(inner_movies.rating)
        FROM movies AS inner_movies
        WHERE inner_movies.release_date = main_movies.release_date) AS year_average
    )
FROM movies AS main_movies
WHERE main_movies.rating > (
    SELECT avg(inner_movies.rating)
    FROM movies AS inner_movies
    WHERE innre_movies.release_date = main_movies.release_date
    );

```



## #5.4 Correlated CTEs

```sql
WITH movie_avg_per_year AS (
    SELECT avg(inner_movies.rating)
    FROM movies AS inner_movies
    WHERE inner_movies.release_date = main_movies.release_date
)

SELECT 
    main_movies.title,
    main_movies.director,
    main_movies.rating,
    main_movies.release_date,
    (
        SELECT *
        FROM movie_avg_per_year) AS year_average
    )
FROM movies AS main_movies
WHERE main_movies.rating > (
    SELECT *
    FROM movie_avg_per_year
)
```



## #5.5 Subquery Practice

```sql
-- List movies with a rating higher
-- than the average rating of movies in their genre.

WITH avg_rating_genre AS (
    SELECT avg(inner_movies.rating)
    FROM movies AS inner_movies
    WHERE inner_movies.genre = main_movies.genre
)

SELECT title
FROM movies AS main_movies
WHERE main_moveis.rating > (
    SELECT *
    FROM avg_rating_genre
)


-- Find the directors with a career revenue higher
-- than the average revenue of all directors.

WITH directors_revenues AS (
    SELECT sum(revenue) AS career_revenue
    FROM movies
    WHERE director IS NOT NULL AND revenue IS NOTT NULL
    GROUP BY director
), avg_director_carrer_revenue AS (
    SELECT avg(career_revenue)
    FROM directors_revenues
)

SELECT
    director,
    sum(revenue) AS total_revenue,
    avg_director_carrer_revenue AS peersAVG
FROM movies
WHERE
    director IS NOT NULL
    AND revenue IS NOT NULL
GROUP BY director
HAVING total_revenue > avg_director_carrer_revenue;

```



## #5.6 Final Practice

```sql
WITH director_stats AS (
    SELECT
        director,
        COUNT(*) AS total_movies,
        AVG(rating) AS avg_rating,
        MAX(rating) AS best_rating,
        MIN(rating) AS worst_rating,
        MAX(budget) AS highest_budget,
        MIN(budget) AS lowest_budget
    FROM movies
    WHERE director IS NOT NULL AND budget IS NOT NULL AND rating IS NOT NULL
    GROUP BY director
)

SELECT 
        director,
        total_movies,
        avg_rating,
        best_rating,
        worst_rating,
        highest_budget,
        lowest_budget,
    (
        SELECT title
        FROM movies
        WHERE rating IS NOT NULL AND budget IS NOT NULL AND director = ds.director
        ORDER BY rating DESC
        LIMIT 1
    ) AS best_rated_movie,
    (
        SELECT title
        FROM movies
        WHERE rating IS NOT NULL AND budget IS NOT NULL AND director = ds.director
        ORDER BY rating ASC
        LIMIT 1
    ) AS worst_rated_movie,
    (
        SELECT title
        FROM movies
        WHERE rating IS NOT NULL AND budget IS NOT NULL AND director = ds.director
        ORDER BY budget DESC
        LIMIT 1
    ) AS best_expensive_movie,
    (
        SELECT title
        FROM movies
        WHERE rating IS NOT NULL AND budget IS NOT NULL AND director = ds.director
        ORDER BY rating ASC
        LIMIT 1
    ) AS least_expensive_movie
FROM director_stats AS ds;


-- CREATE INDEX idx_director ON movies (director);
-- DROP INDEX idx_director;
```

