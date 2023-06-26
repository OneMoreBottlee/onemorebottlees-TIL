---
description: https://github.com/OneMoreBottlee/TypeScript-Master/tree/main/S17
---

# \[S17] Module

파일 간 코드를 공유하는 다양한 방법



**Namespace**

코드를 공유하고 TS 에서 고드를 구성하는 오래된 방식

(사용하진 않지만 알아두기)

ES6 이후 import/export 를 사용함



**Module**

<figure><img src="../../../.gitbook/assets/image (157).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (102).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (83).png" alt=""><figcaption></figcaption></figure>

서로 다른 폴더에서 위와 같이 작업하면 에러가 발생해야하지만 발생하지 않는다.

JS는 export나 최상위 await 가 없는 파일을 모듈이 아닌 스크립트로 간주하기에 모든 변수와 타입이 공유 전역 스코프에 있다고 판단한다.

따라서 에러가 발생하지 않고 공유된다.

<figure><img src="../../../.gitbook/assets/image (86).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (119).png" alt=""><figcaption></figcaption></figure>

원본에 export 를 추가하면 모듈로 인식되어 참고하는 곳에서 import 를 사용해야 한다.

컴파일한 파일은 .js 로 변환되기에 import 에서 .js를 사용함



이를 HTML로 브라우저에서 띄워보면 에러가 발생한다.

<figure><img src="../../../.gitbook/assets/image (147).png" alt=""><figcaption></figcaption></figure>

이는 브라우저 내의 JS 가 Common JS 모듈을 이해하지 못해 발생하는 에러다.

(import, export, 모듈 등등 Node.js 에서 사용한 개념을 이해하지 못함)

<figure><img src="../../../.gitbook/assets/image (134).png" alt=""><figcaption></figcaption></figure>

이럴때 tsconfig.json 파일에서 module 설정을 ES6 이후로 변경해주면 다음과 같은 에러로 변경된다.

<figure><img src="../../../.gitbook/assets/image (144).png" alt=""><figcaption></figcaption></figure>

이럴때 html 파일에서 모듈의 타입을 설정해주면

<figure><img src="../../../.gitbook/assets/image (149).png" alt=""><figcaption></figcaption></figure>

에러가 사라지면서 정상적으로 작동한다.

<figure><img src="../../../.gitbook/assets/image (107).png" alt=""><figcaption></figcaption></figure>

브라우저에서 모듈을 사용하기 위한 과정이다.



**Import Type**

import type { } from “”

import {type } from “”
