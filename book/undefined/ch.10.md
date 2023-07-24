# Ch.10 제네릭

지금까지 배운 타입 구문은 알려진 타입을 기반으로 타입을 설정하는 방법이었다. 하지만 때때로 코드는 호출 방식에 따라 다양한 타입으로 작동할 수 있다.

```typescript
function identity(input){
    return input
}
```

위와 같은 코드는 input과 output의 타입이 동일하다. 모든 타입이 input으로 될 수 있기에 any를 쓸 수도 있지만 그럴거면 typescript를 쓰지 말자 ! 이러한 상황에서 input 타입과 output 타입의 관계를 설정할 수 있는데 이를 제네릭이라 한다.

타입스크립트에서 함수와 같은 구조체는 제네릭 타입 매개변수를 원하는 수만큼 선언할 수 있다. 제네릭 타입 매개변수는 제네릭 구조체의 사용법에 따라 타입이 결정된다.&#x20;



### 10.1 제네릭 함수

제네릭은 매개변수 괄호 앞에 < > 로 묶인 타입 매개변수에 별칭을 배치해 생성한다. 그러면 해당 타입 매개변수를 함수의 본문 내부의 매개변수 타입 애너테이션, 반환 타입 애너테이션, 타입 애너테이션에서 사용 가능하다.

```typescript
function identity<T> (input: T){
    return input
}

const str = identity("string") // 타입: "string"
const num = identity(123) // 타입: 123
```



#### 10.1.1 명시적 제네릭 호출 타입

제네릭 함수를 호출할 때 대부분의 타입스크립트는 함수 호출 방식에 따라 타입 인수를 유추한다. 하지만 클래스와 변수 타입과 같이 때로는 타입 인수를 해석하기 위해 타입스크립트에 알려줘야하는 함수 호출 정보가 충분하지 않을 수도 있다. 이러한 경우 제네릭 타입을 명시적으로 알려줌으로써 보완할 수 있다.

이를 명시적 제네릭 타입 인수라 한다. (P.202 예시)

명시적 타입 인수는 항상 제너릭 함수에 지정 가능하지만 필요할 때만 명시적으로 지정하는 것이 좋다.



#### 10.1.2 다중 함수 타입 매개변수

여러개의 타입 매개변수를 쉼표로 구분해 함수를 정의한다. 제네릭 함수의 각 호출은 각 타입 매개변수에 대한 자체 값 집합을 확인할 수 있다.

```typescript
function makeTuple<First, Second> (first: First, second: Second){
    return [first, second] as const
}

let tuple = makeTuple(true, "abc")
// let tuple: readonly [boolean, string]
```

여러 개의 타입 매개변수를 선언하면 해당 함수에 대한 호출은 명시적으로 제네릭 타입을 모두 선언하거나 모두 선언하지 않아야 한다. 아직 일부 타입만을 유출하는 것은 불가능하다고 한다.



### 10.2 제네릭 인터페이스

인터페이스도 제네릭으로 선언할 수 있다.

```typescript
interface Box<T>{
    inside: T
}

let strBox: Box<string> = {
    inside: "abc"
}

let numBox: Box<number> = {
    inside: 123
}

let wrongBox: Box<number> = {
    inside: true // Type 'boolean' is not assignable to type 'number'.(2322)
}
```



#### 10.2.1 유추된 제네릭 인터페이스 타입

타입스크립트는 제네릭 타입을 취하는 것으로 선언된 위치에 제공된 값의 타입에서 타입 인수를 유추한다.

```typescript
interface LinkedNode<Value> {
    next?: LinkedNode<Value>
    value: Value
}

function getLast<Value>(node: LinkedNode<Value>): Value{
    return node.next ? getLast(node.next) : node.value
}

// Date
let lastDate = getLast({
    value: new Date()
})

// String
let lastFruit = getLast({
    next: {
        value: "banana"
    },
    value: "apple"
})

let lastMismatch = getLast({
    next: {
        value: 123
    },
    value: true
    // Type 'boolean' is not assignable to type 'number'.(2322)
})
```

인터페이스가 타입 매개변수를 선언하는 경우, 해당 인터페이스를 참조하는 모든 타입 애너테이션은 이에 상응하는 타입 인수를 제공해야 한다.



### 10.3 제네릭 클래스

클래스에도 제네릭 선언이 가능하다.

