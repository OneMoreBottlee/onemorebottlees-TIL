# Ch.13 구성 옵션

타입스크립트는 모든 일반적인 자바스크립트 사용 패턴에 맞출 수 있다. 레거시 브라우저에서부터 현대적인 서버 환경까지 다양한 프로젝트에서 작동 가능하다.

이러한 타입스크립트의 구성력은 100개 이상의 풍부한 구성 옵션에서부터 비롯된다.

* tsc에 전달된 명령줄 플래그
* 타입스크립트 구성 파일 TSConfig



### 13.1 tsc 옵션

tsc 명령은 타입스크립트의 대부분 옵션을 -- 플래그로 사용할 수 있다.

tsc --help : 일반적으로 사용하는 CLI 플래그 목록 가져오기

tsc --all 전체 tsc 구성 옵션 목록 확인



#### 13.1.1 pretty 모드

tsc CLI는 색상과 간격의 스타일을 지정해 가독성을 높이는 pretty 모드를 지원한다. 출력 터미널이 여러 색상 텍스트를 지원하는 것을 감지하면 기본적으로 pretty 모드로 설정된다.

색이 없는 CLI 출력을 원하면 tsc 명령에 명시적으로 --pretty false 를 사용한다.



#### 13.1.2 watch 모드

\-w | --watch

watch 모드를 사용하면 종료대신 타입스크립트를 무기한 실행 상태로 유지하고 모든 오류의 실시간 목록을 가져와 터미널을 지속적으로 업데이트한다.

watch 모드는 여러 파일에 걸친 리팩토링 같은 대규모 변경 작업시 특히 유용하다. 타입스크립트의 타입 오류를 일종의 체크리스트로 사용해 정리할 항목으로 사용 가능하다.



### 13.2 TSConfig 파일

모든 파일 이름과 구성 옵션을 항상 tsc에 제공하는 대신, 대부분의 구성 옵션을 디렉터리의 tsconfig.json 파일에 구체적으로 명시할 수 있다.

tsconfig.json은 해당 디렉터리가 타입스크립트 프로젝트의 루트임을 나타낸다. 디렉터리에서 tsc를 실행하면 해당 tsconfig.json 파일의 모든 구성 옵션을 읽는다.

tsc 명령에 tsconfig.json 파일이 있는 디렉터리 경로 또는 tsc가 tsconfig.json 대신 사용할 파일 경로를 -p 또는 --project 플래그에 전달한다.



#### 13.2.1 tsc --init

tsc 명령줄에는 tsconfig.json 파일을 생성하기 위한 --init 명령이 포함되어 있다. 새로 생성된 TSConfig 파일에는 구성 문서에 대한 링크와 사용을 간략하게 설명하는 한 줄 주석과 함께 허용되는 대부분의 타입스크립트 구성 옵션에 대한 링크도 포함된다.



#### 13.2.2 CLI vs 구성

tsc --init에 따라 생성된 TSConfig 파일을 살펴보면 해당 파일의 구성 옵션이 compiler Options 객체 내에 있음을 확인할 수 있다. CLI와 TSConfig 파일에서 사용 가능한 대부분의 옵션은 다음 두 범주 중 하나로 분류된다.

* 컴파일러: 포함된 각 파일이 타입스크립트에 따라 컴파일되거나 타입을 확인하는 방법
* 파일: 타입스크립트가 실행될 파일과 실행되지 않은 파일



### 13.3 파일 포함

기본적으로 tsc는 현재 디렉터리 하위 디렉터리에 있는 공개된 모든 .ts 파일에서 실행되고 숨겨진 디렉터리와 node\_modules 디렉터리는 무시한다. 타입스크립트 구성은 실행 파일 목록을 수정할 수 있다.



#### 13.3.1 include

파일을 포함하는 가장 흔한 방법은 tsconfig.json의 최상위 include 속성을 사용하는 방법이다. include 속성에 타입스크립트 컴파일에 포함할 디렉터리와 파일을 설명하는 문자열 배열을 명시한다. 포함할 파일을 더 세밀하게 제어하기 위해 글로브 와일드카드가 허용된다.

