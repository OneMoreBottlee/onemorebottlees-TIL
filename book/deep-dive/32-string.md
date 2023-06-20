# 32장 String

표준 빌트인 객체인 String은 원시 타입인 문자열을 다룰 때 유용한 프로퍼티와 메서드를 제공한다.

### 32.1 String 생성자 함수

표준 빌트인 객체인 String 객체는 생성자 함수 객체다. 따라서 new 연산자와 함께 호출하여 String 인스턴스를 생성할 수 있다.

인수를 전달하지 않고 new 연산자와 함께 호출하면 빈 문자열을 할당한 String 래퍼 객체를 생성한다.\
인수를 전달하고 new 연산자와 함께 호출하면 인수로 전달받은 문자열을 할당한 String 래퍼 객체를 생성한다.

```javascript
const strObj = new String();
console.log(strObj); // String {length: 0, [[PrimitiveValue]]: ""}

const strObj2 = new String('Han');
console.log(strObj2); // {0: "H", 1: "a", 2: "n", length: 3, [[PrimitiveValue]]: "Han"}
```



String 래퍼 객체는 배열과 마찬가지로 length 프로퍼티와 인덱스를 나타내는 숫자 형식의 문자열을 프로퍼티 키로, 각 문자를 프로퍼티 값으로 갖는 유사 배열 객체이면서 이터러블이다. 따라서 배열과 유사하게 인덱스를 사용하여 각 문자에 접근할 수 있다.

단, 문자열은 원시 값으로 변경할 수 없다.

String 생성자 함수의 인수로 문자열이 아닌 값을 전달하면 인수를 문자열로 강제 변환한 후, \[\[StringData]] 내부변슬롯에환된 문자열을 할당한 String 래퍼 객체를 생성한다.

```javascript
let strObj = new String(123);
console.log(strObj)
//String {0: "1", 1: "2", 2: "3", length: 3, [[PrimitiveValue]]: "123" }

strObj = new String(null);
console.log(strObj);
//String {0: "n", 1: "u", 2: "l", 3: "l", length: 4, [[PrimitiveValue]]: "null"}
```



new 연산자를 사용하지 않고 String 생성자 함수를 호출하면 문자열을 반환한다. 이를 이용하여 명시적으로 타입을 변환하기도 한다.

```javascript
String(1) // "1"
String(NaN) // "NaN"
String(Infinity) // "Infinity"

String(true) // "true"
String(false) // "false"
```



### 32.1 length 프로퍼티

length 프로퍼티는 문자열의 문자 개수를 반환한다.

```javascript
'Hello'.length // 5
'hi'.length // 2
```

String 래퍼 객체는 배열과 마찬가지로 length 프로퍼티를 갖는다. 그리고 인덱스를 나타내는 숫자를 프로퍼티 키로, 각 문자를 프로퍼티 값으로 가지므로 String 래퍼 객체는 유사 배열 객체다.



### 32.3 String 메서드

String 객체에는 원본 String 래퍼 객체를 직접 변경하는 메서드가 존재하지 않는다. 그래서 String 객체의 메서드는 언제나 새로운 문자열을 반환한다. 문자열은 변경 불가능한 값(immutable)한 원시 값이기 때문에 String 래퍼 객체도 읽기 전용 객체(read only)로 제공된다.



만약 String 래퍼 객체가 읽기 전용 객체가 아니면, 변경된 String 래퍼 객체를 문자열로 되돌릴 때 문자열이 변경된다. 따라서 String 객체의 모든 메서드는 String 래퍼 객체를 직접 변경할 수 없고, String 래퍼 객체의 메서드는 언제나 새로운 문자열을 생성하여 반환한다.



#### 32.3.1 String.prototype.indexOf

indexOf 메서드는 대상 문자열에서 인수로 전달받은 문자열을 검색해 첫 번째 인덱스를 반환한다. 검색에 실패하면 -1을 반환한다.

