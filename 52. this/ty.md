# this란 무엇인지 설명해주세요

this는 함수의 블록 스코프 내에서 선언 되야 작동한다.

## 함수 호출 방식과 this 바인딩

함수 호출 방식에 의해 this에 바인딩할 어떤 객체가 동적으로 결정된다.
**함수를 선언할 때 this에 바인딩할 객체가 정적으로 결정되는 것이 아니고, 함수를 호출할 때 함수가 어떻게 호출 되었는지에 따라 this에 바인딩할 객체가 동적으로 결정된다.**

## 함수의 호출방식

- 함수호출
- 메소드호출
- 생성자 함수 호출
- 콜백 호출
- apply/call/bind호출

### 1. 함수 호출

기본적으로 this는 전역객체(Global object)에 바인딩된다.
**내부함수는 일반 함수, 메소드, 콜백함수 어디에서 선언되었든 관계없이 this는 전역객체를 바인딩한다.**

- 일반 전역함수는 물론이고 내부함수의 경우도 this는 외부 함수가 아닌 전역 객체에 바인딩된다.

```js
function foo() {
  console.log("foo's this:", this); //windows

  function bar() {
    console.log("bar's this:", this); //window
  }

  bar();
}

foo();
```

- 객체의 메소드의 내부함수일 경우에도 this는 전역객체에 바인딩 된다.

```js
var value = 1;

var obj = {
  value: 100,
  foo: function () {
    console.log("foo's this: ", this); // obj
    console.log("foo's this.value: ", this.value); // 100
    function bar() {
      /* 내부함수 */
      console.log("bar's this: ", this); // window
      console.log("bar's this.value: ", this.value); // 1
    }
    bar();
  },
};

obj.foo();
```

- callback 함수의 경우에도 this는 전역객체에 바인딩된다.

```js
var value = 1;

var obj = {
  value: 100,
  foo: function () {
    setTimeout(function () {
      /* 콜백함수 */ console.log("callback's this: ", this); // window
      console.log("callback's this.value: ", this.value); // 1
    }, 100);
  },
};

obj.foo();
```

`내부한수의 this가 전역객체를 참조하는 것을 회피하는 방법`

- 객체의 this를 변수에 저장해서 사용(var that = this;)
- call,bind,apply로 this
- => 화살표 함수 사용

```js
var value = 1;

var obj = {
  value: 100,
  foo: function () {
    var that = this; // Workaround : this === obj

    console.log("foo's this: ", this); // obj
    console.log("foo's this.value: ", this.value); // 100
    function bar() {
      //console.log("bar's this: ",  this); // window
      // console.log("bar's this.value: ", this.value); // 1

      console.log("bar's that: ", that); // obj
      console.log("bar's that.value: ", that.value); // 100
    }
    bar();
  },
};

obj.foo();
```

### 2. 메소드 호출

함수가 객체의 프로퍼티 값이면 메소드로서 호출된다.
메소드 내부의 this는 해당 메소드를 소유한 객체, 즉 해당 메소드를 호출한 객체에 바인딩 된다.

```js
var obj1 = {
  name: "Lee",
  sayName: function () {
    console.log(this.name);
  },
};

var obj2 = {
  name: "Kim",
};

obj2.sayName = obj1.sayName;

obj1.sayName(); // Lee
obj2.sayName(); // Kim
```

### 3. 프로토타입

프로토타입 객체도 메소드를 가질수있다.
프로토타입 객체 메소드 내부에서 사용된 this도 일반 메소드 방식과 마찬가지로 해당 메소드를 호출한 프로토타입 오브젝트 객체에 바인딩된다.

this의 프로퍼티를 찾을때 우선, 직접 바인딩 되어있는 프로토타입 오브젝트에서 찾고, 없으면 체이닝에 의해 new 생성자로 생성된 객체에서 찾게 된다.