```typescript
class Secret<Key, Value>{
    key: Key;
    value: Value;

    constructor(key: Key, value: Value){
        this.key = key
        this.value = value
    }

    getValue(key: Key): Value | undefined {
        return this.key === key ? this.value : undefined
    }
}

const storage = new Secret(12345, "luggage") // type Secret<number, string>

storage.getValue(1035) // type string | undefined
```

제너릭 인터페이스와 마찬가지로 클래스를 사용하는 타입 애너테이션은 해당 클래스의 제너릭 타입이 무엇인지를 타입스크립트에 나타내야 한다.



#### 10.3.1 명시적 제네릭 클래스 타입

제네릭 클래스 인스턴스화는 제네릭 함수 호출과 동일한 타입 인수 유추 규칙을 따른다. 하지만 생성자에 전달된 인수에서 클래스 타입 인수를 유추할 수 없는 경우 타입 인수의 기본값은 unknown이 된다.

클래스 인스턴스는 명시적 타입 인수를 제공해 기본값 unknown이 되는 것을 피할 수 있다.

P.208 - 209 예시 보기



#### 10.3.2 제네릭 클래스 확장

제네릭 클래스는 extends 키워드 다음에 오는 기본 클래스로 사용 가능하다. 타입스크립트는 사용법에서 기본 클래스에 대한 타입 인수를 유추하지 않는다. 기본값이 없는 모든 타입 인수는 명시적 타입 애너테이션을 사용해 지정해야 한다.

```typescript
class Quote<T> {
    lines: T

    constructor(lines: T){
        this.lines = lines
    }
}

class SpokenQuote extends Quote<string[]>{
    speak() {
        console.log(this.lines.join("\n"))
    }
}

new Quote("abc").lines // type string
new Quote([4,5,4,3,3]).lines // type number[]

new SpokenQuote(["abc", "def"]).lines // type string[]
new SpokenQuote([4,5,34,3]).lines // Type 'number' is not assignable to type 'string'.(2322)
```

제네릭 파생 클래스는 자체 타입 인수를 기본 클래스에 번갈아 전달할 수 있다. 타입 이름은 일치하지 않아도 된다.



#### 10.3.3 제네릭 인터페이스 구현

제네릭 클래스는 모든 필요 매개변수를 제공함으로써 제네릭 인터페이스를 구현한다. 제네릭 인터페이스는 제네릭 기본 클래스를 확장하는 것과 유사하게 작동한다. 기본 인터페이스의 모든 타입 매개변수는 클래스에 선언되어야 한다.



#### 10.3.4 메서드 제네릭

클래스 메서드는 자체 제네릭 타입을 선언할 수 있다. 제네릭 클래스 메서드에 대한 각각의 호출은 각 타입 매개변수에 대해 다른 타입 인수를 갖는다.

```typescript
class CreatePairFactory<Key>{
    key: Key;

    constructor(key: Key){
        this.key = key
    }

    createPair<Value>(value: Value){
        return { key: this.key, value }
    }
}

const factory = new CreatePairFactory("role") // CreatePairFactory<string>
const numPair = factory.createPair(10) // { key: string, value: number }
const strPair = factory.createPair("abc") // { key: string, value: string }
```



#### 10.3.5 정적 클래스 제네릭

클래스의 정적(static) 멤버는 인스턴스 멤버와 구별되고 클래스의 특정 인스턴스와 연결되어 있지 않다. 따라서 정적 클래스 메서드는 자체 타입 매개변수를 선언할 수 있지만 클래스에 선언된 어떤 타입 매개변수에도 접근할 수 없다.



### 10.4 제네릭 타입 별칭

제네릭 타입 별칭은 일반적으로 제네릭 함수의 타입을 설명하는 함수와 함께 사용된다.

```typescript
type CreateValue<Input, Output> = (input: Input) => Output

let creator: CreateValue<string, number>

creator = text => text.length
creator = text => text.toUpperCase() // Type 'string' is not assignable to type 'number'.(2322)
// output이 number 이므로 toUpperCase 불가능
```



#### 10.4.1 제네릭 판별된 유니언

제네릭 타입과 판별된 타입을 함께 사용하면 재사용 가능 타입을 모델링하는 좋은 방법이 된다.

