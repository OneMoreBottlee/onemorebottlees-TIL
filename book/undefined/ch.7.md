# Ch.7 인터페이스

인터페이스는 객체의 형태를 설명하는 방법이다. 별칭과 유사하지만 더 읽기 쉬운 오류 메시지, 더 빠른 컴파일 성능, 클래스와의 더 나은 상호 운용성을 위해 선호된다.



### 7.1 타입 별칭 vs. 인터페이스

```typescript
type Person = {
    born: number;
    name: string;
}

interface Person2 {
    born: number;
    name: string;
}
```

인터페이스와 타입 별칭은 유사하지만 몇 가지 차이점이 있다.

* 인터페이스는 속성 증가를 위해 병합이 가능하다.
* 인터페이스는 클래스가 선언된 구조의 타입을 확인하는데 사용 가능하다.
* 인터페이스에서 타입 검사기가 더 빨리 작동한다.
* 인터페이스는 객체로 간주되어 오류 메시지를 더 쉽게 읽을 수 있다.



### 7.2 속성 타입

#### 7.2.1 선택적 속성

인터페이스에서도 객체 타입과 마찬가지로 모든 객체가 필수적으로 인터페이스 속성을 가질 필요는 없다. 선택적 속성을 설정할 수 있다.

```typescript
interface Person {
    age?: number;
    name: string;
}

const person1: Person = {
    name: "h"
}

// Property 'name' is missing in type '{ age: number; }' but required in type 'Person'.(2741)
const person2: Person = {
    age: 303
}
```



#### 7.2.2 읽기 전용 속성

속성 이름 앞에 readonly 키워드를 추가해 읽기 전용 속성을 설정할 수 있다. readonly 제한자라 부르며 타입 시스템에만 존재하고, 인터페이스에서만 사용 가능하다.

```typescript
interface Person {
    name: string;
    readonly age: number;
}

let person: Person = {
    name: "h",
    age: 500
}

person.name = "han"
person.age = 300 // Cannot assign to 'age' because it is a read-only property.(2540)
```

readonly는 객체가 의도치 않게 수정하는 것을 막기 위한 방법이다. 타입 시스템의 구성 요소일 뿐이라 컴파일된 자바스크립트 코드에는 존재하지 않아 개발 중 속성의 수정을 막는 역할을 수행한다.



#### 7.2.3 함수와 메서드

타입스크립트는 인터페이스 멤버를 함수로 선언하는 두 가지 방법을 제공한다.

* 메서드 구문: member(): void와 같이 객체의 멤버로 호출되는 함수로 선언
* 속성 구문: member: () => void와 같이 독립 함수와 동일하게 선언

```typescript
interface HasBothFunctionTypes {
    property: () => string;
    method(): string;
}

const hasBoth: HasBothFunctionTypes = {
    property: () => "",
    method(){
        return ""
    }
}

hasBoth.property()
hasBoth.method()
```

두 방법 모두 선택적 속성 키워드 ?를 사용 가능하고 대부분 서로 교환하여 사용 가능하다.

주요 차이점은 다음과 같다.

* 메서드는 readonly로 선언 불가능. 속성은 가능
* 인터페이스 병합 처리가 다름
* 타입에서 수행되는 일부 작업이 다르게 처리됨



#### 7.2.4 호출 시그니처

인터페이스와 객체 타입은 호출 시그니처로 선언할 수 있다. 호출 시그니처는 값을 함수처럼 호출하는 방식에 대한 타입 시스템의 설명이다. 호출 시그니처가 선언한 방식으로 호출되는 값만 인터페이스에 할당 가능하다. 즉, 할당 가능한 매개변수와 반환 타입을 가진 함수이다. 호출 시그니처는 함수 타입과 비슷하지만, 콜론 대신 화살표로 표시한다.

```typescript
type FunctionAlias = (input: string) => number;

interface CallSignature {
    (input: string): number;
}

const typedFunctionAlias: FunctionAlias = (input) => input.length
const typedCallSignature: CallSignature = (input) => input.length
```



#### 7.2.5 인덱스 시그니처

타입스크립트는 인덱스 시그니처 구문을 제공해 인터페이스 객체가 임의의 키를 받고, 해당 키 아래 특정 타입을 반환할 수 있음을 나타낸다.

```typescript
interface WordCounts {
    [i: string]: number;
}

const counts: WordCounts = {}

counts.apple = 0
counts.banana = 1

counts.cherry = true
// Type 'boolean' is not assignable to type 'number'.(2322)


counts.str = "hi"
// Type 'string' is not assignable to type 'number'.(2322)
```

인덱스 시그니처는 객체에 값을 할당할 때 편리하지만 타입 안정성을 완벽하게 보장하지는 않는다. 객체가 어떤 속성에 접근하든 간에 값을 반환해야한다.



**속성과 인덱스 시그니처 혼합**

인터페이스는 명시적으로 명명된 속성과 포괄적 용도의 string 인덱스 시그니처를 한번에 포함할 수 있다. 각 속성의 타입은 포괄적 용도의 인덱스 시그니처로 할당 가능해야 한다. 명명된 속성이 더 구체적 타입을 제공하고, 다른 모든 속성은 인덱스 시그니처의 타입으로 대체하는 것으로 혼합해서 사용 가능하다.



\=> 인덱스 시그니처는 어떤 값이든 조건에 맞으면 키로 사용 가능하다. 따라서 단일 사용하지 말고 특정 키를 활용한 다른 속성과 혼합 사용하여 타입 안정성을 높이는 게 좋다&#x20;



**숫자 인덱스 시그니처**

