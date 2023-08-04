# this란 무엇인지 설명해주세요

자바스크립트에서 this는 실행 컨텍스트가 생성될 때 함께 결정됨.
실행 컨텍스트는 함수가 호출되어야 생성되므로 this는 함수를 호출할 때 결정됨

## 전역 공간에서의 this

전역 공간에서의 this는 전역 객체를 가리킴. 왜냐면 전역 컨텍스트를 생성하는 주체가 전역 객체이기 때문임

## 함수 vs 메소드의 this

일반 함수의 this는 전역 객체를 가리키고 메소드의 경우 호출한 주체가 this임

```
var func = function (x) {
    console.log(this, x)
}

func(1) // window{ ... } 1

var obj = {
    method : func
}

obj.method(2)
// { method : f } 2

```

위의 예시에서 함수는 그대로인데 이를 변수에 담아서 호출한 경우와 obj 객체의 메소드로 할당해서 호출한 경우에 this가 달라지는 것을 볼 수 있음.

어떤 함수를 메소드로 호출하는 경우 this는 함수명(프로퍼티명) 앞의 객체임.
일반 함수의 경우는 다름. 일반 함수를 호출할 경우 this는 전역객체를 가리킴. 메소드의 내부함수에서도 마찬가지임

```
var obj1 = {
outer: function () {
    console.log(this) // obj1

    var innerFunc = function () {
        console.log(this)
        // window
        // obj2
    }
    innerFunc()
    // innerFunc()가 실행되면 메소드가 아니라 일반 함수로 호출되었기 때문에 this가 지정되지 않았고 스코프의 최상위 객체인 window에 바인딩됨

    var obj2 = {

        innerMethod: innerFunc
    };
    obj2.innerMethod()
    // 메소드로서 호출했기 때문에 호출한 주체인 obj2가 this가 됨

}

}

obj1.outer();
```

## this를 바인딩하지 않는 함수

ES6에서 함수 내부의 this가 전역 객체를 바라보는 문제를 보완하고자 this를 바인딩하지 않는 화살표 함수를 새로 도입함. 화살표 함수는 실행 컨텍스트를 생성할 때 this바인딩 과정 자체가 빠져 상위 스코프의 this를 그대로 활용할 수 있게 됨

## 콜백 함수 호출 시 그 함수 내부에서의 this

## 생성자 함수 내부에서의 this