```javascript
const str = "hello world"

str.indexOf("l") // 2
str.indexOf("or") // 7
str.indexOf("x") // -1 => 검색 실패시 -1 을 반환한다.

str.indexOf("l", 3) // 3 => 2번째 인수로 검색을 시작할 인덱스를 전달할 수 있다.
```

indexOf 메서드는 대상 문자열에 특정 문자열이 존재하는지 확인할 때 유용하다.

ES6에서 도입된 String.prototype.includes 메서드를 사용하면 가독성이 더 좋다.



#### 32.3.2 String.prototype.search

대상 문자열에서 인수로 전달받은 정규 표현식과 매치하는 문자열을 검색해 일치하는 문자열의 인덱스를 반환한다. 검색에 실패하면 -1을 반환한다.

```javascript
const str = 'hello world'

str.search(/o/) // 4
str.search(/x/) // -1
```



#### 32.3.3 String.prototype.includes

ES6에서 도입된 includes 메서드는 대상 문자열에 인수로 전달받은 문자열이 포함되어 있는지 확인하여 그 결과를 true 또는 false 로 반환한다. 2번째 인수로 검색 시작 인덱스를 전달할 수 있다.

```javascript
const str = 'hello world'

str.includes('hello') // true
str.includes('wor') // true
str.includes('x') // false
str.includes() // false

// 2번째 인수로 검색 시작 인덱스를 전달할 수 있다.
str.includes('l', 3) // true
str.includes('h', 3) // false
```



#### 32.3.4 String.prototype.startsWith

ES6에서 도입된 startsWith 메서드는 대상 문자열이 인수로 전달받은 문자열로 시작하는지 확인하여 true 또는 false를 반환한다. 2번째 인수로 검색 시작 인덱스를 전달할 수 있다.

```javascript
const str = 'hello world'

str.startsWith("he") // true
str.startsWith('x') // false

str.startsWith(' ', 5) // true
```



#### 32.3.5 String.prototype.endsWith

ES6에서 도입된 endsWith 메서드는 대상 문자열이 인수로 전달받은 문자열로 끝나는지 확인하여 true 또는 false를 반환한다. 2번째 인수로 검색할 문자열의 길이를 전달할 수 있다.

```javascript
const str = 'hello world'

str.endsWith("d") // true
str.endsWith("ld") // true
str.endsWith('lo', 5) // true
```



#### 32.3.6 String.prototype.charAt

대상 문자열에서 인수로 전달받은 인덱스에 위치한 문자를 검색하여 반환한다. 인덱스는 문자열의 범위 (0 \~ 문자열 길이-1) 사이의 정수여야 한다. 인덱스가 문자열의 범위를 벗어난 정수인 경우 빈 문자열을 반환한다.

```javascript
const str = 'hello world'

for(let i = 0; i < str.length; i++){
    console.log(str.charAt(i)) // h e l l o   w o r l d
}
```



#### 32.3.7 String.prototype.substring

대상 문자열에서 첫 번째 인수로 전달받은 인덱스에 위치한 문자부터 두 번째 인수로 전달받은 인덱스의 위치한 문자의 바로 이전 문자까지 부분 문자열을 반환한다. 두 번째 인수는 생략 가능하며, 이때 첫 번째 인수부터 마지막 문자까지 부분 문자열을 반환한다.

```javascript
const str = 'hello world'

str.substring(1,4) // "ell"
str.substring(1) // "ello World"

// 첫 번째 인수는 두 번째 인수보다 작은 정수여야 하지만 다음과 같이 인수를 전달해도 정상동작한다.
str.substring(4, 1) // "ell" => 첫 번째 인수 > 두 번째 인수인 경우 두 인수는 교환된다.
str.substring(-2) // "hello world" => 인수 < 0 or NaN 인 경우 0으로 취급된다.
str.substring(1, 100) // "ello World" => 인수 > str.length 인 경우 문자열의 길이로 취급된다.
str.substring(20) // ""
```



indexOf 메서드와 함께 사용하면 특정 문자열을 기준으로 앞뒤에 위치한 부분 문자열을 취할 수 있다.

```javascript
const str = 'hello world'
str.substring(0, str.indexOf(" ")) // "hello"
str.substring(str.indexOf(" ") + 1, str.length) // "world"
```



