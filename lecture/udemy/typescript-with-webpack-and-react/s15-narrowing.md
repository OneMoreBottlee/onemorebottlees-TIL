---
description: https://github.com/OneMoreBottlee/TypeScript-Master/tree/main/S15
---

# \[S15] Narrowing

TS의 핵심

명확하지 않은 타입이 있을 때 (유니온 타입) 타입을 좁히는 과정



**typeof Guards**

<figure><img src="../../../.gitbook/assets/image (139).png" alt=""><figcaption></figcaption></figure>

typeof를 활용해 특정 타입에 대한 동작을 설정한다



**Truthiness Guards**

<figure><img src="../../../.gitbook/assets/image (153).png" alt=""><figcaption></figcaption></figure>

타입 유무를 통해 설정한다.

<figure><img src="../../../.gitbook/assets/image (114).png" alt=""><figcaption></figcaption></figure>

매개변수가 존재하면 문자열, 없으면 null 이라는 조건을 활용해 타입별 동작을 설정한다.



**Equality Narrowing**

<figure><img src="../../../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

동등 연산자를 사용해 타입을 좁힌다.

x의 타입과 y의 타입에서 동일한 부분은 string 이라는 점을 이용한다.



**in Narrowing**

객체의 성질을 사용해 타입을 좁힌다.

<figure><img src="../../../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

2개의 인터페이스에서 특정할 수 있는 속성을 확인해 타입을 좁힌다.

타입에 맞는 동작을 설정한다.



**instanceof Narrowing**

instanceof 연산자의 특징을 사용해 타입을 좁힌다.

<figure><img src="../../../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>

date 의 프로토타입 체인 안에 Date 가 있으면 Date 타입

아니면 string

<figure><img src="../../../.gitbook/assets/image (100).png" alt=""><figcaption></figcaption></figure>

entity 의 프로토타입 체인 안에 User 가 있으면 User 타입

아니면 Company



**타입 명제**

타입 스크립트에만 있는 기능 is OO

<figure><img src="../../../.gitbook/assets/image (166).png" alt=""><figcaption></figcaption></figure>

Cat 인터페이스 / Dog 인터페이스

타입 명제 - animal 이 캣일때 참이 리턴되는 함수

함수를 활용해 참이면 Cat 타입 아니면 Dog 타입



**판별 유니온**

공통 프로퍼티를 공유하는 여러 유형을 설정하고 해당 프로퍼티가 리터럴 값이 되는 것

<figure><img src="../../../.gitbook/assets/image (81).png" alt=""><figcaption></figcaption></figure>

수탉 인터페이스, kind = “수탉”

소 인터페이스, kind = “소”

돼지 인터페이스, kind = “돼지”

타입 - 인터페이스의 집합

kind 값으로 타입을 확인해 분기



**Exhaustiveness check & Never**

이론적으로 도달할 수 없는 곳에 Never를 설정해 의도하지 않은 로직 발생시 에러를 발생시킨다.

<figure><img src="../../../.gitbook/assets/image (43).png" alt=""><figcaption></figcaption></figure>

양 인터페이스

양 타입을 처리하지 않아 default 까지 내려옴

never로 되어있어 에러 발생