```json
{
    "include": ["src", "typings/**/*.d.ts"]
    // src 디렉터리 내 모든 파일과 typings 디렉터리 하위의 중첩된 디렉터리의 .d.ts 파일을 포함한다.
}
```



#### 13.3.2 exclude

프로젝트의 include 파일 목록에 타입스크립트로 컴파일할 수 없는 파일이 포함되는 경우가 있다. 타입스크립트는 TSConfig 파일의 최상위 "exclude" 속성에 경로를 지정하고 include에서 경로를 생략한다. include처럼 타입스크립트 컴파일에서 제외할 디렉터리와 파일을 설명하는 문자열 배열이 허용된다.

기본적으로 exclude에는 컴파일된 외부 라이브러리 파일에 대해 타입스크립트 컴파일러가 실행되지 않도록 \["node\_modules", "bower\_components", "jspm\_packages"]가 포함된다.

```json
{
    "exclude": ["**/external", "node_modules"],
    "include": ["src"]
}
```

exclude는 include의 시작 목록에서 파일을 제거하는 작업만 수행한다.



### 13.4 대체 확장자

타입스크립트는 기본적으로 확장자가 .ts인 모든 파일을 읽을 수 있다. 그러나 일부 프로젝트에서는 JSON 모듈 또는 JSX 구문처럼 확장자가 다른 파일을 읽을 수 있어야 한다.



#### 13.4.1 JSX 구문

JSX 구문은 리액트 같은 UI 라이브러리에서 자주 사용된다. JSX 구문은 기술적으로 자바스크립트가 아니다. 타입스크립트의 타입 정의와 마찬가지로 자바스크립트로 컴파일되는 자바스크립트 구문의 확장이다.

파일에서 JSX 구문을 사용하기 위해서는 다음 두 가지를 수행해야 한다.

* 구성 옵션에서 "jsx" 컴파일러 옵션을 활성화한다.
* .tsx 확장자로 파일의 이름을 지정한다.



**jsx**

타입스크립트가 .tsx 파일에 대한 자바스크립트 코드를 내보내는 방법은 "jsx" 컴파일러 옵션에 사용되는 값으로 결정된다. "preserve" | "react" | "react-native" 중 하나를 사용하며, jsx에 대한 값은 tsc CLI 또는 TSConfig 파일에 제공한다.

```json
{
    "compilerOptions" : {
        "jsx": "preserve"
    }
}
```

바벨 같은 별도의 도구로 코드를 변환하는 것처럼 타입스크립트의 내장된 트랜스파일러를 직접 사용하지 않는 경우 "jsx"에 대해 허용된 값을 사용할 수 있다. Next.js 같은 프레임워크로 구축된 대부분의 웹 앱은 리액트 구성 및 컴파일 구문을 처리한다. 이러한 프레임워크 중 하나를 사용하면 타입스크립트의 내장 트랜스파일러를 직접 구성할 필요가 없다.



**.tsx 파일의 제네릭 화살표 함수**

.tsx 파일에서 화살표 함수에 대한 타입 인수 \<T>를 작성하려 하면 T 요소의 시작 태그에 대한 종료 태그가 없기에 구문 오류가 발생한다.

```typescript
const identity = <T>(input: T) => input
// Error : JSX element 'T' has no corresponding closing tag.
```

이러한 구문 모호성 해결을 위해 타입 인수에 = unknown 제약 조건을 추가할 수 있다.

```typescript
const identity = <T = unknown>(input: T) => input
```



#### 13.4.2 resolveJsonModule

resolveJsonModule 컴파일러 옵션을 true로 설정하면 .json 파일을 읽을 수 있다. 이렇게 하면 .json 파일을 마치 객체를 내보내는 .ts 파일인 것처럼 가져오고 해당 객체의 타입을 const 변수인 것처럼 유추한다.

객체가 포함된 JSON 파일이라면 구조 분해 가져오기를 사용한다.

```json
// activist.json
{
    "activist" : "Mary Astell"
}
```

