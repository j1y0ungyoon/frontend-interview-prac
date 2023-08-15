자바스크립트는 동기식 언어임. 즉, 하나의 작업이 끝나기 전까지 그 뒤의 작업들은 실행되지 않고 하나의 작업이 끝나야지만 비로소 뒤의 작업들이 순서대로 실행됨.

이렇게 한 번에 하나의 task를 수행하는 것은 싱글 스레드에서 코드가 동작할 경우임.
즉 자바스크립트는 task에 대한 요청을 처리할 수 있는 주체가 하나밖에 없기 때문에(실글 스레드) 한 번에 하나의 일만 처리할 수 있음

그러면 자바스크립트는 어떻게 비동기 처리와 동시성에 대한 처리를 할 수 있는 걸까?
브라우저 안에 내장되어있는 자바스크립트 엔진은 싱글 스레드로 동작하지만 브라우저 엔진은 멀티 스레드로 동작함. 멀티 스레드로 동작하는 브라우저가 제공하는 웹 API를 함께 사용해 비동기 동작을 구현할 수 있음

ex: setTimeout함수로 작성된 코드 실행 -> 콜스택에 쌓임 -> 타이머 작동과 타이머 완료 후 콜백 함수를 queue로 보내주는 역할을 브라우저 웹 API가 함

멀티스레드로 동작하는 브라우저 엔진이 자바스크립트에서 작성된 비동기 함수의 동작을 처리해주기 때문에 비동기 동작을 구현할 수 있음

예를 들면 아래의 그림과 같이 작동한다고 보면 쉬움
<img src="https://baeharam.netlify.app/media/js/overview.png" width="50%" height="50%">

결론적으로 자바스크립트 엔진 자체는 콜스택에서 한 번에 하나의 일을 수행하는 동기적 동작을 하지만 브라우저의 task queue, event loop, web API가 자바스크립트 엔진에서 전달받은 일을 엔진 밖에서 독립적으로 수행하기 때문에 브라우저에서 실행되는 자바스크립트 코드가 비동기 방식으로 처리되는 것처럼 보임

## Memory Heap

객체나 함수, 참조 타입 등의 데이터가 저장되는 메모리 할당 영역.
할당 메모리의 공간 크기는 런타임에 의해 결정하기 때문에 heap은 구조화 되지 않은 영역임

## Call Stack

자바스크립가 프로그램을 읽고 실행된 코드의 환경(execution context)을 저장하는 자료구조. 함수 호출 시에 해당 함수의 execution context가 콜스택에 쌓임(사실상 함수 호출 시 함수를 콜스택에 쌓는다고 생각하면 됨)
콜스택에 어떤 함수를 호출하면 스택에 쌓고 다른 함수를 호출하면 이전에 쌓여있던 함수 위에 쌓음. 가장 위에 있는 함수부터 먼저 실행함.
함수 리턴 혹은 함수의 실행이 끝나면 콜스택의 맨 위에 있는 함수를 pop함

## Web API

브라우저에서 제공하는 API
ex: setTimeOut, XMLHttpRequest, DOM API, WebGL, Canvas, 오디오, 비디오 등...

## Queue

- 이벤트 발생 시 실행해야 할 콜백함수가 콜백큐에 쌓임
- 콜스택과는 다르게 가장 먼저 들어온 함수를 먼저 처리함(FIFO)
- task queue, micro task queue로 나뉨

### micro task queue

- 콜 스택이 비어있을 때 가장 먼저 이벤트 루프가 확인하는 큐
- 대표적으로 Promise의 then, catch, finally가 있음

### task queue

- task queue는 micro task queue에 콜백함수가 없으면 이벤트 루프가 확인하는 큐
- 대표적으로 setTimeout, setInterval, addEventListener, 렌더링 작업 등 등이 있음

=> 큐를 나누는 이렇게 두 개로 나누는 이유는 **우선순위**를 정해 원활한 페이지 동작을 가능하게 하기 위함임.
Promise로 데이터를 전달받아 웹 페이지를 반영하는 코드 있음 -> setTimeout이 Promise보다 먼저 실행되면 페이지에 반영해야하는 데이터를 늦게 전달받기 때문에 원활한 웹페이지 동작을 기대할 수 없음 -> 이런 경우를 예방하기 위해 큐에서도 우선순위를 나누는 것임

따라서 마이크로태스트 큐와 태스크 큐가 같이 사용되었다고 가정하면 우선순위에 따라 이벤트 루프가 마이크로 태스트큐를 우선적으로 콜 스택으로 보내 실행할 것임

## Event Loop

- call stack 과 queue를 감시함
- 콜 스택이 비어있으면 먼저 들어온 순서대로 큐에 있는 콜백 함수들을 콜 스택으로 옮김
- queue에도 우선순위가 있어 마이크로 태스크 큐를 먼저 콜 스택에 보내고 그 다음 태스크 큐를 콜 스택으로 보냄

[여기](http://latentflip.com/loupe/?code=JC5vbignYnV0dG9uJywgJ2NsaWNrJywgZnVuY3Rpb24gb25DbGljaygpIHsKICAgIHNldFRpbWVvdXQoZnVuY3Rpb24gdGltZXIoKSB7CiAgICAgICAgY29uc29sZS5sb2coJ1lvdSBjbGlja2VkIHRoZSBidXR0b24hJyk7ICAgIAogICAgfSwgMjAwMCk7Cn0pOwoKY29uc29sZS5sb2coIkhpISIpOwoKc2V0VGltZW91dChmdW5jdGlvbiB0aW1lb3V0KCkgewogICAgY29uc29sZS5sb2coIkNsaWNrIHRoZSBidXR0b24hIik7Cn0sIDUwMDApOwoKY29uc29sZS5sb2coIldlbGNvbWUgdG8gbG91cGUuIik7!!!PGJ1dHRvbj5DbGljayBtZSE8L2J1dHRvbj4%3D)를 참조하면 상세한 예시를 볼 수 있음. (jsConference에서 이벤트 루프에 대해 설명한 사람이 시각적으로 보여주기 위해서 만든 사이트임)

**참고**

- https://youtu.be/8aGhZQkoFbQ
- https://medium.com/sjk5766/javascript-%EB%B9%84%EB%8F%99%EA%B8%B0-%ED%95%B5%EC%8B%AC-event-loop-%EC%A0%95%EB%A6%AC-422eb29231a8
- https://ko.javascript.info/event-loop
- https://baeharam.netlify.app/posts/javascript/event-loop
