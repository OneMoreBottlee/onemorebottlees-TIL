# Ch.15 타입 운영

### 15.1 매핑된 타입

타입스크립트는 다른 타입의 속성을 기반으로 새로운 타입을 생성하는 구문을 제공한다. 즉, 하나의 타입에서 다른 타입으로 매핑한다. 타입스크립트의 매핑된 타입은 다른 타입을 가져와 해당 타입의 각 속성에 대해 일부 작업을 수행하는 타입이다.

매핑된 타입은 키 집합의 각 키에 대한 새로운 속성을 만들어 새로운 타입을 생성한다. 매핑된 타입은 인덱스 시그니처와 유사한 구문을 사용하지만 \[K in OriginalType]과 같이 in을 사용해 다른 타입으로부터 계산된 타입을 사용한다.

```typescript
type NewType = {
    [K in OriginalType]: NewProperty;
}
```

매핑된 타입에 대한 일반적인 사용 사례는 유니언 타입에 존재하는 각 문자열 리터럴 키를 가진 객체를 생성하는 것이다.

```typescript
type Animals = "alligator" | "baboon" | "cat"

type AnimalCounts = {
    [K in Animals]: number;
}

// type AnimalCounts = {
//     alligator: number;
//     baboon: number;
//     cat: number;
// }
```

존재하는 유니언 리터럴을 기반으로 한 매핑된 타입은 인터페이스 선언 공간을 절약하는 편리한 방법이다. 매핑된 타입은 다른 타입에 대해 작동하고 멤버에서 제한자를 추가하거나 제거할 수 있을 때 특히 유용해진다.



#### 15.1.1 타입에서 매핑된 타입

일반적으로 매핑된 타입은 존재하는 타입에 keyof 연산자를 사용해 키를 가져오는 방식으로 작동한다. 존재하는 타입의 키를 매핑하도록 타입에 지시하면 새로운 타입으로 매핑한다.

```typescript
interface AnimalVariant {
    alligator: boolean;
    baboon: number;
    cat: string;
}

type AnimalCounts = {
    [K in keyof AnimalVariant]: number;
}

// type AnimalCounts = {
//     alligator: number;
//     baboon: number;
//     cat: number;
// }
```

매핑된 타입의 멤버 값은 동일한 키 아래에서 원래 타입의 해당 멤버 값을 참조할 수 있다.

```typescript
interface AnimalVariant {
    alligator: boolean;
    baboon: number;
    cat: string;
}

type AnimalCounts = {
    [K in keyof AnimalVariant]: AnimalVariant[K] | number;
}

// type AnimalCounts = {
//     alligator: number | boolean;
//     baboon: number;
//     cat: string | number;
// }
```

매핑된 타입은 멤버 집합을 한 번 정의하고 필요한 만큼 여러 번 새로운 버전을 다시 생성할 수 있다.



**매핑된 타입과 시그니처**

매핑된 타입은 객체 타입의 메서드와 프로퍼티 구문을 구분하지 않는다. 매핑된 타입은 메서드를 원래 타입의 프로퍼티로 취급한다.

```typescript
interface Researcher {
    researchMethod(): void;
    researchProperty: () => string;
}

type JustProperties<T> = {
    [K in keyof T]: T[K]
}

type ResearcherProperties = JustProperties<Researcher>

// type ResearcherProperties = {
//     researchMethod: () => void;
//     researchProperty: () => string;
// }
```



#### 15.1.2 제한자 변경

매핑된 타입은 접근 제어 제한자인 readonly와 ?도 변경 가능하다. 인터페이스와 동일한 구문을 사용해 매핑된 타입의 멤버에 readonly와 ?를 배치할 수 있다.

```typescript
interface Player {
    KimMJ: string;
    LeeKI: string;
}

type ReadonlyPlayer = {
    readonly [K in keyof Player]: Player[K];
}

// type ReadonlyPlayer = {
//     readonly KimMJ: string;
//     readonly LeeKI: string;
// }

type OptionalReadonlyPlayer = {
    [K in keyof ReadonlyPlayer]? : ReadonlyPlayer[K]
}

// type OptionalReadonlyPlayer = {
//     readonly KimMJ?: string | undefined;
//     readonly LeeKI?: string | undefined;
// }

// OptionalReadonlyPlayer는
// readonly [K in keyof Player]? : Player[K]로 대체 가능
```



