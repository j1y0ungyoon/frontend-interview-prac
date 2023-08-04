# 고차 함수란 무엇인지 설명해주실 수 있을까요?

> # 고차함수(Higher-Order function,HOF)
>
> 함수를 매개변수로 사용하거나 함수를 반환하는 함수
> 고차 함수는 인자로 받은 함수를 필요한 시점에 호출하거나 클로저를 생성하여 반환한다.
> 자바스크립트의 함수는 일급 객체로 값을 인자처럼 전달할 수 있으며 반환할 수도 있다.

- 자바스크립트의 메서드 filter(),map(),reduce() 함수도 고차 함수 개념을 활용해 만들어져있다.

```javascript
//함수를 인자로 전달, 반환하는 고차함수
function makeCounter(predicate){
    let num = 0;
    return function(){
        num = predicate(num
        return num;)
    };
}

//보조 함수
function increase(n){
    return ++n;
}
function decrease(n){
    return --n;
}

//함수를 인수로 전달 -> 클로저 반환
const increaser = makeCounter(increase);
console.log(increaser()); //1
console.log(increaser()); //2

const decreaser = makeCounter(decreaser);
console.log(decreaser()); //-1
console.log(decreaser()); //-2
```

고차 함수는 외부 상태 변경이나 가변(mutable) 데이터를 피하고 불변성(Immutbility)을 지향하는 함수형 프로그래밍에 기반을 두고 있다. 함수형 프로그래밍은 순수 함수(Pure function)와 보조함수의 조합을 통해 로직 내에 존재하는 조건문과 반복문을 제거하여 복잡성을 해결하고 변수의 사용을 억제하여 상태 변경을 피하려는 프로그래밍 패러다임이다.

- 조건문이나 반복문은 로직의 흐름을 이해하기 어렵게해 가독성을 해치고, 변수의 값은 누군가에 의해 언제든 변경될 수 있어 오류 발생의 근본적인 원인이될 수 있다.

**함수형 프로그래밍은 순수함수를 통해 부수 효과를 최대한 억제하여 오류를 피하고 프로그램의 안정성을 높이는 노력의 한 방법이다.**

## 출처 - 모던 자바스크립트 Deep Dive
