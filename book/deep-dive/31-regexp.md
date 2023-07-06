# 31장 RegExp

### 31.1 정규 표현식이란?

정규 표현식이란 일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어이다.

대부분의 프로그래밍 언어와 코드 에디터에 내장되어 있고, JS에는 ES3부터 도입했다고 한다.

정규 표현식은 문자열을 대상으로 패턴 매친 기능을 제공한다. 패턴 매칭 기능이란 특정 패턴과 일치하는 문자열을 검색하거나 추출 또는 치환할 수 있는 기능을 말한다.

예를 들어, 휴대폰 전화번호를 확인하는 기능을 구현한다고 가정해보자. 한국의 전화번호는 3개의 숫자 - 4개의 숫자 - 4개의 숫자 로 이루어져 있고, 이를 정규 표현식 없이 확인하려면 반복문이나 조건문을 활용해 한 문자씩 확인하는 방법을 사용해야 한다.

정규 표현식을 사용한다면 다음과 같이 패턴을 정의하고 테스트하는 것으로 확인할 수 있다.

```jsx
const tel = '010-1234-5678'
const regExp = /^\\d{3}-\\d{4}-\\d{4}$/
regExp.test(tel) // true
```

여러 기호를 혼합하기에 가독성이 좋지는 않지만, 비교적 간단하다는 점은 부정할 수 없다.



### 31.2 정규 표현식의 생성

정규식 리터럴과 RegExp 생성자 함수를 사용해 생성할 수 있다.

**정규식 리터럴**

```jsx
const target = "Is this all there is?"
const regExp = /is/i
regexp.test(target) // true
```

**RegExp 생성자 함수**

```jsx
const target = "Is this all there is?"
const regExp = new RegExp(/is/i)
regexp.test(target) // true

// new RegExp(pattern[, flags])
```



### 31.3 RegExp 메서드

#### 31.3.1 RegExp.exec

인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색해 매칭 결과를 배열로 반환한다. 결과가 없는 경우 null을 반환한다. g 플래그를 지정해도 첫 번째 매칭 결과만 반환한다.

```jsx
const target = "Is this all there is?"
const regExp = /is/

regExp.exec(target)
// ['is', index: 5, input: 'Is this all there is?', groups: undefined]
```

#### 31.3.2 RegExp.test

test 메서드는 인수로 전달받은 문자열에 대해 정규 표현식 패턴을 검색해 매칭 결과를 불리언 값으로 반환한다.

```jsx
const target = "Is this all there is?"
const regExp = /is/

regExp.test(target) // true
```

#### 31.3.3 RegExp.match

match 메서드는 대상 문자열과 인수로 전달받은 정규 표현식과의 매칭 결과를 배열로 반환한다. g 플래그를 지정하면 모든 매칭 결과를 배열로 반환한다.

```jsx
const target = "Is this all there is?"
const regExp1 = /is/
regExp1.match(target)
// ['is', index: 5, input: 'Is this all there is?', groups: undefined]

const regExp2 = /is/g
regExp2.match(target) // ["is", "is"]
```



### 31.4 플래그

패턴과 함께 정규 표현식을 구성하는 플래그는 정규 표현식의 검색 방식을 설정하기 위해 사용한다. 옵션이므로 선택할 수 있고, 순서와 상관없이 하나 이상의 플래그를 동시에 설정할 수 있다.

| 플래그 | 의미          | 설명                                 |
| --- | ----------- | ---------------------------------- |
| i   | Ignore case | 대소문자 구분하지 않고 패턴을 검색한다.             |
| g   | Global      | 대상 문자열 내 패턴과 일치하는 모든 문자열을 전역 검색한다. |
| m   | Multi line  | 문자열의 행이 바뀌더라도 패턴 검색을 계속한다.         |



### 31.5 패턴

정규 표현식은 패턴과 플래그로 구성된다. \*\*`패턴`\*\*은 문자열의 일정한 규칙을 표현하기 위해 사용하며, `플래그`는 정규 표현식의 검색 방식을 설정하기 위해 사용한다.

패턴은 /로 열고 닫으며 문자열의 따옴표는 생략한다. 또한 패턴은 특별한 의미를 가지는 메타인자나 기호로 표현할 수 있다. 어떤 문자열 내 패턴과 일치하는 문자열이 존재할 때 ‘정규 표현식과 매치한다’고 표현한다.

#### 31.5.1 문자열 검색

```jsx
/is/ // is 문자열과 매치하는 패턴. 플래그가 생략되어 정규표현식과 매치한 첫 결과만 반환한다.
/is/i // is 문자열과 매치하는 패턴. 대소문자 구분 X
/is/g // is 문자열과 매치하는 패턴. 전역 검색
```

#### 31.5.2 임의의 문자열 검색

.은 임의의 문자 한 개를 의미한다. 내용은 상관없다.

```jsx
const target = "Is this all there is?"
const regExp = /.../g // 임의의 3자리 문자열과 매치하는 패턴. 전역 검색

target.match(regExp)
// [ 'Is ', 'thi', 's a', 'll ', 'the', 're ', 'is?' ]
```

#### 31.5.3 반복 검색

