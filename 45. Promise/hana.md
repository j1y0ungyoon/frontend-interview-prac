# Promise란 무엇인지 설명해주실 수 있을까요?

자바스크립트에서 프로미스란 비동기 작업을 처리하기 위한 객체입니다.
자바스크립트에서 비동기 처리를 위한 방법 중 하나로 콜백함수를 사용함. 하지만 전통적인 콜백 패턴은 콜백헬* 로 인해 가독성이 나쁘고, 비동기 처리 중 발생한 에러의 처리가 어려워 여러 개의 비동기 처리를 한번에 처리하는 데 한계가 있었음(에러 처리의 한계*).
이러한 문제들을 극복하기 위해 ES6에서 비동기 처리를 위한 방법으로 프로미스를 도입함. 프로미스는 콜백 패턴이 가진 단점을 보완하고 비동기 처리 시점을 명확하게 표현할 수 있음

- 콜백헬
  비동기 함수의 처리 결과를 가지고 다른 비동기 함수를 호출해야하는 경우, 함수의 호출이 중첩(nesting) 되어 복잡도가 높아지는 현상

<img src="https://poiemaweb.com/img/callback-hell.png" width="50%" height="50%">

- 에러 처리의 한계

```
try {
  setTimeout(() => { throw new Error('Error!'); }, 1000);
} catch (e) {
  console.log('에러를 캐치하지 못한다..');
  console.log(e);
}
```

위의 코드는 try 안의 setTimeout 함수가 실행되면 1초 후에 에러를 발생하고자 함. 하지만 catch 블록에서 에러를 캐치하지 못함. 왜냐하면 setTimeout 함수는 비동기 함수이므로 콜백 함수가 실행될 때까지 기다리지 않고 바로 종료되어 콜스택에서 제거됨. 이후 setTimeout의 콜백함수는 task queue로 이동한 후 이벤트 루프가 콜스택을 감시하다 콜스택이 비어있을 때 콜스택으로 setTimeout의 콜백함수를 이동시켜 실행됨.  
그런데 에러가 발생하면 자바스크립트는 콜스택을 거슬러 올라가며 예외를 처리할 수 있는 catch 블록을 찾음. 그러나 setTimeout의 콜백함수는 이미 콜스택에서 제거되었으므로 이 콜백함수에서 발생한 에러는 catch 블록에서 캐치할 수 없음.

## 프로미스의 생성

프로미스는 Promise 생성자 함수는 비동기 작업을 수행할 콜백함수를 인자로 받는데, 이 콜백함수는 resolve와 reject 함수를 인자로 전달받음. 또한 프로미스는 비동기 처리가 성공이나 실패했는지 등의 상태 정보를 가짐

| 상태      | 의미                                      | 구현                                             |
| --------- | ----------------------------------------- | ------------------------------------------------ |
| pending   | 비동기 처리가 아직 수행X                  | resolve or reject 함수가 아직 호출되지 않은 상태 |
| fulfilled | 비동기 처리가 수행된 상태(성공)           | resolve 함수가 호출된 상태                       |
| rejected  | 비동기 처리가 수행된 상태(실패)           | reject 함수가 호출된 상태                        |
| settled   | 비동기 처리가 수행된 상태(성공 또는 실패) | resolve 또는 reject 함수가 호출된 상태           |

## 프로미스의 후속 처리 메소드

Promise로 구현된 비동기 함수는 Promise 객체를 반환해야함. Promise로 구현된 비동기 함수를 호출하는 측에서는 Promise 객체의 후속 처리 메소드(then, catch)를 통해 비동기 처리 결과 또는 에러 메시지를 전달받아 처리함. Promise 객체는 상태를 갖는다고 했는데, 이 상태에 따라 후속 처리 메소드를 체이닝 방식으로 호출함. Promise의 후속 처리 메소드는 아래와 같음

- then
  then 메소드는 두 개의 콜백 함수를 인자로 전달 받음. 첫 번째 콜백 함수는 성공 시 호출되고 두 번째 함수는 실패 시 호출됨. then 메소드는 Promise를 반환함

