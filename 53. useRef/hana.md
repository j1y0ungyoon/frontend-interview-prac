# useRef 에 대해 아는 만큼 설명해주실 수 있을까요?

자바스크립트에서는 DOM을 조작할 때 getElementById나 querySelector같은 함수를 사용해 DOM을 선택하고 조작함.

리액트에서는 virtual DOM을 사용해 real DOM과 비교해 바뀐 부분이 바뀐 부분이 있다면 바뀐 부분만 변경을 해줌. 하지만 가끔 리액트에서도 직접 DOM을 선택해야하는 경우가 있음

특정한 요소에 포커스를 줘야하거나 스크롤바 위치를 가져오거나 HTML5 비디오 관련 라이브러리를 사용하거나 그래프 관련 라이브러리를 사용할 때도 특정 DOM에 적용하기 때문에 DOM을 직접 선택해야하는 경우들이 생김

이럴 때 리액트에서는 useRef를 사용함
useRef는 {current.initialValue} 형태의 객체를 리턴함. ref.current 형태로 접근 가능함

즉, useRef는 컴포넌트에 일부 값을 저장해야지만 렌더링 로직에는 영향을 미치지 않는 경우에 사용하는 게 좋음(ex: 브라우저 API, 외부 라이브러리롸 작업 등)

## useRef를 사용하는 목적

useRef는 두 가지의 경우에 사용됨

1. 특정 DOM에 접근해 제어해야 할 경우
   앞에서 말했다싶이 직접 DOM 요소에 접근해야할 경우에 사용함.

2. 렌더링과 관계없이 값을 변경하고 싶은 경우
   useRef를 사용해 반환된 객체({current.initialValue})는 컴포넌트의 전 생애주기를 통해 유지됨. 즉, 컴포넌트가 계속 렌더링되어도 컴포넌트가 언마운트되기 전까지 값이 그대로 유지됨.
   따라서 컴포넌트 값의 변경은 관리해야하지만 리렌더링을 발생시킬 필요는 없을 때 useRef를 활용할 수 있음

예를 들면 특정 타이머 함수가 얼마나 많은 횟수 호출되었는지를 추적하고 싶다고 가정함. 이런 정보는 UI와 직접적으로 관련되어 있지 않으므로 정보가 업데이트될 때마다 컴포넌트를 리렌더링할 필요는 없음. 그러나 이 정보를 추적하고 관리하는 것은 중요할 수 있습니다.

이런 상황에서 useRef를 사용하면 .current 속성을 통해 타이머 함수가 호출된 횟수를 저장하고 업데이트할 수 있음. 그리고 이 값은 컴포넌트가 언마운트되기 전까지 유지됨

즉, "컴포넌트 값의 변경을 관리한다"라는 말은 useRef로 생성된 ref 객체의 .current 속성 값을 우리가 원하는 방식으로 업데이트하거나 사용할 수 있다는 의미임. 하지만 이런 변경사항들이 컴포넌트의 리렌더링을 발생시키지 않으므로, UI와 직접적인 연관이 없는 값을 저장하거나 관리하는 데 유용함

## useState와 useRef의 차이점

useState는 상태값을 관리하기 위해 사용되고, 상태가 업데이트될 때마다 컴포넌트가 리렌더링되며, UI에 반영이 됨
반면 useRef는 컴포넌트의 생애주기동안 계속 유지되며, 값이 변경되어도 컴포넌트가 리렌더링되지 않음.

| ref                                                               | state                                                                                 |
| ----------------------------------------------------------------- | ------------------------------------------------------------------------------------- | ---- |
| useRef(initialValue)는 { current : initialValue} 반환             | useState(initialValue)는 state의 변수와 state 설정자함수([state, setState])           | 반환 |
| 변경 시 리렌더링X                                                 | 변경 시 리렌더링O                                                                     |
| 렌더링 프로레스 외부에서 current 값을 수정하고 업데이트할 수 있음 | state setting 함수를 사용해 state 변수를 수정해 리렌더링 개디열에 추가해야함          |
| 렌더링 중에는 current 값을 읽거나 쓰면 X                          | 언제든지 state를 읽을 수 있음. 각 렌러딩에는 변경되지 않는 자체 state snapshot이 있음 |

