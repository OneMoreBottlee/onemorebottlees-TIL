---
description: https://github.com/OneMoreBottlee/TypeScript-Master/tree/main/S13
---

# \[S13] TS Class

<figure><img src="../../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

타입을 설정해주는 이 부분이 JS와 다름



**Readonly**

![](<../../../.gitbook/assets/image (112).png>)

Class 에서도 Readonly 설정 가능

프로퍼티에 대한 쓰기 여부



**Public / Private**

![](<../../../.gitbook/assets/image (7).png>)

Public 클래스 외부에서 변경, 접근, 쓰기의 가능을 알린다.

readonly 는 못바꿈



![](<../../../.gitbook/assets/image (67).png>)

Private 클래스 외부에서 변경, 접근, 쓰기 불가능하게 설정

TS 의 클래스 - 런타임 이전 오류 알림

JS 의 클래스 - 런타임에 작동

오류는 발생하지만 작동은 함 - JS의 미덕?!



**단축 구문**

<figure><img src="../../../.gitbook/assets/image (63).png" alt=""><figcaption></figcaption></figure>

파라미터 프로퍼티 단축 구문을 사용할 수 있음



**Getter / Setter**

![](<../../../.gitbook/assets/image (62).png>)

TS의 Getter / Setter는 JS와 같다.

0 이하에서 에러를 설정했지만 TS가 오류를 보여주진 않는다.



**Protected**

<figure><img src="../../../.gitbook/assets/image (30).png" alt=""><figcaption></figcaption></figure>

private 설정은 외부 액세스와 자식 클래스에서도 액세스를 금지한다.

하지만 자식 클래스에서는 액세스를 허용해야하는 경우가 있다.

이때 Protected 설정을 사용한다.



![](<../../../.gitbook/assets/image (75).png>)

외부 액세스는 방지하지만, 자식 클래스에서의 액세스는 허용한다.



**Interface**

<figure><img src="../../../.gitbook/assets/image (102).png" alt=""><figcaption></figcaption></figure>

인터페이스 설정 가능하다

다중 인터페이스도 가능



**abstract**



패턴을 정의하고 자식 클래스에서 시행되야 하는 메서드를 정의하는데 사용된다.

기능과 타입 설정을 함께 하는 일종의 하이브리드

<figure><img src="../../../.gitbook/assets/image (145).png" alt=""><figcaption></figcaption></figure>

인스턴스화는 못하지만 abstract 키워드를 붙여 메서드를 필수로 표시하고 자식 클래스에서 해당 메서드를 반드시 시행하도록 설정할 수 있다.

(interface와 비슷하지만 다름)
