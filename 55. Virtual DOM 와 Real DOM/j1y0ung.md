# Virtual DOM이란 무엇이고 Real DOM과의 차이는 무엇인가요?

## DOM

DOM은 Document Object Model의 약자로 문서 객체 모델 (HTML XML 문서의 프로그래밍 interface)이다.
웹페이지는 일종의 문서(document)로 이 문서는 웹 브라우저를 통해 내용이 해석되어 웹 브라우저 화면에 나타나거나 HTML 소스 자체로 나타나기도한다.( 동일한 문서를 사용하여 다른 형태로 나타낼 수 있다. ) DOM은 동일한 문서를 표현하고, 저장, 조작하는 방법을 제공한다.

DOM은 웹 페이지의 객체 지향표현이며, 자바스크립트와 같은 스크립팅 언어를 이용해 DOM을 수정할 수 있다. - 출처 MDN

- 웹 페이지를 수정, 생성하는데 사용되는 모든 property, method, event 들은 objects로 구성된다.

- DOM은 tree형식의 자료구조를 가지고있다.
  - tree의 자료구조는 하나의 root node에서 시작되어 parent node child node를 가질 수 있다.
  - DOM tree의 root node는 document이며 <html> 은 root element 이다.

> node
> 웹 페이지의 모든 요소는 노드로 표현된다. 노드에는 여러가지 종류가 있으며 (element jnode, attribute node, text node, comment node) 등이 있다.

> API (web or XML page / Application Programming interface) 는 DOM과 Javascript (scripting language) 로 구성되어있다.

## Virtual DOM

Virtual DOM 가상돔은 실제 DOM에 접근하여 조작하는 대신 이를 추상화한 자바스크립트 객체를 구성하여 사용하는 것이다.

DOM 상태를 메모리에 저장하고, 변경 전과 변경 후의 상태를 비교 뒤 최소한의 내용만 반영하는 기능이있다(성능향상).
가상DOM은 DOM의 상태를 메모리 위에 계속 올려두고 DOM에 변경이 있는 경우 해당 변경 내용을 반영한다.

### React가 Virtual DOM 을 반영하는 절차

1. 데이터가 업데이트 되면 전체 UI를 Virtual DOM에 리렌더링한다.
2. 이전 Virtual DOM에 있던 내용과 현재의 내용을 비교한다. (가상 돔 끼리 비교)
3. 바뀐 부분만 실제 DOM에 적용이 된다. (컴포넌트가 업데이트 될 때, 레이아웃 계산이 한번만 이루어진다.)

### 가상돔을 사용하는 이유

실제 DOM에는 브라우저가 화면을 그리는데 필요한 모든 정보가 들어있어 실제 DOM을 조작하는 작업이 무겁기 때문에 React에서 실제 DOM의 변경 사항을 빠르게 파악하고 반영하기 위해서 내부적으로 가상 DOM을 만들어 관리한다.

- Virtual DOM은 DOM의 요약본이라고 볼 수 있다.
- DOM이 꼭 느리다고 볼 순 없다.