새로운 타입의 제한자 앞에 -를 추가해 제한자를 제거할 수도 있다.

```typescript
interface Player {
    KimMJ?: string;
    readonly LeeKI: string;
}

type ReadonlyPlayer = {
    -readonly [K in keyof Player]: Player[K];
}

// type ReadonlyPlayer = {
//     KimMJ?: string | undefined;
//     LeeKI: string;
// }

type OptionalReadonlyPlayer = {
    [K in keyof ReadonlyPlayer]-? : ReadonlyPlayer[K]
}

// type OptionalReadonlyPlayer = {
//     KimMJ: string;
//     LeeKI: string;
// }

// OptionalReadonlyPlayer는
// -readonly [K in keyof Player]-? : Player[K]로 대체 가능하다
```



#### 15.1.3 제네릭 매핑된 타입

매핑된 타입은 제네릭과 결합해 단일 타입의 매핑을 다른 타입에 재사용할 수 있을때 완전한 힘을 발휘한다. 매핑된 타입은 매핑된 타입 자체의 타입 매개변수를 포함해 keyof로 해당 스코프에 있는 모든 타입 이름에 접근 가능하다.

제네릭 매핑된 타입은 데이터가 애플리케이션을 통해 흐를 때, 데이터의 변형을 나타내는데 유용하다. 예를 들어 어플리케이션 영역에서 기존 타입의 값을 가져올 수 있지만 데이터를 수정하는 것은 허용하지 않는 것이 좋다.... ?

P.341 -342 예시보기



### 15.2 조건부 타입

타입스크립트의 타입 시스템은 논리 프로그래밍 언어의 하나이다. 타입 시스템은 이전 타입에 대한 논리적 검사를 바탕으로 새로운 구성을 생성한다. 조건부 타입의 개념은 기존 타입을 바탕으로 두 가지 가능한 타입 중 하나로 확인되는 타입이다. 삼항 연산자 조건문처럼 보인다.

조건부 타입에서 논리적 검사는 항상 extends의 왼쪽 타입이 오른쪽 타입이 되는지 또는 할당 가능한지 여부에 있다.

```typescript
type CheckStrAgainstNum = string extends number ? true : false
// type CheckStrAgainstNum = false
```



#### 15.2.1 제네릭 조건부 타입

조건부 타입은 타입 매개변수를 포함한 해당 스코프에서 모든 타입 이름을 확인 가능하다. 즉, 다른 모든 타입을 기반으로 새로운 타입을 생성하기 위해 재사용 가능한 제네릭 타입을 작성할 수 있다.

```typescript
type CheckAgainstNumber<T> = T extends number ? true: false;

type CheckString = CheckAgainstNumber<"string">
// type CheckString = false

type CheckString = CheckAgainstNumber<3>
// type CheckNumber = true

type CheckString = CheckAgainstNumber<number>
// type CheckNumber = true
```



```typescript
type CallableSetting<T> =
    T extends () => any
        ? T 
        : () => T

type GetNumberSetting = CallableSetting<() => number[]>
// type GetNumberSetting = () => number[]

type StringSetting = CallableSetting<string>
// type StringSetting = () => string
```

조건부 타입은 객체 멤버 검색 구문을 사용해 제공된 타입의 멤버에 접근 가능하고, extends 절과 결과 타입에서 그 정보를 사용할 수 있다.

자바스크립트 라이브러리에서 사용하는 패턴 중 조건부 제네릭 타입에도 적합한 한 가지 패턴은 함수에 제공된 옵션 객체를 기반으로 함수 반환 타입을 변경하는 것이다.

예를 들어, 함수가 값을 찾을 수 없는 경우 undefined를 반환하는 대신 새로운 프로퍼티를 사용해 함수가 오류를 발생시키도록 변경할 수 있다.

조건부 타입을 제네릭 타입 매개변수와 결합하면 프로그램의 제어 흐름을 어떻게 변경할 것인지 타입 시스템에 더 정확히 알릴 수 있다.



#### 15.2.2 타입 분산

조건부 타입은 유니언에 분산된다. 결과 타입은 각 구성 요소에 조건부 타입을 적용하는 유니언이 됨을 의미한다. 즉, ConditionalType\<T | U>는 Condition\<T> | Condition\<U>와 같다.

