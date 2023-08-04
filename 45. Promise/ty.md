# promise 란?

## 비동기 처리란?

현재 실행중인 작업과는 별도로 다른 작업을 수행하는 것을 말한다.

## 콜백함수

콜백함수란 비동기 작업이 완료되면 호출되는 함수의 의미로서, 비동기 함수의 배개변수로 함수 객체를 넘기는 기법을 말한다.
그래서 함수 내부에서 함수 호출을 통해 비동기 작업의결과를 받아서 인자로 주면 이를 통해 후속 처리 작업을 수행할 수 있다.

### 콜백지옥

여러개의 비동기 작업을 순차적으로 수행해야 할때 콜백함수가 중접되어 코드의 깊이가 깊어지고 그로인해 가독성이 떨어져 코드 흐름 파악하기가 어려워져 유지 보수에도 힘들어진다.

이러한 콜백 지옥을 개선하기 위해 promise가 등장 하였다.

```js
function increaseAndPrint(n, callback) {
  setTimeout(() => {
    const increased = n + 1;
    console.log(increased);
    if (callback) {
      callback(increased); // 콜백함수 호출
    }
  }, 1000);
}

increaseAndPrint(0, (n) => {
  increaseAndPrint(n, (n) => {
    increaseAndPrint(n, (n) => {
      increaseAndPrint(n, (n) => {
        increaseAndPrint(n, (n) => {
          console.log("끝!");
        });
      });
    });
  });
});
```

## 1.promise 객체

비동기 처리를 위해 쓰던 콜백함수의 콜백지옥을 개선하기 위해 promise가 생기면서 코드 가독성이 좋아 졌다.

promise 객체는 비동기 작업의 최종 완료 또는 실패를 Array나 Object 처럼 독자적인 객체라고 보면된다.

### 기본 사용법

Promise 객체를 생성하려면 new 키워드와 Promise 생성자 함수를 사용하면 된다.
promise 생성자 안에 두개의 매개 변수를 가진 콜백함수(executor 이라고 부름.)를 넣게 되는데

1. 성공했을 경우 (resolve)
2. 실패했을 경우 (reject)

```js
const myPromise = new Promise((resolve, reject) => {
  // 비동기 작업 수행
  const data = fetch("서버로부터 요청 할 url");

  if (data) {
    resolve(data); // 요청이 성공하여 데이터가 있으면
  } else {
    resject("Error"); // 요청이 실패하여 데이터가 없다면
  }
});
```

### 프로미스 객체 처리

promise 객체는 비동기 작업이 완료된 이후에 작업결과에 따라 `.then()`과 `.catch`메서드 체이닝을 통해 성공과 실패에 대한 후속처리를 진행할수 있다.

**.then()**
처리가 성공하여 `resolve`를 호출하게 되면 바로 `.then`으로 이어 져 `then` 메서드 콜백 함수에서 성공에 대한 추가 처리를 진행한다.
`resolve()` 함수의 매개변수의 값이 `then` 메서드의 콜백 함수 인자로 들어가 `then` 메서드 내부에서 프로미스 객체 내부에서 다른 값을 이용 할수 있게 된다.

**.cath()**
처리가 실패하여 프로미스 객체 내부에서 `reject('error')`를 호출하게 되면, 바로 `.catch()`로 이어져`catch`메서드의 콜백 함수에서 성공에 대한 추가 처리를 진행한다.

```js
myPromise
  .then((value) => {
    // 성공적으로 수행했을 때 실행될 코드
    console.log("data", value); // 위에서 return resolve(data)의 data 값이 출력
  })
  .catch((error) => {
    // 실패했을 때 실행될 코드
    console.error(error); //  위에서 return reject('error'의 'error' 출력
  })
  .finally(() => {
    //성공하든 실패하든 무조건 실행될 코드
    //
  });
```

## 2.프로미스 함수 등록

