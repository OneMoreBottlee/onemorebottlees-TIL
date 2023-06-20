# #2 Overview of TypeScript

### #2.0 How Typescript Works

TS 는 프로그래밍 언어.

Strongly Typed Programming Language \
\=> 코드를 컴파일해서 다른 종류의 코드로 변환하는 프로그래밍 언어\
TS => JS



TS를 컴파일하는 과정에서 에러를 확인하면 컴파일이 중지되고, 에러를 리턴함

\=> 런타임 이전에 잘못된 부분을 확인 할 수 있음



### #2.1 Implicit Types vs Explicit Types

TS의 타입 시스템 - 타입 추론, 명시적 선언

```typescript
let a = "hello" // let a : string
a = "bye"

a = 1 // error : type number is ~~
// 변수 선언시 string으로 했으면 string으로 덮을 수 있음
// JS는 문제없지만 TS에서는 문제가 됨

let b : boolean = "x" // error : "x"는 boolean이 아님
// 타입을 선언하는 방식, 명시적으로 타입을 선언함
// 명시적 선언은 최소화 하는게 좋음 => 타입 추론을 활성화

let c = [1, 2, 3] // let c : number[]
c.push("1") // error : number array에 string을 추가하면 타입이 섞임
```



### #2.2 Types of TS part One

기본 타입 - number / string / boolean / ...\[]



Optional 선택적 변수

```typescript
const player : {
    name: string,
    age?: number // => ? 는 optional 선택적 변수
} = {
    name: "nico",
}

if(player.age && player.age < 10) { // undefined 인 경우도 확인
    
}
```



type 선언

```typescript
// Alias 별칭 타입 선언
type Age = number;
type Player = {
    name: string,
    age?: Age
}

const nico: Player = { 
    name: "nico"
}

const nicoco : Player = {
    name: "nicoco",
    age: 150
}

//////////////////////////////////

type Name = string
type Age = number
type Player = {
    name: Name,
    age?: Age
}

// 함수에 타입 선언 => argument에 타입 설정 & return value에 타입 설정
function playerMaker(name: string) : Player { // < : Player 부분
    return {
        name
    }
}

// 화살표 함수인 경우
const playMaker = (name: string) : Player => ({name})

const test = playerMaker("LEE")
test.age = 12
```



### #2.3 Types of TS part Two

readonly - 읽기 전용,, 최초 선언 후 수정 불가. immutability 부여\
\=> TS에서는 보호받지만 JS에서는 적용 안됨

```typescript
type Age = number;
type Name = string;
type Player = {
    readonly name: Name // <= 읽기 전용
    age?: Age
}

const playerMaker = (name:string) : Player => ({name})
const nic = playerMaker("nic")
nic.age = 12
nic.name = "name" // error

const numbers : readonly number[] = [1,2,3,4]
numbers.push(1) // error
```



Tuple - 정해진 개수와 순서의 요소를 가져야할 때 사용, readonly 사용 가능

```typescript
const player1: [string, number, boolean] = [] // error
const player2: [string, number, boolean] = ["nic", 1, true]
const player3: readonly [string, number, boolean] = ["nic", 1, true]
player3[0] = "aa" // error
```



undefined, null, any 타입

```typescript
let a : undefined = undefined // 선언X 할당O
let b : null = null // 선언O 할당ㅌ
let c : any = any // TS에서 빠져나오고 싶을때 사용, 가능한 사용하지 말자
```



### #2.4 Types of TS part Three

TS에만 있는 타입들

unknown - 어떤 타입인지 모르는 데이터

```typescript
let a : unknown;

let b = a + 1; // error => a가 number인지 모르기 때문

////

if(typeof a === 'number'){ // a 의 타입이 number 일 때
    let b = a + 1
}

if(typeof a === 'string'){ // a 의 타입이 string 일 때
    let b = a.toUpperCase()
}
```



void - 아무것도 return 하지 않는 함수. 지정하지 않아도 알아서 적용됨

```typescript
function hello() {console.log('x')}

const a = hello()
a.toUpperCase() // error toUpperCase 가 없음
```



never\
ㄴ 함수가 절대 return하지 않을때 - 함수의 exception이 발생할때

```typescript
function hello(): never {
    throw new Error("xxx")
}

function bye(name: string|number){
    if(typeof name === "string"){
        name // string
    }else if (typeof name === "number"){
        name // number
    }else{
        name // never
    }
}
```