```js
function Person(name) {
  this.name = name;
}

Person.prototype.getName = function () {
  return this.name;
};

var me = new Person("Lee");
console.log(me.getName());
// Lee
// 우선 프로토타입에서 name프로퍼티를 찾는다. 없으니 체이닝에 의해 me 객체에서 찾아서 반환

Person.prototype.name = "Kim";
console.log(Person.prototype.getName());
// Kim
// 우선 프로토타입에서 name프로퍼티를 찾는다. 찾았으니 반환.
```

### 4. 생성자 함수 호출

기존함수에 new 연산자를 붙여서 호출하면 해당 함수는 생성자 함수로 동작하며 반대로 생각하면 함수가 아닌 일반 함수에 new 연산자를 붙여 호출하면 생성자 함수 처럼 동작할 수 있다.(혼란 방지를 위해 대문자를 사용한다.)

함수 앞에 new 키워드가 붙이고 선언할 때, this를 해당 객체에 바인딩합니다.

```js
// 생성자 함수
function Person(name) {
  this.name = name;
}

var me = new Person("Lee");
console.log(me); // Person&nbsp;{name: "Lee"}

// new 연산자와 함께 생성자 함수를 호출하지 않으면 생성자 함수로 동작하지 않는다.
var you = Person("Kim");
console.log(you); // undefined
```

### 5. 콜백함수

콜백함수 안에서 this는 콜백함수가 일반 함수이기 때문에 전역 객체를 참조한다.
함수가 복사되어 callback 파라미터에 담기게 되어 메소드의 this는 전역객체 window를 가리키게 된다.
해결하기위해서는 call, apply를 사용한다.

```js
let userData = {
  signUp: "2020-10-06 15:00:00",
  id: "minidoo",
  name: "Not Set",
  setName: function (firstName, lastName) {
    this.name = firstName + " " + lastName;
  },
};

function getUserName(firstName, lastName, callback) {
  callback(firstName, lastName);
}

getUserName("PARK", "MINIDDO", userData.setName);

console.log("1: ", userData.name); // Not Set
console.log("2: ", window.name); // PARK MINIDDO

function getUserName(firstName, lastName, callback) {
  callback.call(userData, firstName, lastName);
}

getUserName("PARK", "MINIDDO", userData.setName);

console.log("1: ", userData.name); // PARK MINIDDO
console.log("2: ", window.name); // 빈칸. 왜냐하면 변수가 없으니까
```

### 6. apply/call/bind 호출

call(), apply(), bind()는 this를 필요에 따라 변경할 수 있도록 함수를 제공합니다.
apply() 메소드를 호출하는 주체는 func함수이며 .apply() 메소드는 this를 특정 객체에 바인딩할 뿐 본질적인 기능은 함수 호출이라는 것이다.

```js
// 1. 이것은 객체의 메소드
var man = {
  name: "john",
  // 2. 객체의 메소드 안에서 함수를 선언하는 것이니까 내부 함수
  hello: function () {
    function getName() {
      // 3. 여기서 this가 무엇을 가리키고 있을까?
      return this.name;
    }
    // 4. 이번에는 call()을 통해 현재 문맥에서의 this(man 객체)를 바인딩해주었다
    console.log("hello " + getName.call(this));
  },
};

// 이번에는 메소드를 실행시키면 john가 뜬다!
// this가 man 객체로 바인딩 된 것을 확인할 수 있다
man.hello();
```

### 7. 화살표 함수

화살표 함수 안에서 this는 언제나 상위 스코프의 this를 가리킨다.
화살표 함수는 함수를 선언할 때 this에 바인딩할 객체가 정적으로 결정된다.
화살표 함수는 call, apply, bind 메소드를 사용하여 this를 변경할 수 없다.

## 마무리

this는 기본적으로 window이지만,
객체 메서드, bind call apply, new일 때 this가 바뀐다.
언한 function의 this는 항상 window라는 것 (strict 모드에서는 undefined !!)

this 값은 런타임에 결정된다
함수를 선언할 때 this를 사용할 수 있다.
다만, 함수가 호출되기 전까지 this엔 값이 할당되지 않는다.

화살표 함수는 자신만의 this를 가지지 않는다는 점에서 독특하다.
화살표 함수 안에서 this를 사용하면, 외부에서 this 값을 가져온다.