```
import { useRef, useState } from 'react;
import "./styles.css";

export default function App() {
  const [stateCount, setStateCount] = useState(0);
  const refCount = useRef(0);
  return (
    <div className="App">
      <button
        onClick={() => {
          setStateCount((prev) => prev + 1);
        }}
> State
      </button>
      <button
        onClick={() => {
          refCount.current += 1;
}} >
        Ref
      </button>
      <br />
      <br />
      <div>useState Count: {stateCount}</div>
      <div>useRef Count: {refCount.current}</div>
</div> );
}
```

state버튼을 누르면 useState의 stateCount 값이 증가하고, 렌더링되어 변화된 값이 화면에 적용되는 걸 볼 수 있음(해당 상태가 변경될 때마다 컴포넌트가 리렌더링되어 변경된 상태값이 화면에 반영됨). 하지만 ref버튼을 누르면 변경된 값이 화면에 렌더링되고 있지 않음. useRef 내부적으로는 값이 변경되고 있지만 useRef는 current 값이 변경되어도 컴포넌트를 리렌더링하지 않음. 따라서 화면이 리렌더링되지 않으므로 반영이 되고 있지 않을 뿐임.
state 버튼을 다시 눌러 컴포넌트를 리렌더링을 시키면 useRef의 값도 올라감. state가 변경되었을 때, refCount.current의 값도 마찬가지로 화면에 렌더링됨.
useRef는 컴포넌트의 생애주기 동안(언마운트 되기 전까지) 변화하는 값을 가지고 있다가 다른 상태 변경(ex: useState, useReducer 등)으로 인해 컴포넌트가 리렌더링되면 그 시점에서의 최신refCount.current의 값이 화면에 렌더링됨.(useState와 다르게 current의 값이 변화할 때마다 화면 리렌더링x)

## useRef 사용예시

- 동영상 재생 및 일시 정지

```
import { useState, useRef } from 'react';

export default function VideoPlayer() {
  const [isPlaying, setIsPlaying] = useState(false);
  const videoRef = useRef(null)

  function handleClick() {
    const nextIsPlaying = !isPlaying;
    setIsPlaying(nextIsPlaying);

    if(nextIsPlaying) videoRef.current.play()
    else videoRef.current.pause()
  }


  return (
    <>
      <button onClick={handleClick}>
        {isPlaying ? 'Pause' : 'Play'}
      </button>
      <video width="250"
        ref={videoRef}
        onPlay={() => setIsPlaying(true)}
        onPause={() => setIsPlaying(false)}
        >
        <source
          src="https://interactive-examples.mdn.mozilla.net/media/cc0-videos/flower.mp4"
          type="video/mp4"
        />
      </video>
    </>
  )
}

```

동영상을 정지하거나 재생하려면 state로는 부족함
<video>에 대한 DOM요소에서 play()와 pause() 호출해 조작해주어야 함
따라서 ref 필요함

- 이미지 스크롤

```
import { useState, useRef } from 'react';
import { flushSync } from 'react-dom';

export default function CatFriends() {
  const indexRef = useRef(0)
  const [index, setIndex] = useState(0);
  return (
    <>
      <nav>
        <button onClick={() => {
      flushSync(() => {
          if (index < catList.length - 1) {
            setIndex(index + 1);
          } else {
            setIndex(0);
          }
      })
      indexRef.current.scrollIntoView({
  behavior: 'smooth',
  block: 'nearest',
  inline: 'center'
});
        }}

          >
          Next
        </button>
      </nav>
      <div>
        <ul>
          {catList.map((cat, i) => (
            <li key={cat.id}
              ref={index === i ? indexRef : null}
              >
              <img
                className={
                  index === i ?
                    'active' :
                    ''
                }
                src={cat.imageUrl}
                alt={'Cat #' + cat.id}
              />
            </li>
          ))}
        </ul>
      </div>
    </>
  );
}

const catList = [];
for (let i = 0; i < 10; i++) {
  catList.push({
    id: i,
    imageUrl: 'https://placekitten.com/250/200?image=' + i
  });
}


```

DOM 노드의 scrollView()를 사용해 DOM 요소를 조작함
flushSync()은 스크롤 전에 React가 DOM을 업데이트하도록 강제하는 데에 필요한 함수임. 사용하지 않으면 indexRef.current가 항상 이전에 선택된 항목을 가리키게 되어 원하는 대로 스크롤이 안 됨

- input필드에 focus

