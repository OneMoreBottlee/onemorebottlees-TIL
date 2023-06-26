# 37장 Set과 Map

### 37.1 Set

Set 객체는 중복되지 않는 유일한 값들의 집합이다. 배열과 유사하지만 아래 같은 차이가 있다.

| 구분           | 배열 | Set 객체 |
| ------------ | -- | ------ |
| 중복 포함 여부     | O  | X      |
| 요소 순서 의미     | O  | X      |
| 인덱스 접근 가능 여부 | O  | X      |

Set은 수학적 집합을 구현하기 위한 자료구조로, Set 객체의 특징은 수학적 집합의 특성과 일치한다.&#x20;



#### 37.1.1 Set 객체의 생성

Set 생성자 함수로 생성한다. 인수를 전달하지 않으면 빈 Set 객체가 생성된다.

```javascript
// 인수 x
const set = new Set()
console.log(set) // Set(0){}

// 인수 o
const set = new Set([1, 2, 3, 3])
console.log(set) // Set(3){1, 2, 3} > 중복 요소인 3은제거됨

const set2 = new Set("hello")
console.log(set2) // Set(4){"h", "e", "l", "o"} > 중복 요소인 l은 제거됨

// 중복을 허용하지 않는 특성을 활용해 배열에서 중복된 요소를 제거할 수 있다.
// 배열의 중복 요소 제거
const uniq = array => array.filter((v, i, self) => self.indexOf(v) === i)
console.log(uniq([2, 1, 2, 4, 3, 4])) // [2, 1, 3, 4]

// Set의 중복 요소 제거
const uniq2 = array => [...new Set(array)]
console.log(uniq([2, 1, 2, 4, 3, 4])) // [2, 1, 3, 4]
```



#### 37.1.2 요소 개수 확인

Set 객체의 요소 개수를 확인할 때는 Set.prototype.size 프로퍼티를 사용한다.

```javascript
const { size } = new Set([1, 2, 3, 3])
console.log(size) // 3

// size 프로퍼티는 getter 함수만 존재하므로 숫자를 할당하여 요소 개수를 변경할 수 없다.
const set = new Set([1, 2, 3])
set.size = 10 // 무시됨
console.log(set.size) // 3
```



#### 37.1.3 요소 추가

Set 객체에 요소를 추가할 때는 Set.prototype.add 메서드를 사용한다.

```javascript
const set = new Set()
console.log(set) // Set(0) {}

set.add(1)
console.log(set) // Set(1) {1}

// add 메서드는 새로운 요소가 추가된 Set 객체를 반환하므로 연속적으로 호출할 수 있다.
// 중복된 요소의 추가는 허용되지 않고, 무시된다.
set.add(1).add(2) // Set(2) {1, 2} > 중복된 요소 1의 추가는 무시됨

// 자바스크립트의 모든 값을 요소로 저장할 수 있다.
const sett = new Set()
sett
    .add(1)
    .add('a')
    .add(true)
    .add(undefined)
    .add(null)
    .add({})
    .add([])
    .add(() => {})

console.log(sett)
// Set(8) {1, 'a', true, undefined, null, {}, [], () => {}}
```



#### 34.1.4 요소 존재 여부 확인

Set 객체에 특정 요소가 존재하는지 확인하려면 Set.prototype.has 메서드를 사용한다. has 메서드는 특정 요소의 존재 여부를 나타내는 불리언 값을 반환한다.

```javascript
const set = new Set([1, 2, 3])
console.log(set.has(1)) // ture
console.log(set.has(4)) // false
```



#### 34.1.5 요소 삭제

Set 객체의 특정 요소를 삭제하려면 Set.prototype.delete 메서드를 사용한다. Set은 순서에 의미가 없기에 인수로 인덱스가 아닌 삭제하려는 요소값을 전달해야 한다.

```javascript
const set = new Set([1, 2, 3])
set.delete(2)
console.log(set) // Set(2) {1, 3}

set.delete(1)
console.log(set) // Set(1) {3}

// 존재하지 않는 값을 삭제하려하면 무시한다.
set.delete(4)
console.log(set) // Set(1) {3}

// 불리언 값을 반환하므로 연속적으로 호출할 수 없다.
set.delete(4).delete(1).delete(2) // TypeError : ~~~
```



#### 37.1.6 요소 일괄 삭제

