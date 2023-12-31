# Ch.2 타입 시스템

### 2.1 타입의 종류

타입은 자바스크립트에서 다루는 값의 형태에 대한 설명이다. 형태란 값에 존재하는 속성과 메서드, typeof 연산자가 설명하는 것을 의미한다.

타입스크립트의 기본 타입은 자바스크립트의 기본 원시 타입인 일곱가지 타입이다.&#x20;

* null
* undefined
* boolean
* string
* number
* bigint
* symbol



타입스크립트는 변수의 타입을 유추할 수 있다.

```typescript
let bestSong = Math.random() > 0.5 ? "Chain of Folls" : "Respect"
// bestSong의 타입은 string이다.
```



#### 2.1.1 타입 시스템

타입 시스템은 프로그래밍 언어가 프로그램에서 가질 수 있는 타입을 이해하는 방법에 대한 규칙 집합이다.

타입 시스템의 작동 과정

1. 코드를 읽고 존재하는 모든 타입과 값을 이해한다.
2. 각 값이 초기 선언에서 가질 수 있는 타입을 확인한다.
3. 각 값이 추후 코드에서 어떻게 사용될 수 있는지 모든 방법을 확인한다.
4. 값의 사용법이 타입과 일치하지 않으면 사용자에게 오류를 표시한다.



#### 2.1.2 오류 종류

타입스크립트를 작성하는 동안 가장 자주 접하는 오류는 다음의 두 가지이다.

