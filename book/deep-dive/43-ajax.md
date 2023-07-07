# 43장 Ajax

### 43.1 Ajax란?

Ajax(Asynchronous JavaScript and XML)란 자바스크립트를 사용하여 브라우저가 서버에 비동기 방식으로 데이터를 요청하고, 응답 데이터를 수신해 웹페이지를 동적으로 갱신하는 프로그래밍 방식을 말한다. 브라우저에서 제공하는 Web API인 XMLHttpRequest 객체를 기반으로 동작한다. 1999년 MS가 개발한 XMLHttpRequest는 2005년 웹 브라우저에서 자바스크립트와 Ajax를 기반으로 동작하는 구글 맵스를 통해 성능을 입증했다.

전통적인 방식은 완전한 HTML을 서버로부터 전송받아 처음부터 다시 렌더링하는 방식으로 동작했는데, 화면이 전환되면 새로운 HTML 파일을 전송받아 웹페이지 전체를 다시 렌더링해야 했다. 따라서

* 변경할 필요 없는 부분까지 다시 전송받아 불필요한 데이터 통신이 발생
* 화면 전환이 일어나면 화면이 순간적으로 깜빡이는 현상이 발생
* 동기적으로 동작하기에 서버로부터 응답이 올때까지 다음 처리가 블로킹됨

같은 단점이 발생했다.

Ajax는 위와 같은 단점을 보완해&#x20;

* 필요한 데이터만 전송받아 필요한 데이터 통신만 발생
* 변경할 필요가 없는 부분은 다시 렌더링하지 않음
* 비동기 방식으로 동작하기에 서버에 요청을 보낸 후 블로킹이 발생하지 않음

와 같은 장점이 있다.



### 43.2 JSON

JSON(JavaScript Object Notation)은 클라이언트와 서버 간 HTTP 통신을 위한 텍스트 데이터 포맷이다. JS에 종속되지 않는 언어 독립형 데이터 포맷으로 대부분의 프로그래밍 언어에서 사용 가능하다.



#### 43.2.1 JSON 표기 방식

JSON은 JS의 객체 리터럴과 유사하게 키와 값으로 구성된 순수한 텍스트다.

```javascript
{
    "name" : "Kim",
    "age" : 23,
    "alive" : true,
    "hobby" : ["traveling", "tennis"]
}

// 큰따옴표로 키를 묶어야 한다.
// 값은 객체 리터럴과 같은 방식이지만 문자열은 반드시 큰따옴표로 묶어야 한다.
```



#### 43.2.2 JSON.stringify

JSON.stringift 메서드는 객체를 JSON 포맷의 문자열로 변환한다. 클라이언트가 서버로 객체를 전송하려면 객체를 문자열화해야하는데 이를 직렬화라 한다.



#### 43.2.3 JSON.parse

JSON.parse 메서드는 JSON 포맷의 문자열을 객체로 변환한다. 서버로부터 전송된 JSON 데이터는 문자열이다. 이 문자열을 객체로 사용하기 위해 JSON 포맷의 문자열을 객체화해야 하는데 이를 역직렬화라 한다.



### 43.3 XMLHttpRequest

브라우저는 주소창이나 form 태그, a 태그 등을 통해 HTTP 요청 전송 기능을 기본 제공한다. 이 외에 자바스크립트를 사용해 HTTP 요청을 전송하려면 XMLHttpRequest 객체를 사용한다. Web API인 XMLHttpRequest 객체는 HTTP 요청 전송과 응답 수신을 위한 다양한 메서드와 프로퍼티를 제공한다.



#### 43.3.1 XMLHttpRequest 객체 생성

XMLHttpRequest 생성자 함수를 호출하여 생성한다. 이 객체는 브라우저에서 제공하는 Web API 이므로 브라우저 환경에서만 정상적으로 실행된다.



#### 43.3.2 XMLHttpRequest 객체의 프로퍼티와 메서드

P.822 - P.824 확인하기



#### 43.3.3 HTTP 요청 전송

HTTP 요청을 전송하는 경우 다음 순서를 따른다.

1. XMLHttpRequest.prototype.open 메서드로 HTTP 요청을 초기화한다.
2. 필요에 따라 XMLHttpRequest.prototype.setRequestHeader 메서드로 특정 HTTP 요청의 헤더 값을 설정한다.
3. XMLHttpRequest.prototype.send 메서드로 HTTP 요청을 전송한다.



#### 43.3.4 HTTP 응답 처리

서버가 전송한 응답을 처리하려면 XMLHttpRequest 객체가 발생시키는 이벤트를 캐치해야 한다. HTTP 요청의 현재 상태를 나타내는 readyState 프로퍼티 값이 변경된 경우 발생하는 readystatechange 이벤트를 캐치하여 HTTP 응답을 처리할 수 있다. onreadystatechange 이벤트 핸들러 프로퍼티에 할당한 이벤트 핸들러는 HTTP 요청의 현재 상태를 나타내는 xhr.readyState가 XMLHttpRequest.DONE인지 확인해 서버의 응답이 완료되었는지 확인한다.

서버의 응답이 완료되면 HTTP 요청에 대한 응답 상태를 나타내는 xhr.state가 200인지 확인해 정상 처리와 에러 처리를 구분한다.

readystatechange 이벤트 대신 load 이벤트를 캐치해도 된다. load 이벤트는 HTTP 요청이 성공적으로 완료되는 경우 발생한다. 따라서 load 이벤트를 캐치하는 경우 xhr.readyState가 XMLHttpRequest.DONE인지 확인할 필요가 없다.



