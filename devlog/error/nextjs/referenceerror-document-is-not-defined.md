---
description: 230708 퍼블리싱 연습중
---

# ReferenceError : document is not defined

### 문제 상황

공통 레이아웃 중 헤더를 추가하면서 발생한 에러...



### 문제 원인

NextJS는 서버사이드 렌더링 후 클라이언트 렌더링을 진행한다. 이때 Document를 찾지 못해 발생되는 에러다. Document는 브라우저에서만 작동하는 기능이므로 당연스럽게 에러가 발생한 것이었다.

[관련 링크](https://nextjs.org/docs/pages/building-your-application/optimizing/lazy-loading#with-no-ssr)



### 문제 해결

해당 컴포넌트에서 서버 사이드 렌더링을 비활성화해 클라이언트 사이드 렌더링만 되도록 설정한다.

```typescript
import dynamic from 'next/dynamic'

export default function Layout (props: { children: React.ReactNode }) {
    // SSR 작동시 Document 사용 불가능함
    // > navbar에서 document를 사용하고 있음 > 오류 발생
    // > 해당 컴포넌트에서 ssr 서버 사이드 렌더링을 비활성화
    const Navbar = dynamic(() => import("./navbar"), {ssr: false})
    return (
        <>
            <Navbar />
            <main>{props.children}</main>
        </>
    )
}
```

이후 에러 없이 정상적으로 작동한다.

<figure><img src="../../../.gitbook/assets/image (1) (1) (2) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (2) (4).png" alt=""><figcaption></figcaption></figure>

