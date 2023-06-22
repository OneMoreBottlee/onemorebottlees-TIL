---
description: https://github.com/OneMoreBottlee/TypeScript-Master/tree/main/S14
---

# \[S14] Generics ⭐⭐⭐⭐⭐

매우 중요한 개념 but 복잡하다

까다로운 개념과 구문으로 혼란…

**Generics**

Generics allow us to define reusable functions and classes that work with multiple types rather than a single type.

TS의 여러 타입에서 사용할 수 있는 재사용 함수나 재사용 클래스를 정의할 수 있게 해주는 특수 기능



**사용 예시 1**

< 이곳 > 이곳에 원하는 타입을 넣어 사용할 수 있다.

```tsx
const num : number[] = []
const nums: Array<number> = []
const colors: Array<string> = []
```

DOM 을 다룰때

```tsx
const inputEl = document.querySelector<HTMLInputElement>("#username")!
console.log(inputEl.value)

inputEl.value = "Howw"

const btn = document.querySelector<HTMLButtonElement>(".btn")!
console.log(btn)
```



**첫 번째 제네릭 작성하기**

<figure><img src="../../../.gitbook/assets/image (73).png" alt=""><figcaption></figcaption></figure>

숫자를 넣으면 숫자가, 문자를 넣으면 문자가, 불리언을 넣으면 불리언이

입력한 타입 값 그대로 출력되는 함수가 있다고 해보자.

위와 같이 각 타입별로 하나씩 입력하거나

<figure><img src="../../../.gitbook/assets/image (154).png" alt=""><figcaption></figcaption></figure>

any 로 처리 해야된다.

하지만 첫번째 방법은 비효율적이고, 두번째 방법은 타입스크립트의 취지와 맞지 않다.

이러한 상황에서 제너릭을 사용한다.

<figure><img src="../../../.gitbook/assets/image (123).png" alt=""><figcaption></figcaption></figure>

입력하는 타입과 출력하는 타입이 같다.

숫자는 숫자, 문자열은 문자열, 불리언은 불리언.

이렇게 제네릭이 여러가지 타입을 받을 수 있도록 반환한다.



**제네릭 함수**

함수에서도 위와 같이 설정할 수 있다.

입력값의 타입이 확실하지 않을때 제너릭을 사용한다.

<figure><img src="../../../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (167).png" alt=""><figcaption></figcaption></figure>



**추론된 제너릭 타입 파라미터**

타입스크립트는 추론으로 타입을 확인할 수 있기에 위와 같이 타입 값을 일일이 넣지 않아도 된다.

<figure><img src="../../../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (142).png" alt=""><figcaption></figcaption></figure>

입력값이 명확하다면 타입스크립트의 추론을 믿고, 불분명하다면 제너릭을 넣는게 좋다.



**제너릭, 화살표 함수 및 TSX 파일**

<figure><img src="../../../.gitbook/assets/image (151).png" alt=""><figcaption></figcaption></figure>

React, TS, 화살표 함수를 사용한다면 , 를 기억하자



**여러 타입을 가진 제너릭**

타입이 여러개인 제너릭이 필요한 경우 다음과 같이 설정한다.

<figure><img src="../../../.gitbook/assets/image (55).png" alt=""><figcaption></figcaption></figure>

예) 두 객체를 합치는 함수

T, U 를 하나의 제너릭에 넣고 분배

<figure><img src="../../../.gitbook/assets/image (132).png" alt=""><figcaption></figcaption></figure>



**타입 제한 추가하기**

<figure><img src="../../../.gitbook/assets/image (134).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (121).png" alt=""><figcaption></figcaption></figure>

제너릭을 사용할 때 원하지 않는 타입이 들어가는 경우도 있다.

그러면 위와 같이 에러없이 작동하고 원하지 않는 결과가 탄생한다.

이런 일을 방지하고자 타입을 제한할 수 있다.

<figure><img src="../../../.gitbook/assets/image (99).png" alt=""><figcaption></figcaption></figure>

extends 타입 을 추가하면서 에러가 발생하게 된다.

조건에 인터페이스를 추가할 수 도 있다.

<figure><img src="../../../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>



length 가 없기에 인터페이스를 추가한다.

<figure><img src="../../../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>

에러가 사라진다.



**기본 타입 파라미터**

타입 파라미터에 기본값을 설정할 수 있다.

<figure><img src="../../../.gitbook/assets/image (133).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

\<T = number> 로 기본값이 설정됨

\<string>을 추가하면 string 타입이 됨



**Class**

제너릭을 사용해 클래스를 만들 수 있다.

<figure><img src="../../../.gitbook/assets/image (62).png" alt=""><figcaption></figcaption></figure>
