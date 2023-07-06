# 15장 let, const 키워드와 블록 레벨 스코프 ⭐⭐⭐

### 15.1 var 키워드로 선언한 변수의 문제점

ES5 까지 변수를 선언하는 유일한 방법은 var 키워드였다. 하지만 독특한 특징이 있어 주의를 기울이지 않으면 문제를 발생시킬 수 있다.

#### 15.1.1 변수 중복 허용

var 키워드로 선언한 변수는 중복 선언이 가능하다.

```jsx
var x = 1;
var y = 1;

//var 키워드는 같은 스코프 내에서 중복 선언을 허용한다.
//초기화 문이 있는 변수 선언문은 JS 엔진에 의해 var 키워드가 없는 것처럼 동작한다.
var x = 100; 
var y;

console.log(x) // 100
console.log(y) // 1 => 초기화문이 없는 변수 선언문은 무시된다.
```

위와 같이 동일한 변수를 중복 선언하면서 값을 할당하면 먼저 선언된 변수 값이 변경되는 부작용이 발생한다.

#### 15.1.2 함수 레벨 스코프

var 키워드로 선언한 변수는 오로지 **함수**의 코드 블록만을 지역 스코프로 인정한다. 함수 외부에서 var 키워드로 선언한 변수는 코드 블록 내에서 선언해도 모두 전역 변수가 된다.

```jsx
var x = 1;

if(true){
	var x = 10;
}

console.log(x) //10

var i = 10;

for (var i = 0; i < 5; i++){
	console.log(i); // 0 1 2 3 4
}

console.log(i); // 5
```

이처럼 의도치 않은 전역 변수가 중복 선언될 수 있다.

#### 15.1.3 변수 호이스팅

var 키워드로 변수를 선언하면 변수 호이스팅에 의해 변수 선언문이 스코프의 선두로 끌어 올려진 것처럼 동작한다. 즉, var 키워드로 선언한 변수는 변수 호이스팅에 의해 선언문 이전에 참조할 수 있다.

```jsx
// 이 시점에 foo 변수는 선언되어 있음 - 선언
// undefined로 초기화된 상태 - 초기화
console.log(foo); //undefined

foo = 123; // - 할당

console.log(foo); // 123

var foo; // - 선언
```

이러한 코드는 가독성을 떨어뜨리고 오류를 발생시킬 수 있다.



### 15.2 let 키워드

ES6에서는 var의 단점을 보완하기 위해 let과 const를 도입한다.

#### 15.2.1 변수 중복 금지

let 키워드로 이름이 같은 변수를 중복 선언하면 문법 에러가 발생한다.

```jsx
let bar = 123;
let bar = 456; // SyntaxError
```

#### 15.2.2 블록 레벨 스코프

let 키워드로 선언한 변수는 모든 코드 블록을 지역 스코프로 인정하는 블록 레벨 스코프를 따른다.

```jsx
let foo = 1; // 전역 변수
{
	let foo = 2; // 지역 변수
	let bar = 3; // 지역 변수
}

console.log(foo); // 1
console.log(bar); // ReferenceError
```

#### 15.2.3 변수 호이스팅

let 키워드로 선언한 변수는 변수 호이스팅이 발생하지 않는 것처럼 동작한다. (동작함)

런타임 이전에 암묵적으로 “선언 단계”와 “초기화 단계”가 한번에 진행되는 var 키워드와 달리 “선언 단계”와 ”초기화 단계”가 분리되어 진행되었기 때문이다.

```jsx
// 암묵적으로 VAR === undefined가 설정됨
console.log(VAR); // undefined
var VAR;
console.log(VAR); // undefined
VAR = 1;
console.log(VAR); // 1

console.log(LET); // ReferenceError
let LET;
console.log(LET); // undefined
let = 1;
console.log(LET); // 1
```

그렇다면 let 키워드로 선언한 변수는 초기화 단계 전까지 어떻게 될까? 변수를 참조할 수 없는 공간인 TDZ 구간이 설정된다. 그래서 ReferenceError가 발생한다.

JS는 모든 선언을 호이스팅한다. 하지만 let, const, class와 같이 ES6에서 도입된 선언문은 호이스팅이 발생하지 않는 것처럼 동작할 뿐이다.

#### 15.2.4 전역 객체와 let

let 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다.



### 15.3 const 키워드

const 키워드는 상수를 선언하기 위해 사용한다. 하지만 반드시 상수만을 위한 키워드는 아니다.

#### 15.3.1 선언과 초기화

const 키워드로 선언한 변수는 반드시 선언과 동시에 초기화해야 한다. 그렇지 않으면 문법 에러가 발생한다.

```jsx
const foo; // SyntaxError: Missing initailizer in const declaration
```

const로 선언한 변수는 let으로 선언한 변수와 같이 블록 레벨 스코프를 가지며, 변수 호이스팅이 발생하지 않는 것처럼 동작한다.

#### 15.3.2 재할당 금지

const로 선언한 변수는 재할당이 금지다.

#### 15.3.3 상수

상수는 재할당이 금지된 변수를 말한다. const 키워드로 선언된 변수에 원시 값을 할당한 경우 원시값은 변경할 수 없는 값이고, 변경할 수 없다.

#### 15.3.4 const 키워드와 객체

하지만 객체를 할당한 경우 값을 변경할 수 있다.

```jsx
const person = {
	name: "Lee",
};

person.name = 'Kim';

console.log(person); // {name: 'Kim'}
```

const 키워드는 재할당을 금지할 뿐 “불변”을 의미하지는 않는다.



### 15.4 var vs. let vs. const

* var 키워드는 사용하지 않는다
* 재할당이 필요한 경우만 let을 사용하고 변수의 스코프를 최대한 좁게 만든다.
* 변경이 필요없는 원시 값과 객체에는 const 키워드를 사용한다.
* **일단 const를 쓰고 값이 바뀌면 let을 써라**