```
import { useRef } from 'react'
export default function Page() {
  const focusRef = useRef(null)

  const handleFocusClick = () => {
    focusRef.current.focus()
  }
  return (
    <>
      <nav>
        <button onClick={handleFocusClick}>Search</button>
      </nav>
      <input
        placeholder="Looking for something?"
        ref={focusRef}

      />
    </>
  );
}

```

search 버튼을 누르면 input에 focus가 감
input에 ref를 추가하고 DOM 노드에서 focus()를 호출함

- 별도의 컴포넌트로 input 필드에 focus 하기

```
import SearchButton from './SearchButton.js';
import SearchInput from './SearchInput.js';
import { useRef } from 'react'

export default function Page() {
  const focusRef = useRef(null);

  const handleFocusClick = () => {
    focusRef.current.focus()
  }
  return (
    <>
      <nav>
        <SearchButton onClick={handleFocusClick} />
      </nav>
      <SearchInput ref={focusRef} />
    </>
  );
}
// App.js


export default function SearchButton({onClick}) {
  return (
    <button onClick={onClick}>
      Search
    </button>
  );
}
// SearchButton.js


import { forwardRef } from 'react'

export default forwardRef(
  function SearchInput(props, ref) {
  return (
    <input
      placeholder="Looking for something?"
      ref={ref}
    />
  );
 }
)

// SearchInput.js

```

리액트의 props는 ref를 포함하지 않음. 따라서 일반적인 방법으로는 부모 컴포넌트에서 자식 컴포넌트로 ref를 전달할 수 없음. 하지만 부모 컴포넌트가 자식 컴포넌트의 DOM 요소에 접근해야할 경우가 가끔 생김. 이 때 forwardRef를 사용하면 부모컴포넌트에서 생성한 ref를 자식 컴포넌트에 직접 연결할 수 있음

위 예제에서 Page라는 부모 컴포넌트에서 SearchInput이라는 자식 컴포넌트의 DOM 요소(input 태그)에 접근해야함. 이런 경우 forwardRef를 사용하여 SearchInput으로 ref를 전달하고 이 ref를 input 요소에 연결함으로써 Page에서 input 요소에 직접 접근할 수 있게됨.
하지만 이런 특별한 경우를 제외하고는 대부분의 경우 state와 props만으로 충분함

**참고**

- https://react.vlpt.us/basic/10-useRef.html
- https://react-ko.dev/learn/manipulating-the-dom-with-refs
- https://react-ko.dev/learn/referencing-values-with-refs#differences-between-refs-and-state
- https://react-ko.dev/reference/react/useRef
- https://www.w3schools.com/react/react_useref.asp

---

## 연관 질문

### useState에 대해서 설명해주세요

데이터를 저장하고 관리할 수 있는 state를 가지고 setState를 통해 state의 상태를 관리하는 상태관리 훅임
setState는 기존의 state를 복사해 새롭게 state를 갱신하기 때문에 객체의 불변성을 지킬 수 있음. 이를 통해 발생할 수 있는 오류들을 미리 방지할 수 있음.
예를 들면 useState를 통해 불변성을 유지하지 않고 임의로 상태를 변경하면 리액트가 상태 변경을 알아채지 못해 컴포넌트가 불필요하게 재렌더링되거나 혹은 이전 상태와 현태 상태를 리액트가 비교하지 못해 변경된 상태를 UI에 반영하지 못함.
그리고 setState를 통해 state에 변화가 생기면 알아서 리렌더링을 해주기 때문에 훨신 쉽게 상태관리를 할 수 있음

### useEffect에 대해서 설명해주세요

useEffect는 componentDidMount, componentDidUpdate, componentWillUnmount룰 하나로 통합한 리액트 훅 중에 하나이며, 컴포넌트 렌더링과 관련된 side effect 를 처리하고(데이터 가져오기, 수동 DOM 조작 등), 의존성 배열을 통해서 필요한 상황에서만 실행되도록 관리할 수 있는 훅임

useEffect 훅을 사용하면 리액트 컴포넌트가 화면에 렌더링 된 후 비동기 작업을 처리함.

```
useEffect(() => {
  console.log('componentDidMount')

  return () => {
    // cleanUp
    console.log('componentWillUnmount')
  }
}, [array dependencies])
// deps에 포함된 상태나 프롭스가 변경될 때마다 다시 렌더링 됨(componentDidMount와 componentDidUpdate를 합친 것처럼 동작)
// deps에 빈배열을 넣은 경우 componentDidMount처럼 동작함. 즉, 마운트되고 첫 렌더링이 끝난 직후에만 첫번째 인자인 함수가 실행됨
```

