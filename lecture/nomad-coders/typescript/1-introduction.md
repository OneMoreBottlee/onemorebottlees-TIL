# #1 Introduction

### #1.2 Welcome

TS는 JS 기반으로 만든 언어

JS의 여러 문제를 해결하고 보완하기 위해 만들어짐 & 더 나은 개발자가 되고,  경험할 수 있도록 해줌

TS에 빠지면 JS로 돌아갈 수 없단다\~



### #1.3 Who Sholud Take This Course

### #1.4 Software Requirements

with node.js & VS code

\=> MS 가 만든 TS는 MS 가 만든 VS Code 에서 최고의 효율



### #1.5 Why not JS

타입 스크립트의 존재 이유...\
Type Script

**타입 안정성** !!\
ㄴ 버그가 줄어든다.\
ㄴ 런타임 에러가 줄어든다.\
ㄴ 생산성이 늘어난다.



**JS**

매우 유연한 언어임.

멍청하고 이상한 코드도 에러없이 작동한다.

```javascript
// 1.
[1, 2, 3, 4] + false // '1,2,3,4false' => ?????

// 2.
function divide(a, b){
    return a / b
}

divide(2, 3) // 0.6666...666
divide("xxxxx") // NaN => ??? 2개의 인자를 보내야하는데도 error가 아님
// 2개의 인자는 숫자 타입이어야하지만 문자열이 포함되도 error가 아님
```



\*런타임 에러 - 콘솔 안에서 발생하는 에러

```javascript
const nico = { name: "nico" }

nico.hello() // error => but 실행 후 확인한에러임. 실행되기 전에 알려주는게 더 적합함
```



이러한 JS의 독특한 특징을 보완한 언어가 바로 TS !

