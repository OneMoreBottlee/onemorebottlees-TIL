# #3 Functions

### #3.0 Call Signatures

```typescript
type Add = (a:number, b:number) => number

const add: Add = (a, b) => a + b
```



### #3.1 Overloading

Function Overloading 은 외부 라이브러리에 자주 보이는 형태로, 하나의 함수가 여러 개의 Call Signature를 가질 때 발생한다.

```typescript
type Add = {
    (a: number, b: number) : number,
    (a: number, b: string) : number
}

const add : Add = (a, b) => a + b // err => b가 string일 경우 + 가 안됨

const add : Add = (a, b) => {
    if(typeof a === "string") return a
    return a + b
}
```

여러 개의 argument 인 경우, 모든 변수를 안쓸 수 도 있다.

```typescript
type Add = {
    (a: number, b: number) : number,
    (a: number, b: number, c: number) : number
}

const add : Add = (a, b, c) => { // err => c
    return a + b
}

const add : Add = (a, b, c?: number) => { // c => optional
    if(c) return a + b + c
    return a + b
}

```



### #3.2 Polymorphism

다형성 - 여러 타입을 받아들임으로써 여러 형태를 가지는 것

```typescript
type SuperPrint = {
    <T>(arr: T[]) => T // <---> : Generic
}

const superPrint: SuperPrint = (arr) => arr[0]

superPrint([1,2,3,4])
superPrint([true, false, true])
superPrint([1, 2, false, true, "hello"])
// 여러 타입이 포함되어도 TS가 각 타입을 추론
```

Concrete Type - number, string, void ...

[Generic](https://www.typescriptlang.org/docs/handbook/2/generics.html#handbook-content) - 타입의 placeholder



### #3.3 Generics Recap

\=> Generic ... any와 비슷한데?!

\=> no ! any 사용은 타입 안정성을 포기할 뿐 아니라 출력값의 타입을 보호할 수 없음



요청한 제네릭 순서에 따라 매개변수에 알아서 적용함

```typescript
type SuperPrint = <T, M>(a: T[], b: M) => T

const superPrint: SuperPrint = (arr) => arr[0]

superPrint([1,2,3,4], "x")
superPrint([true, false, true], 1)
superPrint([1, 2, false, true, "hello"], false)
```



### #3.4 Conclusions

```typescript
// 위와 같음
function superPrint <T> (a: T[]) {
    return a[0]
}
```



제너릭은 커스텀 및 재사용이 가능함

```typescript
type Player<E> = {
    name: string
    extraInfo: E
}

type NicExtra = {
    favFood: string
}

type NicPlayer = Player<NicExtra>

const nic : NicPlayer = {
    name: "nico",
    extraInfo: {
        favFood: "Kimchi"
    }
}

const kim: Player<null> = {
    name: "Kim",
    extraInfo: null
}
```



제너릭은 다양한 곳에서 사용 가능함

```typescript
useState<number>()
```



제너릭은 선언 시점이 아닌, 생성 시점에 타입을 명시하여 하나의 타입이 아닌 다양한 타입을 사용할 수 있도록 하는 기법이다.

