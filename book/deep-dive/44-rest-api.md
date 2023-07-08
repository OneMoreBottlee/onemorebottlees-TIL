# 44장 REST API

REST는 HTTP를 기반으로 클라이언트가 서버의 리소스에 접근하는 방식을 규정한 아키텍처.

REST API는 REST를 기반으로 서비스 API를 구현한 것을 의미함.



REST는 [로이 필딩](#user-content-fn-1)[^1]의 2000년 논문에서 처음 소개되었다. 발표 당시의 웹이 HTTP를 제대로 사용하지 못하는 상황을 보고 HTTP 장점을 최대한 사용할 수 있는 아키텍처로서 REST를 소개했고 이는 HTTP 프로토콜을 의도에 맞게 디자인하도록 유도하였다. REST의 기본 원칙을 성실히 지킨 서비스 디자인을 RESTful이라 표현한다.



### 44.1 REST API의 구성

REST API는 자원, 행위, 표현의 3가지 요소로 구성된다. REST는 자체 표현 구조로 구성되어 REST API 만으로 HTTP 요청을 이해할 수 있다.

자원 Resource - URI

행위 Verb - HTTP 요청 메서드

표현 Representations - 페이로드



### 44.2 REST API 설계 원칙

REST에서 가장 중요한 기본적인 원칙은 2가지다.

1. URI는 리소스를 표현해야 한다.

URI는 리소스를 표현하는데 중점을 두어야 한다. 리소스를 식별할 수 있는 이름은 동사보다 명사를 사용한다. 따라서 이름에 get과 같은 행위의 표현이 들어가서는 안된다.

```
# bad
GET /getTodos/1
GET /todos/show/1

# good
GET /todos/1
```



2. 리소스에 대한 행위는 HTTP 요청 메서드로 표현한다.

HTTP 요청 메서드는 클라이언트가 서버에게 요청의 종류와 목적을 알리는 방법이다. 주로 5가지 요청 메서드를 사용하여 CRUD를 구현한다.

<table><thead><tr><th width="174" align="center">HTTP 요청 메서드</th><th align="center">종류</th><th width="219" align="center">목적</th><th align="center">페이로드</th></tr></thead><tbody><tr><td align="center">GET</td><td align="center">index/retrieve</td><td align="center">모든/특정 리소스 취득</td><td align="center">X</td></tr><tr><td align="center">POST</td><td align="center">create</td><td align="center">리소스 생성</td><td align="center">O</td></tr><tr><td align="center">PUT</td><td align="center">replace</td><td align="center">리소스의 전체 교체</td><td align="center">O</td></tr><tr><td align="center">PATCH</td><td align="center">modify</td><td align="center">리소스의 일부 수정</td><td align="center">O</td></tr><tr><td align="center">DELETE</td><td align="center">delete</td><td align="center">모든/특정리소스 삭제</td><td align="center">X</td></tr></tbody></table>



### 43.3 JSON Server를 이용한 REST API 실습

#### 43.3.1 JSON Server 설치

P.832

#### 43.3.2 db.json 파일 생성

P.832 - P.833

#### 43.3.3 JSON Server 실행

P.833 - P.834

#### 43.3.4 GET 요청

P.834 - P.836

#### 43.3.5 POST 요청

P.837 - P.838

#### 43.3.6 PUT 요청

P.838 - P.839

#### 43.3.7 PATCH 요청

P.839 - P.840

#### 43.3.8 DELETE 요청

P.840 - P.841



[^1]: HTTP 1.0과 1.1 스펙 작성에 참여하고, 아파치 HTTP 서버 프로젝트의 공동 설립자이\
    . [https://shoark7.github.io/programming/knowledge/what-is-rest](https://shoark7.github.io/programming/knowledge/what-is-rest)
