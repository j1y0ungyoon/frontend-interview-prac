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

콜백 함수도 함수이기 때문에 기본적으로 this가 전역 객체를 참조하지만 제어권을 받은 함수에서 콜백 함수에 별도로 this가 될 대상을 지정한 경우에는 그 대상을 참조하게 됨

```
// 1
setTimeout(function () { console.log(this) }, 200) // window { ... }

// 2
[1,2,3,4,5].forEach(function (x) {
    console.log(this, x) // window { ... } 1 2 3 4 5
})

// 3
document.body.innerHTML += <button id="a">click</button>
document.body.querySelector('#a').addEventListener('click', function(e) {
    console.log(this, e) // 버튼 엘리먼트, 클릭 이벤트가 찍힐 것임
})
```

예시 1번과 2번은 콜백함수를 호출할 때 this를 지정하지 않았기 때문에 전역 객체를 참조함
3번은 콜백함수를 호출할 때 자신의 this가 상속되도록 정의됨(. 앞의 부분이 this)

결국 콜백함수의 제어권을 가지는 함수(메소드)가 콜백 함수에서 this를 무엇으로 할지 결정하고 특별히 정의하지 않는 경우라면 기본적으로 함수와 마찬가지로 전역 객체를 바라봄

## 생성자 함수 내부에서의 this

자바스크립트는 함수에 생성자로서 역할을 함께 부여함. new 명령어와 함께 함수를 호출하면 해당 함수가 생성자로서 동작하게 됨. 그리고 어떤 함수가 생성자 함수로서 호출된 경우 내부에서의 this는 곧 새로 만들 구체적인 인스턴스 자신이 됨.

생성자 함수를 호출하면 우선 생성자의 prototype 프로퍼티를 참조하는 **proto**라는 프로퍼티가 있는 인스턴스를 만들고 미리 준비된 공통 속성 및 개성을 해당 객체(this)에 부여함

```
var Cat = function (name, age) {
    this.bark = 'meow'
    this.name = name
    this.age = age
}

var rey = new Cat('rey', 5)
var jerry = new Cat('jerry', 3)

console.log(rey, jerry)
// Cat {bark: 'meow', name: 'rey', age: 5}
// Cat {bark: 'meow', name: 'jerry', age: 3}

```

## this를 바인딩하는 방법

### call

call 메소드의 첫 번째 인자를 this로 바인딩하고 이후 인자들을 호출할 함수의 매개변수로 사용함

```
var func = function (a, b, c) {
    console.log(this, a, b, c)
}

func(1,2,3) // window { ... } 1 2 3
func.call({x : 1}, 4, 5, 6) // { x:1, 4 5 6}

```

### apply

call과 똑같고 두 번째 인자를 배열로 받아서 그 배열의 요소들을 호출할 함수의 매개변수로 지정한다는 점만 다름

```
var func = function (a, b, c) {
    console.log(this, a, b, c)
}

func(1,2,3) // window { ... } 1 2 3
func.apply({x : 1}, [4, 5, 6]) // { x:1, 4 5 6}


var obj = {
    a : 1,
    method: function (x,y) {
        console.log(this.a, x, y)
    }
}

obj.method.apply({a : 4}, [5,6]) // 4 5 6
```

### bind

ES5에서 추가된 기능으로 call과 비슷하지만 즉시 호출하지는 않고 넘겨 받은 this 및 인수들을 바탕으로 새로운 함수를 리턴하기만 하는 메소드임. 즉, bind는 함수에 this를 미리 적용하는 것과 부분 적용 함수를 구현하는 두 가지 목적을 모두 지님

```
var func = function (a, b, c) {
    console.log(this, a, b, c)
}

func(1,2,3) // window { ... } 1 2 3

var bindFunc = func.bind({x : 9})
bind.func(1,2,3) // { x : 9} 1 2 3

var bindFunc2 = func.bind({ x : 1 }, 4, 5)

bindFunc2(6,7) // { x : 1 } 4 5 6 7
bindFunc2(8,9) // { x : 1 } 4 5 8 9

```

## 화살표함수의 this

화살표 함수는 실행 컨텍스트 생성 시 this를 바인딩하는 과정이 제외됨. 따라서 함수 내부에는 this가 없고 접근하고자 하면 스코프체인에서 가장 가까운 this에 접근하게 됨
