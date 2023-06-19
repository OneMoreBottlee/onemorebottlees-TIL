# \[S19] React

**Typescript 시작**

```jsx
npx create-react-app my-app --template typescript
```

<figure><img src="../../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

타입스크립트 컴포넌트를 만드는 기본

출력값에 JSX.Element 타입를 지정한다.

![](<../../../.gitbook/assets/image (88).png>)

지금은 주로 사용하지 않지만 Function Component, FC를 사용했다고 함



**TS 프로퍼티**

<figure><img src="../../../.gitbook/assets/image (120).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (131).png" alt=""><figcaption></figcaption></figure>

<div data-full-width="false">

<figure><img src="../../../.gitbook/assets/image (123).png" alt=""><figcaption></figcaption></figure>

</div>

props 로 내려준다.

<figure><img src="../../../.gitbook/assets/image (140).png" alt=""><figcaption></figcaption></figure>

interface를 만들어 props의 타입을 설정할 수 있음



**ShoppingList Component**

![](<../../../.gitbook/assets/image (87).png>)

<figure><img src="../../../.gitbook/assets/image (116).png" alt=""><figcaption></figcaption></figure>

상위 컴포넌트인 App 에서 데이터를 내려주고,

<figure><img src="../../../.gitbook/assets/image (113).png" alt=""><figcaption></figcaption></figure>

하위 컴포넌트 ShoppingList에서 Props의 타입을 설정하고, 데이터를 받아 표현한다.



**useState**

<figure><img src="../../../.gitbook/assets/image (122).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (151).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>

중복되는 인터페이스를 파일에 모아 하나의 인터페이스를 재사용할 수 있음



**useRef**

<figure><img src="../../../.gitbook/assets/image (150).png" alt=""><figcaption></figcaption></figure>

useRef 를 사용할때 \<HTMLInputElement>를 사용함
