# Ch.5 함수



### 5.1 함수 매개변수

명시적 타입 정보가 선언되지 않으면 절대 타입을 알 수 없다.  타입스크립트는 이를 any로 간주하여 어떤 타입이든 가능하게 한다. 하지만 any를 쓸 바엔 타입스크립트를 안쓰고 말지...

변수와 마찬가지로 함수 매개변수의 타입을 선언할 수 있다.

```typescript
function sing (song: string) {
    console.log("Singing: ${song}!')
}
```



#### 5.1.1 필수 매개변수

자바스크립트에서는 인수의 개수와 상관없이 함수를 호출할 수 있다. 하지만 타입스크립트는 함수에 선언된 모든 매개변수가 필수라 가정한다. 따라서 함수가 선언될 때의 인수의 개수보다 적거나 많은 수의 인수를 호출하면 타입 오류의 형태로 에러를 알려준다.

```typescript
function test(first: string, second: number, third: string){
    console.log(`${first}, ${second}, ${third}`)
}

test("일", 2) // Expected 3 arguments, but got 2.(2554)
test("일", 2, "3")
test("일", 2, "3", 4) //Expected 3 arguments, but got 4.(2554)

```

함수에 필수 매개변수 required parameter 제공을 강제하면 예상되는 모든 인숫값을 함수 내에 존재하도록 강제해 타입 안정성을 강화할 수 있다. 인수의 개수를 충족하지 못하면 위와같이에러를 발생한다.



#### 5.1.2 선택적 매개변수

자바스크립트에서 매개변수가 제공되지 않으면 기본값으로 undefined가 설정되고, 값을 제공하지 않아도 실행된다. 타입스크립트에서도 이와 같은 설정을 할 수 있다.

이를 선택적 매개변수라 하며, 타입 애너테이션의 : 앞에 ?를 추가하면 된다. 항상 | undefined가 유니언 타입으로 추가되어 있고 선택 사항으로 인수 제공이 필수가 아니다.

```typescript
function test(first: string, second: number, third?: string){
    console.log(`${first}, ${second}, ${third}`)
}

test("일") // Expected 2-3 arguments, but got 1.(2554)
test("일", 2)
test("일", 2, "3")


```

이러한 선택적 매개변수는 | undefined를 포함하는 유니언 타입 매개변수와는 다르다. 값이 undefined일 지라도 항상 제공 되어야 하기 때문이다.



그리고 함수에서 사용되는 모든 선택적 매개변수는 마지막 매개변수여야 한다.

```typescript
// A required parameter cannot follow an optional parameter.(1016)
function test(first?: string, second: number, third: string){ 
    console.log(`${first}, ${second}, ${third}`)
}

test("일") // Expected 3 arguments, but got 1.(2554)
test("일", 2) // Expected 3 arguments, but got 2.(2554)
test("일", 2, "3")

```

아니면 위와 같이 구문 오류가 발생한다.



#### 5.1.3 기본 매개변수

자바스크립트는 선택적 매개변수를 선언할 때 =으로 기본값을 제공할 수 있다.

타입스크립트에서도 유사한 역할을 하며, 기본적으로 값이 제공되기에 암묵적으로 함수 내부에 | undefined 유니언 타입이 추가된다.&#x20;

```typescript
function test (one: string, two = 2){
    console.log(`${one}, ${two}`)
}

test("1")
test("1", 5)
test("1", "6") // Argument of type 'string' is not assignable to parameter of type 'number'.(2345)
test("1", undefined)

```



#### 5.1.4 나머지 매개변수

자바스크립트에서 ...스프레드 연산자는 함수 선언의 마지막 매개변수에 위치하고, 단일 배열에 저장되어야 한다.

타입스크립트는 이러한 나머지 매개변수의 타입을 일반 매개변수와 유사하게 선언할 수 있다.

```typescript
function test (one: string, two: number, ...others: number[]){
    console.log(`${one}, ${two}, ${others}`)
}

test("1", 5)
test("1", 8, 1,5,6,7)
test("2", 3, "string") // Argument of type 'string' is not assignable to parameter of type 'number'.(2345)

```



### 5.2 반환 타입

