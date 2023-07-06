# #7 ANIMATIONS

{% embed url="https://codesandbox.io/s/motion-56gmgc?file=/src/App.js" %}



### #7.1

[Examples | Framer for Developers](https://www.framer.com/docs/examples/)

Framer Motion

React용 production-ready 모션 라이브러리 (오픈 소스)

[https://www.framer.com/motion](https://www.framer.com/motion)

[https://github.com/framer/motion](https://github.com/framer/motion)



npm i framer-motion

import { motion } from "framer-motion”



motion 사용시 기존의 \<div> 은 사용할 수 없음

\<motion.div> 로 사용해야함



CRACO

CRACO(Create React App Configuration Override)는 create-react-app을 위한 쉽고 이해하기 쉬운 configuration 레이어입니다. 애플리케이션 root에 단일 configuration(예: craco.config.js) 파일을 추가하고 eslint, babel, postcss configuration 등을 사용자 지정하여 'eject'를 사용하지 않고 create-react-app 및 사용자 지정의 모든 이점을 얻으십시오.



[https://github.com/gsoft-inc/craco](https://github.com/gsoft-inc/craco)

[https://github.com/gsoft-inc/craco/blob/master/packages/craco/README.md#installation](https://github.com/gsoft-inc/craco/blob/master/packages/craco/README.md#installation)



craco.config.js

```

module.exports = {

webpack: {

configure: {

module: {

rules: [

{

type: 'javascript/auto',

test: /\\.mjs$/,

include: /node_modules/,

},

],

},

},

},

}

```



### #7.2 Animation

Framer Motion의 애니메이션은 모션 컴포넌트의 유연한 animate 속성을 통해 제어된다.

간단한 애니메이션은 animate props에서 직접 값을 설정할 수 있다.

initial - 시작 / transition - 변화 방법 / animate - 종료



### #7.3 **Variants part**

Variants 여러 애니메이션을 합칠 수 있음 & 깔끔한 코드

```typescript
const variant = {
    start: {},
    end: {}
}

motion.div variant={variant} initial=“start” end=”end”
```



### #7.4

부모 요소의 애니메이션과 props를 자식 요소에도 전달한다.

& 각 자식 요소에 각각 딜레이 주는 등의 미친 기능도 가능하다.



delayChildren: 딜레이 시간(초) 후 하위 애니메이션 시작

staggerChildren: 하위 컴포넌트의 애니메이션에 지속 시간(초)만큼 시차 예를 들어, staggerChildren이 0.01이면 첫 번째 자식은 0초, 두 번째 자식은 0.01초, 세 번째 자식은 0.02초 지연. 계산된 stagger 딜레이가 delayChildren에 추가된다.

inherit: boolean

부모로부터 variant 변경 사항을 상속하지 않도록 하려면 false로 설정.



### #7.5 Gestures

whileHover - 호버 중일때 작동할 애니메이션

whileTap - 클릭 중일때 작동할 애니메이션



Drag

drag: boolean | “x” | “y”

끌기 활성화- 기본 false 양방향 드래그를 원하면 true, 특정 방향으로 하려면 x or y 설정

whileDrag - 드래그 중일때 작동할 애니메이션

dragConstraints - 드래그가 허용되는 범위 (1. 픽셀 크기 이용 2. ref 이용)

dragSnapToOrigin - 원래 자리로 돌아감

dragElastic - 외부 제약 조건에서 허용되는 이동 정도 0=움직임 없음 1=전체 움직임 기본 0.5 false가능



### #7.7 MotionValues

특정 값을 추적하지만 state가 아니다?!

.set() .get() 메서드로 값을 업데이트하거나 지정할 수 있다.



### #7.8 useTransform

특정값을 다른 값으로 변환해준다.

useTransform(x, input, output) ⇒ x의 값을 input 범위에서 output 범위로 변환한다.



### #7.9 useScroll

뷰포트가 스크롤 될 때 업데이트되는 MotionValues 값을 리턴한다.

scrollX - 실제 수평 스크롤 픽셀

scrollY - 실제 수직 스크롤 픽셀

scrollXProgress - 0\~1 사이의 수평 스크롤

scrollYProgress - 0\~1 사이의 수직 스크롤

이 값을 이용해 스크롤 위치에 따라 애니메이션을 추가할 수 있음 !!



### #7.a package.json 한번에 업데이트하기

npm-check-updates 사용



npm install -g npm-check-updates // 패키지 설치

ncu -u // 업데이트 가능한 항목 확인

npm install // 업데이트



### #7.10 SVG Animation

fontAwsome 에서 svg 파일 가져오기

path(SVG) ⇒ 모양을 정의하는 엘리먼트

transition 각각 줄 수 있음 ㅁㅊㄷ !



Line drawing

svg 엘리먼트에 'pathLength', 'pathSpacing', 'pathOffset' 속성을 사용하여 Line drawing 애니메이션을 만들 수 있습니다.

[https://www.framer.com/docs/examples/#line-drawing](https://www.framer.com/docs/examples/#line-drawing)



path (SVG)

path SVG 엘리먼트는 모양을 정의하는 일반 엘리먼트입니다.모든 기본 모양은 path 엘리먼트로 만들 수 있습니다.

path의 속성 d는 경로의 모양을 정의합니다.

[https://developer.mozilla.org/en-US/docs/Web/SVG/Element/path](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/path)



Path

motion.path 컴포넌트는 세 가지 강력한 SVG path 속성인 pathLength, pathSpacing 및 pathOffset을 가지고 있습니다. 수동 경로 측정이 필요 없이 모두 0과 1 사이의 값으로 설정됩니다.



Line drawing

선 그리기 애니메이션은 pathLength, pathSpacing 및 pathOffset의 세 가지 특수 속성을 사용하여 많은 SVG 요소로 만들 수 있습니다.

ex) motion.circle initial=\{{ pathLength: 0 \}} animate=\{{ pathLength: 1 \}}

[https://www.framer.com/docs/examples/#line-drawing](https://www.framer.com/docs/examples/#line-drawing)

*   코드

    ```jsx
    const Svg = styled.svg`
      width: 300px;
      height: 300px;
      color: white;
      stroke: white;
      stroke-width:3;
    `
    const svg = {
      start: {
        fill: "rgba(255,255,255,0)",
        pathLength: 0
      },
      end: {
        fill: "rgba(255, 255, 255, 1)",
        pathLength: 1,
      }
    }

    return 
    	...
    			<Svg
            xmlns="<http://www.w3.org/2000/svg>"
            viewBox="0 0 488 512">
            <motion.path
              variants={svg}
              initial="start"
              animate="end"
              transition={{
                default: { duration: 3 },
                fill: { duration: 2, delay: 3 }
              }}
              d="M488 261.8C488 403.3 391.1 504 248 504 110.8 504 0 393.2 0 256S110.8 8 248 8c66.8 0 123 24.5 166.3 64.9l-67.5 64.9C258.5 52.6 94.3 116.6 94.3 256c0 86.5 69.1 156.6 153.7 156.6 98.2 0 135-70.4 140.8-106.9H248v-85.3h236.1c2.3 12.7 3.9 24.9 3.9 41.4z" />
          </Svg>
    ```



### #7.11 AnimatePresence

조건부 showing

\<AnimatePresence>\</AnimatePresence> 사이에 조건부 등장&삭제 기능이 있으면 animate 기능

initial=””, animate=””, exit=””

exit - 사라질때 발생할 애니메이션

*   코드

    ```jsx
    import { AnimatePresence } from "framer-motion";

    const boxxVariants = {
      start: {
        opacity: 0,
        scale: 0
      },
      end: {
        opacity: 1,
        scale: 1,
        rotateZ: 360
      },
      leaving: {
        opacity: 0,
        scale: 0,
        y: 50
      }
    }

    function ...
    	const [showing, setShowing] = useState(false);
      const toggleShowing = () => setShowing((prev) => !prev)

    return 
    		...
    		<button onClick={toggleShowing}>Click</button>
    		      <AnimatePresence >
    		        {showing ? <Box variants={boxxVariants} initial="start" animate="end" exit="leaving" /> : null}
    		      </AnimatePresence>
    ```



### #7.12 \~ 13 Slider

next

prev



### #7.13

custom

각 애니메이션 컴포넌트에 대해 동적 variants 를 다르게 적용할 때 사용하는 사용자 지정 데이터

*   코드

    ```jsx
    const Box = styled(motion.div)`
      width: 400px;
      height: 200px;
      display: flex;
      justify-content: center;
      align-items: center;
      background-color: rgba(255, 255, 255, 1);
      border-radius: 40px;
      font-size: 28px;
      box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1), 0 10px 20px rgba(0, 0, 0, 0.06);
      position: absolute;
      top:100px;
    `
    const box = {
      start: (back: boolean) => ({
        x: back ? -500 : 500,
        opacity: 0,
        scale: 0,
        transition: { duration: 0.5 }
    }),
      end: {
        x: 0,
        opacity: 1,
        scale: 1,
        transition: {
          duration: 0.5,
        }
      },
      exit: (back: boolean) => ({
        x: back ? 500 : -500,
        opacity: 0,
        scale: 0,
        transition: { duration: 0.5 }
      }),
    }

    function ...
    const [visible, setVisible] = useState(1);
      const [back, setBack] = useState(false);
      const nextPlease = () => {
        setBack(false);
        setVisible((prev) => (prev === 10 ? 1 : prev + 1))
      }
      const prevPlease = () => {
        setBack(true);
        setVisible((prev) => (prev === 1 ? 10 : prev - 1))
      }

    return
    		... 
    			<AnimatePresence custom={back} mode="wait">
            <Box key={visible}  custom={back} variants={box} initial="start" animate="end" exit="exit">{visible}</Box>
          </AnimatePresence>
          <button onClick={prevPlease}>prev</button>
          <button onClick={nextPlease}>next</button>
    ```



### #7.14

애니메이션을 적용하지 않아도 알아서 적용함



layout

변화 발생하면 자동 animation 적용



Animate between components

동일한 layoutId prop을 가진 모션 컴포넌트 간 애니메이션을 적용한다.

*   코드

    ```jsx
    const Circle = styled(motion.div)`
      background-color: #00a5ff;
      height: 100px;
      width: 100px;
      border-radius: 50px;
      box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1), 0 10px 20px rgba(0, 0, 0, 0.06);
    `

    function ...
    	const [clicked, setClicked] = useState(false)
      const toggleClicked = () => setClicked((prev) => !prev)

    return ...
    		<Wrapper onClick={toggleClicked} style={{ background: gradient }}>
          <Box>
            {!clicked ? <Circle layoutId="circle" /> : null}
          </Box>
          <Box>
            {clicked ? <Circle layoutId="circle" /> : null}
          </Box>
        </Wrapper>
    ```



### #7.15

### #7.16

[Motion](https://codesandbox.io/s/motion-56gmgc?file=/src/App.js)



