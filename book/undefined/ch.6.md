# Ch.6 배열

자바스크립트 배열은 매우 유연하고 내부에 모든 타입의 값을 혼합해 저장할 수 있다. 자바스크립트의 배열 대부분은 하나의 특정 타입 값만 가진다. 다른 타입의 값을 추가하면 혼란을 줄 수 있고, 오류가 발생할 수 있기 때문이다.

타입스크립트는 초기 배열에 어떤 데이터 타입이 있는지 기억하고, 배열이 해당 데이터 타입에서만 작동하도록 제한한다. 이를 통해 배열의 데이터 타입을 하나로 유지할 수 있다.

타입스크립트가 초기 배열에 담긴 요소를 통해 배열 타입을 유추하는 방법은 변수의 방식과 유사하다. 타입스크립트는 값이 할당되는 방식에서 코드의 의도된 타입을 이해하려고 시도하며 배열도 예외는 아니다.



### 6.1 배열 타입

타입스크립트는 변수에 타입 애너테이션을 제공해 배열이 포함해야 하는 타입을 알려준다.

```typescript
let array: number[]

array = [1,3,5,6]
array = ["gg", 1, true]
// Type 'string' is not assignable to type 'number'.(2322)
// Type 'boolean' is not assignable to type 'number'.(2322)
```

배열의 요소 타입 다음에 \[]를 사용한다.



#### 6.1.1 배열과 함수 타입

괄호는 애너테이션의 어느 부분이 함수 반환 부분이고 어느 부분이 배열 타입 묶음인지 나타내기 위해 사용한다.

```typescript
// string 배열을 리턴하는 함수
let test: () => string[]
// ["a", "b"]

// string을 반환하는 함수의 배열
let test2: (() => string)[]
// [()=>return "a", ()=>return "b"]
```



#### 6.1.2 유니언 타입 배열

배열의 각 요소가 여러 타입 중 하나일 가능성이 있다면 유니언 타입을 사용한다.

```typescript
// string 타입이거나 number 배열
let test: string | number[]

// string 배열 또는 number 배열
let test2: (string | number)[]
```

배열의 요소 타입은 배열에 담긴 요소에 대한 가능한 모든 타입의 집합이다.



#### 6.1.3 any 배열의 진화

빈 배열로 설정된 변수에 타입 애너테이션을 포함하지 않으면, any\[]로 취급된다.&#x20;

any를 쓸 바엔 타입스크립트를 안쓰고 말지 ! any를 쓰지 말자



#### 6.1.4 다차원 배열

2차원 배열 또는 배열의 배열은 두 개의 \[]를 갖는다

```typescript
let test: number[][]

test = [
    [1,2,3],
    [4,5,6],
    [7,8,9]
]
```



### 6.2 배열 멤버

타입스크립트는 인덱스 기반 접근 방식을 이해하는 언어이다. 각 인덱스별 타입을 지정할 수 있다.



#### 6.2.1 주의 사항: 불안정한 멤버

배열은 타입 시스템에서 불안정한 부분이 있다. 기본적으로 타입스크립트는 배열의 모든 요소에 접근한다고 가정하지만 배열의 길이보다 큰 인덱스로 접근하면 undefined를 제공하고 오류를 표시하지 않는다.

```typescript
function testFn(array: string[]){
    console.log(array[135135].length)
}

// 배열의 길이가 135135보다 작지만 에러가 발생하지 않음
testFn(["hi", "hello"])
```



### 6.3 스프레드와 나머지 매개변수

#### 6.3.1 스프레드

... 스프레드 연산자를 사용해 배열을 결합한다. 타입스크립트는 입력된 배열 중 하나의 값이 결과 배열에 포함됨을 이해한다.

```typescript
const test1 = ["a", "b", "c"]
const test2 = [1, 2, 3]

const testSum = [...test1, ...test2]
// const testSum: (string | number)[]
```



#### 6.3.2 나머지 매개변수 스프레드

타입스크립트는 나머지 매개변수로 배열을 스프레드하는 자바스크립트 코드를 인식하고 이에 대해 타입 검사를 수행한다. 나머지 매개변수를 위한 인수로 사용되는 배열은 나머지 매개변수와 동일한 배열 타입을 가져야 한다.

