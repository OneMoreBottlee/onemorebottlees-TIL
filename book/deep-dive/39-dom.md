# 39장 DOM ⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐

이전 장에서 살펴본 바와 같이 브라우저의 렌더링 엔진이 HTML 문서를 파싱하여 브라우저가 이해할 수 있는 자료구조 DOM을 생성한다. DOM은 HTML 문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 API, 즉 프로퍼티와 메서드를 제공하는 트리 자료구조다.

### 39.1 노드

#### 39.1.1 HTML 요소와 노드 객체

HTML 요소는 HTML 문서를 구성하는 개별적인 요소를 의미한다.

```javascript
<div class="greeting"> Hello </div>
//시작태그 어트리뷰트     콘텐츠  종료태그
```

HTML 요소는 렌더링 엔진에 의해 파싱되어 DOM을 구성하는 요소 노드 객체로 변환된다. 이때 HTML 요소의 어트리뷰트는 어트리뷰트 노드로, HTML 요소의 텍스트 콘텐츠는 텍스트 노드로 변환된다.&#x20;

HTML 문서는 HTML 요소들의 집합으로 이루어지며, 각 요소는 중첩 관계를 갖는다. 즉, HTML 요소의 콘텐츠 영역에는 텍스트뿐 아니라 다른 HTML 요소도 포함될 수 있다. 이때 HTML 요소 간에는 중첩 관계에 의한 계층적 부자 관계가 형성된다. 이러한 HTML 요소 간 관계를 반영하여 모든 노드 객체들을 트리 자료 구조로 구성하게 된다.



트리 자료 구조

노드들의 계층 구조로 이루어진다. 부모 노드와 자식 노드로 구성되어 노드 간 계층 구조를 표현하는 비선형 자료구조를 의미한다. 하나의 최상위 노드로부터 시작하며, 0개 이상의 자식 노드를 갖는다. 자식 노드가 없는 노드는 리프 노드라 한다. 이러한노드 객체들로 구성된 트리 자료구조를 DOM이라 한다. 트리로 구조화 되어 DOM 트리라 부르기도 한다.



#### 39.1.2 노드 객체의 타입

DOM은 노드 객체의 계층적 구조로 구성된다. 노드 객체는 12개의 종류가 있고 상속 구조를 가지는게 특징이다. 대표적인 노드 객체는 다음과 같다.



*   문서 노드

    DOM 트리 최상위에 존재하는 루트 노드로서 document 객체를 가리킨다. 브라우저가 렌더링한 HTML 문서 전체를 가리키는 객체로서 전역 객체 window의 document 프로퍼티에 바인딩되어 있다. 따라서 window.document 또는 document로 참조 가능하다.\
    브라우저 환경의 모든 JS 코드는 script 태그에 의해 분리되어 있어도 하나의 전역 객체 window를 공유한다. 따라서 HTML 문서당 document 객체는 유일하다. DOM 트리의 루트 노드이므로 진입점 역할을 담당한다.
*   요소 노드

    HTML 요소를 가리키는 객체다. 요소 간 중첩에 의해 부자 관계를 가지며, 이 관계를 통해 정보를 구조화한다. 따라서, 문서의 구조를 표현한다고 할 수 있다.
*   어트리뷰트 노드

    HTML 요소의 어트리뷰트를 가리키는 객체다. 어트리뷰트가 지정된 HTML 요소의 요소 노드와 연결되어 있다. (단, 부모 노드와 연결되어 있지 않음.) 따라서 어트리뷰트 노드에 접근하려면 먼저 요소 노드에 접근해야 한다.
*   텍스트 노드

    HTML 요소의 텍스트를 가리키는 객체다. 문서의 정보를 표현하고, 요소 노드의 자식 노드이며, 리프 노드이다. 즉, 텍스트 노드는 DOM 트리의 최종단이다. 따라서 텍스트 노드에 접근하려면 먼저 요소 노드에 접근해야 한다.



이 외에도 주석을 위한 Comment 노드, DocumentType 노드, DocumentFragment 노드 등 총 12개의 노드 타입이 있다.



#### 39.1.3 노드 객체의 상속 구조 (p.681 \~ 그림 보면서 같이 공부)