```typescript
// usesActivist.ts
import { activist } from "./activist.json"

console.log(activist) // "Mary Astell"
```

array 또는 number 같은 다른 리터럴 타입을 포함한 JSON 파일이라면 import 구문으로 \*을 사용한다.

```json
// activists.json
[
    "Ida B. Wells",
    "Sojourner Truth"
]
```

```typescript
// useActivists.ts
import * as activists from "./activists.json"
console.log(`${activists.length} activists`) // "3 activists"
```



### 13.5 자바스크립트로 내보내기

바벨 같은 전용 컴파일러 도구의 등장으로 일부 프로젝트에서는 타입스크립트의 역할이 타입 검사만으로 축소되었지만, 타입스크립트 구문을 자바스크립트로 컴파일하기 위해 여전히 타입스크립트에 의존하고 있는 프로젝트도 많다.



#### 13.5.1 outDir

기본적으로 타입스크립트는 출력 파일을 해당 소스 파일과 동일 위치에 생성한다. 경우에 따라 출력 파일을 다른 폴더에 생성하는 것이 나을 수 있다. 타입스크립트의 outDir 컴파일러 옵션을 사용하면 출력 파일의 루트 디렉터리를 다르게 지정할 수 있다.

타입스크립트는 모든 입력 파일에서 가장 긴 공통 하위 경로를 찾아 출력 파일을 저장할 루트 디렉터리를 계산한다. 즉, 모든 입력 소스 파일을 단일 디렉터리에 배치하는 프로젝트는 해당 디렉터리를 루트로 처리한다.

rootDir 컴파일러 옵션은 해당 루트 디렉터리를 명시적으로 지정하기 위해 존재하지만 . 또는 src 이외의 값을 사용할 일은 거의 없다.



#### 13.5.2 target

타입스크립트는 ES3와 같은 오래된 환경에서도 실행할 수 있는 자바스크립트 출력 파일을 생성할 수 있다. 또한 자바스크립트의 최신 구문 기능을 지원한다.

타입스크립트는 자바스크립트 코드 구문을 지원하기 위해 어느 버전까지 변환해야 할지 지정하는 target 컴파일러 옵션을 제공한다. target을 지정하지 않으면 이전 버전과의 호환성을 위해 기본적으로 "es3"가 지정된다. tsc --init은 기본적으로 "es2016"을 지정하도록 설정되어있지만 가능한 최신 구문을 사용하는 것이 좋다.



#### 13.5.3 내보내기 선언

대부분의 패키지는 타입스크립트의 declaration 컴파일러 옵션을 사용해 소스 파일에서 .d.ts 출력 파일을 내보낸다.

.d.ts 출력 파일은 outDir 옵션에 따라 .js 파일과 동일한 출력 규칙에 따라 내보내진다.



**emitDeclarationOnly**

declaration 컴파일러 옵션에 대한 특별한 추가로 타입스크립트가 .js와 .jsx 파일 없이 선언 파일만 내보내도록 지시하는 emitDeclarationOnly 컴파일러 옵션이 존재한다. 이는 외부 도구를 사용해 출력 자바스크립트를 생성하지만 여전히 타입스크립트를 사용해 출력 선언 파일을 생성하려는 프로젝트에 유용하다.

emitDeclarationOnly가 활성화된 경우 declaration 또는 composite 컴파일러 옵션을 활성화해야 한다.



#### 13.5.4 소스 맵

소스 맵은 출력 파일의 내용이 원본 소스 파일과 어떻게 일치하는지에 대한 설명이다. 소스 맵은 출력 파일을 탐색할 때 디버거 같은 개발자 도구에서 원본 소스 코드를 표시하도록 설정한다.



**sourceMap**

타입스크립트의 sourceMap 컴파일러 옵션을 사용하면 .js 또는 .jsx 출력 파일과 함께 .js.map 또는 .jsx.map 소스 맵을 출력할 수 있다. 그렇지 않으면 소스 맵 파일에 해당 출력 자바스크립트 파일과 동일한 이름으로 동일 디렉터리에 생성된다.