```typescript
type ArrayifyUnlessString<T> = T extends string ? T : T[]

type HalfArrayified = ArrayifyUnlessString<string | number>
// type HalfArrayified = string | number[]
```

즉, 조건부 타입은 전체 유니언 타입이 아니라 유니언 타입의 각 구성 요소에 로직을 적용한다.



#### 15.2.3 유추된 타입

조건부 타입은 extends 절에 infer 키워드를 사용해 조건의 임의 부분에 접근한다. extends 절의 타입에 대한 infer 키워드와 새 이름을 배치하면, 조건부 타입이 true인 경우 새로운 타입을 사용 가능함을 의미한다.

```typescript
type ArrayItems<T> = 
    T extends (infer Item)[]
        ? Item
        : T;

type StringItem = ArrayItems<string>
// type StringItem = string

type StringArrayItem = ArrayItems<string[]>
// type StringArrayItem = string

type String2DItem = ArrayItems<string[][]>
// type StringArrayItem = string[]
```

유추된 타입은 재귀적 조건부 타입을 생성하는데에도 사용 가능하다.

```typescript
type ArrayItemsRecursive<T> = 
    T extends (infer Item)[]
        ? ArrayItemsRecursive<Item>
        : T;

type StringItem = ArrayItemsRecursive<string>
// type StringItem = string

type StringArrayItem = ArrayItemsRecursive<string[]>
// type StringArrayItem = string

type String2DItem = ArrayItemsRecursive<string[][]>
// type StringArrayItem = string
```

ArrayItems\<string\[]\[]>는 string\[]이 되지만 ArrayItemsRecursive\<string\[]\[]>는 string이 된다. 재귀적 기능이 가능한 제네릭 타입의 특징이다.



#### 15.2.4 매핑된 조건부 타입

매핑된 타입은 기존 타입의 모든 멤버에 변경 사항을 적용하고 조건부 타입은 하나의 기존 타입에 변경 사항을 적용한다. 이 둘을 함께 사용하면 제네릭 템플릿 타입의 각 멤버에 조건부 로직을 적용할 수 있다.

```typescript
type MakeAllMembersFunction<T> = {
    [K in keyof T]: T[K] extends (...args: any[]) => any
        ? T[K]
        : () => T[K]
}

type MemberFunctions = MakeAllMembersFunction<{
    alreadyFunction: () => string,
    notYetFunction: number,
}>

// type MemberFunctions = {
//     alreadyFunction: () => string;
//     notYetFunction: () => number;
// }
```

매핑된 조건부 타입은 일부 논리적 검사를 사용해 기존 타입의 모든 속성을 수정하는 편리한 방법이다.



### 15.3 never

올바른 위치에 never 타입 애너테이션을 추가하면 타입스크립트가 이전 런타임 코드 뿐 아니라 타입 시스템에서 맞지 않는 코드 경로를 더 공격적으로 탐지한다.



#### 15.3.1 never와 교차, 유니언 타입

bottom 타입인 never는 존재할 수 없는 타입이라는 의미를 갖고 있다. never가 교차 타입(&)과 유니언 타입(|)을 함께 사용하면 다음과 같이 작동한다.

* 교차 타입 & 에 있는 never는 교차 타입을 never로 만든다.
* 유니언 타입 | 에 있는 nevr는 무시된다.

```typescript
type NeverIntersection = never & string // never
type NeverUnion = never | string // string
```

특히 유니언에서 never가 무시되는 동작은 조건부 타입과 매핑된 타입에서 값을 필터링하는데 유용하다.



#### 15.3.2 never와 조건부 타입

제네릭 조건부 타입은 일반적으로 유니언에서 타입을 필터링하기 위해 never를 사용한다. 유니언에서 never는 무시되기에 유니언 타입에서 제네릭 조건부의 결과는 never가 아닌 것이 된다.

```typescript
type OnlyStrings<T> = T extends string ? T : never;

type RedOrBlue = OnlyStrings<"red" | "blue" | 0 | false | "white" | 3>
// type RedOrBlue = "red" | "blue" | "white"
// 조건에서 true인 string 타입만 남음
```

