# closure 설명해주세요.

## closure

어떤 함수 A에서 선언한 변수 a를 참조하는 내부함수 B를 외부로 전달할 경우 A의 실행 컨텍스트가 종료된 이후에도 변수 a가 사라지지 않는 현상을 말함

// 예시

```
var outer = function () {
    var a = 1;
    var inner = function () {
        return a++
    };
    return inner;
}

var outer2 = outer()
console.log(outer2()) // 2
console.log(outer2()) // 3

```

위 코드에서 16번째 줄에서 outer의 실행컨텍스트가 종료됨
outer 함수의 실행이 종료된 상태인에 어떻게 outer의 렉시컬 환경에 접근할 수 있을까?
바로 가비지 컬렉터가 동작하는 방식 때문에 접근할 수 있는 것임. 가비지 컬렉터는 어떤 값을 참조하는 변수가 하나라도 있으면 그 값은 수집 대상에 포함시키지 않음
그래서 위의 예시에서 outer 함수가 inner 함수를 리턴하기 때문에 외부함수인 outer가 종료되더라도 내부함수인 inner는 언젠가 outer2를 실행함으로써 호출될 가능성이 있음. 그래서 가비지 컬렉팅 대상에서 제외된 것임

이처럼 함수의 실행 컨텍스트가 종료된 후에도 렉시컬 환경이 가비지 컬렉터의 수집대상에서 제외되는 경우는 위의 예시와 같이 지역변수를 참조하는 내부함수가 외부로 전달된 경우가 유일함
여기서 외부로 전달된다는 것의 의미는 리턴뿐만 아니라 setInterval.setTimeout이나 eventListener가 있음

## 클로저와 메모리 관리

**메모리 누수**
개발자의 의도와 달리 어떤 값의 참조 카운트가 0이 되지 않아 가비지 컬렉터의 수거 대상이 되지 않는 경우

### 메모리 누수 관리 방법

클로저는 어떤 필요에 의해 의도적으로 함수의 지역변수를 메모리를 소모하도록 함으로써 발생함. 참조 카운트를 0으로 만들어주면 소모되었던 메모리가 가비지 컬렉터에 의해 다시 회수됨
그럼 참조카운트를 어떻게 0으로 만들까? 바로 식별자에 참조형이 아닌 기본형 데이터(보통 null이나 undefined)를 할당하면 됨

// 예시

```

var outer = (function () {
    var a = 1;
    var inner = function () {
        return a++
    };
    return inner;
})()

console.log(outer())
console.log(outer())
outer = null; // outer 식별자의 inner함수 참조를 끊음

```

## 클로저가 사용되는 상황

1. 캡술화 & 은닉화: 클로저를 사용하면 함수 외부에서 접근할 수 없는 변수들을 함수 내부에서만 사용할 수 있는 범위를 설정할 수 있음. 이를 통해 코드의 안전성과 가독성을 높일 수 있음
2. 상태 유지: 클로저는 함수 외부의 값을 내부에서 참조하고 변경할 수 있음. 이를 이용하여 반복적으로 상태 변화를 유지하고 추적할 수 있는 기능을 구현할 수 있음
3. 고차 함수와 함께 사용: 클로저는 다른 함수의 인자로 전달될 수 있는 특징이 있음. 이를 다른 프로그래밍 기법과 조합하여 고차 함수(High-order functions)를 구현할 수 있음
4. 비동기 작업에서 변수 참조: 클로저를 사용하면 비동기 작업 완료 시점에 변수의 값을 사용하거나 변경할 수 있음. 이를 이용하여 간결한 콜백 함수를 작성할 수 있음

// 1번 예시

```
function createCounter() {
  let count = 0;
  return function() {
    count += 1;
    return count;
  };
}

const counter = createCounter();
console.log(counter()); // 1
console.log(counter()); // 2

```

createCounter 함수 내부에 있는 변수 count를 캡슐화함
따라서 상수 outer에 저장된 클로저는 count에 직접 접근할 수 없음
함수 내부에서만 직접 count의 값을 바꿀 수 있음

// 2번 예시