**declarationMap**

타입스크립트는 .d.ts 선언 파일에 대한 소스 맵을 생성할 수도 있다. declarationMap 컴파일러 옵션은 원본 소스 파일에 해당하는 각 .d.ts에 대한 .d.ts.map 소스 맵을 생성하도록 지시한다. declarationMap을 활성화하면 IDE에서 \[Go to Definition] 기능으로 원본 소스 파일로 이동할 수 있다.



#### 13.5.5 noEmit

다른 도구를 이용해 소스 파일을 컴파일하고, 자바스크립트를 출력하는 프로젝트에서 타입스크립트는 파일 생성을 모두 건너뛰도록 지시할 수 있다. noEmit 컴파일러 옵션을 활성화하면 타입스크립트가 온전히 타입 검사기로만 작동한다.



### 13.6 타입 검사

대부분의 타입스크립트 구성 옵션은 타입 검사기를 제어한다.

구성 옵션을 느슨하게 구성하면 오류가 확실할 때만 타입 검사 오류를 보고하도록 하거나,\
구성 옵션을 엄격하게 구성하면 거의 모든 코드를 올바르게 잘 입력하도록 요구할 수 있다.



#### 13.6.1 lib

타입스크립트는 자바스크립트 API에 대한 기본적인 타입 정의와 브라우저 환경에 있는 타입 정의를 포함한다. 뿐만 아니라 지정한 target과 일치하는 최신 자바스크립트 기능을 위한 API도 포함되어 있다.

```json
{
    "compilerOptions" : {
        "lib" : ["dom", "es2021"]
    }
}
```

올바른 런타임 폴리필을 모두 제공하지 않는 상태에서 lib를 수정하지 않도록 주의해야 한다.

ES2020까지 지원하는 플랫폼에서 실행되는 lib이 "es2021"로 설정된 프로젝트에서는 타입 검사 오류가 없을 수 있지만 ES2021 이상에 정의된 API를 사용하려하면 런타임 오류가 발생할 수 있다.

{% hint style="info" %}
lib 옵션은 내장된 언어 API를 나타내는데 사용하고,\
target 옵션은 존재하는 구문 기능을 나타내는데 사용한다.
{% endhint %}



#### 13.6.2 skipLibCheck

타입스크립트는 소스 코드에 명시적으로 포함되지 않은 선언 파일에서 타입 검사를 건너뛰도록 하는 skipLibCheck 컴파일러 옵션을 제공한다. skipLibCheck 옵션은 공유된 라이브러리의 정의가 서로 다르고 충돌할 수 있는 패키지 의존성을 많이 사용하는 어플리케이션에 유용하다.

skipLibCheck는 타입 검사 일부를 건너뛰는 작업으로 타입스크립트 성능을 개선한다. 따라서 대부분의 프로젝트에서 skipLibCheck 옵션을 활성화하는 것이 좋다.



#### 13.6.3 엄격 모드

타입스크립트의 타입 검사 컴파일러 옵션 대부분은 타입스크립트의 엄격 모드로 그룹화된다. 엄격한 컴파일러 옵션은 기본적으로 false이고, 활성화되면 타입 검사기에 일부 추가적인 검사를 켜도록 지시한다.

strict 컴파일러 옵션을 활성화하면 모든 엄격 모드 검사가 활성화된다.

특정 검사를 제외한 모든 엄격 모드 검사를 활성화하고 싶다면 strict를 활성화하고 특정 검사를 명시적으로 비활성화할 수 있다.