타입스크립트는 지각적 perceptive 이다. 함수가 반환 가능한 모든 값을 이해하고 함수가 반환하는 타입을 추론할 수 있다.

```typescript
function test(songs: string[]) {
    for (const song of songs) {
        console.log(`${song}`)
    }

    return songs.length
}

test // function test(songs: string[]): number
```

함수가 여러 개의 반환문을 포함한다면, 타입스크립트는 반환 가능한 모든 타입의 조합으로 반환 타입을 유추한다.

```typescript
function test(songs: string[], index: number) {
    return index < songs.length ? songs[index] : undefined
}

test // function test(songs: string[], index: number): string | undefined
```



#### 5.2.1 명시적 반환 타입

변수와 마찬가지로 타입 애너테이션을 사용해 명시적으로 함수의 반환 타입을 선언하지 않는 것이 좋다. 그러나 반환 타입을 명시적으로 선언하는 방식이 유용할 때가 종종 있다.

* 반환 가능한 값이 많은 함수가 항상 동일한 타입의 값을 반환하도록 강제하는 경우
* 타입스크립트는 재귀 함수의 반환 타입을 통해 타입 유추를 거부함
* 규모가 큰 프로젝트에서 타입스크립트 타입 검사 속도를 높일 수 있음

함수 반환 타입은 다음과 같이 배치한다.

```typescript
function test(one: string): string {
    return one
}

// 화살표 함수의 경우
const test2 = (two: number): number => {
    return two < 5 ? two : 0
}
```



### 5.3 함수 타입

자바스크립트에서는 함수를 값으로 전달할 수 있다. 그러므로 함수를 가지기 위한 매개변수 또는 변수의 타입을 선언하는 방법이 필요하다.

함수 타입 구문은 화살표 함수와 유사하다.

```typescript
let test: () => string
let test: (one: string[], two?: number) => string
```



#### 5.3.1 함수 타입 괄호

함수 타입은 다른 타입이 사용되는 모든 곳에 배치 가능하다. 유니언 타입도 포함되는데 유니언 타입의 애너테이션에서 함수 반환 위치를 나타내거나 유니언 타입을 감싸는 부분에서 괄호를 사용한다.

```typescript
let test: () => string | undefined
// string or undefined

let test2: (() => string) | undefined
// string이 리턴되는 함수 or undefined
```



#### 5.3.2 매개변수 타입 추론

모든 함수에 대해 매개변수를 선언하려면 번거롭다. 다행히 타입스크립트는 선언된 타입의 위치에 제공된 함수의 매개변수 타입을 추론할 수 있다.

```typescript
let singer: (song: string) => string

singer = function (song) {
    return `Singing: ${song.toUpperCase()}!`
}

// (song: string) => string 으로 추론함
```



#### 5.3.3 함수 타입 별칭

함수 타입에서도 타입 별칭을 사용할 수 있다.

```typescript
type Test = (input: string) => number

let test: Test;

test = (input) => input.length
test = (input) => input.toUpperCase() // Type 'string' is not assignable to type 'number'.(2322)
```

타입 별칭은 함수 타입에 유용하다. 반복적으로 작성하는 매개변수와 반환 타입을 갖는 코드 공간을 많이 절약할 수 있다.



### 5.4 그 외 반환 타입

#### 5.4.1 void 반환 타입

일부 함수는 어떤 값도 반환하지 않는다. 예를들어 return 문이 없거나 값을 반환하지 않는 return 문을 가진 함수들이다. 타입스크립트는 void를 활용해 이러한 함수의 반환 타입을 확인 할 수 있따.

```typescript
function test(song: string | undefined): void {
    if(!song){
        return
    }

    console.log(`${song}`)

    return true // Type 'boolean' is not assignable to type 'void'.(2322)
}
```

함수 타입 선언시 void 반환 타입은 매우 유용하다. 함수 타입을 선언할 때 void를 사용하면 함수에서 반환되는 모든 값은 무시된다.

자바스크립트 함수는 실젯값이 반환되지 않으면 기본으로 모두 undefined를 반환하지만 void는 undefined와 다르다. void는 함수의 반환 타입이 모두 무시된다는 것을 의미하고, undefined는 반환되는 리터럴 값이다. 따라서 undefined를 포함하는 대신 void 타입의 값을 할당하려고 하면 타입 오류가 발생한다.

