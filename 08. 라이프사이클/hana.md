# 리액트 컴포넌트의 라이프사이클에 대해 설명해주실 수 있을까요?

- 전체적인 라이프 사이클

<img src="https://i.imgur.com/cNfpEph.png" width="50%" height="50%">

[출처](https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/) 참고

- 마운팅과 업데이트 단계에서 부모 컴포넌트와 자식 컴포넌트 메드 호출 순서
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F5txek%2FbtqEYaoNMkZ%2FKwpalwgZLtAIOO7wREZTB0%2Fimg.png" width="50%" height="50%">

리액트 라이프 사이클은 총 세 가지로 나눌 수 있음

- mounting: 컴포넌트가 화면에 나타남
- updating: 컴포넌트가 업데이트 됨
- unmounting: 컴포넌트가 화면에서 사라짐

## Mounting

컴포넌트의 인스턴스가 생성되어 DOM 상에 삽입되는 단계.
mounting은 라이프 사이클이 종료될 때까지 한 번만 발생함.
마운팅 단계에서 아래의 메소드들이 순서대로 호출됨

- **constructor(생성자)**
  컴포넌트로 만들어지면서 가장 먼저 실행되는 메소드. 초기 state 값을 지정할 수 있음

```
// 클래스형 : 초기 state를 정할 때 constructor를 사용해야 함
// 함수형: useState hook을 사용해 초기 상태를 설정해줄 수 있음

// Class
class Counter extends React.Component {
    constructor(props) {
        super(props);
        this.state = { count: 0 }
    }
}


// Hooks
const Counter = () => {
    const [count, setCount] = useState(0)
}
```

- getDerivedStateFromProps
  props로 받아온 값을 state에 동기화하는 용도로 사용.
  컴포넌트가 마운트될 때와 업데이트될 때 호출됨.
  getDerivedStateFromProps는 최초 마운트할 때와 업데이트할 때 둘 다 render 메소드를 호출하기 직전에 호출됨. state를 갱신하기 위한 객체를 반환하거나 null을 반환해 아무것도 갱신하지 않을 수 있음.
  다른 생명주기 메소드와는 달리 앞에 static을 써야하고 이 안에서는 this를 조회할 수 없음.
  여기서 특정 객체를 반환하게 되면 해당 객체 안에 있는 내용들이 컴포넌트의 state로 설정됨. null을 반환하게 되면 아무것도 발생하지 않음

