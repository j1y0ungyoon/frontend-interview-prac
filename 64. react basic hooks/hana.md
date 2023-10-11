# useEffect, useState, useRouter, useCallback, useMemo 의 등장배경 / 목적 / 구체적인 사용법 / 주의사항에 대해 작성해주세요.(1000자)

## useState

### 등장배경

기존 클래스형 컴포넌트에서만 state를 정의하고, setState 함수를 통해 state를 관리할 수 있었음.
그러나 함수형 컴포넌트에서는 state를 정의하거나 리액트 라이프 사이클에 따라 기능 구현이 어려웠음. 왜냐하면 함수형 컴포넌트는 리렌더링이 발생하면 함수 내에 작성된 모든 코드들이 다시 실행되기 때문임. 때문에 state를 기억(관리)할 수 없어서 함수형 컴포넌트에서는 state를 사용할 수 없었음

하지만 클래스형 컴포넌트는 함수형 컴포넌트에 비해 코드가 복잡하고, 재사용성도 떨어지며, state 관리 및 라이프 사이클을 위해 추가적인 학습이 필요하다는 단점이 있었음.
또한 this가 가리키는 값이 변경되면 예상치 못한 문제를 발생시킬 수 있었음

ex: a를 팔로우 하려고 했으나 팔로우 되기 이전 유저인 b로 바꾸면서 컴포넌트가 리렌더링되고 this가 가리키는 값이 b로 변경되어 b가 팔로우됨

이러한 이유들로 인해 리액트 16.8 버전부터 추가된 Hook에서 함수형 컴포넌트에서 부족했던 기능인 state와 라이프 사이클 기능을 가능하게 해줌

그런데 방금 전 함수형 컴포넌트는 리렌더링이 발생하면 함수내에 작성된 코드들이 다시 실행되어 state를 기억할 수 없다고 했는데 어떻게 가능하게 했을까?

바로 클로저를 통해 함수형 컴포넌트 바깥에 state를 저장하기 때문에, state가 업데이트 되어도 컴포넌트 바깥에 선언된 변수이기 때문에 업데이트 한 이후에도 이 변수들에 접근할 수 있음

이를 체크해보기 위해 node_modules/react/umd/react.development.js를 살펴보았음

```
 function useState(initialState) {
    var dispatcher = resolveDispatcher();
    return dispatcher.useState(initialState);
  }
```

useState는 이니셜 스테이스틀 받아서 resolveDispatcher의 함수의 리턴값인 dispatcher의 useState에 이니셜 스테이트를 전달한 결과를 리턴함

```
function resolveDispatcher() {
    var dispatcher = ReactCurrentDispatcher.current;

    {
      if (dispatcher === null) {
        error('Invalid hook call. Hooks can only be called inside of the body of a function component. This could happen for' + ' one of the following reasons:\n' + '1. You might have mismatching versions of React and the renderer (such as React DOM)\n' + '2. You might be breaking the Rules of Hooks\n' + '3. You might have more than one copy of React in the same app\n' + 'See https://reactjs.org/link/invalid-hook-call for tips about how to debug and fix this problem.');
      }
    } // Will result in a null access error if accessed outside render phase. We
    // intentionally don't throw our own error because this is in a hot path.
    // Also helps ensure this is inlined.

    return dispatcher;
  }
```

resolveDispatcher는 ReactCurrentDispatcher의 안에 있는 current의 값을 리턴하고 있음

```
  var ReactCurrentDispatcher = {
   current: null
  };

```

전역으로 선언된 ReactCurrentDispatcher는 current라는 값을 가지고 있는 변수임

### 목적

함수형 컴포넌트 내에서 state를 사용 및 관리할 수 있게 만든 훅임

### 구체적인 사용법

1. 최상단에 useState 선언하기

```
const App = () => {

// 함수형 컴포넌트 내부의 최상단에 useState 선언하기
const [drink, setDrink] = useState(0)

return (
    <>
    <section>
    <div>오늘 마신 음료수는 총 {drink} 컵 입니다</div>
    <button>PLUS</button>
    <button>MINUS</button>
    </section>
    </>
)
}
```

`const [state, setState] = useState(initial state)`

함수형 컴포넌트 안 최상단에 선언함.
useState 함수에 초깃값을 넣어 호출하면 두 가지 값이 리턴되는데 위의 코드를 예로 들자면 다음과 같은 값이 리턴됨

