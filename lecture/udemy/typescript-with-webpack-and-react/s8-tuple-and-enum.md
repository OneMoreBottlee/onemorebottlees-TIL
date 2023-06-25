---
description: https://github.com/OneMoreBottlee/TypeScript-Master/tree/main/S8
---

# \[S8] Tuple & Enum

**Tuple**

길이와 타입이 고정되어 있는 배열

<figure><img src="../../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

많이 사용되지는 않지만, RGB와 같이 본질적으로 연결되어있는 배열에 사용됨

<figure><img src="../../../.gitbook/assets/image (162).png" alt=""><figcaption></figcaption></figure>

한계) push와 pop 등으로 배열을 수정하면 타입과 상관없이 적용됨



**Enum**

코드 전체에서 재사용할 수 있는 명명된 상수의 집합

![](<../../../.gitbook/assets/image (44).png>)![](<../../../.gitbook/assets/image (90).png>)

(recoil 에서 사용하던 enum 이 이렇게 사용되는군..)

<figure><img src="../../../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

다양한 타입이 섞인 enum도 사용 가능하다. 자동 완성 기능은 덤

컴파일되면서 TS의 대부분의 요소들이 제거되는 것과 달리 Enum은 컴파일되면서 JS에 추가적인 코드로 남게된다.

⇒ 하지만, 이러한 코드 추가는 TS의 원칙과는 다르다.

⇒ 그래서, const 를 붙여 새로운 객체 추가를 방지한다.
