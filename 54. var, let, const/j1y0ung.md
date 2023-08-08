# var, let, const 에 대해 설명해주실 수 있을까요?

## 변수(variable)

> 변수란?
> 자바스크립트에서 변수는 하나의 값을 저장하기 위해 확보한 메모리 공간 자체 또는 그 메모리 공간을 식별하기 위해 붙인 이름이다.

자바스크립트는 매니즈드 언어(managed languaged)이기 때문에 개발자가 직접 메모리를 제어하지 못하고 직접 메모리의 주소를 통해 값을 저장하고 참조할 필요 없이 **변수를 통해 값에 접근이 가능하다**.

### 변수의 선언

변수의 선언에는 `var` `const` `let` 이 있다.
자바스크립트의 변수 선언은 선언->초기화 단계를 거쳐 수행되며

- 선언에는 변수명을 등록하여 자바스크립트 엔진에 변수의 존재를 알림
- 초기화에는 값을 저장하기 위한 메모리 공간을 확보하고 암묵적으로 undefined를 할당해 초기화 한다.

---

## var

`var`은 변수를 선언하는 방식중 하나로 중복선언이 가능하다.
변수를 유연하게 사용할 수 있다는 장점이 있지만, 기존에 선언해둔 변수의 존재를 잊고 값을 재할당하는 실수가 발생할 수 있다.

- 이를 보완하기 위해 ES6 이후에 추가된 변수 선언 방식이 `let` 과 `const` 이다.

## let

`let`은 중복 선언이 불가능한 변수를 선언하는 방식으로 재할당은 가능하다.
`var`와는 다르게 `let`은 해당 변수가 이미 선언되었다는 에러 메세지가 출력 되며 중복선언이 불가능하나, 변수 선언 및 초기화 이후 재할당은 가능하다.

```javascript
//let의 재할당 예시
let name = "jiyoung";
console.log(name); //jiyoung

let name = "soondoo";
console.log(name);
//Uncaught SyntaxError: Identifier 'name' has already been declared

name = "yoonsoondoo";
console.log(name); //yoonsoondoo
```

## const

`const`는 중복 선언이 불가능하고 재할당이 불가능하다.
`let`과 `const`의 차이점은 `immutable`의 여부이다. -immutable
`let`은 값을 재할당할 수 있지만, `const`는 재할당 시 에러메시지가 출력된다.

```javascript
//const 예시
function func() {
  const list = ["A", "B", "C"];
  list = "D";
  console.log(list);
  //TypeError: Assignment to constant variable
  list.push("D");
  console.log(list); // ["A","B","C","D"]
}
```

`const`는 constant(상수)를 뜻한다. 한번만 선언이 가능하며 값을 바꿀 수도 없지만 배열과 오브젝트의 값을 변경하는 것은 가능하다.
**`const`는 불변을 의미하는 것과 다르게, 값을 재할당하는 코드만 불가능하다고 볼 수 있다.**

---

## 스코프 (Scope)

스코프란 유효한 참조 범위를 뜻하며 기존 방법인 `var`로 선언한 변수와 `let`, `const`로 선언한 변수의 스코프는 다르다.

### var: 함수 레벨 스코프 (function-level scope)

함수 내에서 선언한 변수는 함수 내에서만 유요하며 함수 외부에서는 참조할 수 없다. 함수 내부에서 선언한 변수는 **지역변수**이며 함수 외부에서 선언한 변수는 모두 **전역 변수**로 취급된다.

### let, const: 블록 레벨 스코프(block-level scope)

함수, if문, for문, while문, try/catch문 등의 모든 코드 블록({...}) 내부에서 선언된 변수는 코드 블록 내에서만 유효하며 코드 블록 외부에서는 참조할 수 없다.
**코드 블록 내부에서 선언한 변수는 지역 변수로 취급된다.**

---