```typescript
function test() {
    return
}

let testtest: string | undefined

testtest = test() // Type 'void' is not assignable to type 'string | undefined'.(2322)
```

undefined와 void를 구분해서 사용하면 매우 유용하다. 특히 void를 반환하도록 선언된 타입 위치에 전달된 함수가 반환된 모든 값을 무시하도록 설정할 때 유용하다.



#### 5.4.2 never 반환 타입

일부 함수는 값을 반환하지 않을 뿐 아니라 반환할 생각도 없다. never 반환 함수는 (의도적으로) 함수를 발생시키거나 무한 루프를 실행하는 함수이다.

함수가 절대 반환하지 않도록 의도하려면 명시적으로 : never 타입 애너테이션을 추가해 해당 함수 호출 후 모든 코드가 실행되지 않음을 나타내면 된다.

```typescript
function fail(message: string): never {
    throw new Error(`Invariant failure: ${message}`)
}

function test(param: unknown) {
    // string이 아닌 값이 입력되었을때 에러를 알려준다.
    if(typeof param !== "string"){
        fail(`param should be a string, not ${typeof param}`)
    }

    param.toUpperCase()
}
```

\*) void는 아무것도 반환하지 않는 함수를 위한 것이고 never는 절대 반환하지 않을 함수를 위한 것이다.



### 5.5 함수 오버로드

일부 자바스크립트 함수는 선택적 매개변수와 나머지 매개변수만으로 표현할 수 없는 매우 다른 매개변수들로 호출될 수 있다. 이러한 함수는 오버로드 시그니처라고 불리는 타입스크립트 구문으로 설명이 가능하다. 즉, 하나의 최종 구현 시그니처와 그 함수의 본문 앞 서로 다른 버전의 함수 이름, 매개변수, 반환 타입을 여러 번 선언한다.

```typescript
function test(timestamp: number): Date;
function test(month: number, day: number, year: number): Date;
function test(monthOrTimeStamp: number, day?: number, year?: number){
    return day === undefined || year === undefined
        ? new Date(monthOrTimeStamp) : new Date(year, monthOrTimeStamp, day)
}

test(135135)
test(6, 23, 1993)
test(3, 5) // No overload expects 2 arguments, but overloads do exist that expect either 1 or 3 arguments.(2575)


```

위 예시의 test 함수는 아래의 JS 코드로 컴파일된다.

```javascript
function test(monthOrTimeStamp, day, year) {
    return day === undefined || year === undefined
        ? new Date(monthOrTimeStamp) : new Date(year, monthOrTimeStamp, day);
}
```



#### 5.5.1 호출 시그니처 호환성

오버로드된 함수의 구현에서 사용되는 구현 시그니처는 매개변수 타입과 반환 타입에 사용하는 것과 동일하다. 따라서 함수의 오버로드 시그니처에 있는 반환 타입과 각 매개변수는 구현 시그니처에 있는 동일한 인덱스의 매개변수에 할당할 수 있어야 한다. 즉, 구현 시그니처는 모든 오버로드 시그니처와 호환되어야 한다.

```typescript
function test(data: string): string;
function test(data: string, data2: number, data3: string): string
function test(getData: () => string): string;
// Function implementation is missing or not immediately following the declaration.(2391)
// 첫번째 매개변수의 타입이 이전의 test 함수들과 다르기에 호환되지 않아 에러가 발생함
```



### 5.6 마치며

* 타입 애너테이션으로 함수 매개변수 타입 선언하기
* 타입 시스템의 동작을 변경하기 위한 선택적 매개변수, 기본 매개변수, 나머지 매개변수 선언하기
* 타입 애너테이션으로 함수 반환 타입 선언하기
* void 타입으로 사용 가능한 값을 반환하지 않는 함수 알아보기
* never 타입으로 절대 반환하지 않는 함수 알아보기
* 함수 오버로드를 사용해서 다양한 함수 호출 시그니처 설명하기



\----

230718 함수 오버로딩에 대해 조금 더 알아보자

