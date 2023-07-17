# Ch.4 객체

### 4.1 객체 타입

{...} 구문을 사용해 객체 리터럴을 생성하면 타입스크립트는 해당 속성을 기반으로 새로운 객체 타입 또는 타입 형태를 고려한다. 해당 객체 타입은 객체 값과 동일한 속성명과 원시 타입을 갖는다.&#x20;

객체 타입은 타입스크립트가 자바스크립트 코드를 이해하는 핵심 개념이다. null 과 undefined를 제외한 모든 값은 그 값에 대한 실제 타입을 가지므로 타입 스크립트는 모든 값의 타입을 확인하기 위해 객체 타입을 이해해야 한다.



#### 4.1.1 객체 타입 선언

객체 타입은 객체 리터럴과 유사하게 보이지만 필드 값 대신 타입을 사용해 설명한다.

```typescript
// 타입을 선언하고
let me: {
    born: number;
    name: string;
}

// 값을 할당함
me = {
    born: 1995,
    name: "Han"
}

// 타입오류 Type 'string' is not assignable to type {born: number; name: string}
me = "kim"
```



#### 4.1.2 별칭 객체 타입

매번 객체 타입을 설정하는 것은 귀찮은 일이다. 따라서 일반적으로는 타입 별칭을 할당해 사용한다.

```typescript
type Me = {
    born: number;
    name: string;
}

let me: Me

me = {
    born: 1995,
    name: "Han"
}

// 지정된 타입과 달라 에러가 발생한다.
me = "kim"
```



### 4.2 구조적 타이핑

타입스크립트의 타입 시스템은 구조적으로 타입화 (structurally typed) 되어 있다. 즉, 타입을 충족하는 모든 값을 해당 타입의 값으로 사용 가능하다.

```typescript
type FirstName = {
    firstName: string;
}

type LastName = {
    lastName: string;
}

const fullName = {
    firstName: "Lucille",
    lastName: "Clifton"
}

let withFirstName: FirstName = fullName
let withLastName: LastName = fullName
```

구조적 타이핑은 덕 타이핑과 다르다.

* 구조적 타이핑은 타입스크립트의 타입 검사기에서 정적 시스템이 타입을 검사하는 경우이다.
* 덕 타이핑은 런타임에서 사용될 때까지 객체 타입을 검사하지 않는 것을 말한다.

즉, JS는 덕 타입인 반면, TS는 구조적 타입이다.



#### 4.2.1 사용 검사

객체 타입으로 애너테이션된 위치에 값을 제공할 때, 타입스크립트는 해당 객체 타입에 값을 할당할 수 있는지 확인한다. 할당하는 값에는 객체 타입의 필수 속성이 있어야 한다. 객체 타입에 필요한 값이 객체에 없다면 타입스크립트는 타입 오류를 발생시킨다.

```typescript
type Name = {
    first: string;
    last: string;
}

const name: Name = {
    first: "Sarojini",
    last: "Naidu"
}

// Error: Property 'last' is missing in type '{ first: string; }' but required in type 'Name'.(2741)
const halfName: Name = {
    first: "Sarojini"
}
// Name 타입의 필수 요소인 last가 포함되지 않아 에러가 발생함

// Error: Type 'number' is not assignable to type 'string'.
const wrongName: Name = {
    first: "Sarojini",
    last: 15
}
// last의 타입은 string이어야 한다.
```



#### 4.2.2 초과 속성 검사

변수가 객체 타입으로 선언되고, 초깃값에 객체 타입에서 정의된 것보다 많은 필드가 있다면 타입스크립트에서 타입 오류가 발생한다. 따라서 변수를 객체 타입으로 선언하는 것은 타입 검사기가 해당 타입에 예상되는 필드만 있는지 확인하는 방법이다.

```typescript
type Person = {
    born: number;
    name: string;
}

const he: Person = {
    born: 1995,
    name: "kim"
}

// Error : Type '{ born: number; name: string; height: number; }' is not assignable to type 'Person'.
// height는 Person 에 적히지 않은 초과된 객체 프로퍼티다.
const her: Person = {
    born: 1996,
    name: "lee",
    height: 167
}
```