프로미스 객체를 변수에 바로 할당하는 방식도 사용할수 있지만, 보통은 별도로 함수로 감싸서 사용하는 것이 일반적이다.

```js
// 프로미스 객체를 반환하는 함수 생성
function myPromise() {
  return new Promise((resolve, reject) => {
    if ("성공조건") {
      resolve("성공결과값");
    } else {
      reject("에러값");
    }
  });
}

// 프로미스 객체를 방환하는 함수 사용
myPromise()
  .then((result) => {
    //성공시 실행할 함수
  })
  .catch((error) => {
    //실패시 실해할 콜백함수
  });
```

함수로 만들고 그 함수를 호풀하면 프로미스 생성자를 return 함으로서, 곧바로 생성된 프로미스객체를 함수 반환값으로 얻어 사용하는 기법

**함수로 만드는 이유** (프로미스 팩토리 함수 라고 불리기도 한다.)

1. 재사용성 : 반복되는 비동기 작업을 효율적으로 처리
2. 가독성 : 코드 구조가 명확해져, 비동기 작업의 정의와 사용을 분리하여 코드 가동성 높아짐
3. 확장성 : 함수로 만들면 인자를 전달하여 동적으로 비동기 작업을 수행할 수 있다. 여러개의 프로미스 객체를 반환하는 함수들을 연결하여 복잡한 비동기 로직을 구현 가능.

**라이브러리 프로미스**
대부분 자바스크립트의 비동기 라이브러리도 함수 형태로 프로미스 객체를 제공한다.
대표적으로 **fetch()메서드**는 메서드 내에서 프로미스 객체를 생성하여 서버로 부터 데이터를 가져오면 `resolve`하여 .`then()`으로 처리한다.

## 3.프로미스 3가지 상태

프로미스는 비동기 작업의 결과를 약속하는 것이다.
진행중,성공,실패 상태를 나타내는 것을 프로미스 상태(state)라고 부른다.

1. Pending(대기) : 처리가 완료되지 않은 상태(처리 진행중)
2. Fulfilled(이행) : 성공적으로 처리가 완료된 상태
3. Eejected(거부) : 처리가 실패로 끝난 상태

### 1. pending

아직 비동기 처리 로직이 완료되지 않은 상태

```js
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("처리 완료");
  }, 5000);
});
console.log(promise); // Pending (대기) 상태
```

### 2. Fulfilled

이행 상태라고 도 하며 비동기 처리 로직이 성공적으로 완료 된 상태
(위 코드 5초 뒤에 실행후 확인 하면 된다.)
이행된 상태로 변한 프로미스 객체를 바로 **체이닝된 .then() 메서드를 호출하여 처리 결과 값을 받을 수 있다.**

```js
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("처리 완료");
  }, 5000);
});
console.log(promise); // Pending (대기) 상태
```

### 3. Rejected 상태

프로미스 객체가 실패 한 상태

```js
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject("처리 실패");
  }, 5000);
});
```

실패 상태로 변한 프로미스 객체는 바로 체이닝된 .catch() 메서드를 호출하여 처리 실패에 대한 행동을 수행한다.

```js
promise.catch((error) => {
  console.log(error);
  console.log("실패에 대한 후속 조치...");
});
```

## 4. 프로미스 핸들러

성공/실패 결과를 `.then / .catch / .finally`핸들러를 통해 받아 다음 후속 작업을 수행할수 있다.(프로미스 상태에 따라 실행되는 콜백 함수로 보면 된다.)

1. .then() : 이행(완료)되었을 때 콜백함수를 등록하고, 새로운 프로미스를 반환
2. .catch() : 프로미스가 거부 됬을 때 실행할 콜백함수를 등록하고, 새로운 프로미스를 반환
3. .finally() : 프로미스가 이행 또는 거부 와 상관없이 실행할 콜백함수 등록하고, 새로운 프로미스 반환

### 프로미스 체이닝

