---
description: https://github.com/OneMoreBottlee/TypeScript-Master/tree/main/S12
---

# \[S12] Class

**Class in JS**

패턴을 생성하는 역할

객체를 나타낼 때 프로퍼티와 기능을 함께 나타낸다.

인스턴스화 가능하다.

<figure><img src="../../../.gitbook/assets/image (128).png" alt=""><figcaption></figcaption></figure>

변수명 앞에 #을 붙이면 Private 설정. 클래스 외부에서 호출 불가능함

constructor - 생성자. 객체 생성시 호출됨



**Getter / Setter**

접근자 프로퍼티

프로퍼티로 설정하지 않았지만 프로퍼티처럼 행동하는 문법적 설탕.

Getter

![](<../../../.gitbook/assets/image (85).png>)

get 함수명(){}

값을 가져올 수 있다.



Setter

![](<../../../.gitbook/assets/image (160).png>)

즉각적으로 값을 변경하는 프로퍼티와 달리 조건부로 변경하는등 통제할 수 있다.

![](<../../../.gitbook/assets/image (96).png>)![](<../../../.gitbook/assets/image (8).png>)![](<../../../.gitbook/assets/image (87).png>)

SON HM ⇒ Kim MJ 변경 완료



**Static**

개별 인스턴스가 아닌 클래스 차원에서 사용할 기능 설정할때 사용

![](<../../../.gitbook/assets/image (105).png>)![](<../../../.gitbook/assets/image (45).png>)



**Extends**

확장.

![](<../../../.gitbook/assets/image (165).png>)![](<../../../.gitbook/assets/image (88).png>)

Player 의 모든 메서드, 프로퍼티 사용 가능



**Super**

자식 클래스에 생성자를 추가할 때 사용

![](<../../../.gitbook/assets/image (80).png>)![](<../../../.gitbook/assets/image (102).png>)