```typescript
function testFn(str: string, ...strArr: string[]){
    for(const abc of strArr){
        console.log(`${str}, ${abc}`)
    }
}

const testArr = ["a", "b", "c"]
testFn("one", ...testArr)

const testNum = [1, 2, 3]
testFn("two", ...testNum)
// Argument of type 'number' is not assignable to parameter of type 'string'.(2345)
```



### 6.4 튜플

자바스크립트 배열은 이론상 어떤 크기라도 될 수 있다. 하지만 때로는 튜플이라는 고정된 크기의 배열을 사용하는 것이 유용하다. 튜플 배열은 각 인덱스에 알려진 특정 타입을 가지며 요소의 가능한 모든 타입을 갖는 유니언 타입보다 더 구체적이다.&#x20;

```typescript
let test: [number, string]
test = [1, "h"]
test = ["h", "h"] // Type 'string' is not assignable to type 'number'.(2322)
test = [5]
// Type '[number]' is not assignable to type '[number, string]'.
//   Source has 1 element(s) but target requires 2.(2322)
```



#### 6.4.1 튜플 할당 가능성

// 번역이 요상하다 예시보고 이해하자 P.125 - P.127

타입스크립트에서 튜플 타입은 가변 길이의 배열 타입보다 더 구체적으로 처리된다. 즉, 가변 길이의 배열 타입은 튜플 타입에 할당 불가능하다.



**나머지 매개변수로서 튜플**

튜플은 구체적 길이와 요소 타입 정보를 갖는 배열로 간주되므로 함수에 전달할 인수를 저장하는데 특히 유용하다. 타입스크립트는 ... 나머지 매개변수로 전달된 튜플에 정확한 타입 검사를 제공 할 수 있다.



#### 6.4.2 튜플 추론

타입스크립트는 생성된 배열을 튜플이 아닌 가변 길이의 배열로 취급한다. 배열이 변수의 초깃값 또는 함수에 대한 반환값으로 사용되는 경우, 고정된 크기의 튜플이 아니라 유연한 크기의 배열로 가정한다.

타입스크립트에서는 값이 더 구체적인 튜플 타입이어야함을 두 가지 방법으로 나타내는데, 명시적 튜플 타입과 const 어서션을 사용한 방법이다.



**명시적 튜플 타입**

튜플 타입도 함수의 반환 타입에 타입 애너테이션으로 사용 가능하다.

```typescript
// 반환 타입 [string, number]
function test(input: string): [string, number] {
    return [input[0], input.length]
}

const [first, size] = test("hello world")
// first - string
// size - number
```



**const 어서션**

명시적 타입 애너테이션은 코드가 많아질수록 번거롭다. 그 대안으로 const 어서션인 as const 연산자가 있다.&#x20;

const 어서션은 타입스크립트에 타입을 유추할 때 읽기 전용이 가능한 값 형식을 사용하도록 지시한다. as const가 배치되면 배열이 튜플로 처리되어야 함을 나타낸다.

즉, const 어서션은 배열을 고정된 크기의 튜플로 전환하고, 해당 튜플을 읽기 전용으로 수정이 불가능하게 고정할 수 있다.

```typescript
const test: [number, string] = [11, "hh"]
test[0] = 135

const asConst: [number, string] = [11, "hh"] as const
// The type 'readonly [11, "hh"]' is 'readonly' and cannot be assigned to the mutable type '[number, string]'.(4104)

const asConst2 = [11, "hh"] as const
asConst2[0] = 15135
// Cannot assign to '0' because it is a read-only property.(2540)
```



### 6.5 마치며

* \[]로 배열 타입 선언하기
* 괄호를 사용해 함수 배열 또는 유니언 타입 배열 선언하기
* 타입스크립트가 배열 요소를 배열의 타입으로 이해하는 방법
* ... 스프레드와 나머지 매개변수로 작업하는 방법
* 고정된 크기와 배열을 나타내는 튜플 타입 선언하기
* 타입 애너테이션 또는 as const 어서션으로 튜플 생성하기

