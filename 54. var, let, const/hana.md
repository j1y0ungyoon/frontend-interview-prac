# var, let, const 에 대해 설명해주실 수 있을까요?

## var

- 중복 선언이 가능함
- 재할당도 가능함
- 함수 스코프
- 전역 변수로 선언한 경우 window객체의 속성이 됨
  (ex: var a = "hi";
  window.rr = 'hi'
  )
- 호이스팅될 때 undefined라는 값으로 초기화됨

## let

- 붕복선언 x
- 재할당 가능
- 블록 스코프
- 선언문 이전 접근 불가

## const

- 중복 선언 x
- 재할당 x
- 블록 스코프
- 선언문 이전 접근 불가
- 객체가 할당되는 경우 객체 안의 값을 바꿀 수 있음(새로운 값을 재할당하는 게 아니므로)
- 만약 객체의 불변성을 유지하고 싶다면 Object.freeze() 사용

## var, let, const 예시

1. 중복 선언 여부

```
var a = 8;
var a = 10;

console.log(a) // 10
```

var는 중복 선언이 가능하며, 맨 마지막에 재선언된 것으로 덮어씌워짐

let과 const의 경우 중복 선언이 불가능함

```
let a = 8;
let a = 10;

console.log(a) // Uncaught SyntaxError: Identifier 'a' has already been declared

const j = 0;
const j = 4;

console.log(j) // Uncaught SyntaxError: Identifier 'j' has already been declared

```

let과 const의 경우 식별자 a와 j가 이미 선언되어 있어서 문법 에러가 남

2. 재할당 여부

```
var q = 'hihi';
q = 1234;

console.log(q) // 1234


let b = 1223;
b = "hihi"
console.log(b) // "hihi"
```

var와 let의 경우 재할당이 가능함
하지만 const의 경우 상수이기 때문에 재할당이 불가능함

```
const uu = 1
uu = "pp"

// Uncaught TypeError: Assignment to constant variable.
```

const의 경우 재할당이 불가능 하므로 에러가 발생

3. 스코프

```
function a () {
    if(true) {
       var x = 1;
    }
    console.log(x) // (A)
}

a() // 1


function cc () {
    var x = 9;
    if(true) {
        var x = "hoho"
    }
    console.log(x) // "hoho"
}

cc();
```

var는 함수 스코프이기 때문에 함수 블록 안에 있는 var로 선언된 변수에 접근할 수 있음

첫 번째 예시의 경우 console.log(x)가 있는 부분(A)에서 x는 함수 스코프를 가지는 var 키워드로 선언되었고, var x는 함수 a 안에 선언되어 있기 때문에 x의 값에 접근할 수 있음. 따라서 1을 출력함
(let, const로 선언이 되어있었다면 에러가 났을 것임)

두 번째 예시의 경우 var 키워드로 선언된 변수는 함수 스코프를 가지므로 var x = 9 와 var x = "hoho" 는 같은 스코프(함수 스코프)안에 있다고 봄. 따라서 var x가 중복 선언이 되어 콘솔에는 맨 마지막 값인 "hoho"가 출력됨

let이나 const는 블록 스코프를 가지고 있기 때문에 let이나 const로 선언된 변수가 있는 블록 안에서만 유효함 (예시의 경우 x가 선언된 if문 안에서만 유효함)
따라서 let이나 const로 선언했을 때는 아래와 같이 에러가 남

```
function a () {
    if(true) {
       let x = 1;
    }
    console.log(x) // Reference Error: x is not defined
}

a()


function c () {
    if(true) {
       const x = 1;
    }
    console.log(x) // Reference Error: x is not defined
}

a()
```

4. 호이스팅

```
console.log(cat) // 에러가 아니라 undefined가 출력됨

var cat = 'rey';

console.log(cat) // rey

```

호이스팅이란 선언부가 제일 최상단으로 끌어올려진 것 같은 현상임

자바스크립트 엔진이 코드를 위에서 아래로 하나하나 읽어나가면서 environment record에 식별자와 식별자에 바인딩된 값을 기록함
자바스크립트 엔진이 코드를 읽어나가는 과정에서 선언해 둘 것이 있다면 먼저 선언해두는데, 선언하는 과정에서 실행한 실행 컨텍스트(위의 예시에서는 전역 컨텍스트) 안에 새로운 식별자 cat을 environment record에 저장함. cat은 var 키워드로 선언이 되어있기 때문에 undefined로 초기화를 함

```
console.log(cat) // reference error

let cat = 'rey';

console.log(cat) // rey

```

let이나 const로 선언한 경우 자바스크립트 엔진이 환경 레코드에 저장을 하긴 하지만 값을 초기화하지 않음
따라서 cat의 값을 참조할 수 없으므로 참조 에러가 발생함(자바스크립트 엔진이 cat의 값을 읽을 수 없기 때문)

즉, let이나 const로 선언했을 떄 선언 이전에 식별자를 참조할 수 없는 구역인 TDZ(temporal Dead Zone)가 발생함

요약해보자면
var 키워드로 변수를 선언한 경우 선언* 과 초기화* 가 동시에 일어남

let, const의 경우 undefined로 초기화하지 않음
따라서 값이 할당되기 전까지는 변수에 아무런 값이 담기지 않았기 때문에 값을 읽어올 수 없음
따라서 TDZ가 발생함 -> 선언 라인 이전에는 변수를 참조할 수없음

**선언**
메모리 공간을 확보하고 메모리 주소에 식별자와 연결
ex : { cat }

**초기화**
식별자에 암묵적으로 undefined 값 바인딩
ex : { cat : undefined }