DOM을 구성하는 노드 객체는 자신의 구조와 정보를 제어할 수 있는 DOM API를 사용할 수 있다. 이를 통해 노드 객체는 자신의 부모, 형제, 자식 노드를 탐색할 수 있으며, 어트리뷰트와 텍스트를 조작할 수 있다.

노드 객체는 표준 빌트인 객체가 아닌 브라우저 환경에서 추가적으로 제공하는 호스트 객체다.

Object > EventTarget > Node 인터페이스를 상속받는다.

노드 객체에는 노드 객체의 종류(타입)에 상관없이 모든 노드 객체가 공통으로 갖는 기능도 있고, 타입별 기능도 있다.

요소 노드 객체는 HTML 요소가 갖는 공통적 기능이 있다. 예를 들어, input과 div는 모두 style 프로퍼티가 있다. 이처럼 HTML 요소가 갖는 공통적 기능은 HTMLElement 인터페이스가 제공한다.

요소의 종류에 따라 고유한 기능도 있다. 예를 들어 input은 value 프로퍼티가 필요하지만 div에는 필요하지 않다. 따라서 필요한 기능을 제공하는 인터페이스가 HTML 요소의 종류에 따라 각각 다르다.

이처럼 노드 객체는 공통 기능일수록 프로토타입 체인의 상위에, 개별적 기능일 수록 하위에 프로토타입 체인을 구축하여 노드 객체에 필요한 기능, 즉 프로퍼티와 메서드를 제공하는 상속 구조를 갖는다.



**2줄 요약**

> DOM 은 HTML 문서의 계층적 구조와 정보를 표현하고, 노드 타입에 따라 필요한 기능을 DOM API로 제공한다. DOM API를 통해 HTML 구조와 내용, 스타일을 동적으로 조작할 수 있다 !



### 39.2 요소 노드 취득

HTML 구조, 내용, 스타일링을 동적으로 조작하려면 요소 노드 취득이 우선이다.

예를 들어, h1 요소의 텍스트를 변경하려면 DOM 트리 내 존재하는 h1 요소 노드를 취득하고, 취득한 요소 노드의 자식 노드인 텍스트 노드를 변경하면 해당 h1 요소의 텍스트가 변경된다.

이처럼 요소 노드의 취득은 HTML 요소를 조작하는 시작점이다. 이를 위해 DOM은 요소 노드를 취득할 수 있는 다양한 메서드를 제공한다.

#### 39.2.1 id를 이용한 요소 노드 취득

Document.prototype.getElementById() 메서드는 인수로 전달한 id 어트리뷰트 값을 갖는 하나의 요소 노드를 탐색하여 반환한다.

id 값은 HTML 문서 내 유일한 값이어야 하며, class 어트리뷰트와 달리 공백 문자로 구분하여도 여러 개의 값을 가질 수 없다. 하지만 HTML 문서 내 중복된 id 값을 갖는 HTML 요소가 여러 개 존재하더라도 우리의 JS는 에러를 발생하지 않는다. 이러한 경우 첫 번째 요소 노드만 반환한다.



#### 39.2.2 태그 이름을 이용한 요소 노드 취득

Document.prototype.getElementsByTagName()

Element.prototype.getElementsByTagName()

메서드는 인수로 전달된 태그 이름을 갖는 모든 요소 노드들을 탐색하여 반환한다. 메서드에 포함된 Elements가 복수형인 것에서 알 수 있듯이 여러 개의 요소 노드 객체를 갖는 HTMLCollection 객체를 반환한다.

함수는 하나의 값만 반환할 수 있으므로 여러 개의 값을 반환하려면 배열이나 객체와 같은 자료구조에 담아 반환한다. HTML 문서의 모든 요소 노드를 취득하려면 인수로 \*을 전달한다.

Document - 메서드는 DOM의 루트 노드인 문서 노드, document를 통해 호출하며 DOM 전체에서 요소 노드를 탐색하여 반환한다.\
Element - 메서드는 특정 요소 노드를 통해 호출하며, 특정 요소 노드의 자손 노드 중 요소 노드를 탐색하여 반환한다.

탐색값이 없다면 빈 HTMLCollection 객체를 반환한다.



#### 39.2.3 class를 이용한 요소 노드 취득

Document.prototype.getElementsByClassName()