never는 제네릭 타입에 대한 타입 유틸리티를 만들 때 유추된 조건부 타입과 결합한다. infer가 있는 타입 추론은 조건부 타입이 true가 되어야 하므로 false인 경우를 절대 사용하지 않아야 한다. 이때 never를 사용하면 적합하다.

```typescript
type FirstParameter<T extends (...args: any[]) => any> =
    T extends (arg: infer Arg) => any
    ? Arg 
    : never;

type GetsString = FirstParameter<(arg0: string) => void>
// type GetsString = string
```



#### 15.3.3 never와 매핑된 타입

유니언에서 never의 동작은 매핑된 타입에서 멤버를 필터링할 때도 유용하다. 다음 세 가지 타입 시스템 기능을 사용해 객체의 키를 필터링한다.

* 유니언에서 never는 무시된다.
* 매핑된 타입은 타입의 멤버를 매핑할 수 있다.
* 조건부 타입은 조건이 충족되는 경우 타입을 never로 변환하는 데 사용 가능하다.

이 세 기능을 함께 사용하면 원래 타입의 각 멤버를 원래 키 또는 never로 변경하는 매핑된 타입을 만들 수 있다. \[keyof T]로 해당 타입의 멤버를 요청하면 모든 매핑된 타입의 결과 유니언이 생성되고 never는 필터링 된다.

```typescript
type OnlyStringProperties<T> = {
    [K in keyof T]: T[K] extends string ? K : never;
}[keyof T]

interface AllEventData {
    participants: string[];
    location: string;
    name: string;
    year: number;
}

type OnlyStringEventData = OnlyStringProperties<AllEventData>
// type OnlyStringEventData = "location" | "name"
// 조건에 의해 string이 아닌 타입은 never가 되어 무시된다.
```



### 15.4 템플릿 리터럴 타입

템플릿 리터럴 타입은 템플릿 리터럴 문자열처럼 보이지만 추정 가능한 원시 타입 또는 원시 타입 유니언이 있다.

```typescript
type Greeting = `Hello${string}`
// Hello로 시작하는 문자열이어야 한다.

let matches: Greeting = "Hello, world"
let fail1: Greeting = "hi, world"
// Type '"hi, world"' is not assignable to type '`Hello${string}`'.(2322)

let fail2: Greeting = "hello, world"
// Type '"hello, world"' is not assignable to type '`Hello${string}`'.(2322)
```

템플릿 리터럴 타입을 더 좁은 문자열 패턴으로 제한하기 위해 문자열 리터럴 타입과 그 유니언을 타입 보간법에 사용 가능하다. 템플릿 리터럴 타입은 제한된 허용 문자열 집합과 일치해야 하는 문자열을 설명하는데 매우 유용하다.

