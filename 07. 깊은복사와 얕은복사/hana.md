# 깊은 복사와 얕은 복사의 차이는 무엇이고 JS에서 각각을 구현하는 방법은 어떻게 되는지 설명해주실 수 있을까요?

깊은 복사와 얕은 복사를 얘기하기 전에 데이터 타입과 메모리 할당에 대해서 알아야 함
우선 데이터 타입에는 기본형 데이터와 참조형 데이터가 있음
이를 구분하는 기준은 아래와 같음

**[기본형과 참조형의 구분 기준]**

1. **복제의 방식**

   - 기본형 : 값이 담긴 주소값을 바로 복제
   - 참조형 : 값이 담긴 주소값들로 이루어진 묶음을 가리키는 주소값을 복제

2. **불변성**의 여부
   - 기본형 : 불변성을 띔
   - 참조형 : 불변성을 띄지 않음

## 데이터 타입(기본형, 참조형)

1. 메모리를 기준으로 보는 두 가지 개념
   - 변수 / 상수
     - 변수 : 변수 영역 메모리를 변경할 수 있음
     - 상수 : 변수 영역 메모리를 변경할 수 없음
   - 불변하다 / 불변하지 않다
     - 불변하다 : 데이터 영역 메모리를 변경할 수 없음
     - 불변하지 않다 : 데이터 영역의 메모리를 변경할 수 있음
2. 불변값과 불변성(기본형 데이터)

#### 기본형 데이터 타입

- string
- number
- boolean
- null
- undefined
- symbol

기본형 데이터들은 모두 불변값임.

```
var a = 'abc';
a += 'def';

var b = 5;
var c = 5
b = 7
```

첫 번째 변수 a에 문자열 'abc'를 할당했다가 'def'를 추가하면 기존의 'abc'가 'abcdef'로 바뀌는 게 아니라 새로운 문자열을 데이터 영역에 만들고 그 주소를 변수 a에 저장함. 그러므로 'abc'와 'abcdef'는 완전히 다른 별개의 데이터임

두 번째 변수 b를 변수 영역에 할당하고 데이터 영역에 5가 있는지를 확인함 -> 있으면 데이터영역에 있는 5의 주소를 변수 b에 저장하고, 없다면 데이터 영역에 5를 새로 만들고 그 주소를 변수 b에 저장함. 변수 c에서도 5라는 값이 할당되었는데 변수 영역에 c를 할당하고 데이터 영역에서 5를 먼저 찾음 -> 있기 때문에 데이터 영역 5의 주소값을 재활용함

마지막 줄에서는 변수 b의 값을 7로 바꿈 -> 데이터 영역에서 7이 있는지 찾고 있다면 그 주소를 재활용하고 없다면 데이터 영역에 7을 만들어 변수 b에 주소를 저장함

=> 위의 예시처럼 한번 만든 기본형 데이터들은 다른 값으로 바꿀 수 없음.
한 번 만들어진 값은 가비지 컬렉팅되기 전까진 변하지 않음 -> 불변함

3. 가변값과 가변성(참조형 데이터)

```
var obj1 = {
    a : 1,
    b : 'ccc'
}

obj1.a = 100;
```

변수 영역에 obj1을 위한 공간을 만들고 객체의 프로퍼티를 위한 공간이 따로 존재함

예시 코드의 맨 마지막에서 obj1의 a의 값을 100으로 변경한다고 함
변경할 값인 100을 데이터 영역에서 찾고 없으면 데이터 영역에 새롭게 만들고 해당 주소를 obj1을 위한 별도 공간에 있는 a의 영역에 갈아끼워줌
데이터 영역에 저장되어 있는 값은 모두 불변값이지만 obj1을 위한 별도의 공간은 얼마든지 변경이 가능함(다른 값을 대입할 수 있음) 때문에 참조형 데이터를 불변하지 않다(가변하다)라고 함

4. 중첩객체
   객체 안에 또 다른 객체가 들어가는 것을 의미함
   객체는 배열, 함수 등을 포함하는 상위 개념이므로 배열을 포함하는 객체도 중첩객체임

5. 변수 복사 이후 값 변경(객체의 프로퍼티 변경)

```
// 기본형 데이터 복사
var a = 10
var b = a

// 참조형 데이터 복사
var obj1 = { c: 10, d: 'qwer'};
va obj2 = obj1

// 값 변경
b = 15
obj2.c = 20

```

변수 b의 값을 15로 변경할 때, 데이터 영역에서 15가 있는지 찾아보고 있다면 그 주소를 변수 b에 저장함. 없다면 데이터 영역의 새로운 공간에 15를 저장하고 그 주소를 변수 b에 저장함
따라서 변수 a와 b는 다른 주소를 참조함 -> 불변성 유지

obj2.c = 20의 경우 데이터 영역에 20이 없다고 가정한다면 데이터 영역에 20을 새롭게 만들고 그 주소를 변수 영역에 있는 obj2(@1004)의 값(@5002)이 가리키는 변수 영역에서 다시 c를 찾아 그곳에 저장함
변수 obj1를 복사한 obj2는 프로퍼티의 값을 바꾸었지만 저장된 값(@5002)는 바뀌지 않음 -> obj1과 obj2는 같은 객체를 참조하고 있음 -> 그래서 obj1이나 obj2의 프로퍼티 중에 무엇을 바꾸든 두 객체에 모두 변경된 값이 적용됨(같은 주소를 참조하니까)
그래서 개발자가 obj2의 값만을 바꾸었는데 의도치 않게 obj1의 값도 바뀜

그러면 어떻게 해아하나?
원본 객체는 변하지 않고 복사한 객체가 다른 주소를 참고하면 됨 -> 불변 객체 필요함

