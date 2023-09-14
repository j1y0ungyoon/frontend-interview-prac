# JS의 passed by value 와 passed by reference 에 대해 아는 만큼 설명해주실 수 있을까요?

## passed by value(값에 의한 전달)

### immutable

**원시타입**

- string
- number
- boolean
- null
- undefined
- symbol

자바스크립트에서 원시타입은 변경 불가능한 값임.(메모리 영역에서 변경이 불가능하다는 것이지 재할당은 가능)

```
var sayHi = 'hihi'
sayHi = 'hello'
```

첫 번째 줄이 실행되면 메모리에 문자열 'hihi'가 생성되고 식별자 sayHi는 메모리에 생성된 문자열 'hihi'의 문자열을 가리킴.
그리고 두 번째 줄이 실행되면 이미 생성되었던 문자열인 'hihi'를 수정하는 것이 아니라 새로운 문자열 'hello'를 메모리에 생성하고 식별자 sayHi는 새로운 문자열인 'hello'를 가리킴.
이때 문자열 'hihi','hello'는 모두 메모리에 존재함. 즉, 새로운 값을 다시 만듦

```
var user = {
  name: 'Lee',
  address: {
    city: 'Seoul'
  }
};

var myName = user.name; // 변수 myName은 string type

user.name = 'Kim';
console.log(myName); // Lee

myName = user.name;  // 재할당
console.log(myName); // Kim

```

또 다른 예시를 살펴보자!
user.name의 값을 변경했지만 변수 myName의 값은 변경되지 않음. 이는 변수 myName에 user.name을 할당했을 때 user.name의 참조를 할당하는 것이 아니라 immutable한 값인 'Lee'가 메모리에 새로 생성되고 myName은 이를 참조하기 때문임. 따라서 user.name의 값이 변경되어도 변수 myName이 참조하고 있는 'Lee'를 변함없음

### 그래서 passed by value란?

원시 타입은 value(값)으로 전달됨. 즉, 값이 복사되어 전달됨.
원시 값은 한번 정해지면 변경할 수 없음(메모리 상에서)

passed by value는 함수에 값(value)이 전달될 때 해당 값이 복사본이 함수의 매개변수로 전달되는 것을 의미함.

예를 들어 함수에 숫자를 전달하면 함수 내에서 이 숫자의 복사본을 사용하게 됨. 따라서 함수 내에서 해당 값을 변경하더라도 호출한 곳의 변수에는 영향을 주지 않음

```
// passed by value 예시

const iamFunction = (value) => {
  value = 10;
  console.log(`함수 안의 value : ${value}`)
}

let num = 5;
console.log("함수 실행 전 value :", num) // 5

iamFunction(num) // 10

console.log("함수 실행 후 value :", num) // 5

```

num은 숫자이므로 원시타입임. 함수 iamFunction에 num을 전달하면 value 매개변수에는 num의 값인 5가 복사됨. 함수 내에서 value를 수정하더라도 원래의 num 변수에는 영향을 주지 않음

## passed by reference(참조에 의한 전달)

### immutable

원시 타입 이외의 모든 값은 객체타입이며, 객체 타입은 변경가능한(mutable value)임. 즉, 객체는 새로운 값을 다시 만들 필요 없이 직접 변경이 가능함

```
var user1 = {
  name: 'Lee',
  address: {
    city: 'Seoul'
  }
};

var user2 = user1;

user2.name = 'Kim';

console.log(user1.name); // Kim
console.log(user2.name); // Kim

```

객체 user2의 name 프로퍼티에 새로운 값을 할당하면 객체는 변경 불가능한 값이 아니므로 객체 user2는 변경됨. 그런데 변경하지도 않은 객체 user1도 같이 변경됨. 왜냐하면 user1과 user2가 같은 주소를 참조하고 있기 때문임

### 그래서 passed by reference란?

객체(object)가 함수에 전달될 때 해당 객체의 참조(reference)가 전달되는 것을 의미함.
함수 내에서 객체의 속성을 변경하면 호출한 곳의 객체도 변경됨. 그러나 객체를 다른 객체로 대체하는 것은 호출한 곳의 객체에 영향을 주지 않음

```
const iamReferenceFunction = (obj) => {
  obj.key = 'new value';
  console.log(`함수 안의 obj.key : ${obj.key}`)
}


let objj = {key : '원조 value'}
console.log(`함수 실행 전 : ${objj.key}`) // 원조 value

iamReferenceFunction(objj) // new value

console.log(`함수 실행 후 : ${objj.key}`) // new value

```

위의 예시처럼 함수에 객체 objj를 매개변수로 보내면 objj객체가 참조하는 주소를 복사하고, 함수 안에서 변경이 일어나면(객체를 조작하면) 같은 객체의 주소를 참조하고 있으므로 원래 객체도 변경됨

```
function replaceObject(obj) {
  obj = { key: 'new object' }; // 새로운 객체로 대체
}

let myObject = { key: 'original object' };
console.log(`Before Function: ${myObject.key}`);

replaceObject(myObject); // 함수에 객체를 전달하고 대체

console.log(`After Function: ${myObject.key}`); // 원본 객체의 값 출력

```

함수 내부에서 obj를 새로운 객체로 대체함. 그러나 함수 외부에서 myObject의 값을 출력하면 여전히 원본 객체 값('original object')가 출력됨

이것은 함수 내부에서 obj가 새로운 객체를 가리키더라도 함수 외부에서 myObj는 여전히 이전 객체를 참조하고 있기 때문임

**참고**

- https://poiemaweb.com/js-object
- https://poiemaweb.com/js-immutability
