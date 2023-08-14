# Server Side Rendering, Client Side Rendering, Static Site Generation 의 장단점을 설명해주실 수 있을까요?

ssr, csr, ssg isp 모두 다 웹페이지가 렌더링하는 방식을 일컫음
크게 ssr(클라이언트 사이드 렌더링)과 scr(서버 사이드 렌러딩)으로 나뉨. 이외에도 ssg와 isp라는 방법도 있음
이걸 얘기하기 전에 SPA와 MPA에 알아야 함

## SPA(single page application)

- 한 개의 페이지로 구성된 어플리케이션을 의미함
- 웹 어플리케이션에 필요한 모든 정적 리소스(HTML, CSS, Javascript 등)을 최초 렌더링 시 한 번에 다운로드 함. 그 이후에 새로운 요청이 있을 때 서버에 필요한 데이터만 요청해 받아서 페이지를 갱신함
- 결국 SPA를 CSR 방식으로 렌더링한다고 할 수 있음(하지만 SPA가 전부 CSR 방식은 아님)

```
<html>
<head>
  <title>Single Page Application</title>
  <link rel="stylesheet" href="app.css" type="text/css">
</head>
<body>
  <div id="app"></div>
  <script src="app.js"></script>
</body>
</html>
```

### 장점

- 최초 렌더링 시를 제외하고 이후 전체 페이지를 렌더링할 필요가 없기 때문에 깜빡 거림 없이 자연스러운 사용자 경험을 할 수 있음
- 필요한 부분만 부분적으로 로딩
- 서버의 템플릿 연산을 클라이언트로 분산 가능
- 컴포넌트별 개발 용이

### 단점

- 초기 렌더링 속도 느림
  자바스크립트 파일을 번들링해서 한 번에 받기 때문에 느린데, 이는 code splitting으로 해결 가능함

- SEO에 불리함 왜냐면 검색엔진이 색인을 할 만한 컨텐츠가 아래와 같이 HTML 문서에 없기 때문암
- 보안 이슈
  클라이언트측의 쿠키 말고는 사용자에 대한 정보를 저장할 공간이 마땅치 않음

## MPA(multiple page application)

- 여러 개의 페이지로 이루어진 어플리케이션을 의미함
- MPA는 SSR 방식으로 렌더링함
  새로운 페이지를 요청할 때마다 미리 서버에서 렌더링된 정적 리소스를 서버에서 보냄

### 장점

- SEO에 유리함
  이미 만들어진 HTML 파일을 서버로부터 전송받기 때문임. 검색엔진이 페이지를 검색하기 적합함

- 초기 구동 속도가 빠르다
  서버에서 이미 렌더링해서 가져오기 때문임. 하지만 클라이언트가 자바스크립트 파일을 다운로드하고 적용하기 전까지 클릭 웹페이지와의 인터렉션은 되지 않음

### 단점

- 새로운 페이지를 요청할 때마다 전체 페이지를 다시 렌더링함(리로딩) 따라서 새로운 페이지로 이동하면 깜빡임이 발생하고 자연스러운 사용자 경험은 어려움
- 페이지 이동 시 불필요한 템플릿도 중복해서 로딩
- 서버 렌더링에 따른 부하

## CSR(Client Side Rendering)

<img src="https://www.youdad.kr/wp-resource/resources/articles/2022/08/frontend_rendering_csr.jpg" width="50%" height="50%">

React, Vue, Angular 등이 CSR을 사용함

- 클라이언트(브라우저)에서 웹페이지 렌더링하는 것을 말함
- 모든 로직, 데이터 가져오기, 템플릿, 라우팅은 서버가 아니라 클라이언트에서 처리됨
- 자바스크립트 번들의 크기의 영향을 많이 받기 때문에 코드분할 (code splitting)을 고려해야함
- 필요한 것만 필요할 때에 제공해야 함

### 동작 방식

1. 클라이언트가 사이트 접속 -> 특정 페이지를 요청함
2. 서버에서는 빈 HTML 문서(index.html)를 클라이언트에 보냄
   (이 때 아무 내용이 없기 때문에 클라이언트 측에서는 초기에 빈 화면만 보일 것임)
3. index.html에 링크된 자바스크립트 파일을 요청한 후 다운로드 받고 실행함.
4. 받아온 것들을 합쳐 동적 HTML을 생성해서 사용자들이 화면을 볼 수 있고 인터렉션도 가능함. TTV(time to view)와 TTI(time to interact)가 동시에 일어남

### 장점

- 후속 페이지 로드 시간이 더 빠름
  -> 이미 모든 파일을 사전에 로드되었기 때문에 csr의 로드시간이 줄어듦

- 페이지 이동에 걸리는 시간이 짧음
  페이지 최초 접근 시에 페이지 렌더링에 필요한 정적 리소스와 자바스크립트 파일을 모두 다운로드하기 때문에 페이지를 이동할 시 필요한 부분만 추가로 서버에 요청하면 되므로 페이지 이동에 걸리는 시간이 짧음

### 단점