공홈에는 시간의 흐름에 따라 변하는 props에 state가 의존하는 아무 드문 사용 케이스를 위해 존재한다고 함
자세한 예시는 [여기](https://ko.legacy.reactjs.org/docs/hooks-faq.html#how-do-i-implement-getderivedstatefromprops)를 참고

```
// Class
class Example extends React.Component {
  static getDerivedStateFromProps(nextProps, prevState) {
    if (nextProps.value !== prevState.value) {
      return { value: nextProps.value }
    }
    return null
  }
}
```

- **render**
  컴포넌트를 렌더링할 때 필요한 메소드.
  함수 컴포넌트에서는 render를 사용하지 않고도 컴포넌트를 렌더링할 수 있음

```
// Class
class Example extends React.Component {
    render() {
        return <div>component</div>
    }
}


// Hooks
const example = () => {
    return <div>component</div>
}

```

- **componentDidMount**
  컴포넌트츼 첫 렌더링이 끝나고 나면 호출되는 메소드. 이 메소드가 호출되는 시점에는 우리가 만든 컴포넌트가 화면에 나타나있는 상태임. 여기선 외부 라이브러리를 연동하거나 해당 컴포넌트에서 필요로하는 데이터를 요청하기 위해 axios, fetch 등을 통해 데이터를 요청하고 DOM의 속성을 읽거나 직접 변경하는 작업을 진행함

함수형에서는 useEffect를 사용해 이 기능을 구현할 수 있음.
useEffect의 의존성 배열을 빈배열로 설정하면 componentDidMount와 똑같은 메소드를 구현할 수 있음

```
// class
class Example extends React.Component {
    componentDidMount() {
        ... 생략
    }
}

// hooks
const Example = () => {
    useEffect(() => {
        ...생략
    }, [])
}
```

### mounting example coede

```
// class
class MountingExample extends React.Component {
  constructor(props) {
    super(props);
    this.state = { value: 0 };
  }

  static getDerivedStateFromProps(nextProps, prevState) {
    if (nextProps.value !== prevState.value) {
      return { value: nextProps.value };
    }
    return null;
  }

  render() {
    return <div>{this.state.value}</div>;
  }

  componentDidMount() {
    console.log("Component mounted");
  }
}

// hooks
const MountingExample = (props) => {
  const [value, setValue] = useState(0);

  useEffect(() => {
    setValue(props.value);
  }, [props.value]);

  useEffect(() => {
    console.log("Component mounted");
  }, []);

  return <div>{value}</div>;
};

```

## Updating

업데이트는 다음과 같은 4가지 상황에서 발생함

- props가 변경될 때
- state가 변경될 때
- 부모 컴포넌트가 리렌더링 될 때
- this.forceUpdate로 강제로 렌더링을 트리거할 때

컴포넌트가 업데이트될 때 아래의 메소드들이 순서대로 호출됨

- static getDerivedStateFromProps
  mounting에서 나온 메소드. 컴포넌트의 props나 state가 바뀌었을 때 호출됨
- shouldComponentUpdate
  props나 state이 변경되었을 떄 리렌더링을 할지말지 결정하는 메소드.
  이 메소드는 성능 최적화를 위한 목적으로 사용되며, true나 false를 반환해야함. 렌더링을 방지하는 목적으로 사용하면 버그가 날 수 있음.  
  공홈에서도 클래스형에서 React 컴포넌트의 render() 함수가 동일한 props와 state에 대하여 동일한 결과를 렌더링한다면, React.PureComponent를 사용하여 경우에 따라 성능 향상을 누릴 수 있다고 함([참고](https://ko.legacy.reactjs.org/docs/react-api.html#reactpurecomponent))
  함수형에서 props는 React.memo, state는 useMemo를 활용하면 렌더링 성능 개선할 수 있음

```
// class
class Example extends React.Component {
    shouldComponentUpdate(nextProps) {
        return nextProps.value !== this.props.value
        // 이전 값과 현재 값이 같으면 리렌더링을 하지 않음
    }
}

// hooks
const Example = React.memo(() => {
    ...생략
},
    (prevProps, nextProps) => {
        return nextProps.value === prevProps.value
    }
)
```

- render
- getSnapshotBeforeUpdate
  render 메소드에서 만들어진 결과가 브라우저에 실제로 반영되기 전(컴포넌트에 변화가 일어나기 직전의 DOM 상태)의 DOM 상태를 가져와 특정 값을 반환하면 그 다음 발생하게 되는 componentDidUpdate 함수에서 받아와서 사용할 수 있음.
  이 메소드를 사용하는 것은 흔하지 많지만 이 메서드를 사용하면 컴포넌트가 DOM으로부터 스크롤 위치 등과 같은 정보를 이후 변경되기 전에 얻을 수 있다고 함. 이 메소드가 반환하는 값은 componentDidUpdate()에 인자로 전달됨.
  함수형에서는 아직 이 기능을 하는 hook은 없음

```
// class
class Example extends React.Component {
  getSnapshotBeforeUpdate(prevProps, prevState) {
    if (prevProps.list.length < this.props.list.length) {
      const list = this.listRef.current
      return list.scrollHeight - list.scrollTop
    }
    return null
  }
}
```

- componentDidUpdate
  리렌더링이 완료된 후 실행됨. 화면에 우리가 원하는 변화가 모두 반영되고 난 뒤 호출되는 메소드임.
  3번째 파라미터로 getSnapshotBeforeUpdate에서 반환한 값을 조회할 수 있음

```
// class
class Example extends React.Component {
    componentDidUpdate(prevProps, prevState) {
        ...
    }
}

// hooks
const Example = () => {
    useEffect(() => {
        ...
    });
}

```

### updating example code

```
// class
class UpdatingExample extends React.Component {
  shouldComponentUpdate(nextProps) {
    return nextProps.value !== this.props.value;
  }

  render() {
    return <div>{this.props.value}</div>;
  }

  componentDidUpdate(prevProps, prevState) {
    console.log("Component updated");
  }
}

// hooks
const UpdatingExample = React.memo(
  (props) => {
    useEffect(() => {
      console.log("Component updated");
    }, [props.value]);

    return <div>{props.value}</div>;
  },
  (prevProps, nextProps) => {
    return nextProps.value === prevProps.value;
  }
);

```

## Unmounting

컴포넌트가 화면에서 사라지는 단계

- componentWillUnmount
  이 메소드는 컴포넌트가 화면에서 사라지기 직전(DOM에서 제거할 때) 호출됨. componentDidMount에서 등록한 이벤트가 있다면 여기서 제거하는 작업을 수행해야함. 함수형 컴포넌트에서는 useEffect의 cleanUp 함수에서 componentWillUnmount를 구현할 수 있음

```
// class
class Example extends React.Component {
    componentWillUnmount () {
        ...생략
    }
}

// hooks
const Example = () => {
    useEffect(() => {
        return () => {
            ...제거할 이벤트
        }
    }, [])
}
```

### unmounting example code

```
// class
class UnmountingExample extends React.Component {
  componentWillUnmount() {
    console.log("Component unmounted");
  }

  render() {
    return <div>Unmounting Example</div>;
  }
}

// hooks
const UnmountingExample = () => {
  useEffect(() => {
    return () => {
      console.log("Component unmounted");
    };
  }, []);

  return <div>Unmounting Example</div>;
};

```

---

**요약**

- Mounting

  - constructor: 초기 상태 설정
  - static getDerivedStateFromProps: props를 state에 동기화
  - render: 컴포넌트 렌더링 수행
  - componentDidMount: 외부 라이브러리 연동, 데이터 요청 등

- Updating
  - static getDerivedStateFromProps: props를 state에 동기화 (모든 변경 시 호출)
  - shouldComponentUpdate: 리렌더링 여부 결정, 성능 최적화 용도
  - render: 컴포넌트 렌더링 수행
  - getSnapshotBeforeUpdate: render 결과가 실제 DOM에 반영되기 전의 상태 캡처
  - componentDidUpdate: 리렌더링 완료 후 실행, 이전 상태와 현재 상태의 비교
- Unmounting
- componentWillUnmount: 컴포넌트 해제 전에 이벤트 해제 등 필요한 작업 수행

**정리(기술면접에서 물어본다면 아래와 같이 대답할 듯?)**

리액트 라이프 사이클은 크게 4가지로 말할 수 있습니다. 최초로 컴포넌트 객체가 생성될 때 수행되는 componentDidMount, 그리고 초기에 화면을 그려줄 떄와 업데이트될 때 호출되는 render가 있습니다. 또한 컴포넌트의 props나 state가 변경되면 호출되는 componentDidUpdate가 있고 마지막으로 컴포넌트가 제거될 때 호출되는 componentWillUnmount가 있습니다.

**참고**

- https://whales.tistory.com/146
- https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/
- https://ljh86029926.gitbook.io/coding-apple-react/2/undefined-1/what-is-life-cycle
- https://react.vlpt.us/basic/25-lifecycle.html
