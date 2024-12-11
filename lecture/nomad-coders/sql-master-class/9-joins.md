# #9 JOINS

\#9.0 Introduction

```sql
-- Create tables
CREATE TABLE dogs (
dog_id BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
name VARCHAR(50) NOT NULL,
weight DECIMAL(5,2),
date_of_birth DATE,
owner_id BIGINT UNSIGNED,
breed_id BIGINT UNSIGNED,
FOREIGN KEY (owner_id) REFERENCES owners (owner_id) ON DELETE SET NULL,
CONSTRAINT breed_fk FOREIGN KEY (breed_id) REFERENCES breeds (breed_id) ON DELETE SET NULL
);

CREATE TABLE owners (
owner_id BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
name VARCHAR(50) NOT NULL,
email VARCHAR(100) UNIQUE,
phone VARCHAR(20),
address TINYTEXT
);

CREATE TABLE breeds (
breed_id BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
name VARCHAR(50) NOT NULL,
size_category ENUM ('small', 'medium', 'big') DEFAULT 'small',
typical_lifespan TINYINT
);

CREATE TABLE pet_passports (
pet_passport_id BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
blood_type VARCHAR(10),
allergies TEXT,
last_checkup_date DATE,
dog_id BIGINT UNSIGNED UNIQUE,
FOREIGN KEY (dog_id) REFERENCES dogs (dog_id) ON DELETE CASCADE
);

CREATE TABLE tricks (
trick_id BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
name VARCHAR(50) UNIQUE NOT NULL,
difficulty ENUM('easy', 'medium', 'hard') NOT NULL DEFAULT 'easy'
);

CREATE TABLE dog_tricks (
dog_id BIGINT UNSIGNED,
trick_id BIGINT UNSIGNED,
proficiency ENUM('beginner', 'intermediate', 'expert') NOT NULL DEFAULT 'beginner',
date_learned TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
PRIMARY KEY (dog_id, trick_id),
FOREIGN KEY (dog_id) REFERENCES dogs (dog_id) ON DELETE CASCADE,
FOREIGN KEY (trick_id) REFERENCES tricks (trick_id) ON DELETE CASCADE
);

-- INSERT
INSERT INTO breeds (name, size_category, typical_lifespan) VALUES
('Labrador Retriever', 'big', 12),
('German Shepherd', 'big', 11),
('Golden Retriever', 'big', 11),
('French Bulldog', 'small', 10),
('Beagle', 'medium', 13),
('Poodle', 'medium', 14),
('Chihuahua', 'small', 15);



INSERT INTO owners (name, email, phone, address) VALUES
('John Doe', 'john@example.com', '123-456-7890', '123 Main St, Anytown, USA'), ('Jane Smith', 'jane@example.com', '234-567-8901', '456 Elm St, Someplace, USA'), ('Bob Johnson', 'bob@example.com', '345-678-9012', '789 Oak St, Elsewhere, USA'), ('Alice Brown', 'alice@example.com', '456-789-0123', '321 Pine St, Nowhere, USA'), ('Charlie Davis', 'charlie@example.com', '567-890-1234', '654 Maple St, Somewhere, USA'), ('Eva Wilson', 'eva@example.com', '678-901-2345', '987 Cedar St, Anyville, USA'), ('Frank Miller', 'frank@example.com', '789-012-3456', '246 Birch St, Otherville, USA'), ('Grace Lee', 'grace@example.com', '890-123-4567', '135 Walnut St, Hereville, USA'), ('Henry Taylor', 'henry@example.com', '901-234-5678', '864 Spruce St, Thereville, USA'), ('Ivy Martinez', 'ivy@example.com', '012-345-6789', '753 Ash St, Whereville, USA'), ('Jack Robinson', 'jack@example.com', '123-234-3456', '951 Fir St, Thatville, USA'), ('Kate Anderson', 'kate@example.com', '234-345-4567', '159 Redwood St, Thisville, USA');


INSERT INTO dogs (name, date_of_birth, weight, breed_id, owner_id) VALUES
('Max', '2018-06-15', 30.5, 1, 1),
('Bella', '2019-03-22', 25.0, NULL, 2),
('Charlie', '2017-11-08', 28.7, 2, 3),
('Lucy', '2020-01-30', 8.2, NULL, NULL),
('Cooper', '2019-09-12', 22.3, 5, 5),
('Luna', '2018-07-05', 18.6, 6, 6),
('Buddy', '2016-12-10', 31.2, 1, 7),
('Daisy', '2020-05-18', 6.8, NULL, 8),
('Rocky', '2017-08-25', 29.5, 2, 9),
('Molly', '2019-11-03', 24.8, 3, NULL),
('Bailey', '2018-02-14', 21.5, 5, 11),
('Lola', '2020-03-27', 7.5, 4, 12),
('Duke', '2017-05-09', 32.0, NULL, 1),
('Zoe', '2019-08-11', 17.8, 6, 2),
('Jack', '2018-10-20', 23.6, NULL, 3),
('Sadie', '2020-02-05', 26.3, 3, 4),
('Toby', '2017-07-17', 8.9, 7, NULL),
('Chloe', '2019-04-30', 20.1, 6, 6),
('Bear', '2018-01-08', 33.5, 2, 7),
('Penny', '2020-06-22', 7.2, 4, NULL);

INSERT INTO tricks (name, difficulty) VALUES
('Sit', 'easy'),
('Stay', 'medium'),
('Fetch', 'easy'),
('Roll Over', 'hard'),
('Shake Hands', 'medium');


INSERT INTO dog_tricks (dog_id, trick_id, proficiency, date_learned) VALUES
(1, 1, 'expert', '2019-01-15'),
(1, 2, 'intermediate', '2019-03-20'),
(14, 3, 'expert', '2019-02-10'),
(2, 1, 'expert', '2019-07-05'),
(2, 3, 'intermediate', '2019-08-12'),
(3, 1, 'expert', '2018-03-10'),
(3, 2, 'expert', '2018-05-22'),
(13, 4, 'beginner', '2019-11-30'),
(4, 1, 'intermediate', '2020-05-18'),
(5, 1, 'expert', '2020-01-07'),
(11, 3, 'expert', '2020-02-15'),
(5, 5, 'intermediate', '2020-04-22'),
(7, 1, 'expert', '2017-06-30'),
(7, 2, 'expert', '2017-08-14'),
(12, 3, 'expert', '2017-07-22'),
(16, 4, 'intermediate', '2018-01-05'),
(7, 5, 'expert', '2017-09-18'),
(10, 1, 'intermediate', '2020-03-12'),
(10, 3, 'beginner', '2020-05-01'),
(15, 1, 'expert', '2019-02-28'),
(14, 2, 'intermediate', '2019-04-15'),
(18, 1, 'intermediate', '2019-09-10'),
(18, 5, 'beginner', '2020-01-20');


INSERT INTO pet_passports (dog_id, blood_type, allergies, last_checkup_date) VALUES
(1, 'DEA 1.1+', 'None', '2023-01-05'),
(2, 'DEA 1.1-', 'Chicken', '2023-02-22'),
(3, 'DEA 4+', 'None', '2023-03-08'),
(5, 'DEA 7+', 'Beef', '2023-04-12'),
(7, 'DEA 1.1+', 'None', '2023-01-10'),
(10, 'DEA 3-', 'Dairy', '2023-05-03'),
(12, 'DEA 5-', 'None', '2023-03-27'),
(15, 'DEA 1.1-', 'Grains', '2023-04-20'),
(18, 'DEA 7+', 'None', '2023-04-03'),
(20, 'DEA 4+', 'Pollen', '2023-06-22');,
```



