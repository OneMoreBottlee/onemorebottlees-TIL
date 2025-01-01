# #15 Transactions

## #15.1 Intoruction

Transaction - 유저 데이터를 다루는 어플리케이션에서 사용하기 좋은 sql 코드 작성할 수 있게 해줌, 여러 sql 쿼리에 거쳐 데이터 무결성을 강제해주는 등 중요 데이터를 다루는 산업에서 사용하는 개념

Transaction Control Language



## #15.2 Transaction Are Awesome

```sql
CREATE TABLE accounts (
  account_id BIGINT PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
  account_holder VARCHAR(100) NOT NULL,
  balance DECIMAL(10, 2) NOT NULL CHECK (balance >= 0)
);

drop table accounts;

INSERT INTO accounts (account_holder, balance) VALUES
('SON', 1000.00),
('KIM', 2000.00);

BEGIN; -- TRANSACTION 시작 // MYSQL에서는 START TRANSACTION

  SELECT * FROM accounts;

  UPDATE accounts SET balance = balance + 1500 WHERE account_holder = 'SON';
  
  SELECT * FROM accounts;
  UPDATE accountS SET balance = balance - 1500 WHERE account_holder = 'KIM';

COMMIT; -- TRANSACTION 종료

SELECT * FROM accounts;
```



## #15.3 ACID

transaction 시스템은 ACID 라는 특징이 있다.



Atomic - All or Nothing 모든 작업이 성공하거나 작동하지 않아야 함

Consistent - 유효한(valid) 상태에서 또 다른 유효(valid) 상태가 되어야 함

Isolated - 하나의 transaction에서 실행된 변경 사항이 commit 되기 전까지 다른 transaction에서 확인할 수 없음

Durablity - transaction이 실행되면 데이터베이스 전원이 나가거나 서버가 죽거나 하는 상황에도 변경 사항들이 영구적으로 유지되는 걸 확신할 수 있어야 한다.



auto commit mode 에서는 SELECT FROM, UPDATE, DELETE 등 모든 명령문이 그 자체로 하나의 작은 transaction 취급됨



## #15.4 Savepoints







## #15.5&#x20;





