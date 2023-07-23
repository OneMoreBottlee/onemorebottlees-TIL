# Ch.9 타입 제한자

### 9.1 top 타입

top 타입은 시스템에서 가능한 모든 값을 나타내는 타입이다. 모든 타입은 top 타입에 할당할 수 있다.

#### 9.1.1 any 다시 보기

any 타입은 모든 타입의 위치에 제공될 수 있다는 점에서 top 타입처럼 작동할 수 있다.

다만 any는 타입스크립트가 해당 값에 대한 할당 가능성 또는 멤버에 대해 타입 검사를 수행하지 않도록 명시적으로 지시하기에 타입스크립트의 사용 의도와 거리가 있다. 어떤 값이든 될 수 있음을 나타내려면 unknown 타입이 훨씬 안전하다.



#### 9.1.2 unknown

타입스크립트에서 unknown 타입은 진정한 top 타입이다. 모든 객체를 unknown 타입의 위치로 전달할 수 있다는 점에서 any 타입과 유사하다. 주요 차이점으로는 타입스크립트는 unknown 타입의 값을 훨씬 더 제한적으로 취급한다는 점이다.

* 타입스크립트는 unknown 타입 값의 속성에 직접 접근할 수 없다.
* unknown 타입은 top 타입이 아닌 타입에는 할당할 수 없다.

```typescript
function greetComedianSafety(name: unknown){
    if(typeof name === "string"){
        console.log(`${name.toUpperCase()}`)
    }else{
        console.log("...")
    }
}
```



### 9.2 타입 서술어

instanceof, typeof와 같은 자바스크립트 구문으로 타입을 좁히는 방법은 로직을 함수로 감싸면 타입을 좁힐 수 없게된다.

타입스크립트에는 인수가 특정 타입인지 여부를 나타내기 위해 boolean 값을 반환하는 함수를 위한 특별 구문이 있다. 이를 타입 서술어 type predicate 라고 부르며 '사용자 정의 타입 가드' user-defined type guard 라고도 부른다.

타입 서술어의 반환 타입은 매개변수의 이름, is 키워드, 특정 타입으로 선언할 수 있다.

```typescript
function typePredicate(input: WideType): input is NarrowType;
```

타입 서술어는 이미 한 인터페이스의 인스턴스로 알려진 객체가 더 구체적인 인터페이스의 인스턴스인지 여부를 검사하는데 자주 사용된다.

```typescript
interface Comedian {
    funny: boolean
}

interface StandupComedian extends Comedian {
    routine: string;
}

function isStandupComedian (value: Comedian): value is StandupComedian {
    return 'routine' in value
}

function workWithComedian(value: Comedian){
    if(isStandupComedian(value)){
        console.log(value.routine)
    }

    console.log(value.routine)
    // Property 'routine' does not exist on type 'Comedian'.(2339)
}
```

타입 서술어는 속성이나 값의 타입을 확인하는 것 이상을 수행해 잘못 사용하기 쉬우므로 가능하면 피하는 것이 좋다. 대부분은 간단한 타입 서술어만으로 충분하다.



### 9.3 타입 연산자

키워드나 기존 타입의 이름만 사용해 모든 타입을 나타낼 수는 없다. 때로는 기존 타입의 속성 일부를 변환해 두 타입을 결합하는 새로운 타입을 생성해야 할 때도 있다.



#### 9.3.1 keyof

자바스크립트 객체는 일반적으로 string 타입인 동적값을 키로 사용해 값을 찾는다. 타입 시스템에서 이러한 키를 표현하려면 상당히 까다로울 수 있다. string 같은 포괄적인 원시 타입을 사용하면 유효하지 않은 키가 허용될 수 있기 때문이다.

```typescript
interface Ratings{
    audience: number;
    critics: number;
}

function getRatings(ratings: Ratings, key: string): number {
    return ratings[key]
    //   Element implicitly has an 'any' type because expression of type 'string' can't be used to index type 'Ratings'.
    //   No index signature with a parameter of type 'string' was found on type 'Ratings'.(7053)
}

const ratings: Ratings = { audience: 66, critics: 84 }

getRatings(ratings, 'audience')
getRatings(ratings, 'not valid') // 허용되면 안되지만 허용됨

```