## #9.1 CROSS JOIN

JOIN 여러 테이블을 연결해 하나의 결과 도출



CROSS JOIN - 테이블의 모든 데이터 연결, 거의 사용하지 않음

```sql
SELECT * FROM dogs CROSS JOIN owners;

/**
    SELECT * FROM CROSS JOIN owners WHERE ...;
    FROM > JOIN > WHERE > SELECT > GROUP BY, HAVING ... 의 순서로 실행
**/
```



## #9.2 INNER JOIN

INNER JOIN - 연결되는 row 선택 가능, CROSS JOIN 과 비슷하지만 다름

<figure><img src="../../../.gitbook/assets/image (271).png" alt=""><figcaption></figcaption></figure>

```sql
SELECT * FROM dogs INNER JOIN owners;

SELECT * FROM dogs INNER JOIN owners ON dogs.owner_id = owners.owner_id;

-- 개주인 이름이 같은 데이터 출력
SELECT dogs.name as dog_name, owners.name as owner_name
FROM dogs
INNER JOIN owners ON dogs.owner_id = owners.owner_id;


-- & 견종이 같은 데이터
SELECT
  dogs.name as dog_name,
  owners.name as owner_name,
  breeds.name as breed_name
FROM dogs
  INNER JOIN owners ON dogs.owner_id = owners.owner_id
  JOIN breeds ON dogs.breed_id = breeds.breed_id;
  
-- USING
SELECT
  dogs.name as dog_name,
  owners.name as owner_name,
  breeds.name as breed_name
FROM dogs
  INNER JOIN owners USING (owner_id)
  JOIN breeds USING (breed_id);
```

JOIN = INNER JOIN



## #9.3  OUTER JOIN

애매하거나 의미가 불분명한 row를 확인할 때 사용

<figure><img src="../../../.gitbook/assets/image (272).png" alt=""><figcaption></figcaption></figure>

