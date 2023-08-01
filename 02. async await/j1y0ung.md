# async/await 이란 무엇인지 설명해주실 수 있을까요?

async/ await란 지바스크립트의 비동기 처리 패턴 중 가장 최근에 나온 문법이다.
비동기 처리 방식인 콜백 함수와 프로미스의 단점을 보완하고 개발자가 읽기 좋은 코드를 작성할 수 있게 도와준다.
ES8에 해당하는 문법으로 Promise를 쉽게 사용 할 수 있게 해준다.

함수 선언 할 때 함수 앞 부분에 `async` 키워드를 붙여 사용하고 Promise의 앞부분에 `await`을 넣어주면 해당 프로미스가 끝날때까지 기다렸다가 다음 작업을 수행할 수 있다.

- async & awiat는 Promise 객체를 반환하며 then을 사용할 수 있다.

## async/await 등장 이유

async await가 생기기 이전에는 JavaScript의 비동기 처리 방식이 콜백 지욕을 야기 시켰ㄷ.ㅏ 코드가 복잡해지고 가독성이 떨어져 Promise가 등장했지만, Promise로도 복잡한 비동기 처리를 해결하기 어려워 asyc/awiat가 등장했다.

## 예외처리

try catch를 통해 예외러치를 할 수 있다. ( Promise에서 에러처리를 위해 .catch()를 사용한것과 동일하다)

## Top-level await

ES2022 전에는 await는 async와 같이 사용해야했는데 업데이트 되며 await 단독 사용이 가능해졌다.

- 모듈 최상위 수준에서 await를 사용이 가능해졌다.
- top-level await는 모듈에서만 사용 가능하며 전역공간에서느 사용할 수 없다.
