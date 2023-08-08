# Lazy loading과 Code splitting에 대해 아는 만큼 설명해주실 수 있을까요?

## Lazy Loading

레이지 로딩은 필요 시점가지 객체의 초기화를 연기시키기 위해 컴퓨터 프로그래밍에 흔이 사용되는 디자인 패턴의하나로 적절하게 사용될 경우 프로그램 운영 차원에서 효율적이다.

## code-splitting

코드 스플릿팅이란 불필요한 코드, 중복되는 코드 없이 적절한 사이즈의 코드가 적절한 타이밍에 적절한 로딩시간으로 되도록 하는 것이다.

### code-splitting 방법

1. 페이지별
2. 모듈 별
3. 페이지, 모듈 혼용

### React.lazy

코드 스플리팅에는 여러가지 기법이 있지만, React에서 간단하게 사용할수 있는 React.lazy()를 제공한다.

- 클라이언트 사이드 렌더링만 가능한 함수로, 서버사이드 렌더링에서 코드 스플리팅을 사용하고 싶다면 서드파티 라이브러리 'Loadable Components'를 사용해야한다.

```javascript
import React.{lazy,Suspense} from "react";

const DashBoard = lzy(() => import ("./Routes/DashBoard"));
const Exchanges = lazy(() => import("./Routes/ExchangeS");
```

```javascript
<Suspense fallback-{<div></div>}>
    <Exchanges/>
<Suspense/ >
```

- 코드 스플리팅을 적용한 페이지를 최초로 이동시 화면의 깜박임 등이 발생한다.
- suspense의 fallbakc으로 로딩 화면을 생성할 수 있다.
  **필수적으로 Lazy 된 컴포넌트는 Suspsnes 내에서 선언되어야한다.**

### code splitting을 하는 이유

SPA의 특징으로 맨 처음 로딩할 때 페이지의 전체 리소스를 다운받게 된다. 당장 보지 않아도 될 페이지의 리소스까지 기다리고 다운받고 나면 처음 페이지가 렌더링 된 뒤 사용자가 페이지를 볼 수 있게 된다.
code splliting과 lazy-loading을 하게 되면 초기에 로딩이 필요 없는 리소스 까지 다운받지 않아도 된다.

- ligthouse의 unuse javascript이다.
