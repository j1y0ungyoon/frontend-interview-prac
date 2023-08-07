# 실행 컨텍스트?

실행 컨텍스트 (Execution Context)는 scope,hoisting,this, function, closure 등의 동작 원리를 담고 있는 자바스크립트 핵심원리이다.

**실행 컨텍스트란 실행할 코드에 제공할 환경 정보들을 모아놓은 객체이다.**

## 실행 컨텍스트

실행 컨텍스트는 동일한 환경에 있는 코드들을 실행할 때 필요한 환경 정보들을 모아 객체를 구성하고, 이를 콜 스택에 쌓아올렸다가, 가장 위에 쌓여있는 컨텍스트 와 관려 있는 코드들을 실행하는 식으로 전체 코드의 환경과 순서를 보장한다.

어떤 실행 컨텍스트가 활성화 되는 시점에 선언된 변수를 위로 끓어올리고 외부 환경 정보를 구성하고, this 값을 설정ㅎ나느 등의 동작을 수행한다.

```js
var x = "xxx";

function foo() {
  var y = "yyy";

  function bar() {
    var z = "zzz";
    console.log(x + y + z);
  }
  bar();
}
foo();
```

함수를 실행하면 처음 자바스크립트 코드를 **실행하는 순간 전역 컨텍스트가 콜 스택에 담긴다.** 전역 컨텍스트는 코드 내부에서 벼 ㄹ도의 실행 명령이 없어도 브라우저에서 자동으로 실행하므로 자바스크립트 파일이 열리는 순간 전역 컨텍스트가 활성화된다고 이해하면된다. 참고로 브라우저의 경우에는 window객체가 전역 실행컨텍스트가 된다.

이제 콜 스택에서 전역 컨텍스트와 관련되 코드들을 순차로 진행하다가 **foo함수를 호출하면 자바스크립트 엔진은 foo에 대한 환경 정보를 수집해서 foo 실행 컨텍스트를 생성한 후 콜 스택에 담는다.** 그러면 콜스택의 맨 위에 foo 실행 컨텍스트가 놓였으므로 전역 컨텍스트와 관련된 코드의 실행을 중지하고 foo 실행 컨텍스트와 관련된 코드, **즉 foo 함수 내부의 코드들을 순차 적으로 실행한다.**

그리고 bar함수의 실행 컨텍스트가 스택의 가장 위에 담기면 foo 실행 컨텍스트와 관련되 코드의 실행을 중단하고 bar 험수 내부의 코드를 순서대로 진행한다. 코드가 모두 진행되고 bar 함수의 실행이 종료되면 bar 실행 컨텍스트가 콜스택에서 제거 되고, 그러면 foo 실행 컨텍스트가 콜 스택 맨위에 존재하게 되므로 중단 했떤 부분이 이어서 실행하게 된다.

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbRgruI%2FbtrOUOibKnC%2F8U2Zh6ZmhkcSvWy8Mmjz3K%2Fimg.png">

어떤 실행 컨텍스트가 활성화될 때 자바스크립트 엔진은 해당 컨텍스트와 관련된 코드들을 실행하는데 필요한 환경 정보들을 수집해서 실행 컨텍스트 객체에 저장한다.

## 실행 컨택스트의 구성

저장되는 환경 정보

- variable Environment : 현재 컨텍스트 내의 식별자들에 대한 정보 + 외부 환경 정보. 선언 시점의 Lexical Environment의 스냅샷으로, 변경 사항은 반영되지 않는다.
- Lexical Environment : 처음에는 Variable Environment와 같지만 변경 사항이 실시간으로 반영된다.
- this Binding : this 식별자가 바라봐야 할 대상 객체를 의미한다.

Variable Environment에 담기는 내용은 Lexical Environment와 같지만 최초 실행 시의 스냅샷을 유지한다는 점이 다르다. 실행 컨텍스트를 생성할 때 Variable Environment에 정보를 먼저 담은 다음, 이를 그대로 복사해서 Lexical Environment를 만들고, 이후에는 Lexical Environment를 주로 활용한다.

Variable Environment와 Lexical Environment의 내부는 environment Record와 outer Environment Reference로 구성되어 있다. 초기화 과정 중에는 사실상 완전히 동일하고 이후 코드 진행에 따라 서로 달라지게 된다.

이 두 환경의 내부는 다시 Environment Record 와 Outer Environment Reference 로 구성된다.

