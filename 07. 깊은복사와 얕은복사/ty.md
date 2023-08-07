# 깊은 복사 와 얕은 복사 ?

## 데이터 타입

### 1. 원시값(기본형 - primitive)

변수에 원시값을 저장하면 변수의 메모리 공간에 실제 데이터 값이 저장된다. 할당딘 변수를 조작하려고 하면 저장된 실제 값이 조작된다.

- number
- string
- boolean
- null
- undefined
- symbol(es6 추가)

#### 원시값을 복사한 경우

원시값을 저장한 변수는 실제 데이터 값이 저장되는데, 변수(b)의 값을 변경해도 변수(a)의 값이 변경되지 않는다.

```js
let a = 12;
let b = a;

b = 22;

console.log(a); //12
console.log(b); //22
```

### 2. 참조값(참조형 - reference)

변수에 객체를 저장하면 독맂벅인 메모리 공간에 값을 저장하고, 변수에 저장된 메모리 공간의 참조(위치 값)를 저장하게 된다.
그래서 할당된 변수를 조장하는 것은 사실 객체 자체를 조작하는 것이 나닌해당 객체의 참조를 조작하는 것이다.

- object
- array
- function
- date
- RegExg

#### 참조값을 복사한 경우

참조값을 변수에 저장할 때는 독립적인 메모리 공간에 실제 값을 저장하고
변수에는 실제 값이 아닌 **실제값을 담고 있는 메모리 위치 (주소)값을 저장**한다.
때문에 참조값을 원시값처럼 복사하면, **메모리 주소값을 복사하기에 값을 변경하면 같이 변경된다.**

```js
const a = { name: "maru", age: 12 };
const b = a; //{name:'maru', age:12}
b.name = "sunza";

// b의 값을 변경했지만 a의 값도 영향을 받는다.
conosole.log(a);{ name: "sunza", age: 12 };
console.log(b)
```

### 3. 깊은 복사와 얕은 복사 의 차이

1. 자료형의 깊은 복사
   원시값을 복사할때 그 값은 또다른 독립적인 메모리 공간에 할당하기 때문에 복사를 하고 값을 수정해도 기존 원시값을 저장한 변수에는 영향을 끼치지 않는다.
   **이처럼 실제 값을 복사 하는 것을 깊은 복사**라고한다 (하지만 자료형의 깊은 복사 이다.)

2. 참조형의 얕은 복사
   참조값을 복사할 때는 변수가 객체의 참조를 가리키고 있기 때문에 복사된 변수 또한 객체가 저장된 메모리 공간의 참조를 가리키고 있다.
   그래서 복사를 하고 객체를 수정하면 두 변수는 똑같은 참조를 가리키고 있기 때문에 기존 객체르 저장한 변수에 영향을 끼친다.
   **이처럼 객체의 참조값(주소값)을 복사하는것을 얕은 복사**라 한다.

> 얕은 복사는 참조 값틀 복사하고, 깊은 복사는 객체의 실제 값을 복사한다.

## 객체의 얕은 복사와 깊은 복사

<img src="https://blog.kakaocdn.net/dn/BHMmS/btq7SLCFzf3/wDY82NjsXrkUf0vOfocj2k/img.png">

### 1. 얕은 복사

#### Array.prototype.slice()

start부터 end 인덱스까지 기존 배열에서 추출하여 새로운 배열을 리턴하는 메소드 입니다. 만약 start와 end를 설정하지 않는다면, 기존 배열을 전체 얕은 복사한다.

```js
const original = [
  {
    a: 1,
    b: 2,
  },
  true,
];
const copy = original.slice();

console.log(JSON.stringify(original) === JSON.stringify(copy)); // true

// 복사된 배열에만 변경.
copy[0].a = 99;
copy[1] = false;

console.log(JSON.stringify(original) === JSON.stringify(copy)); // false

console.log(original);
// [ { a: 99, b: 2 }, true ]
console.log(copy);
// [ { a: 99, b: 2 }, false ]
```

배열 안에 객체를 수정하고자 할 경우 얕은 복사를 수행하는 것을 볼 수 있다. 하지만 원시값은 기본적으로 깊은 복사라 기존 변수에 있는 값과는 다른 값을 도출하는 것을 볼 수 있다.

#### Object.assign()