[참고](https://www.typescriptlang.org/docs/handbook/2/template-literal-types.html)

```typescript
type Brightness = "dark" | "light"
type Color = "blue" | "red"

type BrightnessAndColor = `${Brightness}-${Color}`
// type BrightnessAndColor = "dark-blue" | "dark-red" | "light-blue" | "light-red"
```

타입스크립트는 템플릿 리터럴 타입이 모든 [원시 타입](#user-content-fn-1)[^1] 또는 그 조합을 포함하도록 허용한다.



#### 15.4.1 고유 문자열 조작 타입

문자열 타입 작업 지원을 위해 타입스크립트는 고유 제네릭 유틸리티 타입을 제공한다.

* Uppercase
* Lowercase
* Capitalize
* Uncapitalize

각각은 문자열을 갖는 제네릭 타입으로 사용 가능하다.

```typescript
type FormalGreeting = Capitalize<"hello.">
// type FormalGreeting = "Hello."
```

이러한 고유 문자열 조작 타입은 객체 타입의 속성 키를 조작하는데 매우 유용하다.



#### 15.4.2 템플릿 리터럴 키

템플릿 리터럴 타입은 원시 문자열 타입과 문자열 리터럴 사이의 중간 지점이므로 여전히 문자열이다. 따라서 문자열 리터럴을 사용할 수 있는 모든 위치에서 사용 가능하다.

```typescript
type DataKey = "location" | "name" | "year"

type ExistenceChecks = {
    [K in `check${Capitalize<DataKey>}`]: () => boolean
}

// type ExistenceChecks = {
//     checkLocation: () => boolean;
//     checkName: () => boolean;
//     checkYear: () => boolean;
// }

function checkExistence(checks: ExistenceChecks){
    checks.checkLocation() // boolean
    checks.checkName() // boolean
    checks.checkWrong() // Error
}
```



#### 15.4.3 매핑된 타입 키 다시 매핑하기

타입스크립트는 템플릿 리터럴 타입을 사용해 원래 멤버를 기반으로 매핑된 타입의 멤버에 대한 새로운 키를 생성할 수 있다. 매핑된 타입에서 인덱스 시그니처에 대한 템플릿 리터럴 타입 다음에 as 키워드를 배치하면 결과 타입의 키는 템플릿 리터럴 타입과 일치하도록 변경된다. 이렇게 하면 매핑된 타입은 원래 값을 계속 참조하면서 각 매핑된 타입 속성에 대한 다른 키를 가질 수 있다.

```typescript
interface DataEntry<T> {
    key: T;
    value: string;
}

type DataKey = "location" | "name" | "year"

type DataEntryGetters = {
    [K in DataKey as `get${Capitalize<K>}`]: () => DataEntry<K>
}

// type DataEntryGetters = {
//     getLocation: () => DataEntry<"location">;
//     getName: () => DataEntry<"name">;
//     getYear: () => DataEntry<"year">;
// }
```

키를 다시 매핑하는 작업과 다른 타입 운영을 결합해 기존 타입 형태를 기반으로 하는 매핑된 타입을 생성할 수 있다.&#x20;

```typescript
const config = {
    location: "unknown",
    name: "anonymous",
    year: 0,
}

type LazyValues = {
    [K in keyof typeof config as `${K}Lazy`]: () => Promise<typeof config[K]>
}

// type LazyValues = {
//     locationLazy: () => Promise<string>;
//     nameLazy: () => Promise<string>;
//     yearLazy: () => Promise<number>;
// }

async function withLazyValues(configGetter: LazyValues){
    await configGetter.locationLazy // type string

    await configGetter.missingLazy() // Error
}
```

자바스크립트에서 객체의 키는 string 또는 Symbol이 될 수 있다. 하지만 Symbol 키는 원시 타입이 아니기에 템플릿 리터럴 타입으로 사용 불가능하다. 이러한 제한 사항을 피하기 위해 string과 교차 타입 & 을 사용해 문자열이 될 수 있는 타입만 사용하도록 강제할 수 있다. string & symbol은 never가 되므로 전체 템플릿 문자열은 never가 되고 무시된다.

```typescript
const someSymbol = Symbol("")

interface HasStringAndSymbol {
    StringKey: string;
    [someSymbol]: number
}

type TurnIntoGetters<T> = {
    [K in keyof T as `get${string & K}`]: () => T[K]
}

type GettersJustString = TurnIntoGetters<HasStringAndSymbol>
// type GettersJustString = {
//     getStringKey: () => string;
// }
// Symbol은 never가 되어 무시된다.
```



### 15.5 타입 운영과 복잡성

타입 운영은 오늘날 모든 프로그래밍 언어에서 가장 강력한 최첨단 타입 시스템 기능이다. 복잡한 타입 운영을 사용하는 대부분의 개발자는 오류의 디버그를 할 수 있을 만큼 익숙하지 않다. 따라서 최소한으로 사용하고, 다른 사람을 위해 이해하기 쉬운 이름을 남기고, 어려움을 느낄 수 있는 부분에 설명을 남겨야 한다.



### 15.6 마치며

* 기존 타입을 새로운 타입으로 변환하기 위해 매핑된 타입 사용하기
* 조건부 타입을 사용해 타입 운영에 로직 도입하기
* 교차, 유니언, 조건부 타입, 매핑된 타입과 never가 상호작용하는 방법 배우기
* 템플릿 리터럴 타입을 사용해 문자열 타입의 패턴 나타내기
* 타입 키를 수정하기 위해 템플릿 리터럴 타입과 매핑된 타입 결합하기



\----

굉장히 유용한 내용이다.

하지만 아직 이정도로 타입을 사용한 경험이 없어 언제 쓸지 감이 안잡힌다.

그때가 오면 여기를 다시 읽어보자 !

[^1]: string, number, bigint, boolean, null, undefined (symbol은 제외)
