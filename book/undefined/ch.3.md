# Ch.3 유니언과 리터럴

타입스크립트가 해당 값을 바탕으로 추론을 수행하는 두 가지 핵심 개념

* 유니온 union : 값에 허용된 타입을 두 개 이상의 가능한 타입으로 확장하는 것
* 내로잉 narrowing : 값에 허용된 타입이 하나 이상의 가능한 타입이 되지 않도록 좁히는 것

다른 프로그래밍 언어에서는 불가능하지만 타입스크립트에서는 가능한 '코드 정보에 입각한' 추론을 해내는 강력한 개념이다.



### 3.1 유니언 타입

'이거 혹은 저거' 와 같은 타입을 유니언 union 이라 한다. 유니언 타입은 값의 정확한 타입을 모르지만, 옵션 중 하나라는 것을 알고 있는 경우에 코드를 처리하는 개념이다.

<figure><img src="../../.gitbook/assets/image (4) (2).png" alt=""><figcaption><p>유니언 타입</p></figcaption></figure>

사진과 같이 타입스크립트는 | 연산자를 사용해 유니언 타입을 알려준다.&#x20;



#### 3.1.1 유니언 타입 선언

<figure><img src="../../.gitbook/assets/image (3) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

변수의 초깃값이 있더라도 변수에 대한 명시적 타입 애너테이션을 제공하는 것이 유용할 때 유니언 타입을 사용한다. 유니언 타입 선언은 타입 애너테이션으로 타입을 정의하는 모든 곳에서 사용 가능하다.



#### 3.1.2 유니언 속성

값이 유니언 타입일 때, 타입스크립트는 접근 가능한 모든 타입에 공통된 속성에 접근할 수 있다.&#x20;

<figure><img src="../../.gitbook/assets/image (2) (3) (1).png" alt=""><figcaption></figcaption></figure>

이처럼 접근 가능한 모든 타입에 공통적으로 존재하지 않는 속성에 대한 접근을 제한하는 것은 안전 조치에 해당된다. 객체가 어떤 속성을 포함한 타입으로 확실하게 알려지지 않은 경우, 타입스크립트는 해당 속성을 사용하려고 시도하는 것이 안전하지 않다고 여긴다. 그 속성이 존재하지 않을 수도 있기 때문이다.

유니언 타입으로 정의된 여러 타입 중 하나의 타입으로 된 값의 속성을 사용하려면 값이 구체적인 타입이라는 것을 타입스크립트에 알려야 한다. 이 과정을 내로잉이라 한다.



### 3.2 내로잉

내로잉은 값이 정의, 선언 혹은 이전에 유추된 것보다 더 구체적인 타입임을 코드에서 유추하는 것을 말한다. 타입스크립트가 값의 타입이 이전보다 더 좁혀졌음을 알게되면 값을 더 구체적인 타입으로 취급하여  효과적인 사용이 가능하다. 타입을 좁히는데 사용하는 논리적 검사를 타입 가드라고 한다.



#### 3.2.1 값 할당을 통한 내로잉

변수에 값을 직접 할당하면 변수의 타입을 할당된 값의 타입으로 좁힌다.

<figure><img src="../../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

string 타입의 값을 할당한 후 string 타입의 toUpperCase()를 사용할 수 있다.



#### 3.2.2 조건 검사를 통한 내로잉

if 문을 통해 변수의 값을 좁히는 방법으로 타입을 확인할 수 있다.

<figure><img src="../../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

number 타입의 값인 경우 number 타입의 toFixed()를 사용할 수 있다.



#### 3.2.3 typeof 검사를 통한 내로잉

typeof 연산자를 사용할 수 있다.

<figure><img src="../../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

test의 타입을 확인해 string인 경우와 number인 경우로 구분해 로직을 실행한다. 이러한 typeof 검사는 타입을 좁히기 위해 자주 사용하는 방법이라고 한다.



### 3.3 리터럴 타입

리터럴 타입은 더 구체적인 버전의 원시 타입이다. 원시 타입 값 중 하나가 아닌 특정 원싯값으로 알려진 타입이 리터럴 타입이다.

<figure><img src="../../.gitbook/assets/image (68).png" alt=""><figcaption></figcaption></figure>

예시를 보면 test는 "HelloType" 이라는 값을 가진 string 타입으로 보인다. 하지만 "HelloType" 이라는 타입이다. 원시 타입인 string은 모든 문자열의 집합이지만 "HelloType" 타입은 "HelloType"이라는 하나의 문자열만 나타낸다.

<figure><img src="../../.gitbook/assets/image (73) (2).png" alt=""><figcaption></figcaption></figure>

반면, let 으로 선언하면 string 타입으로 나타나는 것을 확인할 수 있다. 이를 통해 리터럴 타입의 범위는 해당 타입이 가질 수 있는 가능한 모든 값의 조합이라 생각할 수 있다.&#x20;



#### 3.3.1 리터럴 할당 가능성

<figure><img src="../../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

