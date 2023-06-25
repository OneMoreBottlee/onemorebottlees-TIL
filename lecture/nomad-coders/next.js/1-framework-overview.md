# #1 FRAMEWORK OVERVIEW

### **#1.0 Library vs Framework**

**Library**

개발자로서 사용하는것? 라이브러리를 사용해 무언가를 할수있음 리액트를 사용하면 루트가 되는 index 파일에서 시작해서 자유롭게 원하는 코드를 추가함



**Framework**

코드를 불러오는것, 특정한 규칙에 따라 코드가 실행됨.



⇒

라이브러리는 자유롭게 코드를 작성해 원하는 구현을 만들어 내는것이고, 프레임워크는 정해진 틀 안에서 규칙에 따라 코드를 작성하는 것



### **#1.1 Pages**

PAGES 폴더 안에 원하는 url로 파일명을 정해 넣고 export default 설정을 하면 알아서 url이 설정된다.

컴포넌트 이름은 중요하지 않음.

index 는 홈페이지 역할을 하게 된다.

jsx를 사용하려면 import react 할 필요 없이 return에 추가한다. ⇒ useState 와 같은 기능 사용시에는 import

404 페이지 - 존재하지 않는 페이지, next.js



### **#1.2 Static Pre Rendering**

CSR - CRA

Client에서 렌더링을 진행하기에 사용자 환경에 따라 자바스크립트 파일이 다운로드 되고, 작동하기 전까지 느리게 작동할 수 있음



SSR - Next.js && Gatsby.js

미리 렌더링 된 HTML 파일을 받아 작동함 ⇒ 연결 속도가 느리거나 JS 가 없더라도 큰 틀은 유지됨



Hydration - react.js를 프론트엔드 안에서 실행하는 것 ⇒ next.js는 react.js를 동작시켜 미리 페이지를 만드는데 이 과정에서 컴포넌트들이 렌더링 되고 html로 변환됨 ⇒ react.js 가 로딩되면 미리 생성된 html과 상호작용이 결합되어 작동하게 된다.



### **#1.3 Routing**

nav bar를 만들고 라우팅 기능을 활용하려면

```jsx
import Link from "next/link";
```

를 추가하고

```jsx
<Link href={"/"}>Home</Link>
<Link href="/about">About</Link>
```

와 같이 사용한다.

Link 태그에 설정한 내용을 내부의 a 태그에 전달하는 개념이다.



**Router**

useRouter

현재 위치한 페이지의 location 정보를 확인할 수 있다.

<figure><img src="../../../.gitbook/assets/image (79).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (54).png" alt=""><figcaption></figcaption></figure>

이 기능을 활용해 현재 위치한 nav를 보여줄 수 있음

<figure><img src="../../../.gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (52).png" alt=""><figcaption></figcaption></figure>



### **#1.4 CSS modules**

NextJS에 CSS를 추가하는 방법

1. style
2. Module 이름.module.css 라는 이름의 파일에 원하는 스타일을 작성한다.

![](<../../../.gitbook/assets/image (74).png>)![](<../../../.gitbook/assets/image (17).png>)

![](<../../../.gitbook/assets/image (88).png>)

**className={styles.nav} 이 부분으로** className을 프로퍼티 형식으로 적용해 모듈의 스타일이 적용됨



여러 개의 className 설정

className={”이름1 이름2”} 와 같은 형태가 되야되기에 백틱으로 연결하거나 배열을 join하는 방법을 이용함

<figure><img src="../../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

### **#1.5 Style JSX**

NextJS에 CSS를 추가하는 방법

3. JSX style 태그에 JSX를 추가해 원하는 스타일을 추가한다. 부모 컴포넌트에서 같은 클래스네임을 사용하고 있더라도 적용되지 않음 ⇒ 클래스네임이 랜덤으로 정해지기 때문에

<figure><img src="../../../.gitbook/assets/image (48).png" alt=""><figcaption></figcaption></figure>

### **#1.6 Custom App**

**Global** **Style**

<figure><img src="../../../.gitbook/assets/image (75).png" alt=""><figcaption></figcaption></figure>

jsx global을 설정하면 글로벌 스타일이 설정됨



**App Component**

\_app.js 가장 먼저 렌더링되면서 템플릿 && global 설정의 초석이 됨

<figure><img src="../../../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

중복되는 컴포넌트나 style을 설정해 놓을 수 있음

Component 에 props 로 전달되는 내용이 바뀌면서 화면도 변경됨

### **#1.7 Recap**

***

위 내용 복습
