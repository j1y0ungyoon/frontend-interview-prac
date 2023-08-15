# 일급 객체에 대해서 설명해주세요

## 일급 객체(First-class-object)

생성, 대입, 연산, 인자 또는 리턴값으로서의 전달 등 프로그래밍 언어의 기본적 조작을 제한없이 사용할 수 있는 대상을 말함

아래의 조건을 만족하면 일급 객체로 간주함

1. 익명 함수로 표현이 가능함

```
const add = function(a, b) {
    return a + b
}
```

2. 변수나 자료구조(객체, 배열)에 저장할 수 있음

```
const addNum = function(a,b) {
    return a + b
}

const functionArray = [myFunction, function(y) { return y + 5; }];

```

3. 함수의 매개변수에 전달할 수 있음

```
function applyOperation(func, x, y) {
  return func(x, y);
}

const result = applyOperation(function(a, b) { return a * b; }, 3, 4);

```

4. 리턴값으로 사용할 수 있음

```
function multiplyBy(factor) {
  return function(x) {
    return x * factor;
  };
}

const double = multiplyBy(2);
const triple = multiplyBy(3);

const result1 = double(5); // 10
const result2 = triple(5); // 15

```

자바스크립트의 함수는 위의 조건을 모두 만족하므로 자바스크립트의 함수는 일급객체임.
따라서 자바스크립트의 함수는 변수 같이 사용할 수 있으며 코드의 어디에서든지 정의할 수 있음

**참고**

- https://poiemaweb.com/js-function#3-first-class-object-%EC%9D%BC%EA%B8%89-%EA%B0%9D%EC%B2%B4

```

```
