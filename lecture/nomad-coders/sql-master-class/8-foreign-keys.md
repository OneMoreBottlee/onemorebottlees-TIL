# #8 Foreign Keys

## #8.1 Data Normalization

데이터베이스 정규화

* Normal form에 따라 Database를 설계하고 디자인하는 방법에 대한 모범 사례
* Database에서 중복되는 data를 제거할 수 있게 해줌, data가 한 곳에만 저장되도록 함
* 중복되지 않은 data는 수정 / 추가 / 제거 등이 빠르다



## #8.2 Entities

```sql
CREATE TABLE dogs (
    dog_id BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50) NOT NULL,
    breed_name VARCHAR(50) NOT NULL,
    breed_size_category ENUM('small', 'medium', 'big') DEFAULT 'small',
    breed_typical_lifespan TINYINT,
    date_of_birth DATE,
    weight DECIMAL(5,2),
    owner_name VARCHAR(50) NOT NULL,
    owner_email VARCHAR(100) UNIQUE,
    owner_phone VARCHAR(20),
    owner_address TINYTEXT
);
-- DOG / BREED / OWNER 3개의 entity로 구분 가능

CREATE TABLE dogs (
  dog_id BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(50) NOT NULL,
  date_of_birth DATE,
  weight DECIMAL(5,2),
  owner_id BIGINT UNSIGNED,
  breed_id BIGINT UNSIGNED
);

CREATE TABLE owners (
  owner_id BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
  owner_name VARCHAR(50) NOT NULL,
  owner_email VARCHAR(100) UNIQUE,
  owner_phone VARCHAR(20),
  owner_address TINYTEXT
);

CREATE TABLE breeds (
  breed_id BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
  breed_name VARCHAR(50) NOT NULL,
  breed_size_category ENUM('small', 'medium', 'big') DEFAULT 'small',
  breed_typical_lifespan TINYINT
);
```



## #8.3 Foreign Keys

Foreign Key - 다른 table에 있는 column을 참조하는 column

Foreign Key Constraints - Foreign Key에 제약을 줌으로써 일관된 데이터를 저장할 수 있음

```sql
CREATE TABLE dogs (
  dog_id BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(50) NOT NULL,
  date_of_birth DATE,
  weight DECIMAL(5,2),
  owner_id BIGINT UNSIGNED,
  breed_id BIGINT UNSIGNED,
  FOREIGN KEY (owner_id) REFERENCES owners (owner_id),
  CONSTRAINT breed_fk FOREIGN KEY (breed_id) REFERENCES breeds (breed_id)
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
  size_category ENUM('small', 'medium', 'big') DEFAULT 'small',
  typical_lifespan TINYINT
);

INSERT
  INTO breeds (name, size_category, typical_lifespan)
  VALUES ('Golden Retriever', 'big', 12);

INSERT
  INTO owners (name, email, phone, address)
  VALUES ('Adam Smith', 'adam@smith.com', '11223344455', '9010 St. Scotland');

INSERT
  INTO dogs (name, date_of_birth, weight, breed_id, owner_id)
  VALUES ('Champ', '2022-03-15', 10.5, 1, 1);
```



## #8.4 ON DELETE

ON DELETE

CASCADE - 데이터가 삭제되면 연관된 데이터도 함께삭제

SET NULL - NOT NULL이 아닐 때 사용 가능, NULL

SET DEFAULT - 기본값 설정

```sql
CREATE TABLE dogs (
  dog_id BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(50) NOT NULL,
  date_of_birth DATE,
  weight DECIMAL(5,2),
  owner_id BIGINT UNSIGNED,
  breed_id BIGINT UNSIGNED,
  FOREIGN KEY (owner_id) REFERENCES owners (owner_id) ON DELETE CASCADE,
  constraint breed_fk FOREIGN KEY (breed_id) REFERENCES breeds (breed_id) ON DELETE SET DEFAULT
);

ALTER TABLE dogs
	DROP FOREIGN KEY owner_fk;
	ADD CONSTRAINT owner_fk FOREIGN KEY (owner_id) REFERENCES owners (owner_id) ON DELETE SET NULL;
```



## #8.5 One-To-Many and One-To-One

<figure><img src="../../../.gitbook/assets/image (267).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (269).png" alt=""><figcaption></figcaption></figure>

## #8.6 Many-To-Many

<figure><img src="../../../.gitbook/assets/image (270).png" alt=""><figcaption></figcaption></figure>

