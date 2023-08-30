# ContextAPI 란 무엇인가요?

리액트에서 기본적으로 제공하는 빌트인 API임
리액트에서는 기본적으로 상위 컴포넌트에서 하위 컴포넌트로 props를 내려 데이터를 전달해주는데, 해당 props가 필요한 하위컴포넌트까지 중간에 있는 여러 컴포넌트들을 거쳐 props를 내려주게 됨.
해당 props가 필요없는 컴포넌트들에도 props를 넘겨야하므로 props를 관리하고 추척하기 어려움.
즉, props drilling이 발생할 수 있음.

contextAPI는 이러한 props drilling 없이 리액트 컴포넌트 간에 어떤 값을 **공유**할 수 있게 해주는 기능을 제공함. 주로 전역적으로 필요한 값을 다룰 때 많이 사용하고, 재사용성이 높은 컴포넌트를 만들 때도 사용함.

Context API는 createContext라는 함수로 생성하고 Context Provider 컴포넌트에 공유하려는 값을 value라는 props로 설정하면 하위 컴포넌트들이 useContext를 사용해 value에 넣은 값에 바로 접근할 수 있어 props drilling을 막고 코드의 가독성을 높일 수 있음.

하지만 provider에서 제공한 value가 달라지면 useContext를 사용하는 모든 컴포넌트가 리렌더링이 됨 -> 필요없는 리렌더링이 발생할 수 있음 -> useMemo 등으로 최적화할 수 있지만 이 또한 연산이 들어가는 작업이기에 제대로 사용하지 않으면 소용없음

즉, Context API는 전역적으로 값이나 상태만을 공유하고 상태관리는 하지 않음. 그래서 상태관리를 위해 전역 상태관리 라이브러리가 필요함
전역 상태관리 라이브러리(ex: redux, recoil, mobX, Jotai)를 사용해 상태 업데이트와 리렌더링을 최적화함. 이런 라이브러리들은 상태 변경을 좀 더 세밀하게 제어하고 필요한 컴포넌트만 업데이트하여 성능을 향상시킬 수 있음

Redux는 action과 reducer 개념을 통해 복잡한 상태 업데이트 로직을 관리함.
Recoil은 useRecoilState와 같은 Hook을 제공하여 React의 useState Hook과 유사한 방식으로 상태를 관리할 수 있음