```
(2) [0, ƒ]
0 : 0
1 : ƒ ()
length : 1
name : "bound dispatchSetState"
arguments : (...)
caller: (...)
[[Prototype]]: ƒ ()
[[TargetFunction]]: ƒ dispatchSetState(fiber, queue, action)
[[BoundThis]]: null
[[BoundArgs]]:
Array(2)
length: 2
[[Prototype]]: Array(0)
```

이렇게 0(위 코드에서 초깃값으로 넣었던 값)와 함수가 리턴되었고 이를 디스트럭처링을 사용해 각각 state와 setState라는 변수와 함수로 할당됨.
현재 상태 값은 state에 저장되고 setState 함수를 호출해 state를 변경시킬 수 있음.
그리고 useState 옆에는 이니셜 스테이트를 적어주면 되는데, 이니셜 스테이트(초깃값)은 여러가지 형태가 될 수 있음(문자열, 객체, 배열, 빈 문자열, 불린, 넘버 등등)

2. setState로 state 관리

```
const App = () => {

const [drink, setDrink] = useState(0)

const addDrink = () => {
    setDrink((prev) => prev + 1)
}

const minusDrink = () => {
    setDrink((prev) => prev - 1 )
}


return (
    <>
    <section>
    <div>오늘 마신 음료수는 총 {drink} 컵 입니다</div>
    <button onClick={addDrink}>PLUS</button>
    <button onClick={minusDrink}>MINUS</button>
    </section>
    </>
)
}
```

setState를 통해 state를 변경 및 관리하고, state가 바뀌면 리액트가 자동으로 버추얼돔과 리얼돔을 비교해 바뀐 부분만 리렌더링을 함

### 주의사항

- state의 값을 직접 변경 X
  불변성이 지켜지지 않음

- setState 함수로 state 업데이트 시, 업데이트가 즉시 반영되지 않을 수 있음
  setState 함수는 비동기로 작동하므로, 즉시 업데이트가 되지 않을 수 있음

## useEffect

### 등장배경

클래스형 컴포넌트에서 사용되던 라이프 사이클 메소드를 함수형 컴포넌트에서 사용할 수 있게 도입됨
기존의 클래스형 컴포넌트에서는 아래 코드와 같이 라이프 사이클 메소드(componentDidMount, componentDidUpdate, componentWillUnmount)를 사용해 사이드 이펙트를 처리함

#### 사이드 이펙트란?

리액트 컴포넌트는 함수로 정의되며, 대부분의 리액트 함수는 순수함수처럼 동작함.  
즉, 입력(props)을 받으면 예측 가능한 jsx를 return하는 것을 의미함

순수함수는 주어진 입력에 대해 항상 동일한 결과를 리턴하고 외부 상태나 변수에 영향을 주지 않음. 하지만 사이드 이펙트를 가진 함수는 무엇인가를 하기 위해 리액트 컴포넌트 외부와 함께 수행되기 때문에 순수함수와는 달리 외부 상태를 변경하거나, 외부와 상호작용해 수행한 결과가 어떤지 예측할 수 없음

사이드 이펙트는 다음과 같은 것들이 있음

- 백엔드 서버로 api 데이터 요펑
- 브라우저 api와 상호작용(document, window 직접 사용 등 DOM 조작에 관련됨)
- setTimeout, setInterval 등 타이머 관련 함수

```
import React from "react";

class SectionOne extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0,
    };
  }
  componentDidMount() {
    console.log("컴포넌트 화면에 나타났지롱");
  }
  componentDidUpdate() {
    console.log("컴포넌트 업데이트 발생");
  }
  componentWillUnmount() {
    console.log("컴포넌트 사라짐");
  }
  render() {
    return (
      <div>
        <p>클릭한 횟수{this.state.count}</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          click
        </button>
      </div>
    );
  }
}

```

처음 컴포넌트가 렌더링되고 DOM이 업데이트 되면 componentDidMount가 실행되고 console.log("컴포넌트 화면에 나타났지롱");가 실행됨
그 다음 버튼을 눌러 count state가 변경되면 컴포넌트가 리렌더링되고, componentDidUpdate가 실행됨. 그리고 컴포넌트가 언마운트되어 DOM에서 제거될 때 componentWillUnmount가 실행됨

여기서 만약 컴포넌트의 본문 안에 http 통신 작업이 존재한다면 리렌더링될 때마다 작업을 다시 요청하게 될 것임. api로 요청한 데이터 크기가 클 경우 브라우저의 페인팅 작업이 늦춰져 리소스를 비효율적임