Element.prototype.getElementsByClassName()

메서드는 인수로 전달한 class 어트리뷰트 값을 갖는 모든 요소 노드들을 탐색하여 반환한다. 인수로 전달할 class 값은 공백으로 구분하여 여러 class를 지정할 수 있다.

getElementByTagName 메서드와 마찬가지로 \
Document - 메서드는 DOM의 루트 노드인 document를 통해 호출하며 DOM 전체에서 요소 노드를 탐색하여 반환한다.\
Element - 메서드는 특정 요소 노드를 통해 호출하며 특정 요소 노드의 자손 노드 중 요소 노드를 탐색하여 반환한다.

탐색값이 없다면 빈 HTMLCollection 객체를 반환한다.



#### 39.2.4 CSS 선택자를 이용한 요소 노드 취득

CSS 선택자는 스타일을 적용하고자 하는 HTML 요소를 특정할 때 사용하는 문법이다.

Document.prototype.querySelector()

Element.prototype.querySelector()

메서드는 인수로 전달한 CSS 선택자를 만족시키는 하나의 요소 노드를 탐색하여 반환한다.

* 인수로 전달한 CSS 선택자를 만족시키는 요소 노드가 여러 개인 경우 첫 번째 요소 노드만 반환한다.
* 인수로 전달된 CSS 선택자를 만족시키는 요소 노드가 존재하지 않는 경우 null을 반환한다.
* 인수로 전달한 CSS 선택자가 문법에 맞지 않는 경우 DOMException 에러가 발생한다.



Document.prototype.querySelectorAll()

Element.prototype.querySelectorAll()

메서드는 인수로 전달한 CSS 선택자를 만족시키는 모든 요소 노드를 탐색하여 반환한다.

* 인수로 전달된 CSS 선택자를 만족시키는 요소가 존재하지 않는 경우 빈 NodeList 객체를 반환한다.
* 인수로 전달한 CSS 선택자가 문법에 맞지 않는 경우 DOMException 에러가 발생한다.

HTML 요소의 모든 요소 노드를 취득하려면 인수로 \*를 전달한다.



getElementByTagName 메서드와 마찬가지로 \
Document - 메서드는 DOM의 루트 노드인 document를 통해 호출하며 DOM 전체에서 요소 노드를 탐색하여 반환한다.\
Element - 메서드는 특정 요소 노드를 통해 호출하며 특정 요소 노드의 자손 노드 중 요소 노드를 탐색하여 반환한다.



CSS 선택자 문법을 사용하는 querySelector, querySelectorAll 메서드는 위의 두 메서드보다 다소 느리다고 알려져있지만, 좀 더 구체적인 조건으로 요소 노드를 취득할 수 있고, 일관된 방식으로 요소 노드를 취득할 수 있다는 장점이 있다. 따라서 id 어트리뷰트가 있는 경우 외에는 querySelector, querySelectorAll 메서드를 권장한다고 한다.



#### 39.2.5 특정 요소 노드를 취득할 수 있는지 확인

Element.prototype.matches() 메서드는 인수로 전달한 CSS 선택자를 통해 특정 요소 노드를 취득할 수 있는지 확인한다. 이벤트 위임을 사용할 때 유용한 메서드이다.



#### 39.2.6 HTMLCollection과 NodeList

DOM 컬렉션 객체인 HTMLCollection과 NodeList는 DOM API가 여러 개의 결과값을 반환하기 위한 DOM 컬렉션 객체이다. 노드 객체의 상태 변화를 실시간으로 반영하는 살아있는 객체라는 특징이 있다.



HTMLCollection

getElementsByTagName, getElementsByClassName 메서드가 반환하는 이 객체는 노드 객체의 상태 변화를 실시간으로 반영하는 살아있는 DOM 컬렉션 객체이다. 따라서 살아있는 객체라고 부르기도 한다.

주의점 - p.695 - 697 예제 확인

for 문을 역방향으로 순회하거나, while문으로 객체에 노드 객체가 없을때까지 반복하거나 HTMLCollection 객체를 사용하지 않고 배열로 변환해 고차 함수를 사용하는 방법을 사용한다.



NodeList

HTMLCollection과 달리 대부분의 경우실시간으로 상태변경을 반영하지 않는 객체다. NodeList.prototype.forEach 메서드를 상속받아 사용할 수 있다. (item, entries, keys, values 등)

