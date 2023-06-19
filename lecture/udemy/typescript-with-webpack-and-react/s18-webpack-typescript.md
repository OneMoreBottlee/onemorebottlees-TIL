# \[S18] Webpack 과 TypeScript

**Webpack**

의존성을 가진 수십, 수백 개의 파일로 구성된 복잡한 어플리케이션 처리를 돕는다. 파일들은 전송, 요청, 파일 분류 등의 작업이 이루어지고, 제 3자 라이브러리를 갖는다. 웹팩은 이 파일을 모아 합쳐 브라우저에 넣을 수 있는 번들을 만드는 역할을 한다.

**Webpack 의존성 설치**

```tsx
npm i --save-dev webpack webpack-cli typescript ts-loader
```

webpack - 모듈 번들러

webpack-cli - webpack의 파트너

typescript - packagejson에 넣어주는게 좋음

ts-loader - Webpack 전달자 역할

**소스맵**

단순화된 번들을 가지고 역매핑을 통해 빌드 전 상태를 보여줌으로써 번들을 구성하고 있는 코드가 어디서 오는지 보여준다.