메소드의 첫 번째 인자로 빈 객체를 넣어주고 두 번째 인자로 복사할 객체를 넣어주면 된다.

```js
const object = {
  a: "a",
  number: {
    one: 1,
    two: 2,
  },
};

const copy = Object.assign({}, object);

copy.number.one = 3;

console.log(object === copy); // false
console.log(object.number.one === copy.number.one); // true
```

object 와 copy 를 비교 하면 참조 값이 달라 false 뜬다.
하지만 assign은 깊은 복사가 아니므로 2차원 객체 까지 복사가 이루어 지지 않고 결국 얕은 복사가 이루어졌다. object.number.one 과 copy.number.one 는 같은 참조 값을 가리키고 있다.

이것이 assign 의 한계 이다.

#### spread 연산자

```js
const object = {
  a: "a",
  number: {
    one: 1,
    two: 2,
  },
};

const copy = { ...object };
copy.number.one = 3;

console.log(object === copy); // false
console.log(object.number.one === copy.number.one); // true
```

### 1. 깉은 복사

> 깊은 복사된 객체는 객체 안에 객체가 있을 경우에도 원본과의 참조가 완전히 끊어진 객체를 말한다.

#### JSON.parse && JSON.stringfy

JSON.stringify()는 객체를 json 문자열로 변환하는데 이 과정에서 원본 객체와의 참조가 모두 끊어진다.
객체를 json 문자열로 변환 후, JSON.parse()를 이용해 다시 원래 객체(자바스크립트 객체)로 만들어 준다.

- 단점 : 가장 간단하고 쉽지만 다른 방법에 비해 느리다는 것과 객체가 function일 경우, undefined로 처리한다는 것

```js
const object = {
  a: "a",
  number: {
    one: 1,
    two: 2,
  },
  arr: [1, 2, [3, 4]],
};

const copy = JSON.parse(JSON.stringify(object));

copy.number.one = 3;
copy.arr[2].push(5);

console.log(object === copy); // false
console.log(object.number.one === copy.number.one); // false
console.log(object.arr === copy.arr); // false

console.log(object); // { a: 'a', number: { one: 1, two: 2 }, arr: [ 1, 2, [ 3, 4 ] ] }
console.log(copy); // { a: 'a', number: { one: 3, two: 2 }, arr: [ 1, 2, [ 3, 4, 5 ] ] }
```

#### 재귀 함수를 구현한 복사

- 단점 : 복잡함.

```js
function deepCopy(obj) {
  if (obj === null || typeof obj !== "object") {
    return obj;
  }

  let copy = {};
  for (let key in obj) {
    copy[key] = deepCopy(obj[key]);
  }
  return copy;
}

const obj = {
  a: 1,
  b: {
    c: 2,
  },
  func: function () {
    return this.a;
  },
};

const newObj = deepCopy(obj);

newObj.b.c = 3;
console.log(obj); // { a: 1, b: { c: 2 }, func: [Function: func] }
console.log(obj.b.c === newObj.b.c); // false
```

deepCopy 함수의 인수로 obj 객체를 넣었다. 인수값이 객체가 아닌 경우는 그냥 반환하며, 객체인 경우 객체의 값 만큼 루프를 돌며 재귀를 호출하여 복사된 값을 반환한다. 복사된 newObj 객체를 보면 2차원 객체의 값도 깊은 복사가 이루어 졌으며, 객체의 함수도 제대로 표현되는 것을 확인할 수 있다.

#### Lodash 라이브러리 사용

라이브러리를 사용하면 더 쉽고 안전하게 깊은 복사를 할 수 있다.

- 단점 : 라이브러리 설치, 코딩테스트에는 사용할수 없다.

```js
onst deepCopy = require("lodash.clonedeep")

const object = {
  a: "a",
  number: {
    one: 1,
    two: 2,
  },
  arr: [1, 2, [3, 4]],
};

const copy = deepCopy(object);

copy.number.one = 3;
copy.arr[2].push(5);

console.log(object === copy); // false
console.log(object.number.one === copy.number.one); // false
console.log(object.arr === copy.arr); // false

console.log(object); // { a: 'a', number: { one: 1, two: 2 }, arr: [ 1, 2, [ 3, 4 ] ] }
console.log(copy); // { a: 'a', number: { one: 3, two: 2 }, arr: [ 1, 2, [ 3, 4, 5 ] ] }
```
