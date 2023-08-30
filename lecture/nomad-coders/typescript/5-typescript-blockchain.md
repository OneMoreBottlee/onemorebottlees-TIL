# #5 TypeScript Blockchain

### #5.0 Introduction

타입스크립트 밑바닥부터 설정하기 ! - not TS Playground



### #5.1 Targets

make directory >&#x20;

```typescript
npm init -y // package.json 생성
npm install -D typescript // package.json 에 ts 추가
```

<figure><img src="../../../.gitbook/assets/image (24).png" alt=""><figcaption><p>devDependencies 생성됨</p></figcaption></figure>

make src > make index.ts



make tsconfig.json

#### tsconfig.json

이 파일이 있으면 vs code는 타입스크립트로 작업한다는 것을 즉시 알게되고, 자동완성기능 등 제공해줌



함수를 하나 추가한 ts 파일로 확인해보자.

<figure><img src="../../../.gitbook/assets/image (30).png" alt=""><figcaption><p>index.ts</p></figcaption></figure>

index.ts 파일에는 hello 라는 함수가 하나 포함되어 있다. 이 ts 파일을 컴파일해보자

<figure><img src="../../../.gitbook/assets/image (18) (3).png" alt=""><figcaption><p>tsconfig.json</p></figcaption></figure>

tsconfig.json 파일에 위와 같이 설정하고

<figure><img src="../../../.gitbook/assets/image (25).png" alt=""><figcaption><p>package.json</p></figcaption></figure>

package.json 의 script에 build : tsc를 추가한뒤 npm run build 를 실행하면

<figure><img src="../../../.gitbook/assets/image (23).png" alt=""><figcaption><p>npm run build</p></figcaption></figure>

컴파일한 js 파일이 설정한 폴더에 속해있다.



위 파일의 특징을 보자.\
먼저, 화살표 함수가 일반적인 함수 선언문으로 변경되었고, const 가 var 로 수정되었다.\
어디서나 실행할 수 있도록 낮은 버전의 js 코드로 변경한 것이다 !



이를 tsconfig 에서 원하는 버전으로 변경할 수 있다.

<figure><img src="../../../.gitbook/assets/image (5) (2).png" alt=""><figcaption><p>tsconfig.json</p></figcaption></figure>

컴파일러 옵션에서 타겟을 ES6 로 설정하고 build !



<figure><img src="../../../.gitbook/assets/image (28).png" alt=""><figcaption><p>index.js</p></figcaption></figure>

아까와 달리 ES6에서 사용가능한 const, 화살표 함수로 컴파일 된다.



이처럼 tsconfig.json 에서 ts와 관련된 다양한 기능을 설정할 수 있다.



### #5.2 Lib Configuration

lib - 타입스크립트에게 어떤 API를 사용하고 어떤 환경에서 코드를 실행할지 지정하는 역할.  JS가  어디서실행될지를 알려줌. 설정하지 않으면 default로 \["ES5", "ES6", "ES7", "ES2015.Promise"] 가 설정됨

ex) "lib" : \["ES6"] 으로 설정시

브라우저에서 사용한다는 내용이 없기에 TS에서 에러가 발생한다.

<figure><img src="../../../.gitbook/assets/image (6) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>"lib" : ["ES6"]</p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (2) (2).png" alt=""><figcaption><p>"lib" : ["ES6", "DOM"]</p></figcaption></figure>

lib에 DOM을 추가하면 에러는사라진다.



이처럼 lib은 타입스크립트에게 어떤 API를 사용할 것이고, 어떤 환경에서 코드를 실행할지 알려주는 역할을 한다. 이 설정으로 정말x100 편리한 자동완성 기능을 사용할 수 있다.



### #5.3 Declaration Files

정의 파일 - JS 코드의 모양을 TS에 설명해주는 파일.



ex) JS로 만든 함수 모듈을 TS 에서 사용하려할때

<figure><img src="../../../.gitbook/assets/image (1) (2) (1).png" alt=""><figcaption><p>JS로 작성한 함수 모듈</p></figcaption></figure>

JS에서 작성한 함수 모듈을 TS에 가져와 사용하면 에러가 발생한다.

<figure><img src="../../../.gitbook/assets/image (14) (1) (1).png" alt=""><figcaption><p>TS에서 발생한 에러</p></figcaption></figure>

형식 선언을 찾을 수 없다는 것인데 이는 TS에게 myPackage 내 요소들의 타입을 알려주지 않았기 때문이다. 이를 위해 myPackage.t.ds 파일을 생성해 다음과 같이 타입을 선언해보자.

<figure><img src="../../../.gitbook/assets/image (15) (1).png" alt=""><figcaption><p>myPackage의 타입 선언</p></figcaption></figure>

그러면 TS에서 타입 확인이 가능해지고 에러가 사라진다.&#x20;

<figure><img src="../../../.gitbook/assets/image (26).png" alt=""><figcaption><p>myPagkage 모듈을 확인 가능하다.</p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (17).png" alt=""><figcaption><p>각 함수 모듈의 타입 확인 가능해진 모습</p></figcaption></figure>



이처럼 Declaration Files은 TS 에서 JS 코드를 사용할때  \_.d.ts 에 JS의 모양을 전달해 TS에서 타입 에러를 방지하고 안전하게 사용할 수 있도록 하는 파일을 의미한다.



### #5.4 JSDoc

JS 파일을 TS 와 같이 보호하고 싶을때 JS 첫 줄에 아래와 같이 주석을 추가한다.

<figure><img src="../../../.gitbook/assets/image (7) (2).png" alt=""><figcaption></figcaption></figure>

TS 파일이 JS 파일을 검사해 오류를 확인하고, 위와 같이 오류를 활성시켜 개발자에게 알려준다.

그 오류는 아래와 같이 주석으로 타입을 설정함으로써 제거할 수 있다.

<figure><img src="../../../.gitbook/assets/image (19) (2).png" alt=""><figcaption></figcaption></figure>

이 방법이 JSDoc으로 주석을 사용해 JS 파일에 type 정보를 제공하는 방법이다.



### #5.5 Blocks

블록체인 구현 초기 세팅, TS 효율성 체감하기



ts-node - Node.js용 TS 실행 엔진 및 REPL. 사전 컴파일없이 TS를 직접 실행할 수 있다.\
nodemon



### #5.6 Definitely Typed

타입스크립트로 만들어지지 않은 패키지를 받았는데 타입 정의가 없을때 어떻게 해야 하는지 !



DefinitelyTyped

npm에 존재하는 거의 대부분의 패키지들의 타입이 정의되어 있는 저장소.

{% embed url="https://github.com/DefinitelyTyped/DefinitelyTyped" %}

### #5.7 Chain

블록체인 만들어봄

<figure><img src="../../../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