프로미스 체이닝이란, 프로미스 핸들러를 연달아 연결하는 것을 말한다. 이렇게 하면 여러 개의 비동기 작업을 순차적으로 수행할 수 있다는 특징이 있다.

```js
function doSomething() {
  return new Promise((resolve, reject) => {
    resolve(100);
  });
}

doSomething()
  .then((value1) => {
    const data1 = value1 + 50;
    return data1;
  })
  .then((value2) => {
    const data2 = value2 + 50;
    return data2;
  })
  .then((value3) => {
    const data3 = value3 + 50;
    return data3;
  })
  .then((value4) => {
    console.log(value4); // 250 출력
  });
```

이런식으로 체이닝이 가능한 이유는 then 핸들러에서 값을 리턴하면, 그 반환값은 자동으로 프로미스 객체로 감싸져 반환되기 때문이다.
만일 연결된 이행 핸들러에서 중간에 오류가 있는 처리를 행한다면 예외처리를 함으로써 catch 핸들러에 점프하도록 설정할 수 있다.

```js
function doSomething(arg) {
  return new Promise((resolve, reject) => {
    resolve(arg);
  });
}

doSomething("100A")
  .then((value1) => {
    const data1 = value1 + 50; // 숫자에 문자를 연산

    if (isNaN(data1)) throw new Error("값이 넘버가 아닙니다");

    return data1;
  })
  .then((value2) => {
    const data2 = value2 + 50;
    return data2;
  })
  .catch((err) => {
    console.error(err);
  });
```

만일 catch 핸들러 다음으로 then 핸들러가 이어서 체이닝 되어 있다면, 에러가 처리되고 가까운 then 핸들러로 제어 흐름이 넘어가 실행이 이어지게 된다.

## 5. 프로미스 정적 메서드

프로미스 객체는 생성자 함수 외에도 여러가지 정적 메서드(static method)를 제공하며 **객체를 초기화 와 생성하지 않고도** 바로 사용할 수 있기 때문에 비동기 처리를 보다 효율적이고 간편하게 구현할수 있도록 해준다,
비동기 작업을 수행하지 않는 함수에서도 프로미스의 장점을 활용하고 싶은 경우에 유용하다.

### 1.Promise.resolve()

resolve() 상탤르 정적메서드로 한번에 호출할 수 있도록 현의 기능을 제공 해준다.
프로미스 정적 메서드를 이용하면 프로미스 객체와 전혀 연관없는 함수 내에서 필요에 따라 프로미스 객체를 반환하여 핸들러를 이용할 수 있도록 응용이 가능하다.

```js
// 프로미스 객체와 전혀 연관없는 함수
function getRandomNumber() {
  const num = Math.floor(Math.random() * 10); // 0 ~ 9 사이의 정수
  return num;
}

// Promise.resolve() 를 사용하여 프로미스 객체를 반환하는 함수
function getPromiseNumber() {
  const num = getRandomNumber(); // 일반 값
  return Promise.resolve(num); // 프로미스 객체
}

// 핸들러를 이용하여 프로미스 객체의 값을 처리하는 함수
getPromiseNumber()
  .then((value) => {
    console.log(`랜덤 숫자: ${value}`);
  })
  .catch((error) => {
    console.error(error);
  });
```

### 2.Promise.reject()

거부하는 Promise 객체를 반환

```js
// 주어진 사유로 거부되는 프로미스 생성
const p = Promise.reject(new Error("error"));

// 거부 사유를 출력
p.catch((error) => console.error(error)); // Error: error
```

### 3.promise.all()

배열, Map, Set에 포함된 여러개의 프로미스 요소들을 한꺼번에 비동기 작업을 처리해야 할때 굉장히 유용한 프로미스 정적 메소드이다.
모든 프로미스 비동기 처리가 이행(fulfilled) 될때까지 기다려서, 모든 프로미스가 완료되면 그때 then 핸들러가 실행하는 형태로 보면 된다.

