# 28장 Number

### 28.1 Number 생성자 함수

```jsx
const numObj = new Number()
console.log(numObj) // Number{[[PrimitiveValue]]: 0}

const numObj = new Number(10)
console.log(numObj) // Number{[[PrimitiveValue]]: 10}
```

***

### 28.2 Number 프로퍼티

#### 28.2.1 Number.EPSILON

부동소수점으로 인해 발생하는 오차를 해결하기 위해 사용한다.

#### 28.2.2 Number.MAX\_VALUE

JS에서 표현할 수 있는 가장 큰 양수 값

#### 28.2.3 Number.MIN\_VALUE

JS에서 표현할 수 있는 가장 작은 양수 값

#### 28.2.4 Number.MAX\_SAFE\_INTEGER

JS에서 표현할 수 있는 가장 큰 정수값

#### 28.2.5 Number.MIN\_SAFE\_INTEGER

JS에서 표현할 수 있는 가장 작은 정수값

#### 28.2.6 Number.POSITIVE\_INFINITY

양의 무한대, Infinity와 같다

#### 28.2.7 Number.NEGATIVE\_INFINITY

음의 무한대, -Infinity와 같다

#### 28.2.8 Number.NaN

숫자가 아님을 나타내는 숫자값

***

### 28.3 Number 메서드

#### 28.3.1 Number.isFinite

ES6에서 도입.

인수로 전달된 숫자값이 정상적인 유한수인지 검사해 불리언 값 반환

암묵적 타입 변환 x

#### 28.3.2 Number.isInteger

ES6에서 도입.

인수로 전달된 숫자값이 정수인지 검사해 불리언 값 반환

암묵적 타입 변환 x

#### 28.3.3 Number.isNaN

ES6에서 도입.

인수로 전달된 숫자값이 NaN인지 검사해 불리언 값 반환

암묵적 타입 변환 x

#### 28.3.4 Number.isSafeInteger

ES6에서 도입.

인수로 전달된 숫자값이 안전한 정수인지 검사해 불리언 값 반환

암묵적 타입 변환 x

#### 28.3.5 Number.prototype.toExponential

숫자를 지수 표기법으로 변환해 문자열로 반환한다.

```jsx
(77.1234).toExponential() // "7.71234e+1"
(77.1234).toExponential(4) // "7.7123e+1"
(77.1234).toExponential(2) // "7.71e+1"
```

#### 28.3.6 Number.prototype.toFixed

숫자를 반올림해 문자열로 반환

```jsx
(12345.6789).toFixed() // "12346"
(12345.6789).toFixed(1) // "12345.7"
(12345.6789).toFixed(2) // "12345.68"
```

#### 28.3.7 Number.prototype.toPrecision

인수로 전달받은 전체 자릿수까지 유효하도록 나머지 자릿수를 반올림하여 문자열로 반환.

전체 자릿수로 표현할 수 없는 경우 지수 표기법으로 결과 반환

```jsx
(12345.6789).toPrecision() // "12345.6789"
(12345.6789).toPrecision(1) // "1e+4"
(12345.6789).toPrecision(2) // "1.2e+4"
(12345.6789).toPrecision(6) // "12345.7"
```

#### 28.3.8 Number.prototype.toString

숫자를 문자열로 변환하여 반환한다. 진법을 나타내는 2\~36사이의 정수값을 인수로 전달할 수 있다.

```jsx
(10).toString() // "10"
(16).toString(2) // "10000"
(16).toString(8) // "20"
(16).toString(16) // "10"
```
