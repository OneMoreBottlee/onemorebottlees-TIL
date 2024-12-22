# #12 Full-Text Indexes

## #12.1 Natural Language Search

```sql
CREATE fulltext INDEX idx_overview ON movies (overview);


-- Natural Language Full-Text Search 
SELECT title, overview
FROM movies
WHERE MATCH(overview) AGAINST ('the food');

SELECT title, overview, MATCH(overview) AGAINST ('the food and the drinks') as score
FROM movies
WHERE MATCH(overview) AGAINST ('the food and the drinks');
```



## #12.2 Boolean Mode Search

```sql
-- Boolean Mode Search +aaa bbb -ccc +필수 있으면좋고 -제외
SELECT title, overview, MATCH(overview) AGAINST ('+revenge food -violence' IN BOOLEAN MODE) as score
FROM movies
WHERE MATCH(overview) AGAINST ('+revenge food -vilence' IN BOOLEAN MODE);

-- >가중치 높 <가중치 낮 & "띄어쓰기 있으면"
SELECT title, overview, MATCH(overview) AGAINST ('+travel >asia <europe' IN BOOLEAN MODE) as score
FROM movies
WHERE MATCH(overview) AGAINST ('+travel >asia <europe' IN BOOLEAN MODE);

-- ()
SELECT title, overview, MATCH(overview) AGAINST ('+(action thriller) -horror +love' IN BOOLEAN MODE) as score
FROM movies
WHERE MATCH(overview) AGAINST ('+(action thriller) -horror +love' IN BOOLEAN MODE);

-- *
SELECT title, overview, MATCH(overview) AGAINST ('psycho*' IN BOOLEAN MODE) as score
FROM movies
WHERE MATCH(overview) AGAINST ('psycho*' IN BOOLEAN MODE);
```

[https://dev.mysql.com/doc/refman/8.4/en/fulltext-boolean.html\
\
](https://dev.mysql.com/doc/refman/8.4/en/fulltext-boolean.html)

## #12.3 Query Expansion

```sql
-- Query Expansion Search
-- 2번 검색_ 키워드 포함된 데이터 검색 > 관련성 높은 데이터 추출
-- 검색어가 짧을 때 유용, 데이터베이스에 암시적인 지식 전달하는 것과 같음
SELECT
  title,
  overview,
  MATCH(overview) AGAINST ('KIMCHI' WITH QUERY EXPANSION) as score
FROM movies
WHERE MATCH(overview) AGAINST ('KIMCHI' WITH QUERY EXPANSION);
```