이러한 사이드 이펙트들은 화면을 렌더링하는 데 직접적으로 영향을 주지 않아야 되기 때문에, 렌더링 작업과 사이드이펙트를 구분하여 처리하는 것은 버그를 예방하는 데 도움을 주고, 좋은 사용자 경험을 제공할 수 있음.

클래스형 컴포넌트의 경우 마운트될 때, 업데이트될 때, 언마운트될 때를 각각 라이프 사이클 메소드 안에 따로 적어주어야 했다면, useEffect는 컴포넌트가 이 세 가지 분기에 관한 코드들을 한번에 작성하고 관리할 수 있어 편하고 유지보수에 좋음. 또한 useEffect는 화면에 렌더링된 후 비동기 작업을 처리하기 때문에 컴포넌트 렌더링과 사이드 이펙트를 구분해 처리해줄 수 있는 리액트 훅임

### 목적

기존 클래스형 컴포넌트의 라이프 사이클 메소드를 하나로 통합해 렌더링과 사이드 이펙트(부수효과)를 구분해 처리하는 훅

### 구체적인 사용법

```
useEffect(() => {

// 작업할 내용(콜백 함수)
// componentDidMount

return () => {
    // cleanUp 함수
    // componentWillUnmount
    // 컴포넌트가 언마운트된 후 작동되는 정리함수
}
}, [dependency list])
```

컴포넌트가 렌더링된 후 useEffect 안의 콜백함수에 있는 내용이 실행되고,
디펜던시에 있는 요소가 변경되었을 때 리렌더링되고 콜백안에 있는 내용이 실행됨

즉, 처음 화면이 렌더링된 후에(componentDidMount) 콜백함수가 실행되고 그 이후 디펜던시에 있는 요소가 변경되어 리렌더링되면(componentDidUpdate) 콜백함수가 실행됨

클린업 함수의 경우 클린업 함수를 사용하지 않아도 전체적인 코드의 동작은 가능하지만, 정리를 하지 않는 경우 메모리 누수가 발생해 불필요하게 메모리가 낭비됨. 따라서 메모리 누수 방지를 위해서도 정리 과정은 필요함

### 주의사항

- 무한 루프

```
const App = () => {
    const [count, setCount] = useState(0)

    useEffect(() => {
        setCount(count + 1 )
    })
}

return (
    <div>{count}</div>
)
```

useEffect를 디펜던시 없이 사용하면 매번 해당 컴포넌트가 렌더링될 때마다 setCount가 작동하는데, setState는 state이기 때문에 해당 컴포넌트를 리렌더링 함. 그러면 또 useEffect가 실행되고 이게 계속 반복되어 무한 루프에 걸리게 됨

## useRouter

### 등장배경

초창기 next.js에서는 withRouter라는 고차 컴포넌트를 사용해 클래스 컴포넌트에서 this.props.router를 통해 라우트 정보에 접근할 수 있음. 이를 통해 클래스 기반 컴포넌트에서 라우트 정보를 다룰 수 있음.
그런데 리액트에서 함수형 컴포넌트가 나오고, 16.8버전에서 hook이 등장하면서 함수형 컴포넌트에서도 state 관리와 라이프 사이클 메소드 등을 사용할 수 있게 되고, 이에 따라 next.js에서도 함수형 컴포넌트에서 사용할 수 있는 훅인 useRouter를 도입하게 됨

### 목적

Next.js 프레임워크에서 제공하는 Hook
페이지 간 네비게이션과 URL 매개 변수 및 쿼리 스트링에 접근하는 등의 기능을 제공하기 위해 등장함

### 구체적인 사용법

next.js 13버전이 나오면서 App Router(client component)와 pages Router(server component)로 나뉨

#### App Router에서의 useRouter

```
'use client'

import { useRouter } from 'next/navigation'

export default function Page() {
  const router = useRouter()

  return (
    <button type="button" onClick={() => router.push('/dashboard')}>
      Dashboard
    </button>
  )
}

```

클라이언트 컴포넌트에서는 `use client`를 상단에 작성 후 사용해줌
버전 13으로 넘어오면서 app router에 있는 useRouter의 옵션 중 path와 query가 삭제되고 usePathname과 useSearchParams 훅으로 변경됨

#### Pages Router에서의 useRouter

기존의 useRouter처럼 사용하면 됨

