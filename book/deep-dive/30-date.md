# 30장 Date

표준 빌트인 Date는 날짜와 시간 관련 메서드를 제공하는 **빌트인 객체** 이면서 **생성자 함수** 다.



### 30.1 Date 생성자 함수

Date 는 생성자 함수다. Date 생성자 함수로 생성한 Date 객체는 기본적으로 현재 날짜와 시간을 나타내는 정수값을 가진다. 현재가 아닌 특정 시간을 다루고 싶다면 명시적으로 해당 정보를 인수로 지정한다.

#### 30.1.1 new Date()

인수 없이 new 연산자와 호출하면 현재 날짜와 시간을 갖는 Date 객체를 반환한다. 내부적으로 날짜와 시간을 나타내는 정수값을 갖지만 콘솔에 출력하면 날짜와 시간 정보를 출력한다.

```jsx
new Date() // Thu Feb 09 2023 11:13:23 GMT+0900 (한국 표준시) {}
```

#### 30.1.2 new Date(milliseconds)

숫자 타입의 밀리초를 전달하면 1970년 1월 1일 0시 0분 0초를 기점으로 전달한 밀리초만큼 경과한 날짜와 시간을 나타내는 Date 객체를 반환한다.

```jsx
new Date(0) // Thu Jan 01 1970 09:00:00 GMT+0900 (한국 표준시) {}
```

#### 30.1.3 new Date(dateString)

날짜와 시간을 나타내는 문자열을 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date 객체를 반환한다. Date.parse 메서드에 의해 해석 가능한 형식이어야 한다.

```jsx
new Date('May 26, 2020 10:00:00')
// Thu May 26 2020 10:00:00 GMT+0900 (한국 표준시) {}
```

#### 30.1.4 new Date(year, month\[, day, hour, minute, second, millisecond])

연, 월, 일, 시, 분, 초, 밀리초를 의미하는 숫자를 전달하면 지정된 날짜와 시간을 나타내는 Date 객체를 반환한다. 연, 월은 필수값이다.

```jsx
new Date(2020, 2) // => 월의 2는 3월을 의미한다.
// Sun Mar 01 2020 00:00:00 GMT+0900
```



### 30.2 Date 메서드

#### 30.2.1 Date.now

1970년 1월 1일 기점으로 현재 시간까지 경과한 밀리초를 숫자로 반환한다.

```jsx
const now = Date.now() // 1675909557307
```

#### 30.2.2 Date.parse

1970년 1월 1일 기점으로 전달된 지정 시간까지의 밀리초를 숫자로 반환한다.

```jsx
Date.parse('Jan 2, 1970 00:00:00 UTC') // 86400000

Date.parse('Jan 2, 1970 00:00:00') // 86400000

Date.parse('1970/01/02/09:00:00') // 86400000
```

#### 30.2.3 Date.UTC

1970년 1월 1일 기점으로 전달된 지정 시간까지의 밀리초를 숫자로 반환한다.

```jsx
Date.UTC(1970, 0, 2) // 86400000
```

#### 30.2.4 Date.prototype.getFullYear

Date 객체의 연도를 나타내는 정수를 반환한다.

```jsx
new Date('2020/07/24').getFullYear() // 2020
```

#### 30.2.5 Date.prototype.setFullYear

Date 객체에 연도를 나타내는 정수를 설정한다. 월, 일도 지정 가능

```jsx
const today = new Date()
today.setFullYear(2000)
today.getFullYear() // 2000

today.setFullYear(2000, 2, 1)
```

#### 30.2.6 Date.prototype.getMonth

Date 객체의 월을 나타내는 정수를 반환한다. 0이 1월이다.

#### 30.2.7 Date.prototype.setMonth

Date 객체에 월을 나타내는 정수를 설정한다. 일도 지정 가능

#### 30.2.8 Date.prototype.getDate

Date 객체의 일을 나타내는 정수를 반환한다.

#### 30.2.9 Date.prototype.setDate

Date 객체에 일을 나타내는 정수를 설정한다.

#### 30.2.10 Date.prototype.getDay

Date 객체의 요일을 나타내는 정수를 반환한다. 0 - 일요일 6 - 월요일

#### 30.2.11 Date.prototype.getHours

Date 객체의 시간을 나타내는 정수를 반환한다.

#### 30.2.12 Date.prototype.setHours

Date 객체에 시간을 나타내는 정수를 설정한다. 분, 초, 밀리초도 설정 가능

#### 30.2.13 Date.prototype.getMinutes

Date 객체의 분을 나타내는 정수를 반환한다.

#### 30.2.14 Date.prototype.setMinutes

Date 객체에 분을 나타내는 정수를 설정한다. 초, 밀리초도 설정 가능

#### 30.2.15 Date.prototype.getSeconds

Date 객체의 초을 나타내는 정수를 반환한다.

#### 30.2.16 Date.prototype.setSeconds

Date 객체에 초를 나타내는 정수를 설정한다. 밀리초도 설정 가능

#### 30.2.17 Date.prototype.getMilliSeconds

Date 객체의 밀리초를 나타내는 정수를 반환한다.

#### 30.2.18 Date.prototype.setMilliSeconds

Date 객체에 밀리초를 나타내는 정수를 설정한다.

#### 30.2.19 Date.prototype.getTime

1970년 1월 1일을 기점으로 Date 객체의 시간까지 경과된 밀리초를 반환한다.

#### 30.2.20 Date.prototype.setTime

Date 객체에 1970년 1월 1일을 기점으로 경과된 밀리초를 설정한다.

#### 30.2.21 Date.prototype.getTimezoneOffset

UTC와 Date 객체에 지정된 로캘 시간과의 차이를 분 단위로 반환한다.

#### 30.2.22 Date.prototype.toDateString

문자열로 Date 객체의 날짜를 반환한다.

#### 30.2.23 Date.prototype.toTimeString

Date 객체의 시간을 표현한 문자열을 반환한다.

#### 30.2.24 Date.prototype.toISOString

ISO 8601 형식으로 Date 객체의 날짜와 시간을 표현한 문자열을 반환한다.

#### 30.2.25 Date.prototype.toLocaleString

전달한 로캘을 기준으로 Date 객체의 날짜와 시간을 표현한 문자열을 반환한다.

#### 30.2.26 Date.prototype.toLocaleTimeString

전달한 로캘을 기준으로 Date 객체의 시간을 표현한 문자열을 반환한다.



### 30.3 Date를 활용한 시계 예제

```jsx
(function printNow(){
  const today = new Date()
  
  const dayNames = ['일요일','월요일','화요일','수요일','목요일','금요일','토요일']
  
  const day = dayNames[today.getDay()]
  
  const year = today.getFullYear()
  const month = today.getMonth() + 1
  const date = today.getDate()
  let hour = today.getHours()
  let minute = today.getMinutes()
  let second = today.getSeconds()
  const ampm = hour >= 12 ? "PM" : "AM"
  
  hour %= 12
  hour = hour || 12
  
  minute = minute < 10 ? '0' +  minute : minute
  second = second < 10 ? '0' + second : second
  
  const now = `${year}년 ${month}월 ${date}일 ${day} ${hour}:${minute}:${second} ${ampm}`
  console.log(now)
  setTimeout(printNow, 1000)
}())
```