하지만, childNodes 프로퍼티가 반환하는 NodeList 객체는 실시간으로 노드 객체의 상태 변경을 반영하는 live 객체로 동작하므로 주의해야 한다.



이처럼 HTMLCollection과 NodeList는 예상과 다르게 동작할 수 있어 다루기 까다롭다. 따라서 노드 객체의 상태 변경과 상관없이 안전하게 DOM 컬렉션을 사용하려면 객체를 배열로 변환하여 사용하는 것이 권장된다.



### 39.3 노드 탐색

요소 노드를 취득한 후 취득한 노드를 기점으로 부모, 형제, 자식 노드 등을 탐색할 때가 있다.

이때,

Node.prototype의 parentNode, previousSibling, firstChild, childNodes 프로퍼티,

Element.prototype의 previousElementSibling, nextElementSibling 프로퍼티 등을 활용한다.



#### 39.3.1 공백 텍스트 노드

HTML 요소 사이의 스페이스, 탭, 줄바꿈 등의 공백 문자는 텍스트 노드를 생성한다. 이를 공백 텍스트 노드라 한다. 따라서 노드를 탐색할 때 이 공백 텍스트 노드에 주의해야 한다.&#x20;



#### 39.3.2 자식 노드 탐색

노드를 탐색하기 위해서는 다음과 같은 노드 탐색 프로퍼티를 사용한다.

| 프로퍼티                             | 설명                                                                 |
| -------------------------------- | ------------------------------------------------------------------ |
| Node.proto-.childNode            | 자식 노드를 모두 탐색해 NodeList에 담아 반환한다. 텍스트 노드가 포함되어 있을 수 있다.             |
| Element.proto-.children          | 자식 노드 중 요소 노드만 모두 탐색해 HTMLCollection에 담아 반환한다. 텍스트 노드가 포함되어 있지 않다. |
| Node.proto-.firstChild           | 첫 자식 노드를 반환한다. 텍스트 노드이거나 요소 노드이다.                                  |
| Node.proto-.lastChild            | 마지막 자식 노드를 반환한다. 텍스트 노드이거나 요소 노드이다.                                |
| Element.proto-.firstElementChild | 첫 자식 요소 노드를 반환한다.                                                  |
| Element.proto-.lastElementChild  | 마지막 자식 요소 노드를 반환한다.                                                |



#### 39.3.3 자식 노드 존재 확인

Node.prototype.hasChildNodes 메서드를 활용해 자식 노드의 존재를 확인한다. 자식 노드 중 텍스트 노드가 아닌 요소 노드가 존재하는지 확인하려면 children.length 또는 Element.childElementCount 프로퍼티를 활용한다.



#### 39.3.4 요소 노드의 텍스트 노드 탐색

요소 노드의 텍스트 노드는 요소 노드의 자식 노드다. 따라서 요소 노드의 텍스트 노드는 firstChild 프로퍼티로 접근할 수 있다. firstChild 프로퍼티는 첫 번째 자식 노드를 반환한다. 반환한 노드는 텍스트 노드이거나 요소 노드이다.



#### 39.3.5 부모 노드 탐색

부모 노드를 탐색하려면 Node.prototype.parentNode 프로퍼티를 사용한다. 텍스트 노드는 DOM 트리의 리프 노드이므로 부모 노드가 텍스트 노드인 경우는 없다.



#### 39.3.6 형제 노드 탐색

부모 노드가 같은 형제 노드를 탐색하려면, 다음의 프로퍼티를 사용한다. 텍스트 노드 또는 요소 노드만 반환한다.

Node.pro-.previousSibling

Node.pro-.nextSibling

Element.pro-.previousElementSibling

Element.pro-.nextElementSibling



### 39.4 노드 정보 취득

노드 객체에 대한 정보를 취득하려면 다음과 같은 노드 정보 프로퍼티를 사용한다.

