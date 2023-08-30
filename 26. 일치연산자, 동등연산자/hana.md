# ‘==’와 ‘===’ 연산자의 차이는 무엇인지 설명해주실 수 있을까요?

## 동등 연산자(==)

두 개의 피연산자가 동일한지 확인하고 불린값을 리턴함.
만약 두 개의 피연산의 타입(저료형)이 다르다면 강제로 형변환을 해 비교함

```
console.log(100 == 100) // true
console.log('100' == 100) // true
console.log(0 == false) // true
```

### 동등 연산자에서 두 개의 피연산자가 동일한 타입일 때 다음과 같이 비교

- 객체 : 피연산자들이 동일한 객체를 참조할 때만 true
- 문자열 : 피연산자들이 동일한 문자를 동일한 순서로 되어있을 때 true
- 숫자 : +0과 -0은 동일한 값으로 취급. 두 개의 피연산자들이 NaN이면 false 리턴함
- 심볼 : 피연산자들이 동일한 심볼을 참조할 때만 true

### 동등 연산자에서 두 개의 피연산자가 다른 타입일 때 다음과 같이 비교

- 하나의 피연산자가 심볼이고 나머지 피연산자가 심볼이 아닌 경우 false 리턴
- 하나의 피연산자가 boolean이고 나머지 피연산자가 boolean 이 아닌 경우 boolean을 숫자로 변환함
  true는 1, false는 0으로 변환 후 다시 비교함
- 하나의 피연산자가 숫자이고 나머지 피연산자가 문자열인 경우 문자열을 숫자로 변환함
-

### null & undefined

**동등 연산자**에서 하나의 피연산자가 null이거나 undefined인 경우 true를 반환하기 위해선 다른 하나가 무조건 null이거나 undefined 여야함

```
console.log(null == undefined) // true
console.log(undefined == null) // true
console.log(null == 'okok') // false
```

### null vs 0

동등 연산자에서 null을 숫자형으로 변환 시 0, undefined는 NaN으로 변환됨

```
console.log(null == 0) // false
console.log(null >= 0) // true
console.log(null > 0) // false
```

첫 번째 콘솔에서 false가 나오는 것은 null과 undefined를 비교하는 경우에만 true를 반환하고 그 외의 경우(null이나 undefined를 다른 값과 비교할 때)에는 무조건 false를 반환하기 때문임
두 번째와 세 번째 콘솔에서는 기타 비교 연산자의 동작원리에 따라 null이 숫자형으로 변환되 0이 되어 비교하기 때문임(참고만 하라고 넣어둠)

물론 억지로 null과 0을 비교해서 true를 만들 수 있음

```
0 == !!null
```

!null -> null을 불리언으로 변환하면 false인데 not operator가 붙어서 true가 됨 -> !!null에서 !가 한번 더 붙었으므로 false로 변환됨 -> 0 == false를 비교할 때 불리언 값을 숫자로 변환하기 때문에 false를 숫자형으로 바꾸면 0이 됨 -> 0 == 0은 같기에 true

하지만 이렇게 억지로 사용하는 경우는 거의 없을 듯...
MDN에 나와있길래 참고만 하라고 넣어둠

### undefined

undefined는 다른 값과 비교할 수 없음

```
console.log(undefined > 0) // false
console.log(undefined < 0) // false
console.log(undefined == 0) // false

```

첫 번째와 두 번째 콘솔에서는 undefined가 숫자형으로 변환되어 NaN이 되는데 NaN이 피연산자인 경우 항상 false를 반환함

마지막 콘솔에서는 null과 undefined를 비교하는 경우에만 true를 리턴하고 그 외의 경우에서는 false를 리턴하기 때문에 false 반환함

null과 마찬가지로 억지로 undefined와 0을 비교해서 true를 만들 수 있음

```
0 == !!undefined
```

!undefined -> undefined 불리언으로 변환하면 false인데 not operator가 붙어서 true가 됨 -> !!undefined에서 !가 한번 더 붙었으므로 false로 변환됨 -> 0 == false를 비교할 때 불리언 값을 숫자로 변환하기 때문에 false를 숫자형으로 바꾸면 0이 됨 -> 0 == 0은 같기에 true

하지만 이렇게 억지로 사용하는 경우는 거의 없을 듯...
MDN에 나와있길래 참고만 하라고 넣어둠

## 일치 연산자(===)

두 개의 피연산자가 동일한지 확인하고 불린값을 리턴함.
만약 두 개의 피연산의 값이 다르거나 타입(자료형)이 다르다면 false 리턴함

```
console.log(100 === 100) // true
console.log('okok' === 'okok') // true
console.log('100' === 100) // false
console.log(0 === false) // false
console.log(1 === true) // false
console.log(1 === new Number(1)) // false
```

### null & undefined

**일치 연산자**에서는 null과 undefined를 비교 시 타입(자료형)이 다르기 때문에 false를 리턴함

```
console.log(null === undefined) // false
console.log(null === 'okok') // false
```

### null vs 0

타입(자료형)이 다르기 때문에 false를 리턴함

**정리**

동등연산자(==)와 일치 연산자(===)는 모두 두 개의 피연산자가 다른지 비교함. 차이점으로는 동등 연산자는 피연산자들끼리 타입이 다르다면 강제로 형변환을 해서 비교하고, 일치 연산자는 동등 연산자보다 엄격하게 피연산자들끼리 값이 같고 자료형이 같아야만 true를 반환함

**참고**

- https://ko.javascript.info/comparison
- http://www.tcpschool.com/javascript/js_operator_comparison#:~:text=%EB%8F%99%EB%93%B1%20%EC%97%B0%EC%82%B0%EC%9E%90(%3D%3D)%EB%8A%94,(true)%EC%9D%84%20%EB%B0%98%ED%99%98%ED%95%A9%EB%8B%88%EB%8B%A4.
- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Equality
