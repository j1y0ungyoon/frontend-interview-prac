# Web Vital을 개선할 수 있는 방안에 대해 설명해주실 수 있을까요? 예를 들어 LCP, CLS, FID 각각의 개념, 진단법, 각 지표 개선에 효과적인 조치 방안을 언급해주시면 좋습니다.

web vital은 웹에서 실제 사용자의 경험을 측정하는 측정 항목을 말함.
구글은 이를 core web vital이라고 하는데, 다음 3가지 항목을 측정함

1. LCP(최초 콘텐츠 렌더링 시간, Largest Contentful Paint)

<img src="https://web-dev.imgix.net/image/tcFciHGuF3MxnTr1y5ue01OGLBn2/ZZU8Z7TMKXmzZT2mCjJU.svg" width="50%" height="50%">

- LCP는 사용자에게 가장 큰 콘텐츠를 보여주는 데 걸리는 시간을 뜻함
- 자세하게 말하자면 사용자가 URL을 요청한 시점부터 가장 큰 시각 콘텐츠 요소(ex: 이미지, 비디오, 큰 블록 수준의 텍스트 등)를 렌더링하는 데 걸린 시간을 말함
- 이 측정항목은 방문자가 URL이 실제로 로드되는 것을 확인하는 속도를 표시하므로 중요함
- 페이지가 로드되기 시작한 지 첫 2.5초 안에 LCP가 발생해야 이상적임

2. CLS(레이아웃 변경 횟수, Cumulative Layout Shift)

<img src="https://web-dev.imgix.net/image/tcFciHGuF3MxnTr1y5ue01OGLBn2/dgpDFckbHwwOKdIGDa3N.svg" width="50%" height="50%">

- 웹페이지의 시각적 안정성을 측정함
- 사용자가 페이지에 방문한 후 상주한 시간을 모두 측정하고, 해당 시간 동안 페이지 내부에서 발생하는 레이아웃 및 개별적인 레이아웃 요소들의 변화가 몇 번이나 변경되었는지 측정함
- 숫자가 클수록 레이아웃이 많이 변경되었음을 나타냄
- 팝업과 같이 가독성을 해치는 요소가 얼마나 자주 표시되는지 측정하기 위해서 해당 항목이 있고, 팝업 등이 많은 뉴스 사이트의 경우 CLS값이 높게 평가됨
- CLS 점수가 0.1 미만이 되어야 좋은 사용자 환경을 제공한다고 할 수 있음

3. ~~FID(첫 입력 지연, First Input Delay)~~ -> INP(페인트에 대한 상호작용, interaction to next paint)

<img src="https://web-dev.imgix.net/image/tcFciHGuF3MxnTr1y5ue01OGLBn2/iHYrrXKe4QRcb2uu8eV8.svg" width="50%" height="50%">

- 사용자가 렌더링이 모두 끝난 웹에서 사용자가 웹 페이지와 처음 상호작용했을 때부터(어떤 요소를 클릭하거나 링크를 클릭하는 등) 브라우저가 상호작용에 반응할 때까지의 시간을 말함
- ex: 네이버에 접근 -> 로그인 버튼 누름 -> 로그인 페이지가 모두 로딩될 때까지 경과하는 시간 측정
- 사용자와 상호작용이 많이 일어나는 페이지에서 중요함
- FID가 100ms 미만이 되어야 이상적임

**그런데!!!** 구글에서 2023년 5월 10일 업데이트 한 내용에 따르면 **<U>2024년 3월부터 FID가 INP로 대체</U>**된다고 함