### 얕은 복사와 깊은 복사의 차이점

- 얕은 복사 : 바로 아래 단계의 값만 복사하는 방법
- 깊은 복사 : 내부의 모든 값들을 하나하나 찾아서 전부 복사하는 방법

어떤 객체를 복사할 때 객체 내부의 모든 값을 복사해서 완전히 새로운 데이터를 만들 때 객체의 프로퍼티 중에서 그 값이 기본형 데이터일 경우 그대로 복사하면 되지만 참조형 데이터는 다시 그 내부의 프로퍼티들을 복사해야함. 이 과정을 참조형 데이터가 있을 때마다 재귀적으로 수행해야하지만 깊은 복사가 됨

### 은 복사와 깊은 복사 구현

1. 얕은 복사

(1) Object.assign()
Object.assign(target, source)을 이용하는 방법. Object.assign은 두 번째 인자(source)의 객체 프로퍼티들을 첫 번째 인자(target)의 객체에 반영함

```
const original = {name: '철수', age: 12};
const clone = Object.assign({}, original);
clone.name = '영희'

console.log(original) // {name: '철수', age: 12}
console.log(clone); // {name: '영희', age: 12}
console.log(original === clone) // false

```

(2) 스프레드 연산자

```
const original = {name: '철수', age: 12};
const clone = { ...original};
clone.name = '영희'

console.log(original) // {name: '철수', age: 12}
console.log(clone); // {name: '영희', age: 12}
```

그러나 스프레드 연산자는 얕은 복사만 가능함
객체 안에 또 다른 객체(중첩 객체)나 배열, 함수 등의 객체가 들어있으면 해당 부분은 여전히 원본의 내용을 참조함

```
const original = {profile: {name:'철수', age:12}, grade: 'A'};
const clone = { ...original };

clone.profile.name = '영희';
clone.grade = 'B';

console.log(original) // {profile: {name: '영희', age: 12}, grade: 'A'}
console.log(clone); // {profile: {name: '영희', age: 12}, grade: 'B'}
```

위에서 grade 프로퍼티는 정상적으로 clone 부분만 변경되었지만 profile의 name 부분은 원본이 변경된 것을 알 수 있음.
이는 clone의 내부 객체인 profile은 original의 profile과 동일한 주소를 참조하고 있기 때문임.
따라서 clone의 내부 객체인 profile의 프로퍼티를 변경하는 경우 원본에서도 변경이 발생함.

이러한 복잡한 객체 구조에 대한 복사를 위해서는 깊은 복사를 해야함

(3) slice, concat

- slice

```
const originalArray = [1, 2, 3, { a: 4 }];

const shallowCopyArray = originalArray.slice();

shallowCopyArray[3].a = 10;

console.log(originalArray); // [1, 2, 3, {a: 10}]
console.log(shallowCopyArray); // [1, 2, 3, {a: 10}]

```

- concat

```
const originalArray = [1, 2, 3, { a: 4 }];

const shallowCopyArray = originalArray.concat();

shallowCopyArray[3].a = 20;

console.log(originalArray); // [1, 2, 3, {a: 20}]
console.log(shallowCopyArray); // [1, 2, 3, {a: 20}]

```

2. 깊은 복사

(1) JSON.stringify와 JSON.parse  
문자열화한 다음 다시 파싱하는 방식. 함수, 심벌, undefined, 혹은 순환 참조가 포함된 객체에서는 사용할 수 없음. 또한 prototype을 사용할 수 없음
깊은 복사를 할 떄 많이 사용하지 않는 방법

```
const originalObj = {
  a: 1,
  b: { c: 2 },
};

const deepClone = (obj) => JSON.parse(JSON.stringify(obj));
```

(2) 재귀 함수 사용
객체의 각 프로퍼티를 순회하면서 깊은 복사를 수행하는 방식. 조금 더 복잡하고 코드가 길어짐

```
function deepClone(obj) {
  if (obj === null || typeof obj !== 'object') {
    return obj;
  }

  const clone = Array.isArray(obj) ? [] : {};

  for (const key in obj) {
    if (Object.prototype.hasOwnProperty.call(obj, key)) {
      clone[key] = deepClone(obj[key]);
    }
  }

  return clone;
}

```

(3) lodash와 같은 외부 라이브러리
lodash에서 제공하는 cloneDeep 함수를 사용하면 깊은 복사를 쉽게 할 수 있음.

```
import _ from 'lodash';

const originalObj = {
  a: 1,
  b: { c: 2 },
};

const deepClone = _.cloneDeep(originalObj);

```

---

**요약**

1. 깊은 복사와 얕은 복사의 차이:

- 얕은 복사: 객체의 최상위 요소들을 새로운 객체나 배열로 복사하는 방법. 중첩된 객체나 배열의 경우, 내부 객체와 배열은 여전히 원본을 참조함.
- 깊은 복사: 객체의 모든 값들을 하나하나 찾아서 전부 복사하는 방법. 깊은 복사를 사용하면 중첩된 객체와 배열까지도 완전히 새로운 객체나 배열로 복사하게 됨.

2. 자바스크립트에서 얕은 복사를 구현하는 방법:

- 스프레드 연산자(...)
- Object.assign()
- 배열 메소드 slice()
- 배열 메소드 concat()

3. 깊은 복사를 구현하는 방법:

- JSON 객체를 사용(JSON.stringify()와 JSON.parse()): 제한 사항이 있음(함수, 심벌, undefined, 순환 참조 등을 처리할 수 없음)
- 재귀 함수 사용: 복잡한 객체 구조에 대한 복사를 수행 가능
- 외부 라이브러리 사용(예: lodash의 \_.cloneDeep() 함수): 간단하게 깊은 복사를 수행할 수 있음