```typescript
type Result<Data> = FailureResult | SuccessfulResult<Data>

interface FailureResult {
    error: Error;
    succeeded: false;
}

interface SuccessfulResult<Data> {
    data: Data
    succeeded: true
}

function handleResult(result: Result<string>){
    if(result.succeeded){
        console.log(`abcdef ${result.data}`)
    }else{
        console.log(`failed ${result.error}`)
    }

    result.data
    // Property 'data' does not exist on type 'Result<string>'.
    //  Property 'data' does not exist on type 'FailureResult'.(2339)
}
```



### 10.5 제네릭 제한자

타입스크립트는 제네릭 타입 매개변수의 동작을 수정하는 구문도 제공한다.

#### 10.5.1 제네릭 기본값

타입 매개변수 선언 뒤에 =와 기본 타입을 배치해 기본값이 될 타입 인수를 명시적으로 제공할 수 있다. 기본값은 타입 인수가 명시적으로 선언되지 않고 유추할 수 없는 모든 후속 타입에 사용된다.

```typescript
interface Quote<T = string>{
    value: T
}

let implicit: Quote = { value: "hi" }
let explicit: Quote<number> = { value: 123 }
let mismatch: Quote = { value: 123 } // Type 'number' is not assignable to type 'string'.(2322)
```



타입 매개변수는 동일한 선언 안의 앞선 타입 매개변수를 기본값으로 가질 수 있다. 각 타입 매개변수는 선언에 대한 새로운 타입을 도입하므로 해당 선언 이후의 타입 매개변수에 대한 기본값으로 이를 사용할 수 있다.

```typescript
interface KeyValuePair<Key, Value = Key> {
    key: Key
    value: Value
}

let allExplicit: KeyValuePair<string, number> = {
    key: "abc",
    value: 10
}

let oneDefault: KeyValuePair<string> = {
    key: "abc",
    value: "def"
}

let firstMissing: KeyValuePair = { // Generic type 'KeyValuePair<Key, Value>' requires between 1 and 2 type arguments.(2707)
    key: "abc", // key에 대한 정보를 전달해야함
    value: 10
}
```

기본값이 없는 제네릭 타입은 기본값이 있는 제네릭 타입 뒤에 오면 안된다.



### 10.6 제한된 제네릭 타입

기본적으로 제네릭 타입은 클래스, 인터페이스, 원싯값, 유니언, 별칭 등 모든 타입을 제공할 수 있다. 그러나 일부 함수는 제한된 타입에서만 작동해야 한다.

(예제 보기..)

타입스크립트는 타입 매개변수가 타입을 확장해야 한다고 선언할 수 있으며 별칭 타입에만 허용되는 작업이다.? 타입 매개변수를 제한하는 구문은 매개변수 이름 뒤에 extends 키워드를 배치하고 그 뒤에 제한할 타입을 배치한다.

```typescript
interface WithLength {
    length: number
}

function logWithLength<T extends WithLength>(input: T){
    console.log(`Length: ${input.length}`)
    return input
}

logWithLength("str") // type string
logWithLength([ false, true ]) // type boolean[]
logWithLength({ length: 123 }) // type {length: number}

logWithLength(new Date()) // Argument of type 'Date' is not assignable to parameter of type 'WithLength'.
// Date의 프로퍼티에는 숫자 타입의 length가 없기에 에러가 발생한다.
```



#### 10.6.1 keyof와 제한된 타입 매개변수

keyof 연산자는 제한된 타입 매개변수와도 잘 작동한다. extends와 keyof를 함께 사용하면 타입 매개변수를 이전 타입 매개변수의 키로 제한할 수 있다. 또한 이 방법은 제네릭 타입의 키를 지정하는 유일한 방법이기도 하다.

P.219 Lodash get 메서드 예시



### 10.7 Promise

자바스크립트의 Promise는 네트워크 요청과 같이 요청 후 결과를 받기까지 대기가 필요한 것을 나타낸다. 각 Promise는 대기중인 작업이 성공 resolve 또는 실패 reject 하는 경우의 콜백을 등록하기 위한 메서드를 제공한다.

임의의 값 타입에 대해 유사한 작업을 나타내는 Promise의 기능은 타입스크립트의 제네릭과 자연스럽게 융합된다. Promise는 타입스크립트 타입 시스템에서 최종적으로 resolve된 값을 나타내는 단일 타입 매개변수를 가진 Promise 클래스로 표현된다.



#### 10.7.1 Promise 생성

