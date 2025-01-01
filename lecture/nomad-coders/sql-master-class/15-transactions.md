# #15 Transactions

## #15.0 Introduction

Transaction - 유저 데이터를 다루는 어플리케이션에서 사용하기 좋은 sql 코드 작성할 수 있게 해줌, 여러 sql 쿼리에 거쳐 데이터 무결성을 강제해주는 등 중요 데이터를 다루는 산업에서 사용하는 개념

Transaction Control Language



## #15.1 Transaction Are Awesome

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



## #15.2 ACID

transaction 시스템은 ACID 라는 특징이 있다.



Atomic - All or Nothing 모든 작업이 성공하거나 작동하지 않아야 함

Consistent - 유효한(valid) 상태에서 또 다른 유효(valid) 상태가 되어야 함

Isolated - 하나의 transaction에서 실행된 변경 사항이 commit 되기 전까지 다른 transaction에서 확인할 수 없음

Durablity - transaction이 실행되면 데이터베이스 전원이 나가거나 서버가 죽거나 하는 상황에도 변경 사항들이 영구적으로 유지되는 걸 확신할 수 있어야 한다.



auto commit mode 에서는 SELECT FROM, UPDATE, DELETE 등 모든 명령문이 그 자체로 하나의 작은 transaction 취급됨



## #15.3 Savepoints&#x20;

like checkpoint in game

```sql
BEGIN;
  UPDATE accounts
  SET balance = balance + 1000
  WHERE acount_holde = 'KIM';
  
  SAVEPOINT transfer_one; -- 여기로 ROLLBACK
  
  SELECT * FROM accounts;
  
  UPDATE accounts
  SET account_holder = 'rich KIM'
  WHERE acount_holde = 'KIM';
  
  ROLLBACK TO SAVEPOINT transfer_one; -- 저장한 SAVE POINT로 이동
  ROLLBACK; -- 처음으로 이동

COMMIT;
```



## #15.4 Read Uncommited \~

transaction이 선택할 수 있는 4가지 Isolation Level

Isolation Level - transaction 외부 데이터에 대한 가시성 수준 제어



| Isolation Level  |
| ---------------- |
| Read uncommitted |
| Read committed   |
| Repeatable read  |
| Serializable     |

Isolation Level은 transaction phenomena라 알려진 transaction에서 발생하는 현상을 막아주기도 함. 데이터베이스 전반에 일관성 있는 현상들로 4가지 phenomena는 Isolation 내부에서 일어날 수 있음, transaction 종류에 따라 4가지 현상이 발생할 수 있도록 선택 가능함



Dirty Read - transaction이 commit 되지 않은 transaction이 작성한 데이터를 동시에 읽을 때 발생

Nonrepeatable Read - transaction이 데이터를 다시 읽으려고 할 때 발생, 이미 한번 읽은 데이터가 다른 transaction에서 수정되고 commit 되었을 때

Phantom Read - row(행) 집합을 반환하는 쿼리를 transaction 안에서 재실행할 때 최근 commit 된 다른 transaction에 의해 그 결과가 이전과 다르게 변경되는 것

Serialization Anomaly - 여러 transaction의 성공적인 커밋 결과가 가능한 모든 순서로 transaction을 순차적으로 실행한 결과와 일치하지 않을 때 발생



\---

Locks - 다른 transaction에서 동시에 같은 row 값을 변경하려 할 때 데이터베이스에서 자동으로 lock 걸어 순서대로 작동하도록 함

&#x20;

row를 lock 해 아무도 해당 row를 수정하거나 삭제할 수 없도록

SELECT — FOR UPDATE; 모르는 사이에 값이 변경되지 않음 exclusive lock 아무도 데이터를 수정, 삭제할 수 없음

SELECT — FOR SHARE; 모르는 사이에 값이 변경되지 않음 다른 FOR SHARE 쿼리에서 같은 lock을 공유할 수 있게 함

