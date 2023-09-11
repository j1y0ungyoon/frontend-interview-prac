# 실행 컨텍스트가 무엇인가요?

실행할 코드에 제공할 환경 정보를 모아놓은 객체를 말함.
동일한 환경에 있는 코드들을 실행할 때 필요한 환경 정보들을 모아 컨텍스트를 구성하고 이를 콜스택에 쌓아 올렸다가 가장 위에 있는 컨텍스트와 관련된 코드를 실행하는 식으로 전체 코드의 환경과 순서를 보장함

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