useEffect는 사이드 이펙트를 처리하면서 컴포넌트 성능이나 렌더링에는 영향을 끼치지 않도록 도와주는 역할을 하며, 이는 비동기 작업과 컴포넌트 렌더링을 분리하고 최적화를 가능하게 함

### 사이드 이펙트(부수효과, side effect)란 무엇인가요?

리액트 컴포넌트는 함수로 정의되며, 대부분의 리액트 함수는 순수함수처럼 동작함.  
즉, 입력(props)을 받으면 예측 가능한 jsx를 return하는 것을 의미함

순수함수는 주어진 입력에 대해 항상 동일한 결과를 리턴하고 외부 상태나 변수에 영향을 주지 않음. 하지만 사이드 이펙트를 가진 함수는 무엇인가를 하기 위해 리액트 컴폰넌트 외부와 함께 수행되기 때문에 순수함수와는 달리 외부 상태를 변경하거나, 외부와 상호작용해 수행한 결과가 어떤지 예측할 수 없음

사이드 이펙트는 다음과 같은 것들이 있음

- 백엔드 서버로 api 데이터 요펑
- 브라우저 api와 상호작용(document, window 직접 사용 등 DOM 조작에 관련됨)
- setTimeout, setInterval 등 타이머 관련 함수

### useMemo, useCallback, React.memo 각각의 차이점과 어떨 때 사용하는지 설명해주세요

useMemo(값 캐싱), useCallback(함수 캐싱), memo(React.memo, 컴포넌트 캐싱)의 훅들은 최적화(optimization)를 위해 사용함
최적화란 불필요한 렌더링을 줄여 비용 발생을 줄이는 것을 말함

그러면 어떨 때 리액트에서는 렌더링이 발생할까?

- 컴포넌트의 state가 바뀌었을 때
- 컴포넌트에서 내려 받은 props가 변경되었을 때
- 부모 컴포넌트가 리렌더링된 경우 자식 컴포넌트는 모두 리렌더링 됨

#### useMemo

동일한 함수를 계속 호출해야 한다면 필요없는 렌더링을 한다고 볼 수 있음. 그래서 맨 처음 해당 값을 리턴할 때 그 값을 메모리에 저장함. 이렇게 하면 필요할 때마다 함수를 다시 호출해서 계산하는 것이 아니라 이미 저장한 값(함수가 리턴하는 값이나 아니면 그냥 값 자체)을 꺼내와서 사용할 수 있음. 이런 걸 보통 캐싱이라고 함

```
// as-is
const value = 반환할_함수();

// to-be
const value = useMemo(()=> {
	return 반환할_함수()
}, [dependencyArray]);
```

dependencyArray의 값이 변경될 때만 반환할\_함수()가 호출됨
그 외의 경우에는 메모이제이션 한 값을 가져옴

##### 예시

HeavyComponent 안에서는 const value = heavyWork() 를 통해서 value값을 세팅해주고 있음. 만약 heavyWork가 엄청나게 무거운 작업이라면 다른 state가 바뀔 때 마다 계속해서 호출이 됨. 하지만 useMemo()로 감싸주게 되면 그럴 걱정이 없음

```
// App.js

import "./App.css";
import HeavyComponent from "./components/HeavyComponent";

function App() {
  const navStyleObj = {
    backgroundColor: "yellow",
    marginBottom: "30px",
  };

  const footerStyleObj = {
    backgroundColor: "green",
    marginTop: "30px",
  };

  return (
    <>
      <nav style={navStyleObj}>네비게이션 바</nav>
      <HeavyComponent />
      <footer style={footerStyleObj}>푸터 영역이에요</footer>
    </>
  );
}

export default App;

// HeavyComponent.jsx

import React, { useState, useMemo } from "react";

function HeavyButton() {
  const [count, setCount] = useState(0);

  const heavyWork = () => {
    for (let i = 0; i < 1000000000; i++) {}
    return 100;
  };

	// CASE 1 : useMemo를 사용하지 않았을 때
  const value = heavyWork();

	// CASE 2 : useMemo를 사용했을 때
  // const value = useMemo(() => heavyWork(), []);

  return (
    <>
      <p>나는 {value}을 가져오는 엄청 무거운 작업을 하는 컴포넌트임</p>
      <button
        onClick={() => {
          setCount(count + 1);
        }}
      >
        누르면 아래 count가 올라가요!
      </button>
      <br />
      {count}
    </>
  );
}

export default HeavyButton;

```

