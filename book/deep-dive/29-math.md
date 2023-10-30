# 29장 Math

### 29.1 Math 프로퍼티

#### 29.1.1 Math.PI

```tsx
Math.PI // 3.14159265...
```

원주율 값을 반환한다.

***

### 29.2 Math 메서드

#### 29.2.1 Math.abs

```tsx
Math.abs(-1) // 1
Math.abs('-1') // 1
Math.abs('') // 0
Math.abs(1) // 1
Math.abs('string') // NaN
```

전달된 숫자의 절대값을 반환한다. 숫자는 반드시 0 또는 양수, 숫자가 아니면 NaN 을 반환한다.

#### 29.2.2 Math.round

```tsx
Math.round(1.4) // 1
Math.round(1.6) // 2
Math.round(-1.4) // -1
Math.round(-1.6) // -2
Math.round(1) // 1
Math.round() // NaN
```

전달된 숫자의 소수점 이하를 반올림한 정수를 반환한다. 숫자가 아니면 NaN 을 반환한다.

#### 29.2.3 Math.ceil

```tsx
Math.ceil(1.4) // 2
Math.ceil(1.6) // 2
Math.ceil(-1.4) // -1
Math.ceil(-1.6) // -1
Math.ceil(1) // 1
Math.ceil() // NaN
```

전달된 숫자의 소수점 이하를 올림한 정수를 반환한다. 숫자가 아니면 NaN 을 반환한다.

#### 29.2.4 Math.floor

```tsx
Math.floor(1.4) // 1
Math.floor(1.6) // 1
Math.floor(-1.4) // -2
Math.floor(-1.6) // -2
Math.floor(1) // 1
Math.floor() // NaN
```

전달된 숫자의 소수점 이하를 내림한 정수를 반환한다. 숫자가 아니면 NaN 을 반환한다.

#### 29.2.5 Math.sqrt

```tsx
Math.sqrt(9) // 3
Math.sqrt(-9) // NaN
Math.sqrt(2) // 1.414...
Math.sqrt(1) // 1
Math.sqrt(0) // 0
Math.sqrt() // NaN
```

전달된 숫자의 제곱근을 반환한다.

#### 29.2.6 Math.random

```tsx
Math.random() // 0 에서 1 사이의 랜덤 실수

const random = Math.floor((Math.random() * 10) + 1)
console.log(random) // 1 에서 10 사이의 정수
```

임의의 난수(랜덤 숫자)를 반환한다. 반환하는 난수는 0에서 1 미만의 실수이다.

#### 29.2.7 Math.pow

```tsx
Math.pow(2,8) // 256
Math.pow(2,-1) // 0.5
Math.pow(2) //NaN
```

첫 번째 인수를 밑, 두 번째 인수를 지수로 거듭제곱한 결과를 반환한다.

ES7 에서 도입된 지수 연산자가 가독성이 더 좋다.

```tsx
2 ** 2 ** 2 // 16
Math.pow(Math.pow(2,2),2) // 16
```

#### 29.2.8 Math.max

```tsx
Math.max(1) // 1
Math.max(1,2) // 2
Math.max(1,2,3) // 3
Math.max() // -Infinity
```

전달받은 수 중 가장 큰 수를 반환한다.

배열을 전달받아 배열 요소 중 최대값을 구하려면

```tsx
Math.max.apply(null, [1,2,3]) // 3
Math.max(...[1,2,3]) // 3
```

#### 29.2.9 Math.min

```tsx
Math.max(1) // 1
Math.max(1,2) // 1
Math.max(1,2,3) // 1
Math.max() // Infinity
```

전달받은 수 중 가장 작은 수를 반환한다.

배열을 전달받아 배열 요소 중 최소값을 구하려면

```tsx
Math.min.apply(null, [1,2,3]) // 1
Math.min(...[1,2,3]) // 1
```
