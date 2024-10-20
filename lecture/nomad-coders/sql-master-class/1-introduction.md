---
description: '241020'
---

# #1 Introduction

## #1.3 Why You Should Learn SQL

Data is everywhere

Data is the new oil



많은 기업이 데이터를 활용해 이윤을 창출해낸다. 데이터는 그 자체로도 가치 있지만, 데이터에서 인사이트를 뽑아내고 결과를 도출할 수 있다면 그 가치를 극대화 할 수 있다.



모든 데이터는 데이터베이스 내에 존재한다.  대부분의 데이터베이스는 SQL을 사용한다. 따라서 SQL 을 활용할 수 있다면 데이터를 다룰 수 있다.



SQL은 4번째로 인기있는 언어이지만 개발자만을 위한 언어는 아님.

또한 오랫동안 사용되면서 대체되지 않은 언어이므로 앞으로도 유용할 것이다.

개발자라면 ORM 을 활용할 수 있지만 그 기본인 SQL이 어떻게 작동하는지 알아두는 것은 좋음



## #1.4 What is SQL

* 1970년대 초에 개발됨
* SQUARE => SEQUEL (Structured English Query Language)  => SQL (Structured Query Language)
* [RDBMS ](#user-content-fn-1)[^1]안의 데이터 관리를 위해 사용함
* 선언형 언어 (declarative)
* 크게 이식성이 없음 (portable[^2])
* 영어와 비슷한 문법
* 표준 (Standardized)
* 하나의 언어가 아님 / DDL[^3], DML[^4], TCL[^5], DCL[^6], DQL[^7] 등 다양한 언어가 있음



## #1.5  Course Roadmap

* SQL
  * SQLite
  * MySQL
  * postgreSQL
* noSQL
  * MongoDB
  * Redis
* Connecting



[^1]: RDBMS?

    * Relational Database Management System
    * 관계형 데이터베이스와 대화할 수 있게 해주는 프로그램



    RD?

    * Relational Database
    * sqlite, MySQL, postgreSQL, MariaDB, Oracle SQL
    * 데이터를 정리하는 데이터베이스
    * 행과 열이 있는 테이블에 데이터를 저장하는 데이터베이스

[^2]: 모든 RDBMS를 통틀어 거의 유사함

    문법 자체는 데이터베이스에 따라 다르지 않음

    BUT 데이터베이스에 따라 규칙이 다름



    데이터베이스에 따라 다른 기능을 제공함

[^3]: Data Definition Language



    데이터베이스 구조를 생성, 수정 및 삭제하는데 사용되는 SQL 명령 집합

[^4]: Data Manipulation Language



    DB에 데이터 추가, 삭제, 수정에 사용

[^5]: Transaction Control Language



    DB 내 트랜잭션을 관리하는데 사용

[^6]: Data Control Language



    DB에 대한 액세스 제어, DB 개체 관리에 사용

[^7]: Data Query Language



    DB에서 조작 데이터를 검색하는데 사용