useMemo를 사용하지 않는다면 count state가 변경될 때마다 컴포넌트가 리렌더링되고, heavyWork 함수도 계속 새롭게 만들어져 계속 새로 값을 리턴해야하기 때문에 count가 느리게 올라감
하지만 useMemo를 사용하면 heavyWork에서 리턴한 값을 메모리에 저장하고 있기 때문에(캐싱) count state가 바뀌어 컴포넌트가 리렌더링되어도 캐싱되어있는 값만 가져와서 쓰기 때문에 빠르게 count가 바뀜

##### useMemo의 dependency array

```
import React, { useEffect, useState } from "react";

function ObjectComponent() {
  const [isAlive, setIsAlive] = useState(true);
  const [uselessCount, setUselessCount] = useState(0);

  const me = {
    name: "Ted Chang",
    age: 21,
    isAlive: isAlive ? "생존" : "사망",
  };

  useEffect(() => {
    console.log("생존여부가 바뀔 때만 호출해주세요!");
  }, [me]);

  return (
    <>
      <div>
        내 이름은 {me.name}이구, 나이는 {me.age}야!
      </div>
      <br />
      <div>
        <button
          onClick={() => {
            setIsAlive(!isAlive);
          }}
        >
          누르면 살았다가 죽었다가 해요
        </button>
        <br />
        생존여부 : {me.isAlive}
      </div>
      <hr />
      필요없는 숫자 영역이에요!
      <br />
      {uselessCount}
      <br />
      <button
        onClick={() => {
          setUselessCount(uselessCount + 1);
        }}
      >
        누르면 숫자가 올라가요
      </button>
    </>
  );
}

export default ObjectComponent;
```

위의 예시에서는 `누르면 숫자가 올라가요` 버튼을 누르면 useEffect에 있는 console.log가 계속 찍힘(즉, 계속 리렌더링 되고있음). useEffect의 디펜던시 어레이에 me를 설정해놓았음에도 계속 리렌더링되는 이유는 `누르면 숫자가 올라가요` 버튼을 누르면 uselessCount state가 변경되면서 해당 컴포넌트가 리렌더링되고, 이때 me라는 객체가 새롭게 생성되면서 주소값이 달라지기 때문에 리액트는 me 라는 객체의 상태가 바뀌었다는 것으로 보기 때문임.

이를 해결하기 위해서 useMemo를 활용할 수 있음

```
const me = useMemo(() => {
  return {
    name: "Ted Chang",
    age: 21,
    isAlive: isAlive ? "생존" : "사망",
  };
}, [isAlive]);
```

이렇게 useMemo를 사용하면 me라는 객체는 isAlive라는 값이 변경되기 전까지 항상 같은 주소값을 바라보고 있고, uselessCount가 변경(증가하거나 감소)되어 컴포넌트가 리렌더링되어도 영향을 안 받게 됨

하지만 useMemo를 남발하면 메모리를 너무 많이 확보해야하기 때문에 오히려 성능이 악화될 수 있음 -> 필요할 때만 사용하자

#### useCallback

인자로 들어오는 함수 자체를 memoization함

##### 예시

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

예시에서 +,-버튼과 박스1에 있는 초기화 버튼을 누를 때 모두 App컴포넌트와 Box1 컴포넌트가 리렌더링됨. 그런데 Box1에 React.memo를 했는데 왜 리렌더링이 된걸까? -> App.js에서 만든 initCount 함수를 부모 컴포넌트인 App에서 생성해 자식 컴포넌트인 Box1에 props로 전달하고 있기 때문임. React.memo는 컴포넌트의 props가 변경되지 않았을 때만 컴포넌트의 리렌더링을 방지함. 하지만 initCount 함수는 App 컴포넌트에서 생성된 함수이고 App 컴포넌트가 리렌더링될 때마다 새로운 initCount 함수가 생성(주소값이 달라짐)되므로 하위 컴포넌트인 Box1에서 props가 변경되었다고 간주함

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

##### useCallback의 dependency array

initCount에 아래와 같이 콘솔을 추가함