{m,n}은 앞 패턴이 최소 m번, 최대 n번 반복되는 문자열을 의미한다.

{n}은 앞 패턴이 n번 반복되는 문자열을 의미한다.

{n,}은 앞 패턴이 최소 n번 반복되는 문자열을 의미한다.

```jsx
const target = "A AA B BB Aa Bb AAA"

const regExp = /A{1,2}/g // A 가 최소 1번 최대 2번 반복되는 패턴
target.match(regExp) // ['A', 'AA', 'A', 'AA', 'A']

const regExp2 = /A{2}/g // A 가 2번 반복되는 패턴
target.match(regExp2) // ['AA', 'AA']

const regExp3 = /A{2,}/g // A 가 최소 2번 반복되는 패턴
target.match(regExp3) // ['AA', 'AAA']
```

\+는 앞 패턴이 최소 한 번 이상 반복되는 문자열을 의미한다. 즉, {1,}과 같다.

?는 앞 패턴이 최대 한 번(0번 포함) 이상 반복되는 문자열을 의미한다. 즉, {0,1}과 같다.

#### 31.5.4 OR 검색

|는 or의 의미를 갖는다.

```jsx
const target = "A AA B BB Aa Bb"
const regExp = /A|B/g // A 또는 B가 포함된 패턴
target.match(regExp)
// ['A', 'A', 'A', 'B', 'B', 'B', 'A', 'B']
```

분해되지 않은 단어 레벨로 검색하기 위해 +를 함께 사용한다.

```jsx
const target = "A AA B BB Aa Bb"
const regExp = /A+|B+/g // A 또는 B가 포함된 단어 레벨의 패턴
target.match(regExp)
// ['A', 'AA', 'B', 'BB', 'A', 'B']
```

|를 간단히 표현하려면 \[ ]를 사용한다.

```jsx
const target = "A AA B BB Aa Bb"
const regExp = /[AB]+/g // A 또는 B가 포함된 단어 레벨의 패턴
target.match(regExp)
// ['A', 'AA', 'B', 'BB', 'A', 'B']
```

범위를 지정하려면 \[]내에 -를 사용한다.

```jsx
const target = "A AA B BB Aa Bb 12"
const regExp = /[A-Z]+/g // A ~ Z 가 한 번 이상 반복되는 문자열 패턴
target.match(regExp)
// ['A', 'AA', 'B', 'BB', 'A', 'B']

// 대문자 알파벳 [A-Z]
// 소문자 알파벳 [a-z]
// 숫자 [0-9] === [\\d] <=> 숫자가 아닌 [\\D]
// 알파벳, 숫자, 언더스코어 [A-Za-z0-9_] === [\\w] <==> [\\W]
```

#### 31.5.5 NOT 검색

\[…] 내의 ^ 는 not의 의미를 갖는다.

```jsx
// [^0-9] 숫자를 제외한 문자 === [\\D]
```

#### 31.5.6 시작 위치로 검색

\[…] 밖의 ^ 는 문자열의 시작을 의미한다.

```jsx
// /^http/ http로 시작하는 패턴
```

#### 31.5.7 마지막 위치로 검색

$는 문자열의 마지막을 의미한다.

```jsx
// /com$/ com으로 끝나는 패턴
```



### 31.6 자주 사용하는 정규 표현식

#### 31.6.1 특정 단어로 시작하는지 검사

```jsx
const url = '<https://example.com>';

/^https?:\\/\\//.test(url) // true
// http:// 또는 <https://로> 시작하는지 검사
```

#### 31.6.2 특정 단어로 끝나는지 검사

```jsx
const fileName = 'index.html';
/html$/.test(fileName) // true
// html로 끝나는지 검사
```

#### 31.6.3 숫자로만 이루어진 문자열 검사

```jsx
const target = '12345';
/^\\d+$/.test(target) // true
// 처음과 끝이 숫자인 패턴이 한번 이상 반복되는 문자열
```

#### 31.6.4 하나 이상의 공백 검사

```jsx
const target = " Hi!";
/^[\\s]+/.test(target) // true
```

#### 31.6.5 아이디로 사용 가능한지 검사

```jsx
const id = 'abc123';
/^[A-Za-z0-9]{4,10}$/.test(id) // true
// 알파벳 대소문자 또는 숫자로 이루어진 4~10자리
```

#### 31.6.6 메일 주소 형식에 맞는지 검사

```jsx
const email = "onemorebottlee@gmail.com";
/^[0-9a-zA-Z]([-_\\.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_\\.]?[0-9a-zA-Z])*\\.[a-zA-Z]{2,3}/.test(email)
// true
```

#### 31.6.7 핸드폰 형식에 맞는지 검사

```jsx
const cellPhone = '010-1234-5678';
/^\\d{3}-\\d{3-4}-\\d{4}$/.test(cellPhone) // true
```

#### 31.6.8 특수 문자 포함 여부 검사

```jsx
const target = 'abc#123';
(/[^A-Za-z0-9]/gi).test(target) // true

// 혹은
(/[\\{\\}\\[\\]\\/?.,;:|\\]*~`!^\\-_+<>@\\#$%&\\\\\\=\\(\\'\\"]/gi).test(target) // true
```