- catch
  예외(비동기 처리에서 발생한 에러와 then 메소드에서 발생한 에러)가 발생하면 호출됨. catch 메소드는 Promise를 반환함

## 프로미스 체이닝

프로미스는 후속 처리 메소드를 체이닝해 여러 개의 프로미스를 연결해 사용할 수 있음. 이를 통해 콜백헬을 해결함. Promise 객체를 반환한 비동기 함수는 프로미스 후속 처리 메소드인 then이나 catch 메소드를 사용할 수 있음. 따라서 then 메소드가 Promise 객체를 반환하도록 하면(then 메소드는 기본적으로 Promise를 반환) 여러 개의 프로미스를 연결하여 사용할 수 있음

## 프로미스의 정적 메소드

Promise는 주로 생성자 함수로 사용되지만 함수도 객체이므로 메소드를 가질 수 있음. Promise 객체는 4가지 정적 메소드를 제공함

- Promise.resolve / Promise.reject
  각각 인자로 전달된 값을 resolve 하거나 reject하는 Promise를 생성함

```
// Promise.resolve
const resolvedPromise = Promise.resolve([1, 2, 3]);
resolvedPromise.then(console.log); // [ 1, 2, 3 ]

// 위의 예제를 메소드 없이 만들면 아래와 같음
const resolvedPromise = new Promise(resolve => resolve([1, 2, 3]));
resolvedPromise.then(console.log); // [ 1, 2, 3 ]

// Promise.reject
const rejectedPromise = Promise.reject(new Error('Error!'));
rejectedPromise.catch(console.log); // Error: Error!

// 위의 예제를 메소드 없이 만들면 아래와 같음
const rejectedPromise = new Promise((resolve, reject) => reject(new Error('Error!')));
rejectedPromise.catch(console.log); // Error: Error!

```

- Promise.all
  전달받은 모든 프로미스를 병렬로 처리하고, 모든 프로미스의 처리가 종료될 때까지 기다린 후 아래와 같이 모든 처리결과를 resolve 하거나 혹은 reject 함 - 모든 프로미스의 처리 성공 -> 각각의 프로미스가 resolve한 처리 결과를 배열에 담아 resolve하는 새로운 프로미스 반환함. 첫 번째 프로미스가 resolve한 결과부터 차례대로 배열에 담기 때문에 **처리 순서가 보장됨** - 프로미스의 처리가 하나라도 실패하면 가장 먼저 실패한 프로미스가 reject한 에러를 reject하는 새로운 프로미스를 즉시 반환함

- Promise.race
  가장 먼저 처리된 프로미스가 resolve한 처리 결과를 resolve하는 새로운 프로미스를 반환함.

```
Promise.race([
  new Promise(resolve => setTimeout(() => resolve(1), 3000)), // 1
  new Promise(resolve => setTimeout(() => resolve(2), 2000)), // 2
  new Promise(resolve => setTimeout(() => resolve(3), 1000))  // 3
]).then(console.log) // 3
  .catch(console.log);
```

에러가 발생한 경우 Promise.all과 동일하게 가장 먼저 실패한 프로미스가 reject한 에러를
reject하는 새로운 프로미스 즉시 반환함

```
Promise.race([
  new Promise((resolve, reject) => setTimeout(() => reject(new Error('Error 1!')), 3000)),
  new Promise((resolve, reject) => setTimeout(() => reject(new Error('Error 2!')), 2000)),
  new Promise((resolve, reject) => setTimeout(() => reject(new Error('Error 3!')), 1000))
]).then(console.log)
  .catch(console.log); // Error: Error 3!

```

프로미스를 사용하면 콜백헬을 피할 수 있지만 프로미스 체이닝 방식을 사용해 코드가 길어질 수록 가독성을 해칠 수 있음. 그러나 async/await 구문을 사용하면 비동기 코드를 마치 동기 코드처럼 작성할 수 있음. 이는 가독성을 향상시키고 개발자가 비동기 로직을 더 쉽게 이해하고 디버깅할 수 있도록 도와줌.

**참고**

- https://poiemaweb.com/es6-promise
- https://ko.javascript.info/promise-basics
- https://joshua1988.github.io/web-development/javascript/promise-for-beginners/