```
function createPerson(name, age) {
  return {
    getName() {
      return name;
    },
    getAge() {
      return age;
    },
    setAge(newAge) {
      age = newAge;
    }
  };
}

const person = createPerson("John Doe", 30);
console.log(person.getName()); // "John Doe"
console.log(person.getAge()); // 30

person.setAge(31);
console.log(person.getAge()); // 31

```

createPerson 함수에서 클로저를 이용하여 name과 age 변수를 캡슐화하고 있음.
여기서 클로저를 통해 개체의 메서드들이 사람 정보를 저장하고 있는 내부 상태를 유지하며 변경할 수 있음

// 3번 예시

```
function makeAdder(x) {
  return function(y) {
    return x + y;
  };
}

const add5 = makeAdder(5);
const add10 = makeAdder(10);

console.log(add5(3)); // 8
console.log(add10(3)); // 13

```

makeAdder 함수는 다른 함수(y에 대한 함수)를 반환하는 고차 함수임.
이때 반환된 함수는 인자 x의 값을 참조하여 계산을 수행하는데 이때 클로저를 사용하여 x값을 기억하고 있음

// 4번 예시

```
function asyncFunction(callback) {
  setTimeout(function() {
    callback();
  }, 1000);
}

function createMessagePrinter(message) {
  return function() {
    console.log(message);
  };
}

const messagePrinter = createMessagePrinter("Hello, World!");
asyncFunction(messagePrinter); // 1초 후에 "Hello, World!" 출력

```

createMessagePrinter 함수에서 클로저가 message 변수를 캡슐화함.
setTimeout의 콜백 함수로 전달되는 클로저는 비동기 작업이 완료된 후에 message 변수를 참조해야 하는데 클로저의 특성 덕분에 그 값을 제대로 출력할 수 있음.
위의 예시들에서 클로저는 변수를 캡슐화하고 상태를 유지하는 데 사용되며 고차 함수와 비동기 작업에서 코드의 가독성과 안전성을 높이는 역할을 함.

---

클로저는 함수 A에서 선언한 변수 a를 참조하는 내부함수 B를 외부로 전달할 경우 A의 실행 컨텍스트가 종료된 이후에도 변수 a가 사라지지 않는 현상을 말합니다

```
var outer = function () {
    var a = 1;
    var inner = function () {
        return a++
    };
    return inner;
}

var outer2 = outer()
console.log(outer2()) // 2
console.log(outer2()) // 3

```

위의 예시에서 outer 함수의 실행 컨텍스트는 174번줄에서 끝나는데 어떻게 outer 함수의 실행 컨텍스트에 접근할 수 있는 걸까요? 바로 가비지 컬렉터의 동작 방식 때문입니다. 가비지 컬렉터는 어떤 값을 참조하는 대상이 하나라도 있으면 가비지 컬렉팅 대상에서 제외합니다
예시에서 outer 함수가 inner 함수를 리턴하기 때문에 외부함수인 outer가 종료되더라도 내부함수인 inner는 언젠가 outer2를 실행함으로써 호출될 가능성이 있기 때문에 가비지 컬렉팅 대상에서 제외된 것입니다
이처럼 실행 컨텍스트가 종료되고 나서도 렉시컬 환경이 가비지 컬렉팅 대상에서 제외되는 경우는 위의 예시처럼 지역 변수를 참조하는 내부함수가 외부로 전달(리턴)되는 경우입니다

또한 클로저는 필요에 의해서 함수의 지역변수를 메모리에 저장함으로써 발생합니다 이때 개발자의 의도와 다르게 계속 참조하지 않아도 되는 경우인데도 참조되는 경우 메모리 누수가 일어날 수 있습니다
따라서 메무리 누수를 막기 위해서는 필요없는 참조를 끊으면 되는데 이는 식별자에 참조형 데이터가 아닌 기본형 데이터(null, undefined)를 할당하면 됩니다

또한 클로저를 사용하면 정보 은닉화가 가능해서 외부에서 변수에 접근할 수 없기 때문에 코드의 안정성과 가독성을 높일 수 있습니다
그리고 클로저는 현재 상태를 기억하고 변경된 최신 상태를 유지할 수 있습니다 예를 들면 리액트의 useState로 클로저 구현이 가능합니다
