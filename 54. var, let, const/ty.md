# var, let, const의 차이

## scope

변수를 사용할수 있는 위치의 범위 를 말한다.

## var

var는 함수 내에서 (함수스코프) 선언 했을 경우 지역번수로 활용되며 이외의 범위에서 는 전역변수로 활용된다.

```js
var tester = "hey hi";

function newFunction() {
  var hello = "hello";
}
console.log(hello); // error: hello is not defined
```

hellos는 함수 밖에서 사용할수 없는 지역변수 이기 때문에 에러가 난다.

### var 재선언되고 업데이트 될수 있다.

재선언 되어도 에러가 나지 않는다.

```js
var greeter = "hey hi";
var greeter = "say Hello instead";
```

### var의 호이스팅

```js
var greeter;
console.log(greeter); // greeter is undefined
greeter = "say hello";
```

var 변수는 범위 내에서 맨위로 올려지고, 값은 undefined로 초기화 된다. 그래서 할당되기 전에 콘솔을 찍어도 에러가 나지 않는다.

## let

var의 문제점을 보안하기 위해 let을 사용한다.

let은 블록 범위의 변수 사용은 지역변수가 된다.(해당븡록내에서만 사용이 가능하다.)

```js
let greeting = "say Hi";
let times = 4;

if (times > 3) {
  let hello = "say Hello instead";
  console.log(hello); // "say Hello instead"
}
console.log(hello); // hello is not defined
```

### let은 업데이트가 될수 있지만 재선언 불가능,

let 변수는 범위 내에서 다시 선언할 수 없다.

```js
let greeting = "say Hi";
let greeting = "say Hello instead"; // error: Identifier 'greeting' has already been declared
```

그러나 동일한 변수가 다른 범위 내에서 정의된다면, 에러는 더 이상 발생하지 않습니다.
서로 다른 범위를 가지므로 서로 다른 변수로 취급되기 때문 이다.

```js
let greeting = "say Hi";
if (true) {
  let greeting = "say Hello instead";
  console.log(greeting); // "say Hello instead"
}
console.log(greeting); // "say Hi"
```

### let의 호이스팅

let 선언도 var선언 처럼 맨 위로 끌어올려집니다. undefined(정의되지 않음)으로 초기화되는 var와 다르게 let의 키워드는 초기화되지 않습니다.
선언 이전에 let 변수를 사용하려고 시도한다면 Reference Error(참조 오류)가 발생할 것입니다.

## const

const로 선언된 변수는 일정한 상수 값을 유지한다.
let 선언처럼 const 선언도 선언된 블록 범위 내에서만 접근 가능합니다.

### const는 업데이트, 재선언 불가능

const로 선언된 변수의 값이 해당 범위 내에서 동일하게 유지됨을 의미한다.업데이트하거나 다시 선언할 수가 없다.

```js
const greeting = "say Hi";
greeting = "say Hello instead"; // error: Assignment to constant variable.

const greeting = "say Hi";
const greeting = "say Hello instead"; // error: Identifier 'greeting' has already been declared
```

모든 const 선언은 선언하는 당시에 초기화되어야 한다.
그러나 const 개체는 업데이트할 수 없지만, 개체의 '속성'은 업데이트할 수 있다.

```js
const greeting = {
  message: "say Hi",
  times: 4,
};

greeting.message = "say Hello instead";

// error
greeting = {
  words: "Hello",
  number: "five",
}; // error:  Assignment to constant variable.
```

햐

### const 의 호이스팅

let과 마찬가지로 const 선언도 맨 위로 끌어올려지지만, 초기화되지는 않는다.

## 마무리

- var 선언은 전역 범위 또는 함수 범위이며, let과 const는 블록 범위이다.
- var 변수는 범위 내에서 업데이트 및 재선언할 수 있다. let 변수는 업데이트할 수 있지만, 재선언은 할 수 없다. const 변수는 업데이트와 재선언 둘 다 불가능하다.
- 세 가지 모두 최상위로 호이스팅된다. 하지만 var 변수만 undefined(정의되지 않음)으로 초기화되고 let과 const 변수는 초기화되지 않는다.
- var와 let은 초기화하지 않은 상태에서 선언할 수 있지만, const는 선언 중에 초기화해야한다.
