# 동등 연산자와 일치 연산자

## 동등 연산자 (==)

동등연산자로 피연산자를 비교할 때는 js 엔진에 의해 암묵적인타입 변환이 이루어지는데, 좌항과 우항이 **타입이 다르더라도 값이 같으면 true로 반환**한다.

```js
const a = 1;
const b = "1";

console.log(a == b); //true
```

### null undefined 비교

- 동등연산자는 null과 undefined 두 값을 동일하게 취급하며 두 값을 제외한 다른 값들에 대해서는 항상 false를 반환한다.
- `값이 없다`

```js
console.log(null == undefined); //true
```

## 일치연산자 (===)

일치 연산자는 좌항과 우항의 피연산자가 **값과 타입이 모두 같을 경우에만 true를 반환**한다.

```js
const a = 1;
const b = 1;
const c = "1";

console.log(a === b); // true
console.log(a === b); // false
```

### null undefined 비교

- 일치연산자는 값과 타입을 비교 하여 null과 undefined의 테이터 타입을 다르게 본다.
- `값이 없다`

```js
console.log(null === undefined); //false
```

### NaN

- 유효하지 않는 숫자를 나타내는 값
- NaN 여부를 판단하기 위해서는 isNaN() 내장함수를 사용해야한다.

```js
console.log(NaN === NaN); //false
console.log(isNaN(NaN)); //true

console.log(0 === -0); // true
```
