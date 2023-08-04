# Hoisting이란 무엇인지 설명해주실 수 있을까요?

자바스크립트는 어떤 실행 컨텍스트가 활성화되는 시ㅁ에 선언된 변수를 위 끌어올리고(호이스팅), 외부 환경 정보를 구성하고, this 값을 설정하는 등의 동작을 합니다

## 실행 컨텍스트

실행할 코드에 제공할 환경 정보를 모아놓은 객체를 말함.
동일한 환경에 있는 코드들을 실행할 때 필요한 환경 정보들을 모아 컨텍스트를 구성하고 이를 콜 스택에 쌓아 올렸다가 가장 위에 있는 컨텍스트와 관련된 코드를 실행하는 식으로 전체 코드의 환경과 순서를 보장함

```
var a = 1;

function outer() {
    function inner() {
        console.log(a) // undefined
        var a = 3;
    }
    inner()
    console.log(a) // 1

}

outer()
console.log(1)
```

1. 전역 컨텍스트 (Global Execution Context):

- 변수:
  - a: 1 (전역 변수로 선언 및 할당)
- 함수:
  - outer: 함수 선언문으로 정의된 outer 함수

2. outer 함수 컨텍스트:

- 변수:
  - inner: 함수 선언문으로 정의된 inner 함수 (호이스팅에 의해 함수 선언부만 끌어올려짐)

3. inner 함수 컨텍스트:

- 변수:
  - a: undefined (변수 선언은 호이스팅에 의해 최상단으로 올라가지만, 아직 초기화되지 않았기 때문에 undefined로 설정)

이렇게 실행 컨텍스트가 활성화될 때 자바스크립트 엔진은 해당 컨텍스트에 관련된 코드들을 실행하는데 필요한 환경 정보를 수집해서 실행 컨텍스트 객체에 저장함. 이 객체는 자바스크리븥 엔진이 활용할 목적으로 생성할 뿐 개발자가 확인할 수 없음.

실행 컨텍스트에 담기는 정보들은 다음과 같음

- variableEnvironment : 현재 컨텍스트 내의 식별자들에 대한 정보 + 외부 환경 정보, 선언 시점의 LexicalEnvironment의 스냅샷으로, 변경 사항은 반영되지 않음
- LexicalEnvironment : 처음에는 variableEnvironment와 같지만 변경 사항이 실시간으로 반영됨
- ThisBinding : this 식별자가 바라봐야 할 대상 객체

## variableEnvironment

렉시컬 환경과 같지만 **최초 실행시의 스냅샷을 유지**한다는 점이 다름. 실행 컨텍스트를 생성할 때
variableEnvironment에 정보를 먼저 담은 다음 이를 그대로 복사해서 렉시컬 환경을 만들고 이후에는 렉시컬 환경을 주로 활용함

environmentRecord(스냅샷)와 outerEnvironmentReference(스냅샷)로 나뉨

**environmentRecord(환경 레코드)**

- 실행 컨텍스트 내의 변수와 함수 선언을 저장하는 공간
- 변수명과 해당 변수의 값 또는 참조가 포함
- 이 정보들은 초기화 단계에서 필요하며 변수와 함수를 생성하고 초기화하는데 필요함

**outerEnvironmentReference(외부 환경 참조)**

- 현재 실행 컨텍스트와 연관된 외부 스코프(상위 스코프)를 가리키는 참조
- 스코프 체인을 구현하는데 활용됨

variableEnvironment의 environmentRecord는 현재 실행 컨텍스트의 변수 초기화 이전의 상태를 스냅샷으로 저장함. 즉, 변수와 함수가 선언된 위치에 따라 기록되며, 실행 컨텍스트의 코드가 순차적으로 실행되기 전의 **초기 상태를 기록** 함

## LexicalEnvironment

**environmentRecord(환경 레코드)**

- variable의 environmentRecord와 동일함
- 하지만 lexicalEnvironment의 environmentRecord는 현재 실행 컨텍스트의 코드가 이미 순차적으로 실행된 이후의 상태를 반영함
- 그래서 실행 컨텍스트가 코드를 실행하며 변경되는 변수의 값이 반영됨

**outerEnvironmentReference(외부 환경 참조)**

- variableEnvironment의 outerEnvironmentReference와 동일함
- 스코프 체인을 구현하는데 활용됨

자바스크립트 엔진은 컨텍스트 내부를 처음부터 끝까지 훑어내려가며 순서대로 수집을 함
이렇게 자바스크립트 엔진이 변수 및 함수 선언 정보를 수집하는 과정을 모두 마쳤더라도 아직 실행 컨텍스트가 관여할 코드들은 실행되기 전의 상태임. 코드가 실행되기 전임에도 자바스크립트 엔진은 이밎 해당 환경에 속한 코드의 변수 및 함수 선언들을 모두 알고 있게 됨. 여기서 호이스팅이 나오는데 변수 정보를 수집하는 과정을 이해하기 쉽게 만든 가상의 개념임. 자바스크립트 엔진이 실제로 끌어올리지는 않지만 편의상 끌어올린 것으로 간주해 코드를 해석하기 편하게 한 것이 **호이스팅** 임

### 호이스팅

environmentRecord는 현재 실행될 컨텍스트의 대상 코드 내에 어떤 식별자들이 있는지에만 관심이 있고,
각 식별자에 어떤 값이 할당될 것이지는 관심이 없음. 따라서 변수를 호이스팅할 때 변수명만 끌어올리고 할당 과정은 원래 자리에 그대로 남겨둠. 매개변수의 경우에도 마찬가지임