#### 32.3.8 String.prototype.slice

substring 메서드와 동일하게 작동한다. 단, slice 메서드에는 음수인 인수를 전달해 뒤에서부터 문자열을 자를 수 있다.

```javascript
const str = 'hello world'
str.slice(0, 5) // hello
str.slice(-5) //world
```



#### 32.3.9 String.prototype.toUpperCase

대상 문자열을 모두 대문자로 변경한 문자열을 반환한다.

```javascript
const str = 'HEllo world'
str.toUpperCase() // HELLO WORLD
```



#### 32.3.10 String.prototype.toLowerCase

대상 문자열을 모두 소문자로 변경한 문자열을 반환한다.

```javascript
const str = 'HEllo world'
str.toLowerCase() // hello world
```



#### 32.3.11 String.prototype.trim

대상 문자열 앞 뒤 공백 문자를 제거한 문자열을 반환한다.

```javascript
const str = "    ahah   "
str.trim() // 'ahah'
```

trimStart, trimEnd 를 사용해 앞 뒤 공백을 제거할 수 있다.



#### 32.3.12 String.prototype.repeat

ES6에서 도입된 repeat 메서드는 대상 문자열을 인수로 전달받은 정수만큼 반복해 연결한 새로운 문자열을 반환한다. 인수로 전달받은 정수가 0 이면 빈 문자열을 반환하고, 음수이면 RangeError를 발생시킨다. 인수를 생략하면 기본값은 0이다.

```javascript
const str = 'abc'

str.repeat() // ''
str.repeat(0) // ''
str.repeat(1) // 'abc'
str.repeat(5) // 'abcabcabcabcabc'
str.repeat(5.5) // 'abcabcabcabcabc' 2.5 => 2
str.repeat(-1) // RangeError: Invalid count value
```



#### 32.3.13 String.prototype.replace

대상 문자열에서 첫 번째 인수로 전달받은 문자열 또는 정규표현식을 검색해 두 번째 인수로 전달한 문자열로 치환한 문자열을 반환한다.

```javascript
const str = 'hello world'
str.replace('world', 'Lee') // 'hello Lee'

// 검색된 문자열이 여러개인 경우, 첫 번째 검색된 문자열만 치환한다.
const str2 = 'hello world world'
str2.replace('world', 'Lee') // 'hello Lee world'

// 특수한 교체 패턴
const str3 = 'hello world'
str3.replace('world', '<strong>$&</strong>') // $& = 검색된 문자열

// 정규 표현식 전달 가능
const str4 = 'hello hello'
str4.replace(/hello/gi, 'Lee') // 'Lee Lee'
```

두 번째 인수로 치환 함수를 전달할 수도 있다. 첫 번째 인수로 전달한 문자열 또는 정규 표현식에 매치한 결과를 두 번째 인수인 치환 함수의 인수로 전달하면서 호출하고, 치환 함수가 반환한 결과에 매치 결과를 치환한다.



#### 32.3.14 String.prototype.split

대상 문자열에서 첫 번째 인수로 전달한 문자열 또는 정규 표현식을 검색해 문자열을 구분한 후 분리된 각 문자열로 이루어진 배열을 반환한다. 인수로 빈 문자열을 전달하면 각 문자를 모두 분리하고, 인수를 생략하면 대상 문자열 전체를 단일 요소로 하는 배열을 반환한다.

```javascript
const str = 'How are you doing?'

str.split(' ') // [ 'How', 'are', 'you', 'doing?' ]
str.split(/\s/) // [ 'How', 'are', 'you', 'doing?' ]
str.split('') // [ 'H', 'o', 'w', ' ', 'a', 'r', 'e', ' ', 'y', 'o', 'u', ' ', 'd', 'o', 'i', 'n', 'g', '?' ]
str.split() // [ 'How are you doing?' ]

// 두 번째 인수로 배열의 길이를 지정할 수 있다.
str.split(' ', 3) // [ 'How', 'are', 'you' ]
```

