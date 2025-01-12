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



## #19.4 Updating Documents

```mongodb
db.movies.updateOne(
  {
	_id: ObjectId('66d7b10cc0848d2fccbe1b38')
  },{
	$set: {'director.name': 'Francing Ford Coppola'},
	$currentDate: {updated_at : true}
})

db.movies.findOneAndUpdate(
  {
	_id: ObjectId('66d7b10cc0848d2fccbe1b38')
},{
	$set: {'director.name': 'Francing Ford Coppola'},
	$currentDate: {updated_at : true}
}, {returnNewDocument: true, upsert: true}
)
// 위와 비슷하지만 추가적으로 다른 설정 가능

db.movies.updateMany(
{ director: "Christopher Nolan"},
{ $inc: {rating: 0.2}}
)
// 여러 데이터 값 업데이트

db.movies.updateMany(
{ title: "Inception"},
{ $push: {genres: "Mind Control"}}
)
// 추가

db.movies.updateMany(
{ title: "Inception"},
{ $push: {genres: "Mind Control"}}
)
// 삭제

db.movies.updateMany(
{ title: "The Dark Knight"},
{ $addToSet: {csst: "Micheal Cane!"}}
)
// 없는 데이터 추가

db.movies.updateMany({}, {$rename: {runtime: "duration"}})
// key 변경

db.movies.updateOne(
	{ title: "Inception" },
	{ $set: {
		boxOffice: {~~}
	}
)
// Embeded Documents 업데이트


db.movies.updateMany(
	{ $expr: { $lt: [{$size: "$genres"}, 3] } },
	{ $addToSet: { genres: { $each: ["Other", "Happy"]}}}
)


db.movies.deleteMany({})
// 삭제

```



## #19.5 Aggregating Documents

데이터 집계, group by와 비슷함

```mongodb
db.movies.aggregate(
    [
        {$count: "total_movies"}
    ]
)

db.movies.aggregate([
        {$group: { _id: null, avgRating: { $avg: "$rating"}}}
])

db.movies.aggregate([{$unwind: "$genres"}])
// 배열 해제

db.movies.aggregate([
    {$unwind: "$genres"},
    {$group: {
        _id: "$genres",
        count: {$sum:1}
    }},
    {$sort: {count: -1}}
])


db.movies.aggregate([
    {$group: {
        _id: null,
        oldestMovie: {$min: "$year"},
        newestMovie: {$max: "$year"}
    }}
])

db.movies.aggregate([
    {$group: {
        _id: "$year",
        avgDuration: {$avg: "$duration"},
    }},
    {$sort: {_id: -1}}
])

db.movies.aggregate([
    {$match: {director: {$exists: true}}},
    {$group: {
        _id: "$director",
        movieCount: {$sum: 1},
    }},
    {$sort: {movieCount: -1}}
])

db.movies.aggregate([
    {$unwind: '$cast'},
    {$group: {
        _id: "$cast",
        movieCount: {$sum: 1},
    }},
    {$sort: {movieCount: -1}}
])


db.movies.aggregate([
    {$project: {title:1, director:1, cast:1, _id:0}},
    {$limit: 10}
])
```



