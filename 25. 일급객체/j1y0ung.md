# 일급객체란 무엇인가요?

일급 객체는 First-class Object로 다른 객체들에 일반적으로 적용 가능한 연산을 모두 지원하는 객체를 가리킨다.

## 일급 객체의 조건

- 변수에 할당(assignment)할 수 있다.
- ```javascript
  //함수 표현식
  const mul = function (num) {
    return num * num;
  };
  ```
- 다른 함수를 인자(argument)로 전달 받는다.

```javscript
function mul(num){
    return num*num;
    }

 function mulNum{func,number}{
        return func(number);
}
   let result = mulNum(mul,3); //9

//mul을 인자로 받는 mulNum이 고차함수
//mul은 콜백함수이다.
```

- 다른 함수의 결과로서 리턴될 수 있다.

일급 객체는 `함수`를 데이터(`string`, `number`, `boolean`, `array`, `object`) 다루 듯이 다룰 수 있다. (데이터를 다루다 = 변수에 할당이 가능하다 라는 뜻으로 함수역시 할당이가능하나는 의미이다.)

함수는 일급객체이기 때문에

- 고차함수(higher order function)를 만들수 있다.
- 콜백(callback)을 사용할 수 있다.

> ## 고차함수(Higher-order function)
>
> 함수를 전달인자(argument) 또는 매개변수(parameter)로 받거나 함수를 리턴하는 함수를 말한다.

### 예시

```javascript
// 다른 함수를 인자로 받는 경우
function mul(num) {
  return num * num;
}

function mulNum(func, num) {
  return func(num);
}
// 함수를 리턴하는 경우
function mul(num1) {
  return num2 * num1;
}
```