- environmentRecord: 함수 안의 코드가 실행되기 전에 현재 컨텍스트와 관련된 코드의 식별자 정보(매개변수의 이름, 함수 선언, 변수명 등)가 저장된다. 즉 코드가 실행되기 전에 자바스크립트 엔진은 이미 해당 환경에 속한 코드의 변수명 등을 모두 알고 있게 된다. (호이스팅)
- outerEnvironmentReference(외부 렉시컬 환경에 대한 참조): 상위 스코프를 가리킨다. 즉 현재 environment record보다 바깥에 있는 environment record를 참고한다는 뜻이며 해당 실행 컨텍스트를 생성한 함수의 바깥 환경을 가리킨다. 코드 상에서 어떤 변수에 접근하려고 하면 현재 컨텍스트의 LexicalEnvironment를 탐색해서 발견되면 그 값을 반환하고, 발견하지 못할 경우 다시 outerEnvironmentRecord에 담긴 LexicalEnvironment를 탐색하는 과정을 거친다. 전역 컨텍스트의 LexicalEnvironment까지 탐색해도 해당 변수를 찾지 못하면 undefined를 반환한다.

### EnvironmentRecord

environmentRecord에는 현재 컨텍스트와 관련된 코드의 식별자 정보들이 저장된다.
컨텍스트 전체를 처음부터 끝까지 훑으며 순서대로 식별자들을 수집하는 데 **여기에서 수집되는 식별자들은 매개변수 식별자, 선언된 함수 , var로 선언된 변수의 식별자** 등의 해당한다.

```js
function a(x) {
  console.log(x);
  var a = 1;
  var b = 2;
  function foo() {
    console.log("work");
  }
}

a(1);
```

실행 컨택스트의 environmentRecord에는 식별자 x,a,b와 함수 foo가 수집된다.

environmentRecord는 현재 실행될 컨텍스트의 대상 코드 내에 어떤 식별자들이 있는지만 먼저 수집하기 때문에, 변수를 인식할 때 식별자만 클어올리고 할당과장은 원래 자리에 순서대로 남겨둔다 -> 호이스팅

호이스팅은 코드 해석을 좀더 수훨하게 하기 위해 environmentRecord의 수집 과정을 추상화 한 개념으로, 실행 컨텍스트가 관여하는 코드 집단의 최상단으로 이들을 끌어올린다 라고 해석하는 것이다. 변수 선언과 값 할당이 도이에 이뤄진 문장은 '선언부'만을 호이스팅하고, 할당 과정은 원래 자리에 남아있게 되는데, 여기서 함수 선언문과 함수 표현식의 차이가 발생한다.

## 마무리

1. 실행 컨텍스트는 실행할 코드에 제공할 환경 정보들을 모아놓은 객체이다. 실행 컨텍스트는 전역 컨텍스트와 eval 및 함수 실행에 의한 컨텍스트 등이 있다. 실행 컨텍스트 객체는 활성화되는 시점에 VariableEnvironment, LexicalEnvironment, ThisBinding의 세 가지 정보를 수집한다.
2. 실행 컨텍스트를 생성할 때는 VariableEnvironment와 LexicalEnvironment가 동일한 내용으로 구성되지만 LexicalEnvironment는 함수 실행 도중에 변경되는 사항이 즉시 반영되는 반면, VariableEnvironment는 초기 상태를 유지한다.
3. VariableEnvironment와 LexicalEnvironment는 매개변수명, 변수의 식별자, 선언한 함수의 함수명 등을 수집하는 environmentRecord와 바로 직전 컨텍스트의 LexicalEnvironment 정보를 참조하는 outerEnvironmentReference로 구성돼 있다.
4. 호이스팅은 코드 해석을 좀 더 수월하게 하기 위해 environmentRecord의 수집 과정을 추상화한 개념으로, 실행 컨텍스트가 관여하는 코드 집단의 최상단으로 이들을 '끌어올린다'고 해석하는 것이다.
5. 스코프는 변수의 유효범위를 말한다. outerEnvironmentReference는 해당 함수가 선언된 위치의 LexicalEnvironment를 참조한다. 코드 상에서 어떤 변수에 접근하려고 하면 현재 컨텍스트의 LexicalEnvironment를 탐색해서 발견되면 그 값을 반환하고, 발견하지 못할 경우 다시 outerEnvironmentRecord에 담긴 LexicalEnvironment를 탐색하는 과정을 거친다. 전역 컨텍스트의 LexicalEnvironment까지 탐색해도 해당 변수를 찾지 못하면 undefined를 반환한다.