// 예시

```
function a () {
    var x = 1
    console.log(x)
    var x;
    console.log(x)
    var x = 2;
    console.log(x)

}
a()
```

위의 코드가 호이스팅되면 아래와 같이 된다고 볼 수 있음

```
function a () {
    var x;
    var x;
    var x;

    x = 1
    console.log(x) // 1
    console.log(x) // 1

    x = 2;
    console.log(x) // 2

}
a(1)

```

변수는 선언부와 할당부를 나누어 선언부만 끌어올리는 반면 함수 선언은 함수 전체를 끌어올림

// 예시

```
function a () {

    console.log(b)
    var b = 'bb';
    console.log(b)

    function b() {
        console.log(b)
    }

}

a()
```

위의 예시에 호이스팅이 되면 아래와 같음

```
function a () {
    var b;
    var b = function b() {}
    // 호이스팅이 끝난 상태의 함수 선언문은 함수명으로 선언한 변수에 함수를 할당한 것처럼 여길 수 있음

    console.log(b) // 함수 b
    b = 'bb';
    console.log(b) // 'bb'
    console.log(b) // 'bb'

}
a()
```

### 함수 선언식과 함수 표현식

- 함수 선언식 : function 정의부만 존재하고 별도의 할당 명령이 없는 것
- 함수 표현식 : 정의한 함수를 별도의 변수에 할당하는 것

갑자기 이 개념이 왜 나왔냐면 호이스팅과 연관이 되기 때문임

// 예시

```
console.log(sum(1,2))
console.log(multiply(1,2))

function sum (a,b) {
    return a + b
}

var multiply = function (a,b) {
    return a * b
}

```

위의 예시가 호이스팅 되면 아래와 같음

```
var sum = function sum (a,b) {
    return a + b
}

var multiply;

console.log(sum(1,2)) // 3
console.log(multiply(1,2)) // multiply is not function Error

multiply = function (a,b) {
    return a * b
}

```

함수 선언문은 전체를 호이스팅한 반면 함수 표현식은 변수 선언부만 호이스팅했음.
함수도 하나의 값으로 취급할 수 있다는 것이 바로 이거임. 함수를 다른 변수에 값으로 할당한 것이 바로 함수 표현식임

### 스코프, 스코프 체인

**스코프**
식별자에 대한 유효범위

**스코프체인**
식별자의 유효범위를 안에서부터 바깥으로 차례대로 검색해나가는 것
스코프 체인을 가능하게 하는 게 바로 렉시컬 환경의 두 번째 수집자료인 outerEnvironmentReference임

#### 스코프 체인

outerEnvironmentReference는 현재 호출된 함수가 '선언될 당시' 렉시컬 환경을 참조함
예를 들면 a 함수 안에 b 함수 선언 -> b 함수 내부에 함수 c 선언힌 경우
함수 c의 outerEnvironmentReference는 함수 b의 렉시컬 환경을 참조, 함수 b의 렉시컬 환경 안의 outerEnvironmentReference는 b함수가 선언되던 때 a 함수의 렉시컬 환경을 참조함

각 outerEnvironmentReference는 오직 자신이 선언된 시점의 렉시컬 환경만 참조하고 있으므로 가장 가까운 요소부터 차례대로 접근할 수 있고 다른 순서대로 접근하는 것은 불가능함
이런 특정 때문에 여러 스코프에서 동일한 식별자를 선언한 경우 **무조건 스코프 체인 상에서 가장 먼저 발견된 식별자에만 접근 가능함**(안 -> 바깥 O , 바깥 -> 안 x)

// 예시

```
var a = 1;

function outer() {
    function inner() {
        console.log(a) // undefined
        var a = 3;
    }
    inner()
    console.log(a) // 1

}

outer()
console.log(a)
```

230번째 줄에서 outer 실행 컨텍스트의 environment record에 { inner } 식별자를 저장함
outerEnvironmentReference에는 outer 함수가 선언될 당시의 렉시컬 환경이 담김. outer함수는 전역 공간에서 선언되었으므로 전역 컨텍스트의 렉시컬 환경이 담김
이를 다음과 같이 표시할 수 있음(실행 컨텍스트 이름, environment record 객체) -> [ Global, { a, outer } ]

231번째 줄에서 inner 실행 컨텍스트의 environment record에 { a } 식별자를 저장함
outerEnvironmentReference에는 inner 함수가 선언될 당시의 렉시컬 환경이 담김
inner 함수는 outer함수 내부에서 선언되었으므로 outer 함수의 렉시컬 환경([ outer, { inner } ])를 참조복사함

그리고 스코프 체인 상에 있는 변수라고 해서 무조건 접근 가능한 것은 아님
위의 예시에서 식별자 a는 전역에서도 선언하고 inner 함수 내부에서도 선언함. inner 함수 내부에서 식별자 a에 접근하려면 무조건 inner 스코프의 렉시컬 환경부터 검색할 수 밖에 없음
inner 스코프의 렉시컬 환경에 식별자 a가 존재하므로 스포크 체인 검색을 더이상 진행하지 않고 inner 함수의 렉시컬 환경에 있는 a를 반환함
즉, inner 함수 내부에서 a 변수를 선언했기 때문에 전역 공간에서 선언한 동일한 이름의 a 변수에는 접근할 수 없음(변수 은닉화)
