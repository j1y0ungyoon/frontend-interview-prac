# JSX란 무엇인가요?

JSX(JavaScript XML)란 자바스크립트를 확장한 문법으로, 자바스크립트 파일 안에 HTML과 유사한 마크업을 작성할 수 있도록 해줌. JSX를 사용하면 자바스크립트 코드 내에서도 HTML 코드처럼 바로 UI를 표현할 수 있기 때문에 쉽고 직관적으로 UI를 만들고 렌더링하는 데 도움이 됨. 그래서 리액트에서 UI를 표현할 때 JSX를 사용함.

```
function App () {
    return (
        <div>
        <h1>hello, it's JSX</h1>
        </div>
    )
}
```

하지만 웹 브라우저는 JSX코드를 그대로 이해하지 못함. 따라서 JSX 코드는 웹 브라우저에서 실행되기 전 코드가 번들링되는 과정에서 바벨과 같은 도구를 사용해 일반 자바스크립트로 변환되어야함

```
// 바벨을 사용해 jsx -> 자바스크립트(ES5)로 변환

"use strict";

function App() {
  return React.createElement("div", null, React.createElement("h1", null, "hello, it's JSX"));
}
```

만약 JSX를 사용하지 않는다면 매번 React.createElement 함수를 사용해 개발하면 엄청 불편하고 가독성이 떨어지는 코드를 작성하게 됨. 따라서 JSX를 사용하면 위에서 말했다시피 편하게 UI를 렌더링할 수 있음

## JSX의 장점

- 가독성이 좋고 익숙함
  자바스크립트로 요소를 하나하나 만들어야 한다면 작성하기 불편하고, 코드가 길어지므로 가독성이 떨어짐. HTML 코드를 작성하는 것과 비슷한 JSX를 사용한다면 읽기에도 편하고 코드를 작성하기에도 훨씬 더 편함

- 높은 활용성
  JSX에서 div나 span 같은 HTML 태그를 사용할 수 있을 뿐만 아니라 컴포넌트도 JSX로 작성할 수 있음

```
// src/index.js

import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import * as serviceWorker from './serviceWorker';

ReactDOM.render(<App />, document.getElementById('root'));
```

위의 예시에서 APP 컴포넌트를 HTML 태그로 작성하듯 작성함.

## JSX 문법

1. 단일 루트 요소 리턴하기
   컴포넌트에서 여러 엘리먼트를 리턴하려면 하나의 부모태그로 감싸야 함
   div나 fragment를 사용해 감싸주자

```
<>
  <h1>Hedy Lamarr's Todos</h1>
  <img
    src="https://i.imgur.com/yXOvdOSs.jpg"
    alt="Hedy Lamarr"
    class="photo"
  >
  <ul>
    ...
  </ul>
</>
```

JSX는 HTML처럼 보이지만 내부적으로는 자바스크립트 객체로 변환됨. 하나의 배열로 감싸지 않은 하나의 함수에서는 두 개의 객체를 반환할 수 없음. 따라서 또 다른 태그나 fragment로 감싸지 않으면 두 개의 JSX 태그를 반환할 수 없음

2. 모든 태그 닫기
   JSX 에서는 태그를 명시적으로 닫아야 함. img 같이 자체적으로 닫는 태그도 반드시 <img/>로 작성해야하고, li 같은 래핑 태그 또한 <li>이렇게</li>로 작성해야함

```
<>
  <img
    src="https://i.imgur.com/yXOvdOSs.jpg"
    alt="Hedy Lamarr"
    class="photo"
   />
  <ul>
    <li>Invent new traffic lights</li>
    <li>Rehearse a movie scene</li>
    <li>Improve the spectrum technology</li>
  </ul>
</>
```

3. 자바스크립트 값은 중괄호로 감싸기
   JSX 내부에 자바스크립트 변수를 보여줘야할 때는 중괄호({})로 감싸야 함

```
const name = "okok";

return {
    <>
        <div>Hello, My name in {name}</div>
    </>
}
```

4. 속성명은 대부분 카멜 케이스

JSX에서 컴포넌트에 전달되는 속성을 설정할 때 변수(ex: className)를 사용함.

```
<div className="my-div" dataValue="123">Hello, World!</div>

```

위 코드에서 className와 dataValue는 속성 이름 나타내는 변수이고 이는 카멜케이스로 작성되어있음.

이렇게 변수를 카멜 케이스로 작성하여 JSX가 자바스크립트로 변환될 때 변수 이름에 예약어(ex: class)나 특수 문자(ex: 하이픈(-))가 포함되지 않도록 함. 이렇게 하면 코드의 가독성도 향상되고, 자바스크립트 변수 네이밍 규칙과도 호환성을 유지할 수 있음

**참고**

- https://react-ko.dev/learn/writing-markup-with-jsx
- http://tcpschool.com/react/react_jsx_intro
- https://thebook.io/080203/0041/