```
// count를 초기화해주는 함수
const initCount = useCallback(() => {
  console.log(`[COUNT 변경] ${count}에서 0으로 변경되었습니다.`);
  setCount(0);
}, []);
```

만약 카운트를 10고 초기화 버튼을 누르면 콘솔에는 10에서 0으로라는 문자열이 찍혀야함
하지만 콘솔을 찍어보면 0에서 0으로 변경되었다고 나옴
왜냐하면 useCallback의 count가 0일때의 시점을 기준으로 메모리에 함수를 저장했기 때문임.
따라서 initCount 함수의 디펜던시 어레이에 count를 넣으면 count가 변경될 때마다 새롭게 함수를 할당함

```
// count를 초기화해주는 함수
const initCount = useCallback(() => {
  console.log(`[COUNT 변경] ${count}에서 0으로 변경되었습니다.`);
  setCount(0);
}, [count]);
```

이렇게 하면 count가 변경될 때마다 새롭게 함수를 할당하고 콘솔에는 현재값에서 0으로 변경되었다는 문구가 잘 뜸(의도대로 잘 동작함)

#### memo(React.memo)

부모 컴포넌트가 리렌더링된 경우 자식 컴포넌트에서는 바뀐 것이 없는데 굳이 리렌더링 됨
React.memo를 사용해 컴포넌트를 메모리에 저장해두고 필요할 때 갖다씀. 이렇개 하면 부모 컴포넌트의 state 변경으로 인해 props에 변경이 일어나지 않는 이상 컴포넌트는 리렌더링이 되지 않음(component memorization)

##### 예시

```
// ParentApp.js
import React, { useState } from "react";
import ChildBox1 from "./components/ChildBox1";
import ChildBox2 from "./components/ChildBox2";
import ChildBox3 from "./components/ChildBox3";

const boxesStyle = {
  display: "flex",
  marginTop: "10px",
};

function ParentApp() {
  console.log("ParentApp 컴포넌트 렌더링");

  const [count, setCount] = useState(0);

  const onPlusButtonClickHandler = () => {
    setCount(count + 1);
  };

  const onMinusButtonClickHandler = () => {
    setCount(count - 1);
  };

  return (
    <>
      <h3>React.memo 예제</h3>
      <p>현재 카운트 : {count}</p>
      <button onClick={onPlusButtonClickHandler}>+</button>
      <button onClick={onMinusButtonClickHandler}>-</button>
      <div style={boxesStyle}>
        <ChildBox1 />
        <ChildBox2 />
        <ChildBox3 />
      </div>
    </>
  );
}

export default App;


// ChildBox1.js

import React from "react";

const boxStyle = {
  width: "100px",
  height: "100px",
  backgroundColor: "#91c49f",
  color: "white",
  display: "flex",
  justifyContent: "center",
  alignItems: "center",
};

function ChildBox1() {
  console.log("ChildBox1 렌더링");
  return <div style={boxStyle}>Box1</div>;
}

export default ChildBox1;

// ChildBox2.js

import React from "react";

const boxStyle = {
  width: "100px",
  height: "100px",
  backgroundColor: "#91c49f",
  color: "white",
  display: "flex",
  justifyContent: "center",
  alignItems: "center",
};

function ChildBox2() {
  console.log("ChildBox2 렌더링");
  return <div style={boxStyle}>Box1</div>;
}

export default ChildBox2;


// ChildBox3.js

import React from "react";

const boxStyle = {
  width: "100px",
  height: "100px",
  backgroundColor: "#91c49f",
  color: "white",
  display: "flex",
  justifyContent: "center",
  alignItems: "center",
};

function ChildBox3() {
  console.log("ChildBox3 렌더링");
  return <div style={boxStyle}>Box1</div>;
}

export default ChildBox3;
```

위의 예시를 실행하고 버튼을 눌러보면 ParentApp가 리렌더링되면서 하위 컴포넌트인 ChildBox1,2,3 컴포넌트가 리렌더링되고 있음.
하위 컴포넌트는 바뀌지 않았음에도 불필요한 렌더링이 발생함 -> React.memo를 사용해 해결

```
export default React.memo(ChildBox1);
export default React.memo(ChildBox2);
export default React.memo(ChildBox3);
```

이렇게 하위 컴포넌트에 적용해주면 ParentApp 컴포넌트의 state가 변경이 되어도 자식 컴포넌트들은 리렌더링이 되지 않음.
