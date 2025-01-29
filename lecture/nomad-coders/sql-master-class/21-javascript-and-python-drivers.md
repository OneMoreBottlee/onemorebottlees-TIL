# #21 Javascript and Python Drivers

## #21.0 Introduction

how to talk to db with python & JS



## #21.1 Inserting Data

```python
import sqlite3

conn = sqlite3.connect("users.db")
cur = conn.cursor()

def init_table():
    cur.execute("""
    CREATE TABLE users(
                user_id integer primary key autoincrement,
                username text not null,
                password text not null
                )
    """)
    
    cur.execute("""
    insert into users (username, userid)
                values ('nico', 123), ('lynn', 321);
    """)
    
init_table()


def print_all_users():
            res = cur.excute("select * from users;")
            data = res.fetchall()
            print(data)

print_all_users()

conn.commit() #transaction commit
conn.close()

```



## #21.2 SQL Injection

```python
def i_change_password(username, new_password):
    cur.execute(
        f"UPDATE users
        SET password = '{new_password}'
        WHERE username = 'username'"
    )
# hack 가능성 높음
# 문자열 보간 String Interpolation

def s_change_password(username, new_password):
    cur.execute(
        "UPDATE users
        SET password = ? WHERE username = ?",
        (new_password, username), # ?가 순서대로
    )
```



## #21.3 Placeholders

```python
# 여러 값을 한번에 Insert

data = [("a",123),("a",123),("a",123)...]

cur.executemany("INSERT INTO users (username, password) VALUES (?, ?)", data)


data = [{"name": 123, "password": 456}, {"name": 123, "password": 456}...]

cur.executemany(
    "INSERT INTO users (username, password) VALUES (:name, :password)", data)
```



## #21.4 Querying Data

```python
# DATA
conn = sqlite3.connect(":memory:") #인메모리

import sqlite3

conn = sqlite3.connect(":movies.db")
cur = conn.cursor()
res = cur.excute("select * from movies")

# 1st option_모든 result를 pythone에 저장
all_movies = res.fetchall() 

# 2nd option_특정 batch의 data 가져옴, cursor 고민할 필요 x
first_20 = res.fetchmany(20) 
next_20 = res.fetchmany(20)

# 3rd option_하나씩
res.fetchone()

conn.commit()
conn.close()

```



## #21.5 MySQL and PostgreSQL Drivers

```python
# python 에서 mySQL & PostgreSQL 연결
# 위와 비슷한 방법 connect > excute > 활용
# parameter 활용 & 관리가 조금씩 다름
```



## #21.6 Redis Caching

```python
import redis

r = redis.Redis(host="localhost", port=####, decode_responses=True)

r.set("hello", "world")
print(r.get("hello"))

r.hset("user:1", mapping= {
    "name":"nico",
    "age":19999
})

print(r.hgetall("users:1"))

```



```python
import redis
import sqlite3
import json

r = redis.Redis(host="localhost", port=####, decode_responses=True)

conn = sqlite3.connect("movies.db")
cur = conn.cursor()


# 0.23s
def make_expensive_query():
    res=cur.execute("select count(*), director from movies group by director;")
    return res.fetchall()


# 0.24s -> 0.02s
def make_expensive_query():
    redis_key = "director:movies"
    cached_results = r.get(redis_key)
    if cached_results:
        return json.loads(cached_results)
    else:
        print("cache miss")
        res=cur.execute("select count(*), director from movies group by director;")
        all_rows=res.fetchall()
        r.set(redis_key, json.dumps(all_rows), ex=20)
        return res.fetchall()

v = make_expensive_query()

conn.commit()
conn.close()
r.close()
```



## #21.7 PyMongo