초과 속성 검사는 객체 타입 선언 이후 생성되는 객체 리터럴에 대해서만 발생한다. 생성되어 있는 객체를 제공하면 초과 속성 검사를 우회한다.

```typescript
type Person = {
    born: number;
    name: string;
}

const man = {
    born: 1997,
    name: "park",
    height: 175
}

// 초깃값이 구조적으로 Person과 일치하기에 타입 오류가 발생하지 않는다.
const newMan: Person = man
```



#### 4.2.3 중첩된 객체 타입

자바스크립트 객체는 다른 객체에 중첩될 수 있다. 따라서 타입스크립트의 객체 타입도 중첩된 객체 타입을 나타낼 수 있어야 한다.

```typescript
type Person = {
    name: {
        firstName: string;
        lastName: string;
    }
    age: number
}

const kim: Person = {
    name: {
        firstName: "MJ",
        lastName: "Kim"
    },
    age: 26
}

// Error: Type '{ name: string; }' is not assignable to type '{ firstName: string; lastName: string; }'.
// name 객체에는 firstName과 lastName이 포함되어야 하기에 에러가발생함
const lee: Person = {
    name: {
        name: "KI Lee"
    },
    age: 22
}

// 타입 별칭을 사용해 객체 중첩을 보기 좋게 바꿀 수 있다.
type Name = {
    firstName: string;
    lastName: string
}

type Person = {
    name: Name,
    age: number
}
```



#### 4.2.4 선택적 속성

모든 객체에 객체 타입속성이 필요한 것은 아니다. 선택적인 타입을 지정할 수 있다.

```typescript
type Name = {
    firstName: string;
    lastName: string
}

type Person = {
    name: Name,
    age?: number
}

// age를 선택적 속성으로 변경해 더이상 필수 요소가 아니다.
// age가 없어도 에러가 발생하지 않음
const lee: Person = {
    name: {
        firstName: "KI",
        lastName: "Lee"
    }
}

// Error: Property 'name' is missing in type '{ age: number; }' but required in type 'Person'.(2741)
// 필수 요소인 name이 없기에 에러가 발생함
const kim: Person = {
    age: 26
}
```



### 4.3 객체 타입 유니언

#### 4.3.1 유추된 객체 타입 유니언

변수에 여러 타입의 가능성이 있는 초깃값이 주어지면 타입스크립트는 해당 타입을 객체 타입 유니언으로 유추한다. 유니언 타입은 객체 타입을 구성하고 있는 요소의 타입을 모두 가질 수 있다.

```typescript
const player = Math.random() > 0.5
    ? { name: "kim", age: 26 }
    : { name: "lee", team: "paris" }
    
/** player의 타입은 다음과 같다.
const player: {
    name: string;
    age: number;
    team?: undefined;
} | {
    name: string;
    team: string;
    age?: undefined;
}
*/
```



#### 4.3.2 명시된 객체 타입 유니언

객체 타입의 조합을 명시하면 객체 타입을 더 명확히 정의할 수 있다.

```typescript
typescript
type Kim = { name: string, age: number }
type Lee = { name: string, team: string }
type Player = Kim | Lee

const player: Player = Math.random() > 0.5
    ? { name: "kim", age: 26 }
    : { name: "lee", team: "paris" }

player.name
player.age // Error: Property 'age' does not exist on type 'Player'. Lee 타입인 경우...
player.team // Error: Property 'team' does not exist on type 'Player'. Kim 타입인 경우...
```



#### 4.3.3 객체 타입 내로잉

객체의 형태를 확인하고 타입 내로잉을 적용할 수 있다.&#x20;

```typescript
type Kim = { name: string, age: number }
type Lee = { name: string, team: string }
type Player = Kim | Lee

const player: Player = Math.random() > 0.5
    ? { name: "kim", age: 26 }
    : { name: "lee", team: "paris" }

player.name

if("age" in player){
    player.age
    player.team // Property 'team' does not exist on type 'Kim'.(2339)
}else{
    player.team
    player.age // Property 'age' does not exist on type 'Lee'.(2339)
}

// if(player.age)... 와 같은 방법은 허용하지 않는다.
```



