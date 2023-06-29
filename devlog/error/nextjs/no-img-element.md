---
description: 230626 CSS 연습중
---

# No img element

### 문제상황

오랜만에NextJS 에서 CSS 연습을 진행하다 문득 브라우저 콘솔창을 보니 img 태그 관련 경고가 등장해있었다...\
(사실 VSCode에서 TS가 꾸준히 알려줬지만 애써 무시하고 넘어감...)

발견한 김에 왜 발생했는지 확인하고 싹 다 고쳐보자 !



### 문제원인

<figure><img src="../../../.gitbook/assets/image (2).png" alt=""><figcaption><p>Image 태그의 탄생 비화</p></figcaption></figure>

NextJS 공식 문서를 보면, NextJS에서 일반적인 img 태그를 사용하면 LCP 속도가 느려지고 대역폭이 높아지는 것을 방지하고자 img 태그의 사용을 방지하기 위해 경고를 준다고 한다.



<figure><img src="../../../.gitbook/assets/image (3).png" alt=""><figcaption><p>에러 발생 원인과 수정 방법</p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (14).png" alt=""><figcaption><p>NextJS가 제시하는 2가지 수정 방법</p></figcaption></figure>

해결 방법은 Image 태그를 사용하거나 picture 태그에 img 태그를 포함시키는 것이란다.

첫 번째 방법을 사용해보자.



### 문제해결

즉시 Image 태그를 임포트하고 Image 태그로 바꿨다. 필수 요소인 src, alt, width, height 를 추가하니 잘 작동한다. 다른 img 태그도 모두 수정해서 경고 문구를 없애자 !

<figure><img src="../../../.gitbook/assets/image (29).png" alt=""><figcaption><p>Water Wave</p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (27).png" alt=""><figcaption><p>Text Wave</p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (28).png" alt=""><figcaption><p>Falling Login</p></figcaption></figure>

깔끔\~