- 초기 페이지 로딩 시간이 ssr에 비해 느림
  브라우저에서 렌더링에 필요한 리소스와 번들 자바스크립트 파일을 다운로드 한 후 화면을 그리는 데 걸리는 시간이 길기 때문에 오래 걸리게 됨
  특히 번들 자바스크립트 파일의 용량이 클 수록 다운로드에 많은 시간이 소요되므로 정적 리소스 및 용량이 적은 라이브러리를 사용하고 code splitting을 사용해 번들 파일 용량을 줄이는 것이 좋음

- 데이터를 받아올 떄까지 빈 화면(혹은 로딩) 표출
  CSR 방식은 번들 자바스크립트 실행이 완료된 후 API응답을 받아오기 전까지는 빈 화면(혹은 별도로 설정한 로딩화면 등)이 표출됨
  API에서 데이터를 받아온 후 화면 리렌더링이 완료되면 화면에서 인터렉션이 가능함

- SEO 비친화적
  -> 검색 엔진이 해당 페이지에 처음 방문했을 때 빈 페이지(index.html이 비어있음)이기 때문에 어떤 페이지인지 이해할 수 없기 떄문임

-> 사용자 인터렉션이 중요한 페이지라면 CSR방식이 적절함

## SSR(Server Side Rendering)

<img src="https://www.youdad.kr/wp-resource/resources/articles/2022/08/frontend_rendering_ssr.jpg" width="50%" height="50%">

Next,js, PHP, ASP등이 SSR을 사용함

### 동작 방식

1. 클라이언트가 사이트에 접속 -> 특정 페이지 요청
2. 서버에서는 해당 페이지에 필요한 데이터를 포함해서 이미 렌더링된 HTML 파일을 클라이언트에게 전달함
3. 클라이언트는 HTML파일을 받아옴. 이미 렌더링된 페이지이기 때문에 사용자는 페이지를 볼 수 있지만(TTV) 아직 클릭 등의 인터렉션은 할 수 없음(자바스크립트 코드를 받아오지 않아서)
4. 클라이언트에서 자바스크립트 파일을 요청하고 받아와 실행함
5. 페이지 인터렉션이 가능해짐(TTI)
   -> TTV와 TTI 사이의 공백이 긴 편임

### 장점

1. 최초 로딩 시간이 짧음
   요청한 페이지에 필요한 리소스만 다운로드해 페이지를 생성해 렌더링에 필요한 시간이 짧음

2. SEO에 유리함
   서버에서 페이지를 생성하므로 완성된 HTML 문서를 수집할 수 있기 때문에 검색엔진 최적화에 유리함

### 단점

1. 자바스크립트 실행이 완료될 때까지 인터렉션 불가능함
   SSR방식은 사용자가 페이지를 빨리 볼 수 있지만 자바스크립트 실행이 완료될 때 까지는 클릭 등의 인터렉션이 불가능함
   만약 인터렉션 가능 시점 이전에 버튼 클릭 등의 작업이 발생했다면 인터렉션 가능 시점까지 해당 동작의 수행은 보류됨

2. 깜빡임 이슈
   사용자가 페이지를 이동하거나 무언가를 클릭하면 전체 웹페이지를 다시 서버에서 받아오기 때문에 깜빡임 이슈가 발생함 -> 사용자 경험에 좋지 않음

3. 서버 과부하
   사용자가 많을 수록 인터렉션이 많이 일어나고 이에 필요한 데이터를 서버에 요청해 받아와서 HTML을 만들기 때문에 서버에 과부하가 올 수 있음

## SSG(Static Site Generation)

- 프로젝트 빌드 시에 정적인 페이지를 사전 렌더링하는 방식
- 미리 만들어 둔 페이지를 클라이언트에게 제공하므로 렌더링 속도가 매우 빠름
- SSR과 마찬가지로 완성된 페이지를 클라이언트에게 주므로 SEO에 유리함
- 블로그나 정보성 페이지 등 정적인 데이터를 보여주는 사이트에 적합함

Next에서 사용
리액트+개츠비와 함꼐 사용해서 SSG방식으로 만들 수 있음

## ISR(Incremental Static Regeneration)

- 이미 생성된 웹에 정적 페이지를 새롭게 추가하거나 업데이트 할 수 있는 기능
- ssg는 빌드 시에만 정적 페이지를 생성하지만 isr은 빌드할 때 뿐만 아니라 특정 시간마다 정적 페이지를 재생성할 수 있음

// 예시

```
export async function getStaticProps() {
    const response = await fetch('https://...)
    const jsonData = await response.json()

    return {
        props : {
            jsonData
        },
        revalidate: 100

    }

}

```

getStaticProps의 반환값으로 revalidate값을 주면 해당 시간(예시는 0.1초)마다 필요한 데이터를 업데이트해서 정적 페이지를 재생성함

---

**참조**

- https://hanamon.kr/spa-mpa-ssr-csr-%EC%9E%A5%EB%8B%A8%EC%A0%90-%EB%9C%BB%EC%A0%95%EB%A6%AC/
- https://youtu.be/iZ9csAfU5Os
- https://ajdkfl6445.gitbook.io/study/web/csr-vs-ssr-vs-ssg
- https://www.flavienbonvin.com/data-building-strategy-for-nextjs-app/

**Next.js에서 CSR, SSR, SSG, IRS 적용하기**

- https://www.philly.im/blog/grokking-data-fetching-in-nextjs
- https://theodorusclarence.com/blog/nextjs-fetch-usecase
