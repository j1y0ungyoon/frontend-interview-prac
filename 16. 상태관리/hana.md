# 상태관리의 대표적인 도구 하나와 그것의 원리에 대해 구체적으로 설명해주실 수 있을까요? 예를 들어 리덕스라면 리덕스가 무엇이고 어떻게 활용이 되는지, 어떤 흐름으로 데이터가 들어왔다가 나가는지, 데이터 흐름은 양방향인지 단방향인지, 어떤 함수가 데이터를 가져오게 해주는지 등을 언급해주시면 좋습니다.

저는 전역 상태관리로 recoil, 서버 상태 관리를 위해 react-query를 사용했습니다.

먼저 recoil은 전역 상태관리 라이브러리로 직관적이고 리덕스에 비해 보일러플레이트가 적으며, 러닝커브가 낮다는 장점이 있습니다.
recoil은 atom과 selector를 통해 상태를 관리합니다.
먼저 atom을 정의하고 컴포넌트에서 사용 시 useRecoilState, useSetState 등을 사용해 기존의 리액트에서 상태관리를 하는 것과 비슷하게 사용합니다.
atom 값이 변경되면 해당 atom을 구독하고 있는 모든 컴포넌트가 리렌더링됩니다.
recoil의 데이터 방향은 리액트처럼 단방향입니다.

react-query는 서버 상태 관리를 할 때 많이 사용하며 이 역시도 직관적이고 서버에서 데이터를 가져오고 캐싱하고 업데이트, 동기화를 하는데 사용됩니다.
react-query의 데이터 흐름도 단뱡향이고 다음과 같이 실행됩니다
useQuery(fetch)나 useMutation 훅을 사용해 API 호출 정의를 하고 필요에 따라 자동으로 API 호출을 실행함 -> 해당 useQuery나 useMutation을 구독하는 컴포넌트들은 새로운 데이터로 리렌더링됨
이때 반환된 데이터는 react query가 관리하는 캐시에 저장됨

---

**연관 질문(걍 제가 생각해서 적어봄)**

# flux pattern에 대해서 설명해주세요

## flux pattern이 나온 이유

구 페이스북 현 메타에서 MVC 모델의 단점을 보완하기 위해 발표한 어플리케이션 아키텍쳐임
페북은 MVC 모델을 사용하였음

### MVC 모델

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FFekY0%2FbtscRKxq5Ry%2FSZk5eYbTkzgUdOaIIFvuB1%2Fimg.png" 
width="50%" height="50%" alt="MVC pattern">

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FllLZZ%2FbtscVmJfqaV%2FLl6fRVuqZDVUrpCI7UQli1%2Fimg.png" 
width="50%" height="50%" alt="MVC pattern">

view가 여러 개의 model을 동시에 업데이트하고 model 또한 여러 개의 view를 업데이트 할 수 있음.
그러나 새로운 기능이 추가되거나 규모가 커질 수록 복잡한 데이터 흐름을 가지기 때문에 예측 불가능한 코드를 작성하게 되고, 예상치 못한 버그가 나올 수 있으며, 디버깅이 어려워진다는 단점이 생김.

실제로 페이스북에서는 알림 버그가 발생했었는데, 사용자가 페이스북에 로그인했을 때 화면 위의 메세지 아이콘에 알림이 떠있지만, 그 메세지 아이콘을 눌러 들어가면 아무런 메세지가 없음. 몇 분 뒤에 또 알림이 나타나고 역시 아무 메세지도 없는 상황이 계속 반복되었음.
개발자들은 해당 버그를 고쳤지만 업데이트를 하면 또 같은 현상이 계속 나타났음.
이를 위해 아예 다른 아키텍처를 만들었고 그 아키텍처가 바로 flux pattern임

## flux pattern

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F0aGbh%2FbtscTh9hjw5%2FxTooNuVZrdkaDVIqv90qZk%2Fimg.png" 
width="50%" height="50%" alt="MVC pattern">

기존 MVC와 같은 양방향으로 데이터가 흐르는 것이 아닌 단방향으로만 흐르게 하는 아키텍처 디자인 패턴임
flux pattern에서 데이터는 단방향으로 흐르고, 새로운 데이터를 넣으면 처음부터 다시 흐름이 시작됨
좀 더 자세하게 말해보자면 사용자가 view에서 특정 행위를 통해 action을 발생시키면 dispatcher가 store로 action을 전달함. store는 action에 영향있는(의존적인) 모든 view를 갱신함

이렇게 flux pattern을 사용해 단방향 데이터 흐름을 유지하므로 일관성 있는 데이터 흐름을 구현하는 데 유용하고, 프로그램의 예측 가능성을 높일 수 있음

## flux patter과 Redux

리덕스는 flux 아키텍처 디자인을 기반으로 한 라이브러리임(flux pattern을 비슷하게 적용. 완전히 같지는 않음)
리덕스는 어플리케이션의 상태를 하나의 store에 관리하고 이 store에서 상태를 변경하는 모든 작업은 action을 통해 수행됨. 변경된 상태를 store를 구독하고 있는 컴포넌트들에게 전달해 해당 store를 구독하는 컴포넌트들이 업데이트 됨.

flux와 redux 둘 다 단방향 데이터 흐름을 사용해 데이터의 일관성과 예측 가능성을 유지하고, 모든 상태 변경이 불변성을 유지하도록 구현됨. 리액트와 같이 사용 시 유지 보수성을 높이는 데 도움이 되고, 해당 프로젝트의 복잡성을 낮출 수있음

**참고**

- https://bestalign.github.io/translation/cartoon-guide-to-flux/
- https://yozm.wishket.com/magazine/detail/1663/
- https://watermelonlike.tistory.com/entry/%EB%A6%AC%EC%95%A1%ED%8A%B8%EC%97%90%EC%84%9C-%EC%83%81%ED%83%9C%EA%B4%80%EB%A6%AC%EB%9E%80flux-%EC%82%AC%EC%9A%A9-%EC%9D%B4%EC%9C%A0
- https://oliviakim.tistory.com/93