test를 "HelloType" 타입으로 선언했다. 따라서 변수 test에는 "HelloType" 만이 값으로 할당이 가능하다.



### 3.4 엄격한 null 검사

리터럴로 좁혀진 유니언의 힘은 타입스크립트에서 "[엄격한 null 검사](#user-content-fn-1)[^1]"라고 부르는 타입 시스템 영역인 '잠재적으로 정의되지 않은 undefined 값'으로 작업할 때 특히 두드러진다. 타입스크립트는 '[십억 달러의 실수](#user-content-fn-2)[^2]' 를 바로잡기 위해 엄격한 null 검사를 사용한다.



#### 3.4.1 십억 달러의 실수

<figure><img src="../../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

> I call it my billion-dollar mistake…At that time, I was designing the first comprehensive type system for references in an object-oriented language. My goal was to ensure that all use of references should be absolutely safe, with checking performed automatically by the compiler. But I couldn’t resist the temptation to put in a null reference, simply because it was so easy to implement. This has led to innumerable errors, vulnerabilities, and system crashes, which have probably caused a billion dollars of pain and damage in the last forty years.\
> Tony Hoare, inventor of ALGOL W.

1965년 null 참조를 만든 토니 호어의 말이다. ~~창조주가 실수라고 하다니 null은 참 슬플듯~~

어쨋든, 탄생한 null 타입을 처리하기 위해 수많은 오류와 시스템 충돌이 발생했다고 한다. (자세한 내용은 아래 링크에서 확인)

[Why Null is Bad?](https://www.yegor256.com/2014/05/13/why-null-is-bad.html)

타입스크립트 컴파일러는 엄격한 null 검사의 활성화 여부를 선택할 수 있다. 활성화하면 null 이나 undefined 값으로 인한 오류로부터 안전해 질 수 있다.



#### 3.4.2 참 검사를 통한 내로잉

자바스크립트에서 참 또는 truthy는 && 연산자 또는 if 문처럼 boolean 문맥에서 true로 간주된다. 이를 활용해 타입스크립트는 잠재적인 값 중 truthy로 확인된 일부에 한해 변수의 타입을 좁힐 수 있다.

<figure><img src="../../.gitbook/assets/image (74).png" alt=""><figcaption></figcaption></figure>

test의 타입은 string 일 수도 undefined 일 수도 있다. if 문을 활용해 string 타입을 확정해 undefined 인 경우를 확실히 제거할 수 있다. 혹은 논리 연산자 &&와 ?를 활용해 참 여부를 검사할 수 있다.&#x20;

<figure><img src="../../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

하지만 타입을 확정할 수는 없다.



#### 3.4.3 초깃값이 없는 변수

자바스크립트에서 초깃값이 없는 변수는 기본적으로 undefined가 된다.&#x20;

타입스크립트는 값이 할당되기 전 속성 중 하나에 접근하려 시도하면 다음과 같은 오류 메시지가 나타난다.

<figure><img src="../../.gitbook/assets/image (8) (2).png" alt=""><figcaption></figcaption></figure>

하지만 변수 타입에 undefined가 포함되어 있는 경우에는 오류가 보고되지 않는다.

<figure><img src="../../.gitbook/assets/image (9) (1).png" alt=""><figcaption></figcaption></figure>



### 3.5 타입 별칭

타입스크립트에는 재사용하는 타입에 사용 가능한 [타입 별칭](#user-content-fn-3)[^3]이 있다. 타입 별칭은 |  type 이름 = 타입 | 의 형태를 가지며, 파스칼 케이스로 이름을 지정한다. 타입 별칭은 타입 시스템의 ctrl + c , v 처럼 작동한다.

<figure><img src="../../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>



#### 3.5.1 타입 별칭은 자바스크립트가 아니다.

타입 별칭은 자바스크립트로 컴파일되지 않는다. 타입 시스템에만 존재하는 개념으로 오직 개발 시에만 존재한다.



#### 3.5.2 타입 별칭 결합

타입 별칭은 다른 타입 별칭을 참조할 수 있다.

<figure><img src="../../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

중복되는 부분은 알아서 지워준다.



### 3.6 마치며

* 유니언 타입으로 두 개 이상의 타입 중 하나일 수 있는 값을 나타내는 방법
* 타입 애너테이션으로 유니언 타입을 명시적으로 표시하는 방법
* 타입 내로잉으로 값의 가능한 타입을 좁히는 방법
* 리터럴 타입의 const 변수와 원시 타입의 let 변수의 차이점
* '십억 달러의 실수'와 타입스크립트가 엄격한 null 검사를 처리하는 방법
* 존재하지 않을 수 있는 값을 나타내는 명시적인 | undefined
* 할당되지 않은 변수를 위한 암묵적인 | undefined
* 반복적으로 사용하고 입력이 긴 유니언 타입을 타입 별칭에 저장하는 방법



\---

조금씩 이 번역에 익숙해지는듯...? 중간중간 너무 어색한 부분들이 있긴 하지만....



[^1]: strict null checking

[^2]: The Billion-Dollar Mistake

[^3]: type alias
