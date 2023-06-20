# \[S19] React

**Typescript 시작**

```jsx
npx create-react-app my-app --template typescript
```

<figure><img src="../../../.gitbook/assets/image (76).png" alt=""><figcaption></figcaption></figure>

타입스크립트 컴포넌트를 만드는 기본

출력값에 JSX.Element 타입를 지정한다.

![](<../../../.gitbook/assets/image (152).png>)

지금은 주로 사용하지 않지만 Function Component, FC를 사용했다고 함



**TS 프로퍼티**

<figure><img src="../../../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

<div data-full-width="false">

<figure><img src="../../../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

</div>

props 로 내려준다.

<figure><img src="../../../.gitbook/assets/image (42).png" alt=""><figcaption></figcaption></figure>

interface를 만들어 props의 타입을 설정할 수 있음



**ShoppingList Component**

![](<../../../.gitbook/assets/image (151).png>)

<figure><img src="../../../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

상위 컴포넌트인 App 에서 데이터를 내려주고,

<figure><img src="../../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

하위 컴포넌트 ShoppingList에서 Props의 타입을 설정하고, 데이터를 받아 표현한다.



**useState**

<figure><img src="../../../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (52).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (96).png" alt=""><figcaption></figcaption></figure>

중복되는 인터페이스를 파일에 모아 하나의 인터페이스를 재사용할 수 있음



**useRef**

<figure><img src="../../../.gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>

useRef 를 사용할때 \<HTMLInputElement>를 사용함
