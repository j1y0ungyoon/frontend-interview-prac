# 브라우저 렌더링 과정에 대해 아는 만큼 설명해주실 수 있을까요? 예를 들어 화면에서 DOM이 어떻게 결정되고, CSS는 어떻게 입혀지는지 등을 언급해주시면 좋습니다.

## 브라우저 란?

유저가 선택한 자원을 서버로 부터 받아와서 유저에게 보여준다. 웹페이지, 이미지, 비디오 등

브라우저 엔진 : chrome, safari, firefox, internet Explorer

<img src="https://velog.velcdn.com/images/sylagape1231/post/78c816ae-4739-42eb-9f32-04322806b154/image.png">

1. 사용자인터 페이스
   - 주소 표시줄, 이전, 이후 버튼, 홈버튼, 북마크 버튼 ... 등
2. 브라우저 엔젠 / 레이아웃 엔진
   - 사용자 인터페이스와 렌더링 엔진 사이의 동작을 제어한다.
3. 렌더링 엔진
   - 웹페이지가 표시되는 모든 영역을 제어
   - 요청한 콘텐츠(HTML,CSS)를 파싱하고, 화면에 나타내는 일을 수행한다.
4. 자료저장소/데이터 저장소
   - 쿠카, localStorage, indexDB 같이 로컬에 저장되어 좀 더 오래 유지되어야 하는 데이터들을 보관할 수 있또록 지원하는 영역
5. 통신(Networking)
   - HTTP/HTTPS 네트워크 처리
6. 자바스크립트 해석기 (JS InterPreter) / 자바스크립트 엔진
   - 스크립트(JS 코드)를 파싱할 때 사용하는 엔진
   - HTML 파싱 중 script태그를 만나면, JS 엔진이 제어 권한을 넘겨 받는다. (동기적으로 진행됨 - JS 이 작업을 마칠 때 까지 HTML에서 진행 중인 과정은 잠시 중단)

## 렌더링 이란?

서버로 부터 HTML. CSS, JS 등 작성한 파일을 받아 브라우저에 뿌려 주는 것을 말한다.

## 브라우저 렌더링 동작 과정

1. HTML 파일과 css 파일을 파싱해서 각각 Tree를 만든다. (Parsing)
2. 두 Tree 를 결합하여 Rendering Tree를 만든다 (Style)
3. Rendering Tree에서 각 노드의 위치와 크기를 계산한다. (Layout)
4. 계산된 값을 이용해 각 노드를 화면상의 실제 픽셀로 변환하고, 레이어를 만든다. (Paint)
5. 레이어를 합성하여 실제 화면에 나타 낸다.(Composite)

### 1. parsing

브라우저가 페이지를 렌더링 하려면 가장 먼저 받아온 HTML 파일을 해석 해야한다. (HTML 파일을 해석하여 DOM Tree를 구성)
파싱중 HTM에 CSS가 포함 되어있다면 CSSOM(CSS Objcet Model)Tree를 구성한다.

<img src="https://tecoble.techcourse.co.kr/static/1d5973bb2abd4ea8580e2d6f9f286640/1805d/2021-10-24-browser-rendering-1.png">

### 2. style

style 단계에서 DOM Tree 와 CSSOM Tree를 매칭시켜 Render Tree(실제 화면에 그려질 Tree)를 구성한다.

예를 들면 Render Tree를 구성할 때 visibility: hidden은 요소가 공간 차지를 하고 보이지 않기 때문에 Render Tree에 포함되지만 display:none의 경우 Render Tree에서 제외된다

<img src="https://tecoble.techcourse.co.kr/static/812332bcab15fdc8d05543579dad9f5c/919e0/2021-10-24-browser-rendering-2.png">

### 3. layout

Layout 단계에서는 Render Tree를 화면에 어떻게 배치해야 할 것인지 노드의 정확한 위치와 크기를 계산한다.

루트부터 노드를 순회하면서 노드의 정확한 크기와 위치를 계산하고 Render Tree에 반영한다. 만약 크기 값을 %로 지정했다면, Layout 단계에서 %값을 계산해서 px 단위로 변환한다.

### 4. paint

Paint 단계에서는 Layout단계에서 계산된 값을 이용해 Render Tree의 각 노드를 화면상의 실제 픽셀 로 변환한다. 이 때 픽셀로 변환된 결과는 하나의 레이어가 아니라 여러개의 레이어로 관리된다.

스타일이 복잡할 수록 paint 시간도 늘어난다. 단색배경 보다 그림자 효과 를 넣으면 시관과 작업이 더 많이 필요하다.

### 5. composite

Composite 단계에서는 Paint 단계에서 생성된 레이어를 합성하여 실제 화면에 나타낸다.

https://velog.io/@sylagape1231/%EC%A3%BC%EC%86%8C%EC%B0%BD%EC%97%90-naver.com%EC%9D%84-%EC%B9%98%EB%A9%B4-%EC%9D%BC%EC%96%B4%EB%82%98%EB%8A%94-%EC%9D%BC%EC%9D%84-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%B4%EB%B3%B4%EC%9E%90

https://www.youtube.com/watch?v=FQHNg9gCWpg