타입스크립트에서 Promise 생성자는 단일 매개변수를 받도록 작성된다. 해당 매개변수의 타입은 제네릭 Promise 클래스에 선언된 타입 매개변수에 의존한다. 결과적으로 resolve Promise를 만드려면 Promise의 타입 인수를 명시적으로 선언해야 한다. 명시하지 않으면 unknown으로 가정되기 때문이다. Promise 생성자에 타입 인수를 명시적으로 제공하면 타입스크립트가 결과로 생기는 Promise 인스턴스의 resolve된 타입을 이해할 수 있다.

```typescript
// const resolveUnknown: Promise<unknown>
const resolveUnknown = new Promise((resolve) => {
    setTimeout(() => resolve("Done !"), 1000)
})

//const resolveString: Promise<string>
const resolveString = new Promise<string>((resolve) => {
    setTimeout(() => resolve("Done !"), 1000)
})
```



#### 10.7.2 async 함수

자바스크립트에서 async 를 사용해 선언한 모든 함수는 Promise를 반환한다. 이때 Promise를 명시적으로 언급하지 않더라도 async 함수에서 수동으로 선언된 반환 타입은 항상 Promise 타입이 된다.

```typescript
// function givesPromiseForString(): Promise<string>
async function givesPromiseForString(): Promise<string> {
    return "Done!"
}

// The return type of an async function or method must be the global Promise<T> type.
// Did you mean to write 'Promise<string>'?(1064)
async function givesString(): string{
    return "Done!"
}

// function givesNothing(): Promise<string>
async function givesNothing() {
    return "Done!"
}
```



### 10.8 제네릭 올바르게 사용하기

제네릭은 코드에서 타입을 설명하는데 많은 유연성을 제공할 수 있지만, 복잡해질 수 있다. 따라서 필요한 상황에만 제네릭을 사용하고, 사용할 때는 무엇을 위해 사용하는지 명확히 해야 한다.



#### 10.8.1 제네릭 황금률

제네릭이 필요한 상황인지 판단하는 간단하고 빠른 방법은 해당 타입 매개변수가 최소 2번 이상 사용되었는지 확인하는 것이다. 제네릭은 타입 간 관계를 설명하기 위해 사용하므로 한 곳에만 나타나면 그 관계를 정의할 수 없다. 따라서 각 함수 타입 매개변수는 매개변수에 사용되어야 하고, 적어도 하나의 다른 매개변수, 또는 함수의 반환 타입에서도 사용되어야 한다.

```typescript
function logInput<Input extends string>(input: Input){
    console.log("Hi!", input)
}
```

위와 같은 상황에서 제너릭으로 사용된 Input은 타입 매개변수로 더 많은 매개변수를 반환하거나 선언하는 작업을 하지 않는다. 따라서 Input을 제너릭으로 사용하는 것은 쓸모가 없다. 차라리 매개변수에서만 string 타입을 알리는 것이 좋다.



#### 10.8.2 제네릭 명명 규칙

타입스크립트를 포함한 많은 언어에서 지키는 타입 매개변수에 대한 표준 명명 규칙은 기본적으로 첫 타입 인수로 T를 사용하고, 후속 타입에서 U, V를 사용한다.&#x20;

타입 인수가 어떻게 사용되는지 맥락과 관련된 정보가 알려진 경우에는 해당 용어의 첫 글자를 사용하는 것으로 확장되기도 한다. 예를 들어 상태 관리에서 제네릭 상태를 S로, 데이터 구조의 키과 앖은 K와 V로 등등

하지만 이런 인수명은 혼란스럽다. 차라리 완전히 작성된 이름을 사용하는 편이 가독성을 높이기 좋다.



### 10.9 마치며

* 구조체 간 다른 타입을 나타내기 위한 타입 매개변수 사용법
* 제네릭 함수 호출시 명시적 또는 암시적 타입 인수 제공
* 제네릭 객체 타입을 표현하는 제네릭 인터페이스
* 클래스에 타입 매개변수를 추가하고 클래스의 타입에 미치는 영향 확인
* 타입 별칭, 판별된 유니언에 타입 매개변수 추가
* 기본값과 제한자를 사용한 제네릭 타입 매개변수 수정
* Promise와 async 함수가 제네릭을 사용해 비동기 데이터 흐름을 나타내는 방법
* 제네릭 황금률과 명명 규칙을 포함한 제네릭 모범 사례



\---

클래스 부분은 책의 사례 위주로다시 보기
