# 브라우저 렌더링 과정에 대해 아는 만큼 설명해주실 수 있을까요? 예를 들어 화면에서 DOM이 어떻게 결정되고, CSS는 어떻게 입혀지는지 등을 언급해주시면 좋습니다.

## 렌더링이란?

HTML, CSS, 자바스크립트 등의 문서가 브라우저에 출력되는 과정을 말함
브라우저는 이러한 렌더링을 수행하는 데 필요한 렌더링 엔진을 가지고 있음
ex: chrome : blink, safari: webkit...

## 렌더링 과정

사용자가 브라우저를 통해 웹 사이트에 접속 -> 서버로부터 HTML, CSS 등의 리소스를 다운 받음.
브라우저가 페이지를 렌더링하려면 HTML로는 DOM, CSS로는 CSSOM tree를 생성해야함

### DOM 생성

<img src="https://miro.medium.com/v2/resize:fit:1400/0*rkjgCl-RSVTvRGgS" width="50%" height="50%">

사용자는 서버에서 HTML을 받아 파싱해 이를 DOM으로 변환함
DOM이란 브라우저 내부의 구조에 대한 표현이자 개발자가 자바스크립트를 통해 상호작용할 수 있는 주로 및 API임

**HTML이 DOM Tree로 변환되는 과정**

1. 변환 : 브라우저가 HTML의 원시 바이트를 읽어와서 HTML에 정의된 인코딩(대부분 UTF-8)에 따라 문자로 변환함
   ex: 원시 바이트(01001000) -> 문자("<!DOCTYPE html">)

2. 토큰화 : 1에서 변환한 문자열을 W3C 표준에 지정된 고유 토큰(ex: 시작 태그, 종료 태그, 주석 등 )으로 변환함
   ex: 문자("<!DOCTYPE html">) -> 토큰(<!DOCTYPE html>, <html>, <head>)