```python
from pymongo import MongoClient

client = MongoClient("mongodb://localhost:####")

database = client.get_database("movies")
movies = database.get_collection("movies")

query = {"director":"Christopher Nolan"}

results = movies.find(query)

for movie in results:
    print(movie)

query2 = {"rating":{"$gte": 8}}    
results2 = movies.find(query2)

for movie in results2:
    print(movie)
    
    
# add
new_movie = {
    "title" : "New movie",
    "director": "Al pachino"
}

result = movies.insert_one(new_movie)

print(result)

client.close()
```



## #21.8 Drizzle Introspect

Drizzle - JS || TS 로 사용할 수 있는 ORM

```javascript
// drizzle.config.ts
import {defineConfig} from "drizzle-kit";

export default defineConfig({
    dialect: "sqlite",
    schema: "./drizzle/schema.ts",
    out: "./drizzle",
    dbCredentials: {
        url: ".movies.db",
    },
})
```



```typescript
import {Database} from "better-sqlite3"
import {drizzle} from "drizzle-orm/bun-sqlite"

const sqlite = new Database("movies.db")

const db = drizzle(sqlite);

const results = await db.select({
    id: movies.id
    title: movies.title,
    overview: movies.overview
    }).from(movies).limit(50)

console.log(results)

```



## #21.9 Drizzle Migrate

```javascript
// drizzle.config.ts
import {defineConfig} from "drizzle-kit";

export default defineConfig({
    dialect: "sqlite",
    schema: "./drizzle/schema.ts",
    out: "./drizzle",
    dbCredentials: {
        url: ".users.db",
    },
})
```



```typescript
// make New sqliteDB with Drizzle
import { sqliteTable, integer, text } from "drizzle-orm/sqlite-core";

export const users = sqliteTable("users", {
    userId: integer("user_id", {mode: "number"}).primaryKey({
        autoIncrements: True
    }),
    username: text("username").notNull()
})

export const comments = sqliteTable("comments", {
    commentId: integer("comment_id", {mode: "number"}).primaryKey({
        autoIncrements: True
    }),
    payload: text("payload").notNull(),
    userId: integer("user_id").notNull().references(() => users.userId, {
        onDelete: "cascade",
    })
})
```

```typescript
// index.ts

import { Database } from "bun:sqlite";
import { drizzle } from "drizzle-orm/bun=sqlite";
import { comments, users } from "./schema";
import { eq } from "drizzle-orm";

const sqlite = new Database("users.db");
const db = drizzle(sqlite, {logger: true});

// make user
const result = await db.insert(users).values({username: "nico"}.returning();

// make comment
const result = await db
    .insert(comments)
    .values({payload: "hello drizzle", userId: 1})
    .returning();

//
const result = await db
    .select({
        user: users.username,
        comment: comments.payload,
    })
    .from(comments)
    .leftJoin(users, eq(comments.userId, users.userId));
    
console.log(result);

```

[https://orm.drizzle.team/benchmarks](https://orm.drizzle.team/benchmarks)



## #21.10 Redis

```typescript
import { createClient } from "redis";

const client = createClient();

await client.connect();

await client.set("hello", "world")

const r = await client.get("hello")
console.log(r)

await client.hSet("users:1", {username:"nico", "password:"123"})
const r2 = await client.hGetAll("users:1")
console.log(r2)

await client.disconnect();
```



## #21.11 Mongoose

```typescript


import * as mongoose from "mongoose";

await mongoose.connect("mongodb://localhost:###/movies")

const moviesSchema = new mongoose.Schema({
    title: {type: String, required:true},
    director: {type:String, required:true},
    rating: {
        type: Number,
        required: true,
        min: [1, "No movie deserves less than 1"],
        max: [10, "No movie is better than 10"],
    },
});

const Movie = mongoose.model("Movie", moviesSchema);
// Model을 통해 mongoDB와 통신함

const movie = await Movie.create({
    title: "The Mongoose",
    rating: 5
})

console.log(movie);

const movies = await Movie.find({ director: "Christopher Nolan" });
const movies = await Movie.find({ rating: { $gte: 8.2 }});

console.log(movies);

await mongoose.disconnect()
```



