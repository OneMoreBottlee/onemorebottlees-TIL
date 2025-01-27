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