또 다른 옵션은 리터럴 유니언 타입을 사용하는 것이다. 이 경우 컨테이너에 존재하는 키만 적절하게 제한한다.

```typescript
interface Ratings{
    audience: number;
    critics: number;
}

function getRatings(ratings: Ratings, key: 'audience' | 'critics'): number {
    return ratings[key]
}

const ratings: Ratings = { audience: 66, critics: 84 }

getRatings(ratings, 'audience')
getRatings(ratings, 'not valid') // Argument of type '"not valid"' is not assignable to parameter of type '"audience" | "critics"'.(2345)
```

하지만 객체에 수십개 이상의 프로퍼티가 있다면 일일이 적기엔 상당히 번거롭다.



keyof 연산자를 사용해 기존 타입을 사용하고, 해당 타입에 허용되는 모든 키 조합을 반환할 수 있다.

```typescript
interface Ratings{
    audience: number;
    critics: number;
}

function getRatings(ratings: Ratings, key: keyof Ratings): number {
    return ratings[key]
}

const ratings: Ratings = { audience: 66, critics: 84 }

getRatings(ratings, 'audience')
getRatings(ratings, 'not valid') // Argument of type '"not valid"' is not assignable to parameter of type 'keyof Ratings'.(2345)
```

keyof는 존재하는 타입의 키를 바탕으로 유니언 타입을 생성하는 훌륭한 기능이다. 다른 타입 연산자와 결합되어 유용한 패턴을 만들 수 있다.



#### 9.3.2 typeof

타입 연산자 typeof는 제공되는 값의 타입을 반환한다.

```typescript
const original = {
    medium: "movie",
    title: "Mean Girls"
}

let adaptation: typeof original

if (Math.random() > 0.5){
    adaptation = {...original, medium: "play"}
}else{
    adaptation = {...original, medium: 2} // Type 'number' is not assignable to type 'string'.(2322)
}
```

typeof 타입 연산자는 자바스크립트의 typeof 연산자처럼 보이지만 둘은 차이가 있다.

자바스크립트의 typeof 연산자는 타입에 대한 문자열 이름을 반환하는 연산자이고,

타입스크립트의 typeof 타입 연산자는 타입스크립트에서만 사용가능한 연산자이다.



**keyof typeof**

typeof는 값의 타입을 검색하고 keyof는 타입에 허용된 키를 검색한다. 타입스크립트는 두 키워드를 연결해 값의 타입에 허용된 키를 간결하게 검색할 수 있다. 두 키워드를 함께 사용하면 typeof 타입 연산자를 keyof 타입 연산자와 함께 작업할 때 매우 유용하다.

```typescript
const ratins = {
    imdb: 8.4,
    metacritic: 82
}

function logRating(key: keyof typeof ratins){
    console.log(ratins[key])
}

logRating("imdb")
logRating("invalid") // Argument of type '"invalid"' is not assignable to parameter of type '"imdb" | "metacritic"'.(2345)
```

keyof와 typeof를 결합해 사용하면, 명시적 인터페이스 타입이 없는 객체에 허용된 키를 나타내는 타입에 대한 코드를 작성하고 업데이트하는 수고를 줄일 수 있다.



### 9.4 타입 어서션

타입스크립트는 강력하게 타입화될 때 가장 잘 작동한다. 그러나 경우에 따라 타입을 지정하는게 불가능할 때도 있다. 타입스크립트는 값의 타입에 대한 타입 시스템의 이해를 재정의하기 위한 구문으로 타입 어서션을 제공한다. 다른 타입을 의미하는 값의 타입 다음에 as 키워드를 배치한다. 타입 시스템은 어서션을 따르고 값을 해당 타입으로 처리한다.

```typescript
const rawData = '["one", "two"]'

JSON.parse(rawData)
JSON.parse(rawData) as string []
JSON.parse(rawData) as [string, string]
JSON.parse(rawData) as ["grace", "frankie"]
```

하지만 가능한 타입 어서션을 사용하지 않는 것이 좋다.



#### 9.4.1 포착된 오류 타입 어서션

오류 처리시 타입 어서션이 유용할 수 있다.

```typescript
try {
    // 오류 발생시키는 코드
}catch (error){
    console.log("Oh no", (error as Error).message)
}
```