| 프로퍼티                 | 설명                                                                                                                         |
| -------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| Node.proto-.nodeType | <p>노드 객체의 종류, 즉 노드 타입을 나타내는 상수를 반환한다. 노드 타입 상수는 Node에 정의되어 있다.<br>요소 노드 타입 - 1 / 텍스트 노드 타입 - 2 / 문서 노드 타입 - 9</p>          |
| Node.proto-.nodeName | <p>노드의 이름을 문자열로 반환한다.<br>요소 노드: 대문자 문자열로 태그 이름("UL", "LI" 등) 반환<br>텍스트 노드: 문자열 "#text" 반환<br>문서 노드: 문자열 "#document" 반환</p> |



### 39.5 요소 노드의 텍스트 조작

#### 39.5.1 nodeValue

Node.prototype.nodeValue 프로퍼티는 setter와 getter 모두 존재하는, 참조와 할당이 가능한 접근자 프로퍼티다. 노드 객체의 nodeValue 프로퍼티를 참조하면 노드 객체의 값을 반환한다. 노드 객체의 값은 텍스트 노드의 텍스트로, 텍스트 노드의 nodeValue 프로퍼티에 값을 할당하면 텍스트를 변경할 수 있다.

텍스트를 변경할 요소 노드 취득 > firstChild 프로퍼티를 사용, 취득한 요소 노드의 텍스트 노드 탐색 > nodeValue 프로퍼티를 사용해 텍스트 노드의 값 변경



#### 39.5.2 textContent

Node.prototype.textContent 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티로서 요소 노드의 텍스트와 모든 자손 노드의 텍스트를 모두 취득하거나 변경한다.

요소 노드의 textContent 프로퍼티를 참조하면 요소 노드의 콘텐츠 영역 내 텍스트를 모두 반환한다. 이때 HTML 마크업은 무시된다.

위의 nodeValue도 텍스트를 취득할 수 있지만, 텍스트 노드가 아닌 노드에서는 null을 반환하므로 의미가 없다. 그리고 textContent 프로퍼티 사용과 비교하면 코드가 더 복잡하다.&#x20;

참고로 innerText 프로퍼티도 유사한 동작을 하지만, CSS에 순종적이기에 더 느리기에 사용하지 않는 것이 좋다고 한다.



### 39.6 DOM 조작

DOM 조작은 새로운 노드를 생성하여 DOM에 추가하거나 기존 노드를 삭제 또는 교체하는 것을 말한다. DOM에 새로운 노드가 추가되거나 삭제되면 리플로우와 리페인트가 발생하는 원인이 되므로 성능에 영향을 준다. 따라서 DOM 조작은 성능 최적화를 위해 주의해서 다뤄야 한다.

#### 39.6.1 innerHTML

setter와 getter 모두 존재하는 접근자 프로퍼티로 요소 노드의 HTML 마크업을 취득하거나 변경한다. innerHTML 프로퍼티를 참조하면 요소 노드의 콘텐츠 영역 내 포함된 모든 HTML 마크업을 문자열로 반환한다.

요소 노드의 innerHTML 프로퍼티에 문자열을 할당하면 요소 노드의 모든 자식 노드가 제거되고 할당한 문자열에 포함되어 있는 HTML 마크업이 파싱되어 요소 노드의 자식 노드로 DOM에 반영된다. HTML 마크업 문자열로 간단히 DOM을 조작이 가능하다.

하지만, 몇가지단점이 있다.

