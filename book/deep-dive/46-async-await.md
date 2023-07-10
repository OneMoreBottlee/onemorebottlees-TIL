# 46장 제너레이터와 async/await

### 46.1 제너레이터란?

ES6에서 도입된 제너레이터는 코드 블록의 실행을 일시 중단했다가 필요한 시점에 재개할 수 있는 특수한 함수다.

1. 제너레이터 함수는 함수 호출자에게 함수 실행의 제어권을 양도할 수 있다.
2. 제너레이터 함수는 함수 호출자와 함수의 상태를 주고받을 수 있다.
3. 제너테이터 함수를 호출하면 제너레이터 객체를 반환한다.



### 46.2 제너레이터 함수의 정의

제너레이터 함수는 function\* 키워드로 선언한다. 그리고 하나 이상의 yield 표현식을 포함한다.

```javascript
// 제너레이터 함수 선언문
function* genDecFunc(){
    yeild 1;
}

// 제너레이터 함수 표현식
const getExpFunc = function* () {
    yeild 1;
}

// 제너레이터 메서드
const obj = {
    * genObjMethod(){
        yeild 1;
    }
}

// 제너레이터 클래스 메서드
class MyClass {
    * genClsMethod() {
        yeild 1;
    }
}
```



### 46.3 제너레이터 객체

제너레이터 함수를 호출하면 제너레이터 객체를 생성해 반환한다. 반환한 제너레이터 객체는 이터러블이면서 동시에 이터레이터이다. 따라서 value, done 프로퍼티와 next, return, throw 메서드를 사용할 수 있다.

P.873 예시 보기



### 46.4 제너레이터의 일시 중지와 재개

제너레이터는 yield 키워드와 next 메서드를 통해 실행을 일시 중지했다가 필요한 시점에 다시 재개할 수 있다. yield 키워드는 제너레이터 함수의 실행을 일시 중지시키거나 yield 키워드 뒤에 오는 표현식의 평가 결과를 제너레이터 함수 호출자에 반환한다.

제너레이터 객체의 next 메서드를 호출하면 yield 표현식까지 실행되고 일시 중지된다. 이때 함수의 제어권이 호출자로 양도된다. 이후 필요 시점에서 next 메서드를 호출하면 일시 중지된 코드부터 실행을 재개해 다음 yield 표현식까지 실행되고 또 다시 일시 중지된다.

이때 제너레이터 객체의 next 메서드는 value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환한다. 이 객체의 value 프로퍼티에 yield 표현식에서 yield된 값이 할당되고 done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었는지 나타내는 불리언 값이 할당된다. 마지막 표현식이라면 done 프로퍼티에 true가 할당된다.

이처럼 제너레이터 함수는 next 메서드와 yield 표현식을 통해 함수 호출자와 함수의 상태를 주고받는다. 이러한 특성을 활용해 비동기 처리를 동기 처리처럼 구현 할 수 있다.



### 46.5 제너레이터의 활용

#### 46.5.1 이터러블의 구현

제너레이터 함수를 사용하면 이터레이션 프로토콜을 준수해 이터러블을 생성하는 방식보다 간단히 이터러블을 구현할 수 있다.&#x20;



#### 46.5.2 비동기 처리

제너레이터 함수는 next 메서드와 yield 표현식을 통해 함수 호출자와 함수의 상태를 주고받을 수 있다. 이러한 특성을 활용해 프로미스를 사용한 비동기 처리를 동기 처리처럼 구현할 수 있다.



### 46.6 async/await

ES8에서 도입된 async/await는 제너레이터의 특징을 활용해 간단하고 가독성 좋게 비동기 처리를 동기 처리처럼 동작하도록 구현할 수 있다. async/await는 프로미스를 기반으로 동작한다.



#### 46.6.1 async 함수

async 함수는 async 키워드를 사용해 정의하며 언제나 프로미스를 반환한다. 명시적으로 프로미스를 반환하지 않더라도 async 함수는 암묵적으로 반환값을 resolve하는 프로미스를 반환한다.



#### 46.6.2 await 키워드

await 키워드는 프로미스가 settled 상태가 되면 resolve한 처리 결과를 반환한다. await 키워드는 반드시 async 함수 내부에서 사용해야 한다.



#### 46.6.3 에러 처리

async/await에서 에러 처리는 try ... catch 문을 사용한다. async 함수 내에서 catch 문을 사용해 에러 처리를 하지 않으면 async 함수는 발생한 에러를 reject 하는 프로미스를 반환한다.&#x20;

```javascript
// try ... catch 문으로 에러 캐치하기
const fetch =  require('node-fetch')
const foo = async () => {
    try {
        const wrongUrl = 'https://wrong.url'
        
        const response = await fetch(wrongUrl)
        const data = await response.json()
        console.log(data)
    } catch (err) {
        console.log(err) // TypeError: Failed to fetch
    }
}

// 위 함수의 catch 문은 HTTP 통신 에러뿐 아니라 try 코드 블록 내 발생하는 일반적인 에러까지 캐치한다.
foo()


// catch로 에러 캐치하기
const fetch =  require('node-fetch')
const foo = async() => {
    const wrongUrl = 'https://wrong.url'
    
    const response = await fetch(wrongUrl)
    const data = await response.json()
    return data
}

foo()
    .then(console.log)
    .catch(console.error) // TypeError: Failed to fetch
```



