# Virtual DOM이란 무엇이고 Real DOM과의 차이는 무엇인가요?

## DOM(Document Object Model)

DOM은 Document Object Model의 약자로 웹페이지(HTML이나 XML)의 콘텐츠 구조, 그리고 스타일 요소를 구조화시켜 프로그래밍 언어가 해당 문서에 접근해 읽고 조작할 수 있도록 API를 제공하는 일종의 인터페이스임
즉, 자바스크립트 같은 언어가 웹 페이지에 접근해 조작할 수 있게끔 연결시켜주는 역할을 함
DOM은 HTML 문서를 트리 구조로 표현하는데 이를 DOM Tree라고 부름.

<img src="https://www.w3schools.com/js/pic_htmltree.gif" width="50%" height="50%">

HTML문서는 하나의 최상위 노드(root node)에서 시작해서 자식 노드들을 가지고 아래로 뻗어나가는 구조가 됨

노드의 타입은 총 12개가 있지만 가장 중요한 것은 위의 이미지에서 볼 수 있듯이 총 4가지의 노드가 있음.

### document node

- DOM Tree에서 최상위 루트 노드를 나타내며 document 객체를 가리킴.
- DOM의 진입점이 되는 노드임
- HTML 문서에 오직 1개만 존재함

### element node

- HTML 태그에서 만들어지며(ex: body, h1, div...) DOM Tree를 구성하는 블록인 요소 노드를 말함
- attribute node를 가질 수 있는 유일한 노드(계층적 구조 때문에)

### attribute node

- element node에 대한 정보를 가지고 있음(ex: img 요소의 src 속성은 표시할 이미지의 url을 나타냄)
- 모든 HTML 요소의 '속성'은 attribute node임

### text node

- HTML의 모든 텍스트는 text node임
- 정보를 표현함
- 가장 마지막에 위치하는 자식노드임

-> 위와 같은 노드들이 있어서 자바스크립트 같은 스크립팅 언어가 window의 하위 객체인 document 객체를 통해서 HTML 태그로 이루어진 세부적인 각 노드에 접근하고 조작할 수 있게 됨

## DOM Tree의 문제점

DOM은 트리구조로 되어있어 이해하기 쉽고 탐색하기 좋다는 장점이 있음
하지만 웹이 크고 복잡할 수록 DOM Tree도 거대해짐 -> 변경하고자 하는 노드를 찾으려면 시간이 많이 걸리고, 어느 한 노드에 변경이 생겼을 때 거대한 트리를 전체를 재렌더링하는 비효율적인 면이 발생함

## virtual DOM

위의 문제점을 해결하기 위해 virtual dom이 나타남
virtual DOM은 실제 DOM 문서를 추상화한 개념으로 DOM의 복사본임

- 자바스크립트를 활용해 자바스크립트 객체로 표현되고 메모리 상에서 저장되어 동작하기 때문에 빠름
- 실제 DOM과 같은 속성들을 가지고 있지만 api(ex: getElementBy 등)는 갖고 있지 않음(왜냐면 단순히 가상돔은 자바스크립트 객체이기 때문에) -> 따라서 문서에 직접 접근 x, 수정 x
- 실제로 렌더링되지 않기 때문에 연산 비용이 적음

### virtual dom이 dom에 적용하는 과정

데이터가 변경되면 전체 UI는 virtual dom에 렌더링됨 -> 이전 가상돔에 있던 내용과 업데이트 된 가상 돔을 비교해 바뀐 부분만 실제 돔에 적용함 -> 전체 돔을 렌더링하지 않고 필요한 부분만 업데이트함

## virtual DOM을 무조건 사용해야할까?

정보 제공만 하는 웹페이지라면 인터랙션이 발생하지않음 -> 일반 DOM을 이용하는 것이 성능면에서 더 좋을 수 이씀

SPA로 제작된 큰 규모의 웹 페이지에서는 virtual dom을 사용해 브라우저 연산 양을 줄여 성능을 개선할 수 있음

-> 상황에 맞게 사용하자

## 리액트의 virtual dom 활용법

<img src="https://miro.medium.com/v2/resize:fit:1400/format:webp/1*1Vi0bIyXP7NCjuKR1hDyeQ.png" width="50%" height="50%">

리액트는 2개의 virtual dom을 가지고 있음

1. 변경 이전의 내용을 담고 있는 virtual dom.
2. 변경 이후에 보여질 내용을 가진 virtual dom

리액트는 state가 변경될 때마다 새로 렌더링이 됨
이렇게 state가 변경되어 렌더링이 발생되기 전(실제 dom이 변경되기 전)에 변경 이후 새로 바뀐 내용이 들어간 virtual dom을 만듦 -> 변경 이전의 내용을 가지고 있는 virtual dom과 비교해 어떤 부분이 바뀌었는지 파악함 (diffing) -> batch update를 사용해 바뀐 모든 요소들을 한꺼번에 바뀐 부분만 실제 돔에 적용시킴(reconciliation, 재조정)

**참고**

- https://youtu.be/gc-kXt0tjTM
- https://ko.javascript.info/dom-nodes
- https://youtu.be/PN_WmsgbQCo
- https://atulkawasthi.medium.com/what-is-the-dom-virtual-dom-afea2de36a00