첫째, 사용자로부터 입력받은 데이터를 그대로 innerHTML 프로퍼티에 할당하면 [크로스 사이트 스크립팅 공격](#user-content-fn-1)[^1]에 취약하므로 위험하다.&#x20;

둘째, 요소 노드의 innerHTML 프로퍼티에 HTML 마크업 문자열을 할당하는 경우 요소 노드의 모든 자식 노드를 제거하고 할당한 HTML 마크업 문자열을 파싱하여 DOM을 변경한다. 이는 불필요한 리플로우/리페인팅을 발생하므로 효율적이지 않다.&#x20;

셋째, 새로운 요소를 삽입할 때 삽입 위치를 지정할 수 없다.



#### 39.6.2 insertAdjacentHTML 메서드

기존 요소를 제거하지 않으면서 위치를 지정해 새로운 요소를 삽입한다. innerHTML 프로퍼티보다 효율적이고 빠르다. 크로스 사이트 스크립팅 공격에 취약한 점은 동일하다.



#### 39.6.3 노드 생성과 추가

DOM은 노드를 직접 생성/삽입/삭제/치환하는 메서드도 제공한다.



**요소 / 텍스트 노드 생성**

Document.prototype.createElement 메서드는 요소 노드를 생성하여 반환한다.

Document.prototype.createTextNode 메서드는 텍스트 노드를 생성하여 반환한다.

이 두 메서드는 노드를 생성할 뿐 요소 노드에 추가하지 않는다. 따라서 이후 생성된 노드를 요소 노드에 추가하는 처리가 별도로 필요하다.



**텍스트 노드를 요소 노드의 자식 노드로 추가**

Document.prototype.appendChild 메서드는 인수로 전달한 노드를 호출한 노드의 마지막 자식 노드로 추가한다.



**요소 노드를 DOM에 추가**

Node.prototype.appendChild 메서드를 사용하여 텍스트 노드와 부자 관계로 연결한 요소 노드를 상위 요소 노드의 마지막 자식 요소로 추가한다.



#### 39.6.4 복수의 노드 생성과 추가

DOM을 변경하는 것은 높은 비용이 드는 처리이므로 가급적 횟수를 줄이는 편이 성능에 유리하다.

DocumentFragment 노드를 통해 해결한다. 문서, 요소, 어트리뷰트, 텍스트 노드와 같은 노드 객체의 일종으로 부모 녿가 없어 기존 DOM과 별도로 존재하는 특징이 있다. 이러한 특징을 이용해 DocumentFragment 노드에 자식 노드를 추가해도 기존 DOM은 변경되지 않는다. 또한 DOM에 추가하면 자신은 제거되고 자신의 자식 노드만 DOM에 추가되어 단 한번의 DOM 변경이 발생한다. 따라서 여러 요소 노드를 DOM에 추가하는 경우 DocumentFragment 노드를 사용해 효율적인 DOM 변경을 이뤄낼 수 있다.



#### 39.6.5 노드 삽입

**마지막 노드로 추가**

Node.prototype.appendChild 메서드는 인수로 전달받은 노드를 자신을 호출한 노드의 마지막 자식 노드로 DOM에 추가한다. 이때 노드를 추가할 위치를 지정할 수 없고 항상 마지막 자식 노드로 추가한다.



**지정한 위치에 노드 삽입**

Node.prototype.insertBefore(newNode, childNode) 메서드는 첫 번째 인수로 전달받은 노드를 두 번째 인수로 전달받은 노드 앞에 삽입한다. 두 번째 인수로 전달받은 노드는 반드시 insertBefore 메서드를 호출한 노드의 자식 노드이어야 한다. 그렇지 않으면 DOMException 에러가 발생한다.



#### 39.6.6 노드 이동

DOM에 이미 존재하는 노드를 appendChild 또는 insertBefore 메서드를 사용하여 다시 추가하면 현재 위치에서 노드를 제거하고 새로운 위치에 노드를 추가한다. 즉, 노드가 이동한다.



#### 39.6.7 노드 복사

Node.prototype.cloneNode(\[deep: true | false]) 메서드는 노드의 사본을 생성해 반환한다. deep에 true를 전달하면 깊은 복사하여 모든 자손 노드가 포함된 사본을 생성하고, false를 전달하거나 생략하면 얕은 복사하여 노드 자신만의 사본을 생성한다. 얕은 복사로 생성된 요소 노드는 자손 노드를 복사하지 않으므로 텍스트 노드도 없다.



#### 39.6.8 노드 교체

Node.prototype.replaceChild(newChild, oldChild) 메서드는 자신을 호출한 노드의 자식 노드를 다른 노드로 교체한다.



#### 39.6.9 노드 삭제

Node.prototype.removeChild(child) 메서드는 인수로 전달한 노드를 DOM에서 삭제한다.



### 39.7 어트리뷰트

#### 39.7.1 어트리뷰트 노드와 attributes 프로퍼티

HTML 요소는 여러 개의 어트리뷰트(attribute, 속성)을 가질 수 있다. 동작을 제어하기 위한 추가적인 정보를 제공하는 HTML 어트리뷰트는 HTML 요소의 시작 태그에 이름="값" 형식으로 정의한다.

```javascript
<input id="user" type="text" value="gg"> ...
```

글로벌 어트리뷰트(id, class, style, title, lang, tabindex, draggable, hidden ...)와 이벤트 핸들러 어트리뷰트(onclick, onchange, onfocus, onblur, oninput, onkeypress, onkeydown, onkeyup, onmouseover, onsubmit, onload ...)는 모든 HTML 요소에서 공통적으로 사용할 수 있지만 type, value check 어트리뷰트 등 특정 요소에만 사용 가능한 어트리뷰트도 있다.

HTML 문서가 파싱될 때 HTML 요소의 어트리뷰트는 어트리뷰트 노드로 변환되어 요소 노드와 연결된다. 이때 모든 어트리뷰트 노드의 참조는 NamedNodeMap 객체에 담겨 요소 노드의 attributes 프로퍼티에 저장된다.

따라서 요소 노드의 모든 어트리뷰트 노드는 요소 노드의 Element.prototype.attributes 프로퍼티로 취득할 수 있다. attributes 프로퍼티는 getter만 존재하는 읽기 전용 접근자 프로퍼티이며, 요소 노드의 모든 어트리뷰트 노드의 참조가 담긴 NamedNodeMap 객체를 반환한다.



#### 39.7.2 HTML 어트리뷰트 조작

HTML 어트리뷰트값을 **참조**하려면, Element.prototype.getAttribute(attributeName)

HTML 어트리뷰트값을 **변경**하려면, Element.prototype.setAttribute(attributeName)

HTML 어트리뷰트값을 **확인**하려면, Element.prototype.hasAttribute(attributeName)

HTML 어트리뷰트값을 **삭제**하려면, Element.prototype.removeAttribute(attributeName)

메서드를 사용한다.



#### 39.7.3 HTML 어트리뷰트 vs. DOM 프로퍼티

요소 노드 객체에는 HTML 어트리뷰트에 대응하는 프로퍼티가 존재한다. 이 DOM 프로퍼티들은 HTML 어트리뷰트 값을 초기값으로 가지고 있다. DOM 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티다. 따라서 DOM 프로퍼티는 참조와 변경이 가능하다. 이처럼 HTML 어트리뷰트는 DOM에서 중복 관리되고 있는 것처럼 보인다.

하지만 그렇지 않다.

HTML 어트리뷰트의 역할은 HTML 요소의 초기 상태를 지정하는 것이다. 반면, DOM 프로퍼티는 최신 상태를 저장하고 있다.



**어트리뷰트 노드**

HTML 어트리뷰트로 지정한 HTML 요소의 초기 상태는 어트리뷰트 노드에서 관리한다. HTML 요소에 지정한 어트리뷰트 값은 사용자의 입력에 의해 변하지 않으므로 언제나 동일한 결과를 보인다. setAttribute 메서드는 어트리뷰트에서 관리하는 HTML 요소에 지정한 어트리뷰트 값, 초기 상태 값을 변경한다.



**DOM 프로퍼티**

사용자가 입력한 최신 상태는 DOM 프로퍼티가 관리한다. DOM 프로퍼티는 사용자의 입력에 의한 상태 변화에 반응하여 언제나 최신 상태를 유지한다.



**HTML 어트리뷰트와 DOM 프로퍼티의 대응 관계**

대부분의 HTML 어트리뷰트와 DOM 프로퍼티는 1:1로 대응한다. 언제나 1:1은 아니며 반드시 이름이 일치하는 것도 아니다.

* id 어트리뷰트와 id 프로퍼티는 1:1 대응하며, 동일 값으로 연동한다.
* input 요소의 value 어트리뷰트는 value 프로퍼티와 1:1 대응한다. 하지만 value 어트리뷰트는 초기 상태를, 프로퍼티는 최신 상태를 갖는다.
* class 어트리뷰트는 className, classList 프로퍼티와 대응한다.
* for 어트리뷰트는 htmlFor 프로퍼티와 1:1 대응한다.
* td 요소의 colspan 어트리뷰트는 대응하는 프로퍼티가 존재하지 않는다.
* textContent 프로퍼티는 대응하는 어트리뷰트가 존재하지 않는다.
* 어트리뷰트 이름은 대소문자를 구별하지 않지만, 대응하는 프로퍼티 키는 카멜 케이스를 따른다.



**DOM 프로퍼티 값의 타입**

getAttribute 메서드로 취득한 어트리뷰트 값은 언제나 문자열이다. 하지만 DOM 프로퍼티로 취득한 최신 상태 값은 문자열이 아닐 수 있다. 예를 들어, checkbox 요소의 checked 어트리뷰트 값은 문자열이지만 checked 프로퍼티 값은 불리언 타입이다.



#### 39.7.4 data 어트리뷰트와 dataset 프로퍼티

data 어트리뷰트와 dataset 프로퍼티를 사용하면 HTML 요소에 정의한 사용자 정의 어트리뷰트와 자바스크립트 간 데이터를 교환할 수 있다.



### 39.8 스타일

#### 39.8.1 인라인 스타일 조작

HTMLElement.prototype.style 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티로서 요소 노드의 인라인 스타일을 취득하거나 추가 또는 변경한다. style 프로퍼티를 참조하면 CSSStyleDeclaration 타입의 객체를 반환한다. 이 객체는 다양한 CSS 프로퍼티에 대응하는 프로퍼티를 가지고 있으며, 이 프로퍼티에 값을 할당하면 해당 CSSS 프로퍼티가 인라인 스타일로 HTML 요소에 추가되거나 변경된다.



#### 39.8.2 클래스 조작

.으로 시작하는 클래스 선택자를 사용해 CSS class를 미리 정의한 다음, HTML 요소의 class 어트리뷰트 값을 변경하여 HTML 요소의 스타일을 변경할 수 있다. 이때, HTML 요소의 class 어트리뷰트를 조작하려면 대응하는 요소 노드의 DOM 프로퍼티를 사용한다. 단, class 어트리뷰트에 대응하는 DOM 프로퍼티는 class가 아닌 className, classList이다.



**className**

Element.prototype.className 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티로서 HTML 요소의 class 어트리뷰트 값을 취득하거나 변경한다.



**classList**

Element.prototype.classList 프로퍼티는 class 어트리뷰트의 정보를 담은 DOMTokenList 객체를 반환한다.

DOMTokenList 객체는 다음과 같은 메서드를 제공한다.

* add(...className) - 인수로 전달한 1개 이상의 문자열을 class 어트리뷰트 값으로 추가한다.
* remove(...className) - 인수로 전달한 1개 이상의 문자열과 일치하는 클래스를 class 어트리뷰트에서 삭제한다.
* item(index) - 인수로 전달한 index에 해당하는 클래스를 class 어트리뷰트에서 반환한다.
* contains(className) - 인수로 전달한 문자열과 일치하는 클래스가 포함되어 있는지 확인한다.
* replace(oldClassName, newClassName) - 첫 번째 인수로 전달한 문자열을 두 번째 인수로 전달한 문자열로 변경한다.
* toggle(className\[. force]) - 인수로 전달한 문자열과 일치하는 클래스가 존재하면 제거하고, 존재하지 않으면 추가한다.
* forEach, entries, keys, values, supports ...



#### 39.8.3 요소에 적용되어 있는 CSS 스타일 참조

style 프로퍼티는 인라인 스타일만 반환한다. 따라서 클래스를 적용한 스타일이나 상속을 통해 암묵적으로 적용된 스타일은 style 프로퍼티로 참조할 수 없다. HTML 요소에 적용되어 있는 모든 CSS 스타일을 참조해야 할 경우 getComputedStyle 메서드를 사용한다.

window.getComputedStyle(element\[, pseudo]) 메서드는 첫 인수로 전달한 요소 노드에 적용되어 있는 평가된 스타일을 CSSStyledDeclaration 객체에 담아 반환한다. 평가된 스타일(computed style)이란 요소 노드에 적용되어 있는 모든 스타일이 조합되어 최종적으로 적용된 스타일을 말한다.



### 39.9 DOM 표준

W3C와 WHATWG 가 협력하며 공통된 표준을 만들다가 2018년 4월부터 WHATWG(구글, 애플, 마이크로소프트, 모질라로 구성된 4개의 주류 브라우저 벤더사가 주도하는 단체)이 내놓은 단일 표준을 따르기로 합의했다.





\-- 진짜 중요한 내용임 반드시 복습하자잉

[^1]: XSS: Cross-Site Scripting Attack
