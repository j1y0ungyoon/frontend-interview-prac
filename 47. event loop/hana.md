자바스크립트는 싱글 스레드 언어임. 따라서 call stack을 하나만 사용하기 때문에 동시에 하나의 일만 처리할 수 있음

그러면 자바스크립트는 어떻게 비동기 처리와 동시성에 대한 처리를 할 수 있는 걸까?
웹브라우저가 제공하는 API나 node.js API 등을 통해서 동시에 작업을 하거나 비동기 처리 작업을 할 수 있기 때문임

이런 API와 함께 Callback Queue, Event Loop 등울 사용해 비동기 처리를 관리함

예를 들면 아래의 그림과 같이 작동한다고 보면 쉬움
<img src="https://baeharam.netlify.app/media/js/overview.png" width="50%" height="50%">

## Call Stack

자바스크립가 프로그램을 읽고 실행된 코드의 환경(execution context)을 저장하는 자료구조. 함수 호출 시에 해당 함수의 execution context가 콜스택에 쌓임(사실상 함수 호출 시 함수를 콜스택에 쌓는다고 생각하면 됨)
콜스택에 어떤 함수를 호출하면 스택에 쌓고 다른 함수를 호출하면 이전에 쌓여있던 함수 위에 쌓음. 가장 위에 있는 함수부터 먼저 실행함.
함수 리턴 혹은 함수의 실행이 끝나면 콜스택의 맨 위에 있는 함수를 pop함

## Memory Heap

변수나 함수 등의 객체가 저장되는 메모리 할당 영역

## Web API

브라우저에서 제공하는 API
ex: setTimeOut, XMLHttpRequest, DOM 등...

## Callback Queue

특정 이벤트 발생 시 실행해야 할 콜백함수가 콜백큐에 쌓임
콜스택과는 다르게 가장 먼저 들어온 함수를 먼저 처리함

## Event Loop

- call stack 과 callback queue를 감시함
- 콜 스택이 비어있으면 먼저 들어온 순서대로 콜백 큐에 있는 콜백 함수들을 콜 스택으로 옮김

[여기](http://latentflip.com/loupe/?code=JC5vbignYnV0dG9uJywgJ2NsaWNrJywgZnVuY3Rpb24gb25DbGljaygpIHsKICAgIHNldFRpbWVvdXQoZnVuY3Rpb24gdGltZXIoKSB7CiAgICAgICAgY29uc29sZS5sb2coJ1lvdSBjbGlja2VkIHRoZSBidXR0b24hJyk7ICAgIAogICAgfSwgMjAwMCk7Cn0pOwoKY29uc29sZS5sb2coIkhpISIpOwoKc2V0VGltZW91dChmdW5jdGlvbiB0aW1lb3V0KCkgewogICAgY29uc29sZS5sb2coIkNsaWNrIHRoZSBidXR0b24hIik7Cn0sIDUwMDApOwoKY29uc29sZS5sb2coIldlbGNvbWUgdG8gbG91cGUuIik7!!!PGJ1dHRvbj5DbGljayBtZSE8L2J1dHRvbj4%3D)를 참조하면 상세한 예시를 볼 수 있음. (jsConference에서 이벤트 루프에 대해 설명한 사람이 시각적으로 보여주기 위해서 만든 사이트임)

**참고**

- https://youtu.be/8aGhZQkoFbQ
- https://medium.com/sjk5766/javascript-%EB%B9%84%EB%8F%99%EA%B8%B0-%ED%95%B5%EC%8B%AC-event-loop-%EC%A0%95%EB%A6%AC-422eb29231a8
- https://ko.javascript.info/event-loop
- https://baeharam.netlify.app/posts/javascript/event-loop