3. 렉싱 : 방출된 토큰은 해당 속성 및 규칙을 정의하는 객체로 변환됨
   ex: 토큰(<html>) -> 객체(Node: {type: 'tag, name: 'html', attrs:[]...})

4. DOM 생성 : HTML 마크업에 정의된 여러 태그 간의 관계를 해석해 트리구조로 연결함 -> DOM TREE 생성

ex:  
<img src="https://developer.mozilla.org/ko/docs/Web/Performance/How_browsers_work/dom.gif" width="50%" height="50%">

브라우저는 HTML 마크업을 처리할 때마다 위의 모든 단계를 수행함

#### 그럼 CSSOM은 언제 생성될까?

브라우저 엔진이 HTML 문서를 읽으면서 DOM을 생성함 -> CSS 링크(<link> 태그) 또는 내부 스타일(<style> 태그)를 만나면 잠시 DOM 생성을 일시 중단하고 해당 CSS 코드를 해석해 CSSOM을 생성함
-> CSSOM 생성 완료 -> 다시 DOM 생성을 이어서 진행함

또한 자바스크립트를 만나면 ex: `<script>` 브라우저는 또 작업을 멈추고 자바스크립트 코드를 로드, 파싱, 실행함. 왜냐하면 자바스크립트는 문서의 구성을 바꿀 수 있기 때문임.

위와 같은 작동 방식 때문에 렌더링 차단(render blocking)이 발생할 수 있음.
즉, css나 자바스크립트 처리가 완료 되기 전까지 화면에 요소가 표시 안 될 수도 있음. 따라서 CSS 파일이나 내부 스타일 위치와 순서를 조절해야함(중요 렌더링 경로(critical rendering path))

- 방법1
  외부 css 파일을 <head>에 위치시키면 페이지 로딩 초기 단계에 css를 빠르게 로드하고 DOM 생성 과정에서 렌더링 차단을 최소화할 수 있음 -> 웹 페이지 빠르게 렌더링 -> 사용자 경험 향상.

- 방법2
  자바스크립트 코드를 <body> 맨 아래 위치시키기

- 방법3
  웹팩과 같은 번들러를 이용해 스크립트와 리소스를 번들화해 서버에 대한 요청을 최대한 줄임

이외에도 여러가지 방법이 있음

### CSSOM Tree 생성

<img src="https://miro.medium.com/v2/resize:fit:1400/0*wO7ezCeTdpHyhgWm" width="50%" height="50%">

CSS도 HTML과 마찬가지로 css 파일에 정의된 스타일과 style 태그에 작성된 스타일을 브라우저가 이해하고 처리할 수 있는 형식으로 변환해야 함. DOM 트리를 생성하는 과정과 동일하게 CSSOM 트리를 생성함(위의 이미지 참고)

CSSOM 트리가 완성되면 아래와 같은 구조임
<img src="https://miro.medium.com/v2/resize:fit:1164/0*SMOVnyZjS0-Tp-pp" width="50%" height="50%">

DOM과 CSSOM은 둘 다 트리 구조임. 둘은 각각의 독립적인 자료구조임.

### 렌더

렌더링 과정에는 스타일, 레이아웃, 페인트 그리고 때때로 합성이 포함됨

#### 스타일

<img src="https://miro.medium.com/v2/resize:fit:1400/0*9Xbmy7JUOcRxn2Vh" width="50%" height="50%">

DOM Tree와 CSSOM Tree가 만들어지면 이 두 개를 결합해 렌더링 트리를 생성함. 렌더링 트리에는 페이지를 렌더링하는 데 필요한 노드만 포함되고 스타일 정보를 포함함

#### 레이아웃(리플로우)

<img src="https://miro.medium.com/v2/resize:fit:1232/0*1ZVisC80ge0AllX4" width="50%" height="50%">

렌더트리가 만들어지면 이를 기반으로 브라우저는 뷰포트 내에서 각 노드의 정확한 크기와 위치를 계산함.
이때 상대 측정은 픽셀로 변환됨
또한 보이지 않더라도 계산된 스타일과 함께 어떤 노드가 화면에 표시될시 식별함.

### 페인팅(리페인팅)

레이아웃 과정에서 계산된 정보를 사용해 화면에 각 노드를 그려내는 과정

레이아웃 단계에서 계산된 각 노드들의 위치, 크기, 색상 등을 실제 화면의 픽셀로 변환하게 됨

렌더트리를 따라 페인트 기록이 생성됨

페인트 기록에는 요소를 렌더링하는 순서, 지금까지의 정보(컬러, 보더, 쉐도우, 텍스트, 이미지 등)를 바탕으로 한 페이지를 여러 개의 레이어로 나눈 다음 그 위에
모든 시각적인 부분이 진행됨

### 합성(compositing)

페인팅 단계에서 만든 레이어들을 화면에 픽셀로 표현함
또한 나누어진 레이어들을 하나로 합성해서 페이지를 완성함

## 리플로우, 리페인팅

**리플로우, 리페인트**
기존에 생성한 레이아웃이 사용자와의 인터렉션이나 이벤트로 인해 변경되면 브라우저는 렌더트리를 다시 생성하고 레이아웃 과정을 다시 수행함. 이를 리플로우라고 함

리플로우는 단지 변경사항을 반영하기 위해 수행되고, 실제 그 결과를 화면에 나타나게 하려면 다시 페인딩 단계를 수행해야함(리페인팅)

따라서 리플로우가 일어나면 반드시 리페인트로 발생함

하지만 레이아웃의 영향이 없는 변경(ex: 색상 변경)은 리플로우 없이 리페인트만 실행함

**리플로우가 일어나는 대표 속성들**
position, width, height, margin, padding, border, border-width, font-size, font-weight, line-height, text-align, overflow, float, display, left, top, bottom...

**리페인트만 일어나는 대표 속성들**
background, color, text-decoration, border-style, border-radius, box-shadow, outline...

**중요**
transform, opacity 속성은 리플로우, 리페인트 없이 변경가능(GPU가 관여할 수 있는 속성이기 때문에)

따라서 애니메이션 효과를 주고 싶을 때 position 같은 속성보다는 transform 속성을 사용하는 것이 좋음

\*23.10.1 추가
tailwind에서 GPU를 통해 렌더할 수 있는 기능이 있음
바로 `tarnsform-gpu`임
예를 들어 tarnsform: rotate(3deg)를 적용하려 할 때 CPU를 사용한 것보다 GPU를 사용한 것이 렌더할 때 더 좋은 퍼포먼스를 보여준다면, 이 속성을 붙임으로써 하드웨어를 가속할 수 있음

**참고**

- https://youtu.be/z1Jj7Xg-TkU
- https://medium.com/%EA%B0%9C%EB%B0%9C%EC%9E%90%EC%9D%98%ED%92%88%EA%B2%A9/%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80%EC%9D%98-%EB%A0%8C%EB%8D%94%EB%A7%81-%EA%B3%BC%EC%A0%95-5c01c4158ce
- https://developer.mozilla.org/ko/docs/Web/Performance/How_browsers_work
- https://ajdkfl6445.gitbook.io/study/web/browser-rendering