#### 4.3.4 판별된 유니언

자바스크립트와 타입스크립트에서 유니언 타입으로 된 객체의 또 다른 형태는 객체의 속성이 객체의 형태를 나타내도록 하는 것이다. 이러한 타입 형태를 판별된 유니온 discriminated union 이라 부르고, 객체의 타입을 가리키는 속성이 판별값이다. 타입스크립트는 코드에서 판별 속성을 사용해 타입 내로잉을 수행한다.

```typescript
type Kim = { name: string, age: number, type: 'kim' }
type Lee = { name: string, team: string, type: 'lee' }
type Player = Kim | Lee

const player: Player = Math.random() > 0.5
    ? { name: "kim", age: 26, type: 'kim' }
    : { name: "lee", team: "paris", type: 'lee' }

player.name

if(player.type === "kim"){
    player.age
    player.team // Property 'team' does not exist on type 'Kim'.(2339)
}else{
    player.team
    player.age // Property 'age' does not exist on type 'Lee'.(2339)
}
```

판별된 유니언은 JS 패턴과 TS 타입 내로잉을 결합한 기능이다.



### 4.4 교차 타입

타입스크립트 유니언 타입은 둘 이상의 서로 다른 타입 중 하나의 타입이 될 수 있음을 나타낸다. 자바스크립트의 런타임 | 연산자가 & 연산자에 대응하는 역할을 하는 것처럼, 타입스크립트에서도 & 교차 타입을 사용해 여러 타입을 동시에 나타낸다. 교차 타입은 일반적으로 여러 기존 객체 타입을 별칭 객체 타입으로 결합해 새로운 타입을 생성한다.

```typescript
type Kim = { name: string, age: number }
type Lee = { name: string, team: string }
type Player = Kim & Lee

// 타입 Player는 다음과 같다.
// {
//     name: string,
//     age: number,
//     team: string
// }

const player: Player = {
    name: "son",
    age: 30,
    team: "tot"
}
```



#### 4.4.1 교차 타입의 위험성

교차 타입은 유용하지만, 읽기 복잡하다. 따라서 교차 타입을 사용할 때, 최대한 간결하게 코드를 유지해야 한다.



**긴 할당 가능성 오류**

복잡한 교차 타입을 만들게 되면 할당 가능성 오류 메시지는 읽기 어려워진다. 별칭으로 된 객체 타입으로 분할하면 읽기 쉬워진다.

```typescript
type Name = { firstName: string, lastName: string }
type Team = { team: string }
type Records = { persnal: { goal: number, assist: number } }
type Player = Name & Team & Records

const player: Player = {
    firstName: "HM",
    lastName: "Son",
    team: "tot",
    persnal: {
        goal: 30,
        assist: 30
    }
}

```



**never**

교차 타입은 잘못 사용하기 쉽고, 불가능한 타입을 생성한다. 원시 타입의 값은 동시에 여러 타입이 될 수 없기에 교차 타입의 구성 요소로 함께 결합할 수 없다. 두 개의 원시 타입을 함께 시도하면 never 키워드로 표시되는 never 타입이 된다.

```typescript
type Impossible = number & string

let isNumber: Impossible = 3
// Error : Type 'number' is not assignable to type 'never'.(2322)

let isString: Impossible = "lee"
// Error : Type 'string' is not assignable to type 'never'.(2322)
```

never는 거의 사용하지 않고, 교차 타입을 잘못 사용해 발생한 실수일 가능성이 높지만, 코드에서 불가능한 상태를 나타내기 위해 가끔 등장한다.



### 4.5 마치며

* 타입스크립트가 객체 타입 리터럴의 타입을 해석하는 방법
* 중첩과 선택적 속성을 포함한 객체 리터럴 타입 소개
* 객체 리터럴 타입의 유니언 타입 선언, 추론 및 타입 내로잉
* 판별된 유니언 타입과 판별값
* 교차 타입으로 객체 타입을 결합하는 방법



\---

230717 여전히 해석하기 어려운 번역...

