# Lazy loading과 Code splitting에 대해 아는 만큼 설명해주실 수 있을까요?

spa에서 최초 페이지 로드 시 번들링 되어있는 자바스크립트 파일을 받아옴. 이때 웹 페이지가 크다면 그만큼 받아오는 번들링된 자바스크립트 코드도 큼. 따라서 최초 페이지 로드 시 시간이 증가하게 됨.
이를 해결하기 위해 나온 것이 lazy loading과 code splitting임

## lazy loading

코드나 리소스가 필요한 경우에만 로드되도록 하는 기법임.
주로 이미지나 조건부로 렌더링되는 컴포넌트, 긴 스크롤의 페이지 콘텐츠등과 같이 큰 리소스에서 사용됨
react에서는 React.lazy() 기능을 사용해 컴포넌트를 비동기적으로 로드하는 등으로 사용할 수 있음

// 예시

```
import React, { lazy, Suspense } from 'react';

// React.lazy()를 사용하여 MyComponent를 비동기적으로 로드
const MyComponent = lazy(() => import('./MyComponent'));

const App = () => {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <MyComponent />
      </Suspense>
    </div>
  );
}

export default App;
```

React.lazy()를 사용해 비동기적으로 컴포넌트 로드함
MyComponent가 필요한 경우에만 로드 됨 -> 초기 페이지 속도 빨라짐

## code splitting

자바스크립트 번들을 작게 나누는 기법임.
이렇게 나누어 초기 페이지 로드에 필요한 코드량을 줄여서 페이지 로딩 성능을 향상 시킬 수 있음
주로 라우팅이나 조건부 렌더링 같이 특정 조건에서 실행되는 코드에 사용됨
react에서는 import()문법을 사용해 코드를 분할하고 이렇게 분할된 코드는 웹팩이 자동으로 번들로 만든 후 필요한 경우에만 로드됨

// 예시

```
import React, { lazy, Suspense } from 'react';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';

const Home = lazy(() => import('./Home'));
const About = lazy(() => import('./About'));

const App = () => {
  return (
    <Router>
      <Suspense fallback={<div>Loading...</div>}>
        <Switch>
          <Route exact path="/" component={Home} />
          <Route path="/about" component={About} />
        </Switch>
      </Suspense>
    </Router>
  );
}

export default App;
```

웹팩과 import를 사용해 라우팅에 따라 페이지 컴포넌트 분할함
사용자가 다른 페이지로 이동할 때만 해당 페이지의 코드가 로드되어 페이지 로딩 성능이 향상됨
