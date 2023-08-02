# closure 설명해주세요.

클로저는 반환된 내부함수가 자신이 선언됐을 때의 환경(Lexical environment)인 스코프를 기억하여, 자신이 선언됐을 때의 환경(스코프)밖에서 호출되어도 스코프에 접근 할 수 있는 함수를 말한다.

자신을 포함하고 있는 외부함수보다 내부함수가 더 오래 유지되는 경우, 외부함수 밖에서 내부 함수가 호출되더라도 외부함수의 지역 변수에 접근 할 수 있는데
이러한 함수를 클로저라고 부른다.

** 클로저는 반환된 내부함수가 자신이 선언됐을 때의 환경(lexical environment)인 스코프를 기억하여 자신이 선언됐을 때의 환경 (스코프)밖에서 호출되어도 그 환경(스코프)에 접근할수 있는 함수를 말한다.**

## 클로저의 활용

### 상태유지

클로저가 가낭 유효하게 사용되는 상황은 **현재상태를 기억하고 변경된 최신 상태를 유지** 하는 것이다.

```js
var box = document.querySelector(".box");
var toggleBtn = document.querySelector(".toggle");

var toggle = (function () {
  var isShow = false;

  // ① 클로저를 반환
  return function () {
    box.style.display = isShow ? "block" : "none";
    // ③ 상태 변경
    isShow = !isShow;
  };
})();

// ② 이벤트 프로퍼티에 클로저를 할당
toggleBtn.onclick = toggle;
```

### 전역 변수 사용 억제

변수의 값은 누군가에 의해 언제든지 변경될 수 있어 오류 발생의 근본적 원인이 될 수 있다.
오류를 피하고 프로그램의 안정성을 높이기 위해 클로저는 적극적으로 사용 한다.

```js
var incleaseBtn = document.getElementById("inclease");
var count = document.getElementById("count");

var increase = (function () {
  // 카운트 상태를 유지하기 위한 자유 변수
  var counter = 0;
  // 클로저를 반환
  return function () {
    return ++counter;
  };
})();

incleaseBtn.onclick = function () {
  count.innerHTML = increase();
};
```

### 정보은닉

클로저는 직접적으로 변경되면 안 되는 변수에 대한 접근을 막아 준다.
