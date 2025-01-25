# #20 REDIS

## #20.0 Introduction

속도가 빠른 데이터베이스로 대부분의 경우에 다른 SQL 데이터베이스와 같이 사용

키-값 데이터베이스, In-Memory 데이터베이스로 굉장히 빠른 속도로 작동함

but 메모리를 활용하기에 비용이 많이 듦 ⇒ 다른 데이터베이스 보완하는 방식으로 활용

```
Redis는 캐싱 용도로 사용
```



## #20.2 Strings

```
-- redis

SET hello world -- key > hello / value > world
GET hello -- world

SET hello bye
GET hello -- bye / 값 수정 가능

SET users:1 SON
GET users:1 -- SON / 유일키
DEL users:1 
FLUSHALL -- 전부 지우기

SET users:1 nico NX EX 10 -- 종료일자 지정 가능
GET users:1 -- 10초만 저장함

SET users:1 nico XX -- 값이 존재할 때, users:1의 값으로 nico 넣을 수 있다는 의미

MSET users:1 SON users:2 CHA users:3 PARK
MGET users:1 users:2 users:3 -- 1) SON 2) CHA 3) PARK

SET visitors 0
INCR visiters // 숫자 up
DECR visitors // 숫자 down
INCRBY visitors 10 // 10씩 up
DECRBY visitors 10 // 10씩 down

```



## #20.3 Lists

```
4,294,967,295 까지 저장 가능

LPUSH l 1 -- LPUSH 리스트 왼쪽에 값을 추가
LRANGE l 0 -1 -- LRANGE 리스트 왼쪽부터

RPUSH l 0

LPOP l -- 리스트 왼쪽 값 1개 제거
RPOP l

...
```



## #20.4 Sets

```
중복된 값 허용 x

SADD votes:song:1 user:1
SADD votes:song:1 user:2
SADD votes:song:1 user:3

SADD votes:song:1 user:2 -- 실패, 동일한 값

SMEMBERS votes:song:1 -- 1) user:1 2) user:2 3) user:3

SISMEMBER votes:song:1 user:1 -- 1
SISMEMBER votes:song:1 user:4 -- 0

SCARD votes:song:1 -- 2

SADD votes:song:2 user:3 user:4 user:5
SMEMBERS votes:song:2 -- 1) user:3 2) user:4 3) user:5

SINTER votes:song:1 votes:song:2 -- user:3 1과 2에 투표한 사람
SDIFF votes:song:1 votes:song:2 -- user:1 user:2

SUNION votes:song:1 votes:song:2 -- user:1 user:2... user:5

SREM votes:song:1 user:1 -- delete

...
```



## #20.5 Hashes

```
key와 value의 쌍 

HSET player:1 name son xp 0 health 100
HGET player:1 name -- son
HGETALL player:1 -- name son xp 0 health 100

HINCRBY player:1 xp 10
HGET player:1 xp -- xp 10

HINCRBY player:1 health -10
HGET player:1 health -- health 90

HMGET player:1 xp health -- 10 90

HSET player:1 name park
HGET player:1 name -- name park

HEXISTS player:1 xp -- 1
```



## #20.6 Sorted Sets

```
ZADD scores 1 user:1 2 user:2 10 user:3 6: user:4
ZRANGE scores 0 -1 -- user:1 user:2 user:4 user:3
ZRANGE scores 0 -1 WITHSCORES -- user:1 1 user:2 2 user:4 6 user:3 10
ZREVRANGE scores 0 -1 -- user:3 user:4 user:2 user:1

ZRANK scores user:1 -- 0
ZRANK scores user:3 -- 3

ZADD scores 20 user:1
ZREVRANGE scores 0 -1 WITHSCORES -- user:1 20 user:3 10 user:4 6 user:2 2

ZINCRBY scores 1 user:2
ZSCORE scores user:1 -- 20

ZRANGEBYSCORE scores 7 50 -- user:2 user:3 user:1

ZCOUNT scores 10 inf -- 2

ZREM scores user:2 -- del
```

