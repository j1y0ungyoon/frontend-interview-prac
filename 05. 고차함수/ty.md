# 고차 함수란?

함수를 전달인자(argument)또는 매개변수(parameter)로 받거나 함수를 리턴하는 함수를 말한다.

함수형 프로그래밍의 핵심이기도 하며, 자바스크립트를 함수형 프로그램ㅇ에 알맞는 언어로 만들어주는 특성이기도 하다.

> 함수형 프로그래밍이란?
> 함수를 다른 함수의 파라미터로 넘길 수도 있고 반환(return)값으로 함수를 받을 수도 있는 프로그래밍 형태를 말한다.

## 고차 함수

### 1. forEach()

- for 문을 대체하는 고차 함수
- 반복문을 추상화하여 구현된 메서드. 내부에서 주어진 배열을 순회하면서 연산을 수행

```js
const numberArr = [1, 2, 3, 4];
let total = 0;

numberArr.forEach((it) => {
  total += it;
});
console.log(total); //10
```

### 2. map()

- 순회하면서, 콜백함수에서 실행결과를 리턴한 값으로 이러우진 배열을 만들어 반화

```js
const numberArr = [1, 2, 3, 4, 5];
const numberMapArr = numberArr.map((item) => {
  return item % 2 === 0 ? "even" : "odd"; // 연산한 결과값을 넣어 배열 반환
});

console.log(numberMapArr); // ['odd', 'even', 'odd', 'even', 'odd']
```

> forEach()는 for문을 대체하여 사용하고 map()은 연산의 결과로 새로운 배열을 생성하고자할 때 사용된다.

### 3. find()

- 주어진 배열을 순회하면서 콜백 함수 실행의 반환 값이 true에 해당하는 첫번째 요소를 반환

```js
const numArr = [1, 3, 3, 5, 7];
const objArr = [
  { name: "Harry", age: 20 },
  { name: "Kim", age: 30 },
  { name: "Steve", age: 40 },
];

console.log(
  objArr.find((it) => {
    return it.age === 20;
  })
); //{ name: "Harry", age: 20 },

console.log(numberArr.find((item) => item === 3)); // 3
```

### 4. findIndex()

- 고차함수 finde()의 리턴값이 인덱스 이다.

```js
const objectArr = [
  { name: "Harry", age: 20 },
  { name: "Kim", age: 30 },
  { name: "Steve", age: 40 },
];

console.log(
  objectArr.findIndex((it) => {
    return it.age === 20;
  })
); //0

console.log(objectArr.findIndex((it) => it.name === "kim")); //1
```

### 5. filter()

- 주어진 배열을 순회하면서 콜백 함수의 반환값이 true에 해당하는 요소로만 구성된 새로운 배열 을 생성하여 반환.

```js
const numArr = [1, 2, 3, 4, 5];

const numFilterArr = numArr.filter((it) => {
  return it % 2 === 0;
});

console.log(numFilterArr); // [2,4]
```

### 6. reduce()

- 콜백 함수의 실행된 반환값(initialValue)을 전달 받아 연산의 결과값이 반환
- 첫번째 인자 (accumulator)서 부터 시작해서 배열 값인 두번째 인자 (currentValue)을 순회하여 accumulator += currentValue 을 실행
- forEach, map, filter기능을 reduce로 모두 구현해서 쓸순 있어 고차함수의 부모라고 불림

```js
const numArr = [1, 2, 3, 4, 5];

const sum = numArr.reduce((pre, cur, curIdx, arr) => {
  console.log(
    "Current Index: " +
      curIdx +
      " / Previous Value: " +
      pre +
      " / Current Value: " +
      cur
  );

  return prev + cur;
});

console.log("Sum" + sum);
/*
Current Index: 0 / Previous Value: 0 / Current Value: 1
Current Index: 1 / Previous Value: 1 / Current Value: 2
Current Index: 2 / Previous Value: 3 / Current Value: 3
Current Index: 3 / Previous Value: 6 / Current Value: 4
Current Index: 4 / Previous Value: 10 / Current Value: 5
Sum: 15
*/
```

### 7. sort()

- 배열 정렬
- 복사본이 아닌 원 배열이 정렬됨
- 콜백 함수를 통해 배열의 원소들을 어느 기준으로 정렬할지 지정해야한다.

```js
var arr = ["red", "blue", "green", "white", "black"];
arr.sort(); // [ 'black', 'blue', 'green', 'red', 'white' ]
```

- 문자를 정렬할때는 문제가 없지만, 숫자를 정렬하는 경우에도 ABC 순으로 정렬이 되기 때문에 콜백함수를 넣어 조작 해야 한다.

```js
var arr2 = [1, 2, 3, 10, 50, 70, 8, 4];
arr2.sort(); // [ 1, 10, 2, 3, 4, 50, 70, 8 ]

arr2.sort(function (a, b) {
  console.log(a, b);
});
/*
10 1
2 10
3 2
4 3
50 4
70 50
8 70
*/

arr.sort(function (a, b) {
  if (a > b) return 1;
  if (a === b) return 0;
  if (a < b) return -1;
}); // [ 1, 2, 3, 4, 8, 10, 50, 70 ]
```

### 8.some()

- 배열의 요소들을 주어진 함수(조건)을 통과하는데 한개라도 통과되면 true, 아닐때에는 false를 출력.
- 빈 배열로 함수(조건)을 통과하면 무조건 false를 출력.

```js
const array = [1, 3, 5];

// 짝수인지 체크
const result = array.some((currentValue) => {
  return currentValue % 2 === 0;
});

console.log(result); // 리턴 값 : false
// 그 이유는 array의 3개의 요소 모두 2로 나눌때 나머지가 0이 아니기 때문이다.
// 하나라도 부합한 조건에 맞으면 true, 모두 부합하지 않으면 false

// -----------------------------------------------

const array2 = [1, 2, 3, 5];

const result2 = array2.some((currentValue) => {
  return currentValue % 2 === 0;
});
console.log(result2); // 리턴 값 : true
// 그 이유는 array의 4개의 요소 모두 2로 나눌때 나머지가 0인 요소가 하나라도 있기 때문이다.
// 하나라도 부합한 조건에 맞으면 true, 모두 부합하지 않으면 false
```

### 9. every()

- 배열안의 모든 요소가 주어진 함수(조건)을 모두 통과하면 true, 한 요소라도 통과하지 못하면 false를 출력.
- 빈 배열을 함수에 적용시키면 무조건 true를 반환.

```js
const array = [1, 30, 39, 29, 13];

const result = array.every((currentValue) => {
  return currentValue < 40;
});

console.log(result); // 리턴 값 : true
// 그 이유는 array의 모든 요소가 40보다 작기 때문이다.
// 하나라도 부합한 조건에 맞지 안으면 false, 모두 부합하면 true

// -----------------------------------------------

const array2 = [1, 30, 39, 29, 100, 13];

const result2 = array2.every((currentValue) => {
  return currentValue < 40;
});
console.log(result2); // 리턴 값 : false
// 그 이유는 array의 1개의 요소 100이 40보다 크기 때문이다.
// 하나라도 부합한 조건에 맞지 안으면 false, 모두 부합하면 true
```