```
import { useRouter } from 'next/router'

export default function ReadMore({ post }) {
  const router = useRouter()
  const pid = router.post.id

  return (
    <button
      type="button"
      onClick={() => {
        router.push({
          pathname: '/post/[pid]',
          query: { pid: pid },
        })
      }}
    >
      더보기
    </button>
  )
}
```

### 주의사항

app router와 page router에 있는 useRouter는 이름이 동일하니 import 해오는 루트를 잘 봐야함

- app router의 import 경로 : Next/navigation
- pages router의 import 경로 : Next/router

## useMemo

### 등장배경

useMemo, useCallback 같은 훅들은 최적화를 위해서 사용함.

**최적화란?**
불피요한 렌더링을 줄여 비용 발생을 줄이는 것을 말함

리액트에서는 아래와 같은 경우에 렌더링이 발생함

- 컴포넌트의 state가 바뀌었을 때
- 상위 컴포넌트에서 받은 props가 변경되었을 때
- 부모 컴포넌트가 리렌더링된 경우 자식 컴포넌트는 모두 리렌더링됨

만약 아래와 같이 무거운 연산을 수행하는 함수가 있다고 가정해보자

```
const App = () => {
    const [count, setCount] = useState(0)

    const addExampleFunction = () => {
    let num = 0
    for(let i = 0; i < 1000000000000; i++) {
        num += i

    }
    return num
    }

    let calNum = addExampleFunction()


    const addNum = () => {
        setCount((prev) =>  prev + 1)
    }
return (
    <>
    <div>{calNum}</div>
    <div>{count}</div>
    <button onClick={addNum}>click</button>
    </>
)
}
```

위의 예시에서 버튼을 눌러 state가 변경되면서 리렌더링이 일어나고, 변경될때마다 addExampleFunction가 실행되어 엄청 느리게 동작함.
어차피 addExampleFunction의 리턴값은 똑같기 때문에 새로운 연산을 할 필요가 없고, 재사용을 하면 효율적임

```
let calNum = useMemo(() => {
    return addExampleFunction()
}, [])

```

useMemo를 통해 함수 addExampleFunction()가 리턴한 값을 저장해 재사용하면 리렌더링될 때마다 계산을 다시 않아도 되서 화면이 느려지지 않음.
위의 경우처럼 useMemo는 렌더링이 발생했을 때 이전 렌더링과 현재 렌더링 사이에 값이 동일하다면 다시 함수를 호출시켜 결과를 구하는 대신 기존 메모리에 저장해두었던 값을 재사용하도록 도와줌.

### 목적

함수형 컴포넌트에서 동일한 값을 리턴하는 함수를 계속 호출해야 한다면 필요없는 렌더링을 한다고 볼 수 있음. 그래서 맨 처음 해당 값을 리턴할 때 그 값을 메모리에 저장함. 이렇게 하면 필요할 때마다 함수를 다시 호출해서 계산하는 것이 아니라 이미 저장한 값(함수가 리턴하는 값)을 꺼내와서 사용할 수 있게 도와줌

### 구체적인 사용법

```
const result = useMemo(() => calc(a,b), [])
const result = useMemo(() => calc(a,b), [count])
```

useMemo는 콜백함수와 디펜던시 어레이를 받음. 콜백함수의 결과값은 useMemo의 리턴값으로 재사용하는 결과값이 됨.
디펜던시 어레이에 있는 요소가 변경되었을 때만 콜백함수 실행됨 -> 이 때 렌더링된 과정에서 디펜던시 안의 요소가 변경되었는지를 확인하고, 값이 변경된 경우에만 콜백 함수를 동작시켜 다시 계산한 결과를 리턴하고 메모이제이션함.

즉, 디펜던시에 있는 요소가 변경되기 전까지의 계산 결과를 캐시함 -> 디펜던시에 전달하는 값의 변경 여부에 따라 중복 연산을 최소화할 수 있으므로 컴포넌트 성능을 최적화할 수 있음.

### 주의사항

무분별하게 너무 많이 사용하면 불필요한 메모리 공간을 차지하게 됨 -> 성능이 낮아짐 -> 적절하게 사용하자

## useCallback

### 등장배경

useMemo처럼 컴포넌트 성능을 최적화하기 위해 사용되는 훅임
컴포넌트가 리렌더링될 떄마다 컴포넌트 안에 정의된 콜백 함수들이 재생성되는 것을 방지하기 위해 만들어짐
useCallback은 함수 자체를 메모이제이션함

### 목적

useCallback은 불필요한 리렌더링을 방지하기 위해 특정 함수를 새로 만들지 않고 재사용하고 싶을 때 사용함

