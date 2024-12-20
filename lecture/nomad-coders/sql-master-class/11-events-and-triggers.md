# #11 Events & Triggers

## #11.0 Introduction

Event - database가 해야 할 작업들의 스케줄링 가능

ex) 특정 조건의 row를 특정 요일에 삭제



Trigger - database에서 일어나는 일에 반응 가능

ex) 특정 row 삭제시 삭제에 대한 로그 table 생성



## #11.1 Events

```sql
CREATE TABLE archived_movies LIKE movies;
-- table 구조 복사

DROP TABLE archived_movies;
TRUNCATE TABLE archived_movies; -- table 모든 내용 삭제

DELIMITER $$ -- 다른 DELIMITER 사용
CREATE EVENT archive_old_movies
ON SCHEDULE EVERY 2 MINUTE
STARTS CURRENT_TIMESTAMP + INTERVAL 2 MINUTE
DO -- 1개는 DO만 사용
BEGIN 
	INSERT INTO archived_movies
  SELECT * FROM movies
  WHERE release_date < YEAR(CURDATE()) - 20;
  
  DELETE FROM movies WHERE release_date < YEAR((CURDATE()) - 20;
END$$
DELIMITER ;

DROP EVENT archive_old_movies;
```



## #11.2 Recap

[https://chatgpt.com/share/d5844020-c1d8-46d9-a363-879f3421750f](https://chatgpt.com/share/d5844020-c1d8-46d9-a363-879f3421750f)



show events;

show create event \~\~\~;



## #11.3 Triggers

```sql
-- before - insert, update, delete 전
-- after - insert, update, delete 후

CREATE TABLE records (
	record_id BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
  changes tinytext,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP NOT NULL
);

INSERT INTO movies SELECT * FROM archived_movies WHERE movie_id = 2;

-- INSERT
CREATE TRIGGER before_movie_insert
BEFORE INSERT
ON movies
FOR EACH ROW
INSERT INTO records (changes) VALUES (CONCAT('Will Insert', NEW.title));

CREATE TRIGGER after_movie_insert
AFTER INSERT
ON movies
FOR EACH ROW
INSERT INTO records (changes) VALUES (CONCAT( 'Insert completed: ', NEW.TITLE));

-- UPDATE
CREATE TRIGGER before_movie_update
BEFORE UPDATE
ON movies
FOR EACH ROW
INSERT INTO records (changes) VALUES (CONCAT('Will update title: ', OLD.title, ' -> ', NEW.title));

CREATE TRIGGER after_movie_update
AFTER UPDATE
ON movies
FOR EACH ROW
INSERT INTO records (changes) VALUES (CONCAT('update completed: ', OLD.title, ' -> ', NEW.title));

-- DELETE
CREATE TRIGGER before_movie_delete
BEFORE DELETE
ON movies
FOR EACH ROW
INSERT INTO records (changes) VALUE (CONCAT('Will delete: ', OLD.title));

CREATE TRIGGER after_movie_delete
AFTER DELETE
ON movies
FOR EACH ROW
INSERT INTO records (changes) VALUE (CONCAT('Bye : ', OLD.title));
```



## #11.4 Overpowered Trigger

```sql
SHOW TRIGGERS;

DROP TRIGGER after_movie_update;


TRUNCATE TABLE records;

DROP TRIGGER after_movie_update;

DELIMITER $$
CREATE TRIGGER after_movie_update
AFTER UPDATE
ON movies
FOR EACH ROW
BEGIN
  DECLARE changes TINYTEXT DEFAULT '';
	
  IF NEW.title <> OLD.title THEN
  	SET changes = CONCAT('Title Changed', OLD.title, '->', NEW.title, '\n ');
  END IF;
  
  IF NEW.budget <> OLD.budget THEN
  	SET changes = CONCAT(changes, 'Budget changed ', OLD.budget, '->', NEW.budget);
  END IF;
  
  INSERT INTO records (changes) VALUES (changes);
  
END$$
DELIMITER ;
```

