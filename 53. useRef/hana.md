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