```sql
-- 주인없는 개도 함께 확인
SELECT
  dogs.name as dog_name,
  owners.name as owner_name
FROM dogs
  LEFT JOIN owners USING (owner_id);
```

<figure><img src="../../../.gitbook/assets/image (273).png" alt=""><figcaption></figcaption></figure>

```sql
-- 개 없는 주인 확인
SELECT
  dogs.name as dog_name,
  owners.name as owner_name
FROM dogs
  RIGHT JOIN owners USING (owner_id);
```

LEFT OUTER JOIN = LEFT JOIN

RIGHT OUTER JOIN = RIGHT JOIN



## #9.4 JOINS Practice Part 1

```sql
-- 1. List all dogs with their breed names
SELECT dogs.name, breeds.name
FROM dogs JOIN breeds USING(breed_id);

-- 2. Show all owners and their dogs (if they have any)
SELECT owners.name, dogs.name
FROM owners JOIN dogs USING(owner_id);

-- 3. Display all breeds and the dogs of that breed (if any)
SELECT breeds.name, dogs.name
FROM breeds JOIN dogs USING(breed_id);

-- 4. List all dogs with their pet passport information and owner data
-- (if avaliable)
SELECT 
	d.name, pp.allergies, o.name
FROM dogs d
	JOIN pet_passports AS pp USING(dog_id)
	JOIN owners o USING(owner_id);

-- 5. Show all tricks and the dogs that know them
SELECT t.name, d.name, dt.date_learned, dt.proficiency
FROM tricks t
	JOIN dog_tricks dt USING(trick_id)
	JOIN dogs d USING(dog_id);

-- 6. Display all dogs that don't know a single trick
SELECT dogs.name
FROM dogs LEFT JOIN dog_tricks USING(dog_id)
WHERE dog_tricks.dog_id is null;
```



## #9.5 JOINS Practice Part 2

```sql
-- 1. Show all breeds and the count of dogs for each breed
SELECT breeds.name, COUNT(*)
FROM breeds RIGHT JOIN dogs USING(breed_id)
GROUP BY breeds.name;

-- 2. Display all owners with the count of their dogs, the average dog weight and the average dog age.
SELECT owners.name AS OWNER_NAME,
    count(dogs.dog_id) AS TOTAL_DOGS,
    avg(weight) AS AVG_WEIGHT,
    avg(timestampdiff(YEAR, dogs.date_of_birth, CURDATE())) as AVG_AGE
FROM owners JOIN dogs USING(owner_id)
GROUP BY owners.owner_id

-- 3. Show all tricks and the number of dogs that know each trick ordered by popularity
SELECT tricks.name, count(*) AS TOTAL_DOGS
FROM dog_tricks	JOIN tricks USING(trick_id)
GROUP BY dog_tricks.trick_id
ORDER BY TOTAL_DOGS DESC;

-- 4. Display all dogs along with the count of tricks they know
SELECT dogs.name, count(*) AS CNT_TRICKS
FROM dogs JOIN dog_tricks USING(dog_id)
GROUP BY dogs.dog_id
ORDER BY CNT_TRICKS DESC;

-- 5. List all owner with their dogs and the tricks their dogs know
SELECT owners.name, dogs.name, tricks.name
FROM owners
    JOIN dogs USING(owner_id)
  JOIN dog_tricks USING(dog_id)
  JOIN tricks USING(trick_id);
```



## #9.6 JOINS Practice Part 3

```sql
-- 1. Show all breeds with their average dog weight and typical lifespan
SELECT
  breeds.name,
  avg(dogs.weight) avg_weight,
  breeds.typical_lifespan
FROM breeds JOIN dogs USING(breed_id)
GROUP BY breeds.breed_id, breeds.typical_lifespan


-- 2. Display all dogs with their latest checkup date and the time since their last checkup
SELECT
  dogs.name,
  pet_passports.last_checkup_date,
  TIMESTAMPDIFF(DAY, pet_passports.last_checkup_date, CURDATE())
FROM dogs
  JOIN pet_passports USING(dog_id)


-- 3. Display all breeds with the name of the heaviest dog of that breed
SELECT breeds.breed_id, breeds.name, dogs.name, dogs.weight
FROM breeds JOIN dogs USING(breed_id)
WHERE dogs.weight = (
	SELECT max(d1.weight)
  FROM dogs d1
  WHERE d1.breed_id = breeds.breed_id
)

-- 4. List all tricks with the name of the dog who learned it most recently
SELECT
  tricks.name,
  dogs.name,
  dog_tricks.date_learned
FROM tricks
  JOIN dog_tricks USING(trick_id)
  JOIN dogs USING(dog_id)
WHERE dog_tricks.date_learned = (
  SELECT max(dt.date_learned)
  FROM dog_tricks dt
  WHERE dt.trick_id = tricks.trick_id
  GROUP BY trick_id
)
```