코드 영역이 Error 클래스의 인스턴스를 발생시킬 거라 **확신** 한다면 타입 어서션을 활용해 오류 처리할 수 있다.



하지만 발생 오류가 예상된 오류 타입인지 확인하기 위해 instanceof로 타입 내로잉을 사용하는 것이 더 안전하다.

```typescript
try {
    // 오류 발생시키는 코드
}catch (error){
    console.log("Oh no", error instanceof Error ? error.message : error)
}
```



#### 9.4.2 non-null 어서션

타입 어서션이 유용한 또 하나의 경우는, 이론적으로 null 또는 undefined를 포함할 수 있는 변수에서 null과 undefined를 제거할 때 타입 어서션을 주로 사용한다. 이때 ! 를 사용하는데 null 또는 undefined 타입이 아니라 단언하는 것이다.

```typescript
let maybe = Math.random() > 0.5 ? undefined : new Date()

// 타입이 Date라고 간주됨
maybe as Date

// 타입이 Date라고 간주됨
maybe!
```

non-null 어서션은 값을 반환하거나 존재하지 않는 경우 undefined를 반환하는 Map.get과 같은 API에서 특히 유용하다.



#### 9.4.3 타입 어서션 주의 사항

any 타입과 마찬가지로 타입 어서션은 타입 시스템의 도피 수단이다. 따라서 any 타입과 같이 꼭 필요한 경우가 아니면 가능한 사용하지 말아야 한다. 더 정확한 타입을 갖는 것이 좋다.&#x20;



**어서션 vs 선언**

타입스크립트 타입 검사기는 변수의 타입 애너테이션에 대한 변수의 초깃값에 대해 할당 가능성 검사를 수행한다. 그러나 타입 어서션은 타입스크립트의 타입 검사 중 일부를 건너뛰도록 명시적으로 지시한다. 따라서 타입 애너테이션을 사용하거나 타입스크립트가 초깃값에서 변수의 타입을 유추하도록 하는 것이 바람직하다.



**어서션 할당 가능성**

타입 어서션은 일부 값의 타입이 약간 잘못된 상황에서 필요한 작은 도피 수단일 뿐이다. 타입스크립트는 타입 중 하나가 다른 타입에 할당 가능한 경우에만 타입 어서션을 허용한다. 완전히 관련 없는 두 타입에 타입 어서션이 있는 경우 타입스크립트가 타입 오류를 감지하고 알려준다.

(알아만 두기, 사용하지 말것)

관련없는 타입으로 전환해야 하는 경우 이중 타입 어서션을 사용한다. 먼저 any나 unknown 같은 top 타입으로 전환한 다음, 그 결과를 관련 없는 타입으로 전환한다.



### 9.5 const 어서션

const 어서션은 배열, 원시 타입, 값, 별칭 등 모든 값을 상수로 취급해야 함을 나타내는 데 사용한다. 특히 as const는 수신하는 모든 타입에 다음 세 가지 규칙을 적용한다.

* 배열은 읽기 전용 튜플로 취급
* 리터럴은 일반적인 원시 타입과 동등하지 않고 리터럴로 취급
* 객체의 속성은 읽기 전용으로 간주

```typescript
// a: (string | number)[]
let a = [0, ""]

// b: readonly [0, ""]
let b = [0, ""] as const
```



#### 9.5.1 리터럴에서 원시 타입으로

#### 9.5.2 읽기 전용 객체

as const를 사용해 값 리터럴을 어서션하면 유추된 타입이 가능한 구체적으로 전환된다. 모든 멤버 속성은 readonly가 되고, 리터럴은 일반적인 원시 타입 대신 고유 리터럴 타입으로 간주되며, 배열은 읽기 전용 튜플이 된다. 즉, 값 리터럴에 const 어서션을 적용하면 해당 값 리터럴이 변경되지 않고 모든 멤버에 동일한 const 어서션 로직이 재귀적으로 적용된다.



### 9.6 마치며

* top 타입: 매우 허용도가 높은 any와 제한적인 unknown
* 타입 연산자: keyof로 타입의 키를 알아내고, typeof로 값의 타입 알아내기
* 값의 타입을 변경하기 위해 타입 어서션을 사용하는 경우와 사용하지 않는 경우
* as const 어서션을 사용한 타입 내로잉

