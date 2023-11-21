# event loop 란?

자바스크립트는 싱글 스레드 언어 이다. 한번에 하나의 작업만 수행이 가능한 것이다.
웹 애플리케이션에서는 네트워크 요청이나 이벤트 처리, 타이머와 같은 작업을 멀티로 처리해야 하는 경우가 많다.
오래 걸리고 반복적인 작업들은 자바스크립트 엔진이 아닌 브라우저 내부의 멀티 스레드인 Web APIs에서 비동기 + 논블로킹으로 처리된다.

> 즉, 비동기로 동작하는 핵심요소는 자바스크립트 언어가 아니라 브라우저라는 소프트웨어가 가지고 있다고 보면 된다. Node.js 에서는 libuv 내장 라이브러리가 처리한다.

**싱글 스레드인 자바스크립트의 작업을 멀티 스레드로 돌려 작업을 동시에 처리시키게 하던가, 또는 여러 작업 중 어떤 작업을 우선으로 동작시킬 것인지 결정하는 세심한 컨트롤을 하기 위해 존재하는 것이 바로 이벤트 루프(Event Loop) 이다.**

**이벤트 루프는 브라우저 내부의 Call Stack, Callback Queue, Web APIs 등의 요소들을 모니터링하면서 비동기적으로 실행되는 작업들을 관리하고, 이를 순서대로 처리하여 프로그램의 실행 흐름을 제어한다.(브라우저의 동작 타이밍을 제어하는 관리자)**

<img src="https://miro.medium.com/v2/resize:fit:720/0*6T6KIVRkN9nWb3QU.gif">

### 1. js Engine

자바스크립트 엔진은 코드를 이해하고 실행을 도와주는 역할을 한다. (대표적인 엔진으로 google V8 이다.)
각 브라우저 별로 여러 가지 엔진들이 존재하지만 js엔진은 크게 memory heap 과 call stack으로 이루어져 있다,

- Memory Heap : 메모리 할당이 일어나는 장소
- Call Stack : 코드가 실행될 경우 하나씩 stack의 형태로 쌓이는 장소

### 2. Memory Heap & Call stack

먼저 Memory Heap에 있는 사용자가 작성한 코드들은 Call Stack에서 Stack 방식으로 쌓이며 코드를 실행하게 되는데 이때 동기 함수들은 그대로 실행하게 되고 비동기 함수들은 Web API로 처리하게 되며 일을 분배 한다.
`Stack : 후입선출(LIFO)로 마지막에 들어간 것이 먼저 나가는 방식`

### 3. Web API

Javscript를 사용하면서 우리가 많이 사용하는 API 들은 사실 JavaScript에서 지원하는 것이 아닌 웹 브라우저에서 제공하는 API로 DOM ,AJAX, Timeout 등이 있다.

Call Stack에서 실행된 비동기 함수는 Web API에서 처리를 하게 되고 그동안에 Call Stack은 나머지 동기 함수들을 처리하게 된다

Web API는 비동기 함수들을 처리하며 작업이 완료된 비동기 함수들을 Callback Queue로 넘겨주게 된다.

### callback Queue

Callback Queue는 비동기 함수들을 보관하는 장소로 Event Loop에서 비동기 함수를 꺼내기 전까지는 계속 Queue방식으로 보관하게 된다.
`Queue : 선입선출(FIFO)로 먼저 들어간 것이 먼저 나가는 방식`

### event Loop

Event Loop는 Call Stack과 Callback Queue를 상태를 계속 감시하며 Call Stack에 함수들이 존재하지 않는다면 Callback Queue에 있는 비동기 함수들을 Call Stack에 밀어 넣게 됩니다. 그 후 Call Stack에서 비동기 함수를 실행시키게 된다.

https://blog.toycrane.xyz/%EC%A7%84%EC%A7%9C-%EC%89%BD%EA%B2%8C-%EC%95%8C%EC%95%84%EB%B3%B4%EB%8A%94-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC-c7fbdc44cc97

# 2023.11.21 추가

자바스크립트의 비동기 동작을 이해하는데 핵심적인 개념.
자바스크립트는 싱글스레드 언어 이다. 한 번에 하나의 작업만 처리 할수 있다. 그러나 Event Loop를 사용하므로써 비동기 처리를 가능하게 한다. (서버에 요청을 보내는 동안 다른 스크립트를 실행할 수 있다.)

## 작동방식

1. call Stack : 자바스크립트코드가 실행될 때 함수 호출은 call stack에 추가된다. 함수가 반환되면 call stack에서 제거된다.
2. web APIs: 브라우저에서 제공하는 API(setTimeout, XMLHttpRequest)는 call stack에서 직접 실행되지 않고 web APIs에서 처리 완료되면 task Queue로 보내진다.
3. task Queue : callback함수를 저장한다. call stack이 비어있을 때 Event loop가 task Queue에서 callback을 call stack으로 이동한다. (1. micro task queue 2. 상황에 따라 animationFrame queue 3. task queue)
   1. micro task queue : promise...
   2. task queue : setTimeout...
   3. animaionFrame queue : animation
4. Event Loop: call stack이 비어 있을때 Event Loop는 Task Queue에서 대기중인 callback을 call stack으로 이동 시킨다.

```js
console.log("First");

setTimeout(() => {
  console.log("Second");
}, 0);

console.log("Third");

//First Third Second
```