[**noImplicitAny**](https://www.typescriptlang.org/tsconfig#noImplicitAny)

[**strictBindCallApply**](https://www.typescriptlang.org/tsconfig#strictBindCallApply)

[**strictFunctionTypes**](https://www.typescriptlang.org/tsconfig#strictFunctionTypes)

[**strictNullChecks**](https://www.typescriptlang.org/tsconfig#strictNullChecks)

[**strictPropertyInitialization**](https://www.typescriptlang.org/tsconfig#strictPropertyInitialization)

[**useUnknwonInCatchVariables**](https://www.typescriptlang.org/tsconfig#useUnknwonInCatchVariables)



### 13.7 모듈

#### 13.7.1 module

타입스크립트는 어떤 모듈 시스템으로 변환된 코드를 사용할지 결정하기 위해 module 컴파일러 옵션을 제공한다. ECMA스크립트 모듈로 소스 코드를 작성할 때 타입스크립트는 module 값에 따라 export와 import 문을 다른 모듈 시스템으로 변환할 수 있다.



#### 13.7.2 [moduleResolution](https://www.typescriptlang.org/ko/docs/handbook/module-resolution.html)

모듈 해석은 import에서 가져온 경로가 module에 매핑되는 과정이다. 타입스크립트는 해당 과정에 로직을 지정하는 데 사용할 수 있는 moduleResolution 옵션을 제공한다. 일반적으로 다음의 두 전략 중 하나를 제공하는 것을 선호한다.

* node: 기존 Node.js와 같은 CommonJS 리졸버에서 사용하는 동작
* nodenext: ECMA스크립트 모듈에 대해 지정된 동작에 맞게 조정



#### 13.7.3 CommonJS와의 상호 운용성

자바스크립트 모듈로 작업할 때 모듈의 기본 내보내기와 네임스페이스 출력 간에는 차이점이 있다. 모듈의 기본 내보내기는 내보낸 객체의 .default 속성이다. 모듈의 네임스페이스 내보내기는 내보낸 객체 자체이다.

타입스크립트의 타입 시스템은 ECMA스크립트 모듈 측면에서 파일 가져오기와 내보내기에 대한 합의를 만든다. 그러나 npm 패키지에 의존하는 경우 의존성 중 일부는 CommonJS 모듈로 배포된다. 또한 ECMA스크립트 모듈 규칙을 준수하는 일부 패키지는 기본 내보내기를 포함하지 않지만 개발자들은 간결한 기본 가져오기를 선호한다. 타입스크에는 모듈 형식 간 상호 운용성을 개선하는 몇 가지 컴파일러 옵션을 제공한다.



[**esModuleInterop**](https://www.typescriptlang.org/tsconfig#esModuleInterop)

[**allowSyntheticDefaultImports**](https://www.typescriptlang.org/tsconfig#allowSyntheticDefaultImports)



#### 13.7.4 isolatedModules

바벨과 같은 외부 트랜스파일러는 타입 시스템 정보를 사용해 자바스크립트를 내보낼 수 없다. 프로젝트에서 타입스크립트가 아닌 다른 도구를 사용해 자바스크립트로 변환하는 경우에는 isolationModules를 활성화하는 것이 좋다.



### 13.8 자바스크립트

모든 소스 파일을 타입스크립트로 작성할 필요는 없다. 타입스크립트는 기본적으로 .js와 .jsx 확장자를 가진 파일을 무시하지만, allowJs와 checkJs 컴파일러 옵션을 사용하면 자바스크립트 파일을 읽고 컴파일하고 타입 검사도 할 수 있다.



#### 13.8.1 allowJs

allowJs 컴파일러 옵션은 자바스크립트 파일에 선언된 구문을 타입스크립트 파일에서 타입 검사를 하도록 허용한다. jsx 컴파일러 옵션과 결합하면 .jsx 파일도 검사할 수 있다.

allowJs가 활성화되지 않으면 import 문은 알려진 타입을 갖지 못한다.

또한 allowJs는 ECMA스크립트 target에 맞게 컴파일되고 자바스크립트로 내보내진 파일 목록에 자바스크립트 파일을 추가한다. allowJs 옵션이 활성화된 경우 소스 맵과 선언 파일도 생성된다.



#### 13.8.2 checkJs

자바스크립트 파일도 타입 검사가 가능하다. checkJs 컴파일러 옵션은 allowJs를 true로 설정하고, .js 와 .jsx 파일에서 타입 검사기를 활성화하는 역할을 수행한다. 따라서 checkJs를 활성화하면 타입스크립트가 자바스크립트 파일을 타입스크립트 파일인 것처럼 처리한다. 타입 불일치, 철자 틀린 변수명 등 타입스크립트에서 일반적으로 발생하는 모든 오류를 발생시킬 수 있다.



**@ts-check**

파일 상단에 // @ts-check 주석을 사용해 파일별로 checkJs를 활성화할 수 있다.



#### 13.8.3 JSDoc 지원

allowJs와 checkJs가 활성화되면 타입스크립트는 코드의 모든 JSDoc 정의를 인식한다.

[참고](https://www.typescriptlang.org/docs/handbook/jsdoc-supported-types.html)



### 13.9 구성 확장

더 많은 타입스크립트 프로젝트를 진행하면서 반복적으로 동일한 프로젝트 설정을 작성하게 된다. 반복되는 설정을 확장하거나 복사하도록 할 수 있다.

#### 13.9.1 [extends](https://www.typescriptlang.org/tsconfig#extends)

TSConfig는 extends 구성 옵션을 사용해 다른 TSConfig에서 확장할 수 있다.&#x20;

extends 속성은 절대경로 / 상대경로 중 하나를 사용한다.

절대경로라면 npm 모듈에서 TSConfig를 확장함을 나타낸다.



#### 13.9.2 구성 베이스

특정 런타임 환경에 맞게 미리 만들어진 베이스 TSConfig 파일로 시작할 수도 있다.

미리 만들어진 구성 베이스는 @tsconfig/recommended 또는 @tsconfig/node16과 같은 @tsconfig/ 아래의 npm 패키지 레지스트리에서 사용할 수 있다.

[참고](https://github.com/tsconfig/bases#centralized-recommendations-for-tsconfig-bases)



### 13.10 프로젝트 레퍼런스

대규모 프로젝트에서는 프로젝트의 서로 다른 영역에 서로 다른 구성 파일을 사용하는 것이 유용할 수 있다. 타입스크립트에서는 여러 프로젝트를 함께 빌드하는 프로젝트 레퍼런스 시스템을 정의할 수 있다. 프로젝트 레퍼런스 설정 작업은 단일 TSConfig 파일을 사용하는 것보다 작업이 많지만 핵심적인 이점이 몇 가지 있다.

* 특정 코드 영역에 대해 다른 컴파일러 옵션을 지정할 수 있음
* 타입스크립트는 개별 프로젝트에 대한 빌드 출력을 캐시할 수 있어 빌드 시간이 빨라진다.
* 프로젝트 레퍼런스는 코드의 개별 영역을 구조화하는 데 유용한 [의존성 트리](#user-content-fn-1)[^1]를 실행한다.

프로젝트 레퍼런스를 활성화하기 위해 프로젝트 설정을 구축하는 방법

* TSConfig의 composite 모드는 다중 TSConfig 빌드 모드에 적합한 방식으로 작동하도록 강제한다
* TSConfig의 references는 TSConfig가 의존하는 복합 TSConfig를 나타낸다.
* 빌드 모드는 복합 TSConfig 레퍼런스를 사용해 파일 빌드를 조정한다.



#### 13.10.1 composite

타입스크립트는 프로젝트에서 composite 구성 옵션을 선택해 파일 시스템 입력과 출력이 제약 조건을 준수함을 나타낸다. 이 과정을 통해 빌드 도구가 빌드 입력과 빌드 출력을 비교해 최신 상태인지 여부를 쉽게 확인할 수 있다.

composite이 true일 때는 다음과 같다.

* rootDir 설정이 명시적으로 설정하지 않았다면 기본적으로 TSConfig 파일이 포함된 디렉터리로 설정된다.
* 모든 구현 파일은 포함된 패턴과 일치하거나 파일 배열에 나열되어야 한다.
* declaration 컴파일러 옵션은 반드시 true여야 한다.

이러한 변경은 타입스크립트가 프로젝트에 대한 모든 입력 파일과 일치하는 .d.ts 파일을 생성하도록 강제할 때 유용하다. composite 옵션은 reference 옵션과 함께 사용할 때 가장 유용하다.



#### 13.10.2 references

타입스크립트 프로젝트는 TSConfig에 references 설정이 있는 복합 타입스크립트 프로젝트에서 생성된 출력에 의존함을 나타낼 수 있다. 참조된 프로젝트에서 모듈을 가져오는 것은 출력 .d.ts 선언 파일에서 가져오는 것으로 타입 시스템에 표시된다.

references 옵션은 빌드 모드와 함께 사용할 때 가장 유용하다.



#### 13.10.3 빌드 모드

코드 영역이 프로젝트 레퍼런스를 사용하도록 설정되면 빌드 모드에서 -b 또는 --b CLI 플래그를 통해 tsc를 사용할 수 있다. 빌드 모드는 tsc를 프로젝트 빌드 코디네이터 같은 것으로 향상시킨다. 이를 통해 tsc는 내용과 파일 출력이 마지막으로 생성된 시간을 기준으로 마지막 빌드 이후 변경된 프로젝트만 다시 빌드한다.

빌드 모드는 TSConfig가 제공될 때 다음 내용을 수행한다.

1. TSConfig의 참조된 프로젝트를 찾는다.
2. 최신 상태인지 감지한다.
3. 오래된 프로젝트를 올바른 순서로 빌드한다.
4. 제공된 TSConfig 또는 TSConfig의 의존성이 변경된 경우 빌드한다.

빌드 모드 기능은 최신 프로젝트를 다시 빌드하는 것을 건너뛰도록 해 빌드 성능을 크게 향상시킨다.



**코디네이터 구성**

저장소에서 타입스크립트 프로젝트 레퍼런스를 설정하는 편리한 방법은 빈 파일 배열과 저장소의 모든 프로젝트 레퍼런스에 대한 레퍼런스를 사용해 최상위 레벨의 tsconfig.json을 설정하는 것이다. 최상위 TSConfig는 타입스크립트가 파일 자체를 빌드하도록 지시하지 않는다. 대신 필요에 따라 참조된 프로젝트를 빌드하도록 타입스크립트에 알리는 역할만 한다.&#x20;

package.json 안에 tsc -b를 바로 호출하는 build 또는 compile 스크립트를 표준화하는 것도 좋다.



**빌드 모드 옵션**

빌드 모드는 몇 가지 빌드에 특화된 CLI 옵션을 지원한다.

* \--clean: 지정된 프로젝트의 출력을 삭제한다
* \--dry: 수행할 작업을 보여주지만 실제로는 빌드하지 않는다.
* \--force: 모든 프로젝트가 오래된 것처럼 작동한다.
* \-w | -watch: 일반적인 타입스크립트 watch 모드



### 13.11 마치며

* pretty 모드와 watch 모드를 포함한 tsc 사용법
* tsc --init을 사용해 생성한 것을 포함한 TSConfig 파일 사용법
* 타입스크립트 컴파일러에서 포함될 파일의 변경
* .tsx 파일의 JSX 구문과 .json 파일의 JSON 구문 허용
* 디렉터리, ECMA스크립트 버전 target, 선언 파일, 파일의 소스 맵 출력 변경
* 컴파일에 사용되는 내장 라이브러리 타입 변경
* 엄격 모드와 noImplicitAny, strictNullCheck와 같은 유용한 엄격 플래그
* 다양한 모듈 시스템 지원 및 모듈 해석 변경
* 자바스크립트 파일 포함 허용과 해당 파일에 대한 타입 검사 선택
* 파일 간 구성 옵션 공유를 위한 extends 사용법
* 여러 TSConfig 빌드 조정을 위한 프로젝트 레퍼런스 및 빌드 모드 사용법



\---

역대급으로 힘든 챕터였다.

잘 모르는 내용을 어색하고 이상한 번역으로 공부하려니 죽을맛이다...

typescript 공식문서를 보면서 여차저차 공부했다.

복습할때는 공식문서만 참고하자



[^1]: 특정 프로젝트가 다른 특정 프로젝트에서 파일을 가져오는 것만 허용
