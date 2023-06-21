---
description: 2022.09.25 - 2022.09.26
---

# 바닐라 JS로 그림 앱 만들기

학습한 canvas를 조금 더 알아보기 위해 수강 시작



[배포](https://onemorebottlee.github.io/drawing-app/)

[깃허브](https://github.com/OneMoreBottlee/drawing-app)



<figure><img src="../../.gitbook/assets/image (131).png" alt=""><figcaption></figcaption></figure>



### #0 Introduction

지난 주 배운 Canvas 활용, 그림앱 만들기 강의시작



### #1 The Canvas API

HTML에 Canvas를 추가하고 JS에서 기능 구현

Canvas의 기능에 대해 배우고 기능을 활용해 집, 사람 만들기

```jsx
const canvas = canvas 주소 가져오기
const ctx = canvas.getContext("2d"); // 그림 그리기용 콘텍스트
canvas.width = 가로 크기 정하기
canvas.height = 세로 크기 정하기

ctx.fillRect(x,y,굵기,길이) // 사각형 만들기
ctx.moveTo(x,y) // 선을 긋지 않으면서 펜을 이동하기
ctx.lineTo(x,y) // 선을 그으면서 펜을 이동
ctx.lineWidth = 2; //선 굵기
ctx.stroke() // 그은 선 칠하기
ctx.fill() // 그은 선 채우기
ctx.fillStyle = "white" // 색 정하기
ctx.beginPath() // 새로운 패스 만들기
ctx.arc(x,y, 반지름, startAngle, endAngle) // 원 그리기

```

***

### #2 Painting Board

**기능 추가**

* 그림
* 색
* 채우기
* 클리어
* 지우개



### #3 Meme Maker

이미지 추가, 텍스트 삽입, 이미지 저장, CSS

![](<../../.gitbook/assets/image (162).png>)

