# 리덕스에 대해서 아는 만큼 설명해주실 수 있나요?

## 리덕스가 필요한 이유

기존 useState를 통해 a라는 컴포넌트에서 생성한 state가 다른 컴포넌트에서 필요하다면 props를 통해 state를 보냄
props를 통해 state를 보내려면 다음과 같음

1. 컴포넌트끼리 부모 자식 관계여야 함
2. 부모 컴포넌트에서 props가 필요한 자식 컴포넌트까지 일일히 내려주어야 함 -> 해당 props가 필요하지 않은 컴포넌트까지 거쳐야함
   -> props drilling이 발생함

또한 자식에서 부모 컴포넌트로 값을 보낼 수 없다는 점도 있음

이렇게 기존의 방법으로 state를 공유할 때 불편함이 발생함
이러한 불편함을 없애고자 나온 것이 리덕스임

리덕스는 전역 상태 관리 라이브러리로, state를 공유할 때 props를 내려줄 필요 없이 바로 중앙 store에 있는 global state를 사용하면 됨.
따라서 props drilling을 막을 수 있고, 자식 컴포넌트에서 만든 state를 부모 컴포넌트에서도 사용할 수 있음

## 리덕스의 흐름

<img src="https://velog.velcdn.com/images%2Fannahyr%2Fpost%2F8ee92e54-dbab-4a74-8af1-f8680e8848e3%2Fmodels_redux_animation.gif" width="50%" height="50%">

- view에서 action 발생함
- dispatch에서 action이 일어나게 됨
- action에 의한 reducer 함수가 실행되기 전에 미들웨어가 작동함
- 미들웨어에서 내린 명령을 수행하고 난 뒤 reducer 함수 실행함
- reducer의 실행결과 store에 새로운 값을 저장함
- store의 state를 구독하고 있던 UI에 변경된 값을 줌

## 리덕스의 핵심 요소

- Action
  state가 변하는 것. 말 그대로 액션임

- Reducer
  데이터(state)를 수정하는 함수. action을 통해 어떤 행동을 정의했다면 그 결과 어플리케이션의 상태가 어떻게 바뀌는지 특정하게 되는 함수임

- Store
  action과 action에 따라 상태를 수정하는 reducer를 저장하는 어플리케이션에 있는 단 하나의 객체임.
  store는 state를 계속 체크해서 view에게 변경된 사항을 알려주는 역할을 함

- Dispatch
  store의 내장 함수 중 하나로 reducer에게 action 객체를 발생시키도록 던져주는 것을 의미함.

- Subscribe
  action이 dispatch될 때마다 전달해준 함수를 호출함

- Middleware
  action을 dispatch했을 때 reducer에게 이를 처리하기에 앞서 사전에 지정된 작업들을 실행함
  ex: thunk, saga

## 예시(counter)

```
// src/App.js

import React from "react";
import { useState } from "react";
import { useDispatch, useSelector } from "react-redux";

// 4. Action Creator를 import 합니다.
import { addNumber } from "./redux/modules/counter";

const App = () => {
	// 1. dispatch를 사용하기 위해 선언해줍니다.
  const dispatch = useDispatch();
  const [number, setNumber] = useState(0);
  const globalNumber = useSelector((state) => state.counter.number);

  const onChangeHandler = (event) => {
    const { value } = event.target;
    setNumber(+value);
  };

	// 2. 더하기 버튼을 눌렀을 때 실행할 이벤트핸들러를 만들어줍니다.
  const onClickAddNumberHandler = () => {
		// 5. Action creator를 dispatch 해주고, 그때 Action creator의 인자에 number를 넣어줍니다.
    dispatch(addNumber(number));
  };

  return (
    <div>
      <div>{globalNumber}</div>
      <input type="number" onChange={onChangeHandler} />
			{/* 3. 더하기 버튼 이벤트핸들러를 연결해줍니다. */}
      <button onClick={onClickAddNumberHandler}>더하기</button>
      <button>빼기</button>
    </div>
  );
};

export default App;
```

```
// src/modules/counter.js

// 1. Action value
const PLUS_ONE = "PLUS_ONE";
const MINUS_ONE = "MINUS_ONE";
const MINUS_N = "MINUS_N";
const PLUS_N = "MINUS_N";


// 2. Action Creator
export const plusOne = () => {
    type: PLUS_ONE,
}
export const minusOne = () => {
    type: MINUS_ONE,
}

export const plusN = (payload) => {
  return {
    type: PLUS_N,
    payload: payload;
  };
};

export const minusN = (payload) => {
  return {
    type: MINUS_N,
    payload
  };
};


// 3. initial State
const initialState = {
  number: 0,
};

// 4. Reducer
const counter = (state = initialState, action) => {
  switch (action.type) {
    case PLUS_ONE: // action value에서 선언한 상수를 넣어줌
      return {
        number: state.number + 1,
      };
    case MINUS_ONE:
      return {
        number: state.number - 1,
      };
    case MINUS_N:
      return {
        number: state.number - action.payload,
      };
    case PLUS_N:
      return {
        number: state.number + action.payload,
      };
    default:
      return state;
  }
};


// 5. export default Reducer
export default counter;
```

```
import { createStore } from "redux";
import { combineReducers } from "redux";

/*
1. createStore()
리덕스의 가장 핵심이 되는 스토어를 만드는 메소드(함수) 입니다.
리덕스는 단일 스토어로 모든 상태 트리를 관리한다고 설명해 드렸죠?
리덕스를 사용할 시 creatorStore를 호출할 일은 한 번밖에 없을 거예요.
*/

/*
2. combineReducers()
리덕스는 action —> dispatch —> reducer 순으로 동작한다고 말씀드렸죠?
이때 애플리케이션이 복잡해지게 되면 reducer 부분을 여러 개로 나눠야 하는 경우가 발생합니다.
combineReducers은 여러 개의 독립적인 reducer의 반환 값을 하나의 상태 객체로 만들어줍니다.
*/

const rootReducer = combineReducers({
    counter: counter,
    users,
});
const store = createStore(rootReducer);

export default store;

```

## Ducks pattern

리덕스 모듈을 개발하는 개발자마다 다르게 구현하면 협업할 때 힘듦 -> Erik Rasmuss가 제안한 [Ducks](https://github.com/erikras/ducks-modular-redux) 라는 패턴을 제안하였고, 현재 리덕스 모듈 작성법의 정석으로 여겨지고 있음.

**Ducks pattern**

1. Reducer 함수를 export default함
2. Action creator 함수들을 export함
3. Action type은 **app/reducer/ACTION_TYPE** 형태로 작성함

모듈 파일 1개에 Action type, Action Creator, Reducer 가 모두 들어있는 방식임