때로는 객체의 키로 숫자만 허용하는 것이 좋을 수 있다. 타입스크립트 인덱스 시그니처는 키로 number 타입을 사용 가능하다. 이때 그 속성은 string 인덱스 시그니처 타입으로 할당 가능해야 한다. ?&#x20;



#### 7.2.6 중첩 인터페이스

객체 타입이 다른 객체 타입의 속성으로 중첩될 수 있는 것처럼 인터페이스 타입도 자체 인터페이스 타입 혹은 객체 타입을 속성으로 가질 수 있다.

```typescript
interface Novel {
    author: {
        name: string
    },
    setting: Setting
}

interface Setting {
    place: string;
    year: number
}

let myNovel: Novel

myNovel = {
    author: {
        name: "hong"
    },
    setting: {
        place: 'Seoul',
        year: 1993
    }
}
```



### 7.3 인터페이스 확장

타입스크립트는 확장된 인터페이스를 허용한다. 확장할 인터페이스의 이름 뒤에 extends 키워드를 추가해 확장한 인터페이스 임을 표시한다. 이렇게 하면 파생 인터페이스를 준수하는 모든 객체가 기본 인터페이스의 모든 멤버를 가져야 함을 타입스크립트에 알릴 수 있다.

```typescript
interface TypeScript {
    name: string
}

interface JavaScript extends TypeScript {
    age: number;
}

let test: JavaScript = {
    name: "hh",
    age: 15
}

// Property 'name' is missing in type '{ age: number; }' but required in type 'JavaScript'.(2741)
let test2: JavaScript = {
    age: 20
}
```

인터페이스 확장은 프로젝트의 한 엔티티 타입이 다른 엔티티의 모든 멤버를 포함하는 상위 집합을 나타내는 실용적인 방법이다. 이 덕분에 여러 인터페이스의 관계를 나타내기 위해 동일 코드를 반복 입력하는 수고를 덜 수 있다.



#### 7.3.1 재정의된 속성

파생 인터페이스는 다른 타입으로 속성을 다시 선언해 기본 인터페이스의 속성을 재정의 하거나 대체 가능하다. 타입스크립트의 타입 검사기는 재정의된 속성이 기본 속성에 할당되어 있도록 강요한다. 이렇게 인터페이스 타입의 인터페이스를 기본 인터페이스 타입에 할당 가능하다.

속성을 재선언하는 대부분의 파생 인터페이스는 해당 속성을 유니언 타입의 구체적 하위 집합으로 만들거나 속성을 기본 인터페이스의 타입에서 확장된 타입으로 만들기 위해 사용한다.

```typescript
interface Mother {
    name: string | undefined
}

// 부모 객체에서 상속받은 타입을 구체적으로 좁히는건 가능하다.
interface Son extends Mother {
    name: string
}

// 아예 새로운 타입으로 바꾸는건 불가능함
// Interface 'Daughter' incorrectly extends interface 'Mother'.
//   Types of property 'name' are incompatible.
//     Type 'number' is not assignable to type 'string'.(2430)
interface Daughter extends Mother {
    name: number
}

```



#### 7.3.2 다중 인터페이스 확장

타입스크립트의 인터페이스는 여러 개의 다른 인터페이스를 확장해서 선언 가능하다. 파생 인터페이스는 이름에 있는 extends 키워드 뒤에 쉼표로 인터페이스 이름을 구분해 사용하면 된다.

```typescript
interface Mother {
    mother: string | undefined
}

interface Father {
    father: number
}

interface Son extends Mother, Father {
    son: string
}


const son: Son = {
    mother: "abc",
    father: 55,
    son: "def"
}
```



### 7.4 인터페이스 병합

인터페이스의 중요 특징 중 하나는 서로 병합하는 능력이다. 두 인터페이스가 동일 이름으로 동일 스코프에 선언된 경우, 선언된 모든 필드를 포함하는 더 큰 인터페이스가 코드에 추가된다.

```typescript
interface Merged {
    first: string
}

interface Merged {
    second: number
}

const merge: Merged = {
    first: "abc",
    second: 15
}
```

일반적으로 인터페이스가 여러 곳에 선언되면 코드를 이해하기 어려워지기에 자주 사용되지 않는다.

그러나외부 패키지나 Window 같은 내장된 전역 인터페이스를 보강하는데 특히 유용하다.



#### 7.4.1 이름이 충돌되는 멤버

병합된 인터페이스는 타입이 다른 동일한 이름의 속성을 여러번 선언할 수 없다. 이미 속성이 선언되어 있다면 나중에 병합된 인터페이스에서도 동일한 타입을 사용해야 한다.

```typescript
interface Merged {
    name: string
}

interface Merged {
    name: number // Subsequent property declarations must have the same type.  Property 'name' must be of type 'string', but here has type 'number'.(2717)
}
```

하지만 메서드는 동일 이름이어도 가능하다. 이렇게 하면 메서드에 대한 함수 오버로드가 발생한다.

```typescript
interface Merged {
    diffent(input: string): string
}

interface Merged {
    diffent(input: number): string
}
```



### 7.5 마치며

* 타입 별칭 대신 인터페이스를 사용한 객체 타입 선언하기
* 선택적 속성, 읽기 전용 속성, 함수, 메서드 등 다양한 인터페이스의 속성 타입
* 객체의 포괄적인 속성을 담기 위한 인덱스 시그니처 사용하기
* 중첩된 인터페이스와 extends 상속 확장으로 인터페이스 재사용하기
* 동일 이름의 인터페이스 병합하기



\---

인덱스 시그니처 관련 정보 참고하기