Set 객체의 모든 요소를 일괄 삭제하려면 Set.prototype.clear 메소드를 사용한다. 언제나 undefined를 반환한다.

```javascript
const set = new Set([1, 2, 3])
set.clear()
console.log(set) // Set(0) {}
```



#### 37.1.7 요소 순회

Set 객체의 요소를 순회하려면 Set.prototype.forEach 메서드를 사용한다.&#x20;

```javascript
const set = new Set([1, 2, 3])
set.forEach((v, v2, set) => console.log(v, v2, set))

/*
    첫 번째 인수와 두 번째 인수는 동일한 값이다.
    Array.prototype.forEach 메서드와 인터페이스를 통일하기 위해...
    1 1 Set(3) {1, 2, 3}
    2 2 Set(3) {1, 2, 3}
    3 3 Set(3) {1, 2, 3}
*/
```

Set 객체는 이터러블이다. 따라서 for ... of 문으로 순회할 수 있으며, 스프레드 문법과 배열 디스트럭처링의 대상이 될 수 있다.

또 순서에 의미를 갖지 않지만 Set 객체를 순회하는 순서는 요소가 추가된 순서를 따른다. 다른 이터러블의 순회와 호환성을 유지하기 위함이다.



#### 27.1.8 집합 연산

Set 객체는 수학적 집합을 구현하기 위한 자료구조다. 따라서 Set 객체를 통해 교집합, 합집합, 차집합 등을 구현할 수 있다.

ex) p.649 - 652



### 37.2 Map

Map 객체는 키와 값의 쌍으로 이루어진 컬렉션이다. 객체와 유사하지만 다음과 같은 차이가 있다.

| 구분            | 객체                      | Map 객체       |
| ------------- | ----------------------- | ------------ |
| 키로 사용할 수 있는 값 | 문자열 또는 심벌 값             | 객체를 포함한 모든 값 |
| 이터러블          | X                       | O            |
| 요소 개수 확인      | Object.prototype.length | map.size     |



#### 37.2.1 Map 객체의 생성

Map 생성자 함수로 생성한다. 인수를 전달하지 않으면 빈 Map 객체가 생성된다.

```javascript
// 인수 x
const map = new Map()
console.log(map) // Map(0) {}

// 인수 o
const map2 = new Map(['key1', 'value1'], ['key2', 'value2'])
console.log(map) // Map(2){'key1' => 'valye1', 'key2' => 'value2'}

const map21 = new Map([1, 2]) // TypeError: ~~~

// Map 생성자 함수의 인수로 전달한 이터러블에 중복된 키를 갖는 요소가 존재하면 값이 덮어써진다. 
// 따라서 중복된 키를 갖는 요소가 존재할 수 없다.
const map3 = new Map(['key1', 'value1'], ['key1', 'value2'])
console.log(map3) // Map(1){'key1' => 'valye2'}

```



#### 37.2.2 요소 개수 확인

Map 객체의 요소 개수를 확인할 때는 Map.prototype.size 프로퍼티를 사용한다.

```javascript
const { size } = new Map([['key1', 'value1'], ['key2', 'value2']])
console.log(size) // 2

// size 프로퍼티는 getter 함수만 존재하므로 숫자를 할당하여 요소 개수를 변경할 수 없다.
const map = new Map(['key1', 'value1'], ['key2', 'value2'])
map.size = 10 // 무시됨
console.log(set.size) // 2
```



#### 37.2.3 요소 추가

Map 객체에 요소를 추가할 때는 Map.prototype.set 메서드를 사용한다.

```javascript
const map = new Map()
console.log(map) // Map(0) {}

map.set('key1', 'value1')
console.log(map) // Map(1) {"key1" => "value1"}

// set 메서드는 새로운 요소가 추가된 Map 객체를 반환한다. 따라서 연속적으로 호출할 수 있다.
map.set('key1', 'value1').set('key2', 'value2')

console.log(map) // Map(2) {"key1" => "value1", "key2" => "value2"}

// 중복된 키를 갖는 요소를 추가하면 값이 덮어써지므로 중복된 키를 갖는 요소가 존재할 수 없다.
map.set('key2', 'value100')
console.log(map) // Map(2) {"key1" => "value1", "key2" => "value100"}

// 키 타입에 제한이 없어 객체를 포함한 모든 값을 키로 사용할 수 있다.
// => Map 객체와 일반 객체의 가장 큰 차이점
const mapp = new Map()

const lee = { name: 'Lee' }
const kim = { name: 'Kim' }

map
    .set(lee, 'be')
    .set(kim, 'fe')

console.log(map) // Map(2) {{name: 'Lee'} => 'be', {name: 'Kim'} => 'fe'}
```