이유 : 처음 로드될 때의 시간보다 페이지가 로드된 후 사용자들의 이용 시간 중 90%가 사용됨. 따라서 사용자가 웹과 상호작용을 시작한 시점부터 다음 프레임이 그려질 떄까지의 시간을 최대한 짧게 하는 것이 중요해짐.
(ex: (여기)[https://www.seozoom.com/interaction-to-next-paint-or-inp-the-new-google-core-web-vital/] 중간에 동영상을 봐보셈)

FID는 사용자가 페이지와 **처음** 상호작용할 떄의 시간만 측정함. 하지만 처음 상호작용한 시간만 가지고는 페이지 전체의 모든 상호작용하는 시간을 대표할 수 없음.
따라서 모든 상호작용을 고려해 페이지 전체에서 가장 느린 상호작용과, 상호작용 시점부터 이벤트 핸들러를 거쳐 브라우저가 다음 프레임을 그릴 수 있을 떄까지의 전체 시간을 측정하는 INP로 대체되게 되었음

**INP(페인트에 대한 상호작용, interaction to next paint)**

<img src="https://i0.wp.com/tech.behindmatters.com/wp-content/uploads/sites/5/2022/05/Screenshot-2022-05-31-at-2.39.38-PM.png?w=1566&ssl=1" width="50%" height="50%">

INP는 사용자가 페이지를 방문하는 동안 발생하는 모든 클릭, 탭, 키보드 등의 상호작용의 지연 시간을 관찰해 사용자자와 웹의 상호작용에 대한 페이지의 전반적인 응답성을 평가하는 항목임.

- INP가 200ms 미만 : 페이지 응답성이 좋음
- INP가 200ms 초과 ~ 500ms 미만 또는 500ms : 페이지 응답성이 개선되어야 함
- INP가 500ms 초과 : 페이지 응답성이 좋지 않음

## 측정 방법

1. google Search Console
   구글에서 제시하는 측정 항목이므로 구글 서치콘솔에서 측정할 수 있음.
   구글 서치콘솔에 접속 후 분석을 원하는 사이트 URL을 등록하고 좌측 메뉴에서 [실험] -> [코어 웹 바이탈]을 누르면 됨.
   <img src="https://www.whatap.io/ko/blog/128/img/blog_02_1.webp" width="50%" height="50%">

페이지에 대한 개요를 제공하고, 우측 상단의 보고서 열기를 클릭하면 어떻게 측정되었는지 알 수 있음

2. google PageSpeed Insights

모바일, pc 모두에서 페이지 성능을 확인할 수 있고, LCP, CLS, FID를 측정할 수 있음.
코어 웹 바이탈 평가 합격 및 불합격 여부를 표시함

<img src="https://hjk329.github.io/assets/images/page-speed-insights-mobile-2022-10-20.png" width="50%" height="50%">

3. lightHouse
   성능 점수를 측정할 수 있는 도구. LCP, CLS를 측정할 수 있고, Total Blocking Time(TBT, 총 차단 시간)은 웹 바이탈 항목 중 FID와 관련성이 큼

   <img src="https://hjk329.github.io/assets/images/lighthouse-2022-10-20.png" width="50%" height="50%">

4. Chrome DevTools

performance 탭에서 시간 라인에 표시된 Timings 섹션에서 LCP 측정 가능함.  
performance 탭 하단에 Total Blocking Time을 통해 FID를 파악하는 용도로 사용할 수 있고, Experience 탭에서 CLS 측정 가능함

<img src="https://hjk329.github.io/assets/images/performance-2022-10-20.png" width="50%" height="50%">

## 개선 방안

### LCP

- CLS나 FID에 비해 이 항목을 통과하는 사이트가 적기 때문에 대부분의 사이트가 어려움을 겪는 항목임
- 보통 일반적인 웹의 70~80%는 검색하고 다운로드해야 하는 이미지 리소스를 가지고 있음.
  -> LCP를 개선하기 위해 할 수 있는 효과적인 방법 중 하나는 정적 HTML에서 이미지 소스를 검색할 수 있도록 만드는 것임 -> 이는 브라우저가 LCP 리소스를 가능한 빨리 이를 찾아 로드하는 데 도움이 됨

백드라운드 이미지, 클라이언트 렌더링, 레이지 로딩은 발견 x

```
// background-image는 발견 x
<div style="background-image: url(image-url.jpg)">

// 자바스크립트에 의해 로드되는 img 태그는 발견 x
<img data-src="image-url.jpg" alt="...">
<img ng-src="{{image-url.jpg}}" alt="...">

// 지연로딩되는 <img> 요소들은 무시됨
<img ng-src="{{image-url.jpg}}" alt="..." loading="lazy">
```

LCP 점수를 향상시키기 위해서는 아래와 같은 해결책을 사용하면 좋음

1. 기존 img 태그를 사용하기 & preload 링크를 추가하기

```
<img src="img-url.jpg" alt="...">

<link rel="preload" href="img-url.jpg" as="img">
```

2. fetchpriority API 사용

LCP 리소스가 브라우저에 의해 빨리 발견할 수 있도록 만드는 것이 이미지를 빨리 다운로드 받는다는 의미는 아님 -> 왜냐하면 브라우저는 이미지보다 CSS, 자바스크립트를 우선시하기 때문임
-> 그래서 리소스가 다운로드 시작될 때까지 지연이 생김 -> 그래서 새롭게 나온 fetchpriority API를 사용하면 리소스에서 어떤 것이 더 중요한지 표시할 수 있음.

```
<img src="img-url.jpg" alt="..." fetchpriority="high">

```

위와 같이 fetchpriority 속성(우선순위 가져오기)을 설정하고 LCP 요소를 미리 로드하면 브라우저가 다른 요소보다 더 일찍 해당 요소를 우선순위를 두고 다운로드를 시작할 수 있어서 LCP 시간에 큰 영향을 미칠 수 있음

-> 즉, fetchpriority를 사용하면 이미지가 발견되는 즉시 훨씬 더 빨리 이미지가 다운로드 됨

크롬 기반 브라우저에서 이 API를 사용할 수 있고 사파리는 개발 중임. 하지만 그 외의 브라우저에서는 제공하지 않으므로 해당 API는 무시됨

이미 유명한 자바스크립트 프레임워크들에는 이미 위와 같은 기능들을 이미 제공하고 있음(크롬 aurora 팀과 협업해서 만듦) 예를 들면 아래와 같음

- Next.js: next/image
- Angular: NgOptimizedImage
- Nuxt : `<nuxt-img>`

### CLS

CLS 지표를 향상 시키는 방법은 아래와 같음

1. 페이지에서 로드되는 모든 콘텐츠에 명시적인 크기 설정
   이미지나 동영상 같은 요소에 width와 height 속성을 설정하면 브라우저가 페이지 레이아웃을 계산하는데 필요한 공간을 사전에 예약할 수 있으므로 CLS 점수가 향상됨.

```
video {
   width: 100%;
   height: auto;
   aspect-ratio: 19/9
}
```

만약 헤더와 메인 콘텐츠 사이에 광고 등의 동적 요소가 들어간다면 그 영역의 min-height를 명시적으로 설정해서 공간을 명시하면 CLS의 영향을 줄일 수 있음

2. bfcache(Back-Forward Cache)

bfcache는 사용자가 뒤로가기 버튼이나 앞으로 가기 버튼을 클릭했을 때, 이전 페이지 상태를 캐싱하여 빨리 로딩하게 하는 기능임. bfcache 적용 시 CLS 점수 개선효과가 있음.

처음 페이지를 로드할 때 이미지와 광고 같은 추가 콘텐츠가 로드됨에 따라서 많은 CLS가 발생할 수 있음. bfcache는 사용자가 다른 페이지로 이동한 후 이전에 완전히 로드된 페이지의 전체 스냅샷을 메모리에 저장함. 뒤로가기 버튼을 누르면 즉시 스냅샷이 복원됨.
마찬가지로 사용자가 다시 앞으로 이동하면 스냅을 복원함. 이렇게 bfcache를 사용하면 페이지가 처음부터 다시 렌더링될 경우 발생하는 CLS 로딩이 완전히 제거됨.

bfcache는 기본적으로 활성화 되어있음. 크롬 개발자도구의 [application] -> [background services] -> [Back/forward cache] 에서 확인할 수 있고 테스트도 해볼 수 있음

3. 레이아웃 관련 CSS 속성을 사용하는 animation/transitions 피하기
   레이아웃을 변경하는 애니메이션은 브라우저가 페이지를 다시 레이아웃을 해야하므로 많은 작업이 필요함

```
.box {
   position: absolute;
   top: 10px;
   left: 10px;
   animation: move 3s ease;

}

@keyframes move {

   0% {
      top: calc(90vh - 160px);
   }
}
```

예를 들어 top, left를 사용해 콘텐츠를 이동시키면 주변 콘텐츠가 이동하지 않더라도 레이아웃 변경으로 간주함. 콘텐츠 자체가 변화하고 있고 다른 콘텐츠에 영향을 미칠 가능성이 있음 -> CLS에
영향을 줌

하지만 위 예시의 애니메이션을 transform을 사용해 수행하면 브라우저의 레이아웃 처리에서 콘텐츠가 이동하지 않고 다른 콘텐츠에 영향을 미치지 않고 CLS에 영향을 주지 않음

```
.box {
   position: absolute;
   top: 10px;
   left: 10px;
   animation: move 3s ease;

}

@keyframes move {

   0% {
      transform: translateY(calc(90vh - 160px));
   }
}
```

transform과 opacity 외의 다른 CSS 속성들은 페이지의 재배치(re-layout) 과정에서 변동사항 발생 시 CLS 점수를 떨어뜨릴 수 있음

## FID/INP (First Input Delay/Input Latency)

1. 긴 작업 피하기 혹은 분할하기
   메인 스레드에서 오래 걸리는 JavaScript 작업은 FID 점수를 떨어뜨릴 수 있음.
   여기서 오래 걸린다는 기준은 50ms 이상 걸리는 작업을 말함(크롬 개발자도구와 라이트하우스 기준)

이런 작업을 분할하거나 task scheduling을 컨트롤하기 위해 새로 나온 API를 사용함

- isInputPending()
  allow you to check if there are important input tasks waiting to be processed and to yield early if there are.

- scheduler.postTask()
  allow you to schedule a task with a priority. this allows the browser to only execute the task if there are no higher priority tasks.

- scheduler.yield()
  allow javascript to yield to other higher priority tasks, but then continue processing without yielding to same priority events.
  under development for chrome

2. 불필요한 JavaScript 피하기
   자바스크립트를 최적화하는 것도 좋지만 제일 좋은 것은 자바스크립트를 적게 보내는 것임. 사용하지 않는 코드는 제거하고, 필요한 코드만 로드하는 코드 스플리팅을 적용함

```
// Next.js에서 아래와 같이 사용할 수 있음

// example for beforeInteractive

<script src="http://example.com/script.js"
strategy="beforeInteractive" />

// example for afterInteractive(default)

<script src="http://example.com/script.js"/>

// example for lazyonload

<script src="http://connect.facebook.net/en_US/sdk.js"
strategy="lazyOnload" />

```

3. 큰 렌더링 업데이트 피하기

DOM의 큰 부분을 한번에 변경하면 브라우저가 많은 계산을 해야 하므로 브라우저가 느려지고, FID 점수에 영향을 줌. DOM의 크기를 작게 유지하고 가능한 한 변화를 최소화함.

그리고 정말 중요한 렌더링 작업헤만 사용해야하는 requestAnimationFrame 같은 API를 남발하지 않아야 함. 왜냐하면 이 API를 통해 예약된 작업이 많으면 렌더링 자체가 느려지기 때문임

**참고**

- https://developers.google.com/search/docs/appearance/core-web-vitals?hl=ko
- https://developers.google.com/search/blog/2023/05/introducing-inp?hl=ko
- https://support.google.com/webmasters/answer/9205520?hl=ko
- https://seo.tbwakorea.com/blog/core-web-vital/
- https://yceffort.kr/2021/08/core-web-vital
- https://hjk329.github.io/%EC%B5%9C%EC%A0%81%ED%99%94/core-web-vitals/
- https://www.youtube.com/watch?v=mdB-J6BRReo // 개선 방법