* [**구문 오류**](#user-content-fn-1)[^1]: 타입스크립트가 자바스크립트로 변환되는 것을 차단한 경우
* [**타입 오류**](#user-content-fn-2)[^2]: 타입 검사기에 따라 일치하지 않는 것이 감지된 경우



### 2.2 할당 가능성

자바스크립트는 변수의 초깃값을 읽고 해당 변수가 허용되는 타입을 결정한다. 변수에 새로운 값이 할당되면, 새롭게 할당된 값의 타입이 변수의 타입과 동일한지 확인한다. 해당 변수에 할당된 새로운 값의 타입이 같다면 문제가 없지만 다른 타입의 값이 할당되면 타입 오류가 발생한다.

이처럼 타입스크립트에서 함수 호출이나 변수에 값을 제공할 수 있는지 여부를 확인하는 것을 **할당 가능성**이라 한다. 전달된 값이 예상된 타입으로 할당 가능한지 여부를 확인하는 과정이다.



#### 2.2.1 할당 가능성 오류 이해하기

'Type ... is not assignable to type ...' 형태의 오류는 Type ... 의 타입이 변수 type...의 타입과 다르기에 할당할 수 없다는 오류메시지다. 타입스크립트를 사용하면서 이와같거나 더 복잡한 할당 가능성의 오류를 접하게 되는데 오류 메시지를 주의 깊게 읽으며 오류를 이해해야 한다.



### 2.3 타입 애너테이션

변수에 초깃값이 없는 경우 any 타입으로 간주한다. 초기 타입을 유추할 수 없는 변수를 진화하는 any라고 부르고, 이는 특정 타입을 강제하는 대신 새로운 값이 할당될 때마다 새로운 타입을 덮어 씌운다.&#x20;

```typescript
let a // any 타입
a = "abc" // string 타입으로 전환
a.toUpperCase()

a = 19 // number 타입으로 전환
a.toPrecision(1)
a.toUpperCase() // Error 발생
```

이처럼 무분별한 타입 변경을 허용하게 되면 타입 검사 목적을 쓸모 없게 만들어 타입스크립트를 쓸 이유가 사라진다. 따라서 초깃값을 할당하지 않고 변수의 타입을 선언할 수 있는 구문인 타입 애너테이션을 사용한다.



타입 애너테이션은 변수 이름 뒤에 배치되며 콜론(:)과 타입 이름을 차례대로 기재하여 사용한다. 타입 애너테이션으로 정의한 타입 외의 값을 할당하면 타입 오류가 발생한다.

```typescript
let a: string; // 변수 a의 타입은 string임 !
a = "abc"
a = 123 // Error 변수 a의 타입은 string 이므로 number 는 할당할 수 없다.
```



#### 2.3.1 불필요한 타입 애너테이션

(이 부분은 뭐라고 하는지 모르겠음)

타입 애너테이션은 타입스크립트가 자체적으로 수집할 수 없는 정보를 타입스크립트에 제공할 수 있다.

```typescript
let firstName: string = "Tina"
// 중복인 타입 애너테이션
// 타입 애너테이션을 사용하지 않아도 "Tina"를 통해 string 임을 유추할 수 있음
```

변하지 않는 변수에는 타입 애너테이션을 추가하지 않는 것을 선호한다고 한다. 수동으로 타입 애너테이션 작성, 타입이 변경되거나 복잡한 타입일 때 번거롭기 때문이다. 하지만 코드를 명확하게 문서화하거나 실수로 타입이 변경되지 않도록 타입스크립트를 보호하기 위해 명시적으로 타입 애너테이션을 포함하는 것이 경우에 따라서는 유용하다. ?? (잘 판단해서 타입 애너테이션을 사용해라 라는 의미인듯?)



### 2.4 타입 형태

타입스크립트는 변수에 할당된 값이 원래 타입과 일치하는지 확인하는 것 이상을 수행한다. 타입스크립트는 객체에 어떤 속성이 존재하는지 파악한다. 만약 객체의 특정 속성에 접근한다면, 타입스크립트는 접근하려는 속성이 존재하는지 파악하고, 타입이 무엇인지 등을 확인한다.

타입스크립트는 객체의 형태에 대한 이해를 바탕으로 할당 가능성뿐 아니라 객체 사용과 관련된 문제도 알려준다.



#### 2.4.1 모듈

ECMAScript 2015 에 추가된 모듈은 export 와 import 를 사용해 서로 다른 파일에 작성된 코드를 공유하는 방법이다. 서로 다른 파일에서 코드를 가져와 사용하므로 동일한 이름의 변수가 있다면 오류가 발생할 수 있다. 타입스크립트는 모듈로 연결된 파일을 전역 스코프로 간주하여 모든 스크립트의 변수 중복 여부를 확인하고 알려준다.



### 2.5 마치며

* 타입은 무엇인지 알아보고 타입스크립트가 인식하는 원시 타입 이해하기
* 타입 시스템은 무엇인지 알아보고 타입스크립트의 타입 시스템이 코드를 이해하는 방법 살펴보기
* 타입 오류와 구문 오류의 차이점
* 유추된 변수 타입과 변수 할당 가능성
* 타입 애너테이션으로 변수 타입을 명시적으로 선언하고 any 타입의 진화 방지하기
* 타입 형태에서 객체 멤버 확인하기
* 스크립트 파일과는 다른 ECMA스크립트 모듈 파일의 선언 스코프



\---

타입 애너테이션과 같이 헷갈렸던 하나의 개념에 대한 정의를 파악하기 쉬웠다. 하지만 그 외에 직독직해로 번역한 듯한 문체가 많아 한국어를 해석해야 해 이해하기 어려운 부분이 많았다. 차라리 원서로 공부할껄 라는 생각이 들지만 더 읽어보고 익숙해져보자...!!

[^1]: 타입스크립트가 코드로 이해할 수 없는 잘못된 구문을 감지할 때 발생. 타입스크립트 파일에서 자바스크립트 파일을 생성할 수 없도록 차단한다.

[^2]: 타임스크립트의 타입 검사기가 프로그램의 타입에서 오류를 감지했을 때 발생한다. 자바스크립트로 변환을 차단하지는 않지만, 코드가 실행되면 무언가 충돌하거나 예기치 않게 작동할 수 있음을 나타낸다.