#### 37.2.4 요소 취득

Map 객체에서 특정 요소를 취득하려면 Map.prototype.get 메서드를 사용한다. get 메서드의 인수로 키를 전달하면 Map 객체에서 인수로 전달한 키를 갖는 값을 반환한다.

```javascript
const map = new Map()

const lee = { name: 'Lee' }
const kim = { name: 'Kim' }

map
    .set(lee, 'be')
    .set(kim, 'fe')
    
console.log(map.get(lee)) // be
console.log(map.get(kim)) // fe
```



#### 37.2.5 요소 존재 여부 확인

Map 객체에 특정 요소가 존재하는지 확인하려면 Map.prototype.has 메서드를 사용한다.

```javascript
const lee = { name: 'Lee' }
const kim = { name: 'Kim' }
const map = new Map([[lee, 'be'], [kim, 'fe']])

console.log(map.has(lee)) // true
console.log(map.has('key')) // false
```



#### 37.2.6 요소 삭제

Map 객체의 요소를 삭제하려면 Map.prototype.delete 메서드를 사용한다.

```javascript
const lee = { name: 'Lee' }
const kim = { name: 'Kim' }

const map = new Map([[lee, 'be'], [kim, 'fe']])

map.delete(kim)
console.log(map) // Map(1) {{name: 'Lee'} => 'be'}

// 존재하지 않는 키로 요소를 삭제하려하면 무시된다.
map.delete('park')
console.log(map) // Map(1) {{name: 'Lee'} => 'be'}

// delete 메서드는 성공 여부를 나타내는 불리언 값을 반환하므로 연속적으로 호출 할 수 없다.
map.delete(kim).delete(lee) // TypeError : ~~~
```



#### 37.2.7 요소 일괄 삭제

Map 객체의 요소를 일괄 삭제하려면 Map.prototype.clear 메서드를 사용한다.

```javascript
const lee = { name: 'Lee' }
const kim = { name: 'Kim' }

const map = new Map([[lee, 'be'], [kim, 'fe']])

map.clear()
console.log(map) // Map(0) {}
```



#### 37.2.8 요소 순회

Map 객체의 요소를 순회하려면 Map.prototype.forEach 메서드를 사용한다.

```javascript
const lee = { name: 'Lee' }
const kim = { name: 'Kim' }

const map = new Map([[lee, 'be'], [kim, 'fe']])
map.forEach((v, k, map) => console.log(v, k, map))

/*
be { name: 'Lee' } Map(2){
    { name: 'Lee' } => 'be',
    { name: 'Kim' } => 'fe',
}
fe { name: 'Kim' } Map(2){
    { name: 'Lee' } => 'be',
    { name: 'Kim' } => 'fe',
}
*/
```

Map 객체는 이터러블이다. 따라서 for ... of 문으로 순회할 수 있으며, 스프레드 문법과 배열 디스트럭처링의 대상이 될 수 있다.

Map 객체는 이터러블이면서 동시에 이터레이터인 객체를 반환하는 메서드를 제공한다.

<table><thead><tr><th width="240">Map 메서드</th><th>설명</th></tr></thead><tbody><tr><td>Map.prototype.keys</td><td>Map 객체에서 <br>요소키를 값으로 갖는 이터러블이변서 이터레이터인 객체를 반환한다.</td></tr><tr><td>Map.prototype.values</td><td>Map 객체에서 <br>요소값을 값으로 갖는 이터러블이변서 이터레이터인 객체를 반환한다.</td></tr><tr><td>Map.prototype.entries</td><td>Map 객체에서 <br>요소키와 요소값을 값으로 갖는 이터러블이변서 이터레이터인 객체를 반환한다.</td></tr></tbody></table>

또 순서에 의미를 갖지 않지만 Set 객체를 순회하는 순서는 요소가 추가된 순서를 따른다. 다른 이터러블의 순회와 호환성을 유지하기 위함이다.