### 구체적인 사용법

```
const memoized = useCallback(function, [deps])
```

useMemo와 사용법의 똑같음. 다만 차이는 useMemo는 특정값을 메모이제이션해서 재사용하는 것이고, useCallback은 함수를 메모이제이션해서 재사용하는 것임

```
// App.js

// count를 초기화해주는 함수
  const initCount = () => {
    setCount(0);
  };

  return (
    <>
      <h3>카운트 예제입니다!</h3>
      <p>현재 카운트 : {count}</p>
      <button onClick={onPlusButtonClickHandler}>+</button>
      <button onClick={onMinusButtonClickHandler}>-</button>
      <div style={boxesStyle}>
        <Box1 initCount={initCount} />
        <Box2 />
        <Box3 />
      </div>
    </>
  );

// Box1.js

function Box1({ initCount }) {
  console.log("Box1이 렌더링되었습니다.");

  const onInitButtonClickHandler = () => {
    initCount();
  };

  return (
    <div style={boxStyle}>
      <button onClick={onInitButtonClickHandler}>초기화</button>
    </div>
  );
}

export default React.memo(Box1)

```

예시에서 +,-버튼과 박스1에 있는 초기화 버튼을 누를 때 모두 App컴포넌트와 Box1 컴포넌트가 리렌더링됨. 그런데 Box1에 React.memo(컴포넌트를 메모이제이션)를 했는데 왜 리렌더링이 된걸까? -> App.js에서 만든 initCount 함수를 부모 컴포넌트인 App에서 생성해 자식 컴포넌트인 Box1에 props로 전달하고 있기 때문임. React.memo는 컴포넌트의 props가 변경되지 않았을 때만 컴포넌트의 리렌더링을 방지함. 하지만 initCount 함수는 App 컴포넌트에서 생성된 함수이고 App 컴포넌트가 리렌더링될 때마다 새로운 initCount 함수가 생성(주소값이 달라짐)되므로 하위 컴포넌트인 Box1에서 props가 변경되었다고 간주함

따라서 Box1의 리렌더링을 방지하려면 initCount 함수를 App 컴포넌트 내에서 다시 생성하지 않고 한번 생성된 함수를 재사용해야함. 이때 useCallback 함수를 사용할 수 있음

```
// App.js

// count를 초기화해주는 함수
  const initCount = useCallback(() => {
    setCount(0);
  }, [])

  return (
    <>
      <h3>카운트 예제입니다!</h3>
      <p>현재 카운트 : {count}</p>
      <button onClick={onPlusButtonClickHandler}>+</button>
      <button onClick={onMinusButtonClickHandler}>-</button>
      <div style={boxesStyle}>
        <Box1 initCount={initCount} />
        <Box2 />
        <Box3 />
      </div>
    </>
  );


```

initCount 함수에 useCallback을 사용하면 함수 자체를 메모리에 메모이제이션하고 필요할 때마다 가져다 쓰기 때문에 initCount 함수가 새로 생성되지 않고, 재사용할 수 있음. 또한 하위 컴포넌트에 React.memo를 사용해 리렌더링을 방지할 수 있음

initCount에 아래와 같이 콘솔을 추가함

```
// count를 초기화해주는 함수
const initCount = useCallback(() => {
  console.log(`[COUNT 변경] ${count}에서 0으로 변경되었습니다.`);
  setCount(0);
}, []);
```

만약 카운트를 10고 초기화 버튼을 누르면 콘솔에는 10에서 0으로라는 문자열이 찍혀야함 하지만 콘솔을 찍어보면 0에서 0으로 변경되었다고 나옴. 왜냐하면 useCallback의 count가 0일때의 시점을 기준으로 메모리에 함수를 저장했기 때문임. 따라서 initCount 함수의 디펜던시 어레이에 count를 넣으면 count가 변경될 때마다 새롭게 함수를 할당함

```
// count를 초기화해주는 함수
const initCount = useCallback(() => {
  console.log(`[COUNT 변경] ${count}에서 0으로 변경되었습니다.`);
  setCount(0);
}, [count]);
```

이렇게 하면 count가 변경될 때마다 새롭게 함수를 할당하고 콘솔에는 현재값에서 0으로 변경되었다는 문구가 잘 뜸

### 주의사항

useMemo와 마찬가지로 마구잡이로 사용하면 메모리 공간을 너무 많이 확보하게 되서 성능이 저하됨 -> 적절하게 사용해 최적화하는 데만 사용하자!