```js
// 1. 서버 요청 API 프로미스 객체 생성 (fetch)
const api_1 = fetch("https://jsonplaceholder.typicode.com/users");
const api_2 = fetch("https://jsonplaceholder.typicode.com/users");
const api_3 = fetch("https://jsonplaceholder.typicode.com/users");

// 2. 프로미스 객체들을 묶어 배열로 구성
const promises = [api_1, api_2, api_3];

// 3. Promise.all() 메서드 인자로 프로미스 배열을 넣어, 모든 프로미스가 이행될 때까지 기다리고, 결과값을 출력
Promise.all(promises)
  .then((results) => {
    // results는 이행된 프로미스들의 값들을 담은 배열.
    // results의 순서는 promises의 순서와 일치.
    console.log(results); // [users1, users2, users3]
  })
  .catch((error) => {
    // 어느 하나라도 프로미스가 거부되면 오류를 출력
    console.error(error);
  });
```

### 4. promise.allSettled()

Promise.all() 메서드의 업그레이드 버전으로, 주어진 모든 프로미스가 처리되면 모든 프로미스 각각의 상태와 값 (또는 거부 사유)을 모아놓은 배열을 반환한다.

```js
// 1초 후에 1을 반환하는 프로미스
const p1 = new Promise((resolve) => setTimeout(() => resolve(1), 1000));

// 2초 후에 에러를 발생시키는 프로미스
const p2 = new Promise((resolve, reject) =>
  setTimeout(() => reject(new Error("error")), 2000)
);

// 3초 후에 3을 반환하는 프로미스
const p3 = new Promise((resolve) => setTimeout(() => resolve(3), 3000));

// 세 개의 프로미스의 상태와 값 또는 사유를 출력
Promise.allSettled([p1, p2, p3]).then((result) => console.log(result));
```

### 5. promise.any()

주어진 모든 프로미스 중 하나라도 완료되면 바로 반환하는 정적 메서드이다.
첫번째로 이행(fulfilled) 된 프로미스만을 취급하기 때문에 나머지 promise1과 promise3의 거부(rejected)는 무시되게 된다.

```js
const promise1 = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject("promise1 failed");
  }, 3000);
});

const promise2 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("promise2 succeeded");
  }, 2000);
});

const promise4 = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject("promise4 failed");
  }, 4000);
});

Promise.any([promise1, promise3, promise4])
  .then((value) => {
    console.log(value);
  })
  .catch((error) => {
    console.error(error); // AggregateError: All promises were rejected
    console.error(error.errors); // ["promise3 failed", "promise1 failed", "promise4 failed"]
  });
```

### 6. promise.race()

여러 개의 프로미스 중 가장 먼저 처리된 프로미스의 결과값을 반환
fulfilled(이행), rejected(실패) 여부 상관없이 무조건 처리가 끝난 프로미스 결과값을 반환 한다.

## 5. 프로미스 지옥

프로미스의 then() 메서드가 지나치게 체인되어 반복되면 코드가 장황해지고 가독성이 굉장히 떨어질 수 가 있다.
또한, catch 메서드가 마지막에 한 번만 사용되어 있기 때문에, 중간에 발생할 수 있는 에러나 예외 상황에 대응하기 어렵다.

극복하기 위해 나온 자바스크립트의 신세대 문법이 있는데 바로 **async/await 키워드**이다.
async/await 키워드를 사용하면 비동기 작업을 마치 동기 작업처럼 쓸 수 있어서 코드가 간결하고 가독성이 좋아지게 된다.

```js
try {
  const response = await fetch("");

  if (response.ok) {
    const users = await response.json();
    const logins = users.map((user) => user.login);
    const result = logins.join(", ");
    console.log(result);
  } else {
    throw new Error("Network Error");
  }
} catch (error) {
  console.error(error);
}
```
