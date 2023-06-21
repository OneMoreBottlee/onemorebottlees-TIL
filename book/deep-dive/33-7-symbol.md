# 33장 7번째 데이터 타입 Symbol

### 33.1 심벌이란?

1997년 JS 가 표준화된 이래로 6개의 타입 (문자열, 숫자, 불리언, undefined, null, 객체)이 있었다.

심벌은 ES6에서 도입된 7번째 타입으로 변경 불가능한 원시 값이다. 다른 값과 중복되지 않는 유일무이한 값으로, 이름의 충돌 위험이 없는 유일한 프로퍼티 키를 만들기 위해 사용한다.



### 33.2 심벌 값의 생성

#### 33.2.1 Symbol 함수

심벌 값은 Symbol 함수를 호출하여 생성한다. 이때 생성된 심벌 값은 외부로 노출되지 않아 확인할 수 없으며, 다른 값과 절대 중복되지 않는 **유일무이한 값**이다.

```javascript
const mySymbol = Symbol() // 유일 값 symbol 생성
console.log(typeof mySymbol) // symbol

console.log(mySymbol) // Symbol() 심벌 값은 외부로 노출되지 않아 확인할 수 없다.
```

new 연산자와 함께 호출하지 않는다.\
(다른 생성자 함수는 new를 활용해 객체(인스턴스)를 생성하지만 symbol은 유일한 값이기에 객체(인스턴스)를 생성하지 않음)



선택적으로 문자열을 인수로 전달할 수 있다. 생성된 문자열에 대한 설명으로 디버깅 용도로만 사용되며, 심벌 값 생성에 어떠한 영향도 주지 않는다.

```javascript
const mySymbol1 = Symbol('mySymbol')
const mySymbol2 = Symbol('mySymbol')

console.log(mySymbol1 === mySymbol2) // false => 각 심벌은 유일한 값이다
```

심벌 값도 객체처럼 접근하면 암묵적으로 래퍼 객체를 생성한다.

```javascript
const mySymbol = Symbol('mySymbol')

console.log(mySymbol.description) // mySymbol
console.log(mySymbol.toString()) // Symbol(mySymbol)

// 심벌 값은 암묵적으로 문자열이나 숫자 타입으로 변환되지 않는다.
console.log(mySymbol + "") // TypeError
console.log(+mySymbol) // TypeError

// 단, 불리언 타입으로는 변환됨
console.log(!!mySymbol) // true

// if 문 등에서 확인 가능
if(mySymbol)console.log("mySymbol is not empty !")
```



#### 33.2.2 Symbol.for / Symbol.keyFor 메서드

Symbol.for 메서드는 인수로 전달받은 문자열을 키로 사용해 키와 심벌 값의 쌍들이 저장된 전역 심벌 레지스트리에서 해당 키와 일치하는 심벌 값을 검색한다.

* 검색 성공시, 검색된 심벌 값 반환
* 검색 실패시, 새로운 심벌 값 생성 => 인수로 전달된 키로 전역 심벌 레지스트리에 저장 => 생성된 심벌 값 반환

```javascript
const s1 = Symbol.for('mySymbol') // mySymbol 로 새로운 심벌 값 생성
const s2 = Symbol.for('mySymbol') // mySymbol 로 저장된 심벌 값 반환

console.log(s1 === s2) // true
```

Symbol.for 메서드를 활용하면 전역에서 중복되지 않는 유일한 심벌 값을 단 하나만 생성해 공유할 수 있다.



Symbol.keyFor 메서드를 사용하면 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출할 수 있다.

```javascript
const s1 = Symbol.for('mySymbol')
Symbol.keyFor(s1) // mySymbol

const s2 = Symbol('foo')
Symbol.keyFor(s2) // undefined

// Symbol.for 로 생성된 심벌 값은 Symbol 로 생성된 값과 달리 레지스트리에 저장되어 관리할 수 있다.
```



### 33.3 심벌과 상수

p.608 - 609 예시 보기



### 33.4 심벌과 프로퍼티 키

심벌값으로유일한 프로퍼티 키 만들기

```javascript
const obj = {
    [Symbol.for('mySymbol'] : 1 // 심벌 값을 프로퍼티 키로 사용하려면 대괄호를 사용해야 함
}

obj[Symbol.for('mySymbol')] // 1
```



### 33.5 심벌과 프로퍼티 은닉

심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티는 Object.getOwnPropertySymbols 메서드를 제외하면 찾을 수 없다.

```javascript
const obj = {
    [Symbol('mySymbol')] : 1
}

for(const key in obj){
    console.log(key) // 출력 없음
}

console.log(Object.keys(obj)) // []
console.log(Object.getOwnPropertyNames(obj)) // []


console.log(Object.getOwnPropertySymbols(obj)) // [Symbol(mySymbol)]

const symbolKey1 = Object.getOwnPropertySymbols(obj)[0]
console.log(obj[symbolKey1]) // 1
```



### 33.6 심벌과 표준 빌트인 객체 확장

일반적으로 표준 빌트인 객체에 사용자 정의 메서드를 직접 추가하는 것은 권장되지 않는다. 추후 추가되는 메서드와 이름이 중복되어 에러가 발생될 수 있기 때문이다.

하지만 중복 가능성이 없는 심벌 값으로 프로퍼티 키를 생성해 확장하면 충돌 우려가 없어 안전하게 빌트인 객체를 확장할 수 있다.



### 33.7 Well-Known Symbol

JS가 제공하는 빌트인 심벌 값이 있다. ECMAScript 사양에서 Well-Known Symbol이라 부르는 값으로 JS 엔진의 내부 알고리즘에 사용된다.

예를 들어 for ... of 로 순회 가능한 빌트인 이터러블은 Well-Known Symbol인 Symbol.iterator를 키로 갖는 메서드를 가지며, 이를 호출하면 이터레이터를 반환하도록 규정되어 있다.



이처럼 실벌은 중복되지 않는 상수 값을 생성하는 것은 물론 기존 작성된 코드에 영향없이 새로운 프로퍼티를 추가하는 하위 호환성을 보장하기 위해 추가되었다.



