# #19 MongoDB

## #19.0 Introduction

NOSQL\_Not Only SQL, 데이터 저장 방식이 다름, MongoDB, Redis, Neo...

MongoDB - JSON 문서 활용, 비관계형 데이터베이스



## #19.1 Installation

## #19.2 Creating Documents

Easy, 데이터 형식이나 제약조건, 구조 등 SQL의 제약이 없음 > 실수에 민감해짐

cls - clear



## #19.3 Reading Documents

```mongodb
db.movies.find({director:"Christopher Nolan"})

db.movies.find({rating: { $gte: 8 }})
// $gte 같거나 큰

db.movies.find({ year: { $gt: 2000, $lt: 2010}})
// and

db.movies.find({ $or: [{ rating: {$gt:9}}, {year: {$gte: 2020}})
// or

db.movies.find({ genres: { $in: ["Drama", "Crime"]}})
// 배열 요소

db.movies.find({ genres: { $all: ["Drama", "Crime"]}})
// 배열

db.movies.find({ title: { $regex: /the/i }})
// 정규표현식

db.movies.find({ genres: { $size: 3}})
// 크기

db.movies.find({ director: { $exists: false }})
// 특정 key 값 존재

db.movies.find({ "cast.0": "Keanu Reeves"})
// 배열 내 요소 값

db.movies.find({ "director.alive": true })
// 중첩된 문서 내부 검색

db.movies.find().skip(10)
// skip

db.movies.find().limit(10).skip(10)
// limit

db.movies.find().sort({rating: 1}).limit(10).skip(10)
db.movies.find().sort({rating: -1}).limit(10).skip(10)
db.movies.find().sort({rating: -1, title: 1}).limit(10).skip(10)
// sort
```



