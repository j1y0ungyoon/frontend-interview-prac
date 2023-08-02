# Hoisting이란 무엇인지 설명해주실 수 있을까요?

호이스팅(hoisting)은 변수 선언과 함수 선언을 코드의 맨 위로 끌어올려지는 현상을 말하며 어느 라인 위치에 코드를 선언해도, 실행 되기전 최상단으로 끌어올려지고 실행되게 된다.

## 발생원인

변수생성과 초기화 의 작업이 분리돼서 진행 되기 때문이다. -> 함수 선언과 변수 선언이 위로 호이스팅 된다.

```js
function a() {
  return "a";
}

var b;
var c; //함수 선언은 위로 강제로 끌어져 올라온다. (호이스팅)

console.log(a());
console.log(b());
console.log(c());

b = function bb() {
  return "bb";
};
c = function () {
  return "c";
};
```

a()는 실행되지만 아직 변수 b와 c안에 넣어지지 않는 형태이기 때문에, 함수 선언문이 아니라서 b와 c는 그냥 변수 취급 때문에 실행 오류가 뜬다.

## 함수의 호이스팅

함수를 정의 하는 방법으로 함수표현식과 함수 선언식 두가지가 있다.

```js
//함수 선언식
function add(x, y) {
  return x + y;
}

// 함수 표현식
const add = function (x, y) {
  return x + y;
};
```

**함수 표현식은 함수를 변수에 할당하므로 유연성이 높아 호이스팅이 강제적으로 되지 않는다.**

### 함수 표현식을 권장하는 이유

함수 선언문은 함수가 가장 상단으로 끌어올려지기 때문에 에러가 나지않는다.
그러나 함수 표현식을 사용하게 되면 변수가 먼저 호이팅이 되기때문에 에러가 난다.

#### 함수 선언문

```js
console.log(add(2, 3));

function add(x, y) {
  return x + y;
}

console.log(add(3, 4));
```

모든 콘솔이 작동한다.

#### 함수 표현식

```js
console.log(add(2, 5)); //error

var add = function (x, y) {
  return x + y;
};
```

## 변수의 호이스팅

**변수를 선언 할때는 var키워드는 금지하고 let / const 키워드를 사용하는 것을 권장한다.
왜냐하면 let, const로 변수 선언하면 블록 스코프를 가지게 되므로, 호이스팅이 발생하지 않는다.**

### 변수의 생성과정

1. 선언 - 2. 초기화 - 3. 할당

- var : 1. 선언 및 초기화 - 2. 할당
- let : 1. 선언 - 2.초기화 3. 할당
- const : 1.선어 + 초기화 + 할당

### var/let/const 차이점

1. 중복 선언 가능 여부

- var : 중복선언 가능 - 기존에 선언해둔 변수의 존재를 까먹고 값을 재할당하는 실수 발생
- const, let : 중복 선언 불가능 - 선언한 변수를 다시 선언 할 경우 에러 가 난다. ex) let test = 123, let test = 일이삼 //에러

2. 재할당 가능 여부

- var , let : 재할당 가능 - 변수 선언 및 초기화 이후 반복해서 다른 값을 재할당
- const : 값의 재할당 불가능한 상수 - 처음에 선언 및 초기화 하고 다른 값을 재할당 할수 없다.

3. 변수 스코프 유효 범위
   스코프란 유효한 참조 범위를 말한다.
   var 키워드는 함수 내부에서 선언된 변수 만 지역 변수로 인정하는 **함수 레벨 스코프** 이다. (함수)
   let, const는 모든 블록 내부에서 선언된 변수까지 지역 변수로 인정하는 **블록 레벨 스코프**이다. (if, for while, try/catch)

**4. 변수의 호이스팅 방식**

- var : 변수 호이스팅 발생

```js
console.log(a); // undefined
var a = 10;
console.log(a); // 10

//자바스크립트 엔진
var a = undefined; //호이스팅
console.log(a); // undefined
var a = 10;
console.log(a); // 10
```

자바스크립트 엔진이 1.- 변수를 선언하고 2. - undefined로 초기화 해두어서 에러가 나지 않는다.

- let, const : 변수 호이스팅 발행 하지만 다른 방식으로 작동

```js
console.log(a); //ReferenceError: a is not defined
let a = 10;

//자바스크립트엔진
let a; // 선언만 하고 정의는 하지 않음. var은 정의 하지않아도 undefined가 들어감

console.log(a); //ReferenceError: a is not defined
a = 10;
console.log(a); // 10
```

변수 호이스팅이 발생하지 않는 것 처러 보이지만 1. - 변수 선언만 해두며 2. 초기화 코드 실행 과정에서 변수 선언문을 만났을 때 수행한다.
값을 참조할 수 없어서 호이스팅이 발생하지 않는 것처럼 보이는 것이다.

**TDZ (Temporal Dead Zone)**

```js
let a = 10; // 전역변수 a선언

if (true) {
  console.log(a); // ReferenceError: a is not defined
  let a = 20; // 지역변수 a 선언
}
```

전역 변수 a가 있음에도 지역변수 a 앞에서 console.log()로 참조시 에러가 발생한다.
이유는 TDZ 구간이 만들어 졌기 때문에 변수 선언과 초기화 사이에 일시적으로 변수 값을 참조할 수없는 구간이 생긴다.(TDZ) - 지역변수가 전역변수보다 우선순위를 갖는다.

5.전역객체 프로퍼티 여부

- var : 전역객체의 프로퍼티 이다. (window.a)
- let, const : 전역객체 프로퍼티가 아니다. (window.a => 아님)
