# Event bubbling 과 capturing 을 비교하여 설명해주실 수 있을까요?

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F99BE9C3B5EDBD71301" width="50%" height="50%">

## 버블링

아래와 같은 형태로 중첩된 구조가 있다고 가정하고, 각각의 요소에 이벤트 핸들러를 할당함

```
<style>
  body * {
    margin: 10px;
    border: 1px solid blue;
  }
</style>

<form onclick="alert('form')">FORM
  <div onclick="alert('div')">DIV
    <p onclick="alert('p')">P</p>
  </div>
</form>
```

가장 안쪽에 위치한 p 태그를 클릭하면 아래와 같이 작동함

- p 태그의 onClick 핸들러가 동작함
- 바깥 태그인 div의 onClick 핸들러가 동작함
- 그 위의 태그인 form에 할당된 onClick 핸들러가 동작함
- document 객체를 만날 때까지 각 요소에 할당된 onClick 핸들러가 동작함
  => 따라서 p만 눌렀을 뿐인데 3개의 alert 창이 뜸

이렇게 안쪽에 있는 자식 요소에서 시작해 부모 요소로 거슬러 올라가며 이벤트가 발생하는 것을 버블링이라고 함.

**모든 이벤트는 버블링 됨(default)**
focus와 같이 버블링 되지 않는 몇몇의 이벤트를 제외하고는 대부분의 이벤트는 버블링이 됨

### 버블링 중단하기

핸들러에게 이벤트를 완전히 처리하고 난 후 버블링을 중단하도록 명령할 수 있음
바로 `event.stopPropagation()`을 사용하면 됨

```
<style>
  body * {
    margin: 10px;
    border: 1px solid blue;
  }
</style>

<form onclick="alert('form')">FORM
  <div onclick="alert('div')">DIV
    <p onclick="event.stopPropagation()">P</p>
  </div>
</form>
```

p를 클릭해도 div의 onClick은 동작하지 않음

**event.stopPropagation()과 event.stopImmediatePropagation()의 차이점**

1. event.stopPropagation()
   현재 이벤트의 버블링 단계에서 이벤트 전파를 중단함. 즉, 현재 요소에서 발생한 이벤트가 상위 요소로 전파되는 것을 막음
2. event.stopImmediatePropagation()
   event.stopPropagation()과 비슷하지만 같은 이벤트 타입을 가진 핸들러들도 호출되지 않도록 함. 즉, 버블링이나 캡쳐링을 멈추고, 해당 요소에 등록된 다른 이벤트 핸들러들도 막을 때 사용하면 됨.
   이 메소드를 사용하면 요소에 할당된 특정 이벤트를 처리하는 이벤트 핸들러가 모두 동작하지 않음

**예시**

```
  <body>
    <div id="outer">
      Outer
      <button id="inner">Inner</button>
    </div>

    <script>
      document.querySelector("#outer").addEventListener("click", () => {
        alert("outer clicked!");
      });

      document.querySelector("#inner").addEventListener("click", (event) => {
        alert("inner clicked - First Handler");
      });

      document.querySelector("#inner").addEventListener("click", (event) => {
        alert("inner clicked - Second Handler");
      });
    </script>

```

위의 코드를 실행할 경우 First Handler -> Second Handler -> outer clicked 순으로 버블링이 일어날 것임

event.stopPropagation()을 첫 번째 이벤트 핸들러에 사용한 경우 outer clicked alert는 발생하지 않고 button에 등록된 이벤트 핸들러인 inner clicked - First Handler와 inner clicked - Second Handler만 나타날 것임.

즉, 부모 요소인 #outer로의 이벤트 전파(event propagation)은 막지만 동일한 내부의 다른 이벤트 핸들러는 계속 실행됨

```
  <body>
    <div id="outer">
      Outer
      <button id="inner">Inner</button>
    </div>

    <script>
      document.querySelector("#outer").addEventListener("click", () => {
        alert("outer clicked!");
      });

      document.querySelector("#inner").addEventListener("click", (event) => {
        event.stopPropagation()
        alert("inner clicked - First Handler");
      });

      document.querySelector("#inner").addEventListener("click", (event) => {
        alert("inner clicked - Second Handler");
      });
    </script>

```

그렇다면 부모 요소로의 이벤트 버블링도 막고 같은 요소 내의 다른 이벤트 핸들러도 막으려면 어떻게 해야할까? 바로 event.stopImmediatePropagation()를 사용하면 됨

```
  <body>
    <div id="outer">
      Outer
      <button id="inner">Inner</button>
    </div>

    <script>
      document.querySelector("#outer").addEventListener("click", () => {
        alert("outer clicked!");
      });

      document.querySelector("#inner").addEventListener("click", (event) => {
        event.stopImmediatePropagation()
        alert("inner clicked - First Handler");
      });

      document.querySelector("#inner").addEventListener("click", (event) => {
        alert("inner clicked - Second Handler");
      });
    </script>

```

#inner의 첫 번째 이벤트 핸들러에 event.stopImmediatePropagation()를 사용하면
"inner clicked - First Handler"라는 alert만 발생하고 #inner의 두 번째 이벤트 핸들러와 부모 요소인 outer의 이벤트도 발생하지 않음.

즉, stopImmediatePropagation()는 부모 요소인 #outer로의 이벤트 전파 뿐만 아니라 동일한 요소 내부의 다른 모든 이벤트 핸들러도 중단시킴

## 캡쳐링

<img src="https://miro.medium.com/v2/resize:fit:1400/format:webp/1*QKL7CUUibaldgWReBaakeA.png" width="50%" height="50%">

표준 DOM 이벤트에서 정의한 이벤트 흐름에는 3가지 단계가 있음

1. 캡쳐링 단계 : 이벤트가 하위 요소로 전파되는 단계
2. 타깃 단계 : 이벤트가 실제 타깃 요소로 전달되는 단계
3. 버블링 단계 : 이벤트가 상위 요소로 전파되는 단계

위의 이미지처럼 td 태그를 클릭하면 이벤트가 최상위 조상에서 시작해 아래로 전파(캡쳐링단계) -> 이벤트가 타깃 요소에 도착해 실행(타깃단계) -> 다시 위로 전파(버블링 단계)

이 과정을 통해 할당된 이벤트 핸들러가 호출됨
캡처링 단계를 이용해야하는 경우는 많지 않은데, 캡쳐링 단계에서 이벤트를 잡아내려면 addEventListener의 capture 옵션을 true로 설정하면 됨(default는 false로 버블링 단계에서 이벤트를 잡음)

```
elem.addEventListener(..., {capture: true})

혹은 아래와 같이 true만 써도 됨
elem.addEventListener(..., true)
```

**예시**

```
<style>
  body * {
    margin: 10px;
    border: 1px solid blue;
  }
</style>

<form>FORM
  <div>DIV
    <p>P</p>
  </div>
</form>

<script>
  for(let elem of document.querySelectorAll('*')) {
    elem.addEventListener("click", e => alert(`캡쳐링: ${elem.tagName}`), true);
    elem.addEventListener("click", e => alert(`버블링: ${elem.tagName}`));
  }
</script>

```

위의 예시에서 p 태그를 클릭하면 이벤트 흐름이 어떻게 되는지 살펴보자면 아래와 같음

1. HTML -> BODY -> FORM -> DIV (캡처링 단계, 첫 번째 이벤트 리스너가 실행되면서 왼쪽과 같이 alert가 발생함)
2. p (타깃 단계, 캡처와 버블링 둘 다에 이벤트 리스너를 설정했기 때문에 두 번 호출됨)
3. DIV -> FORM -> BODY -> HTML (버블링 단계, 두 번째 이벤트 리스너가 실행되면서 왼쪽과 같이 alert가 실행됨)

그럼 왜 버블링이나 캡쳐링 같은 이벤트 전파가 필요한 걸까?

이벤트 전파 개념이 없다면 모든 요소에 일일히 이벤트를 등록해야함
하지만 이벤트 전파가 있기 때문에 하나의 부모 요소에만 핸들러를 등록함으로써 코드의 중복을 중리고 메모리 사용량을 줄일 수 있음
ex: 리스트 아이템이 수백 개 있는 목록에서 각각의 아이템에 클릭 핸들러를 붙이는 것보다 리스트 전체에 한 번만 클릭 핸들러를 붙여서 클릭된 아이템을 판별하는 것이 효율적임

---

**정리**
이벤트의 전파 방향에 따라 버블링과 캡쳐링으로 구분함

- 버블링(bubbling): 자식 요소에서 발생한 이벤트가 바깥 부모 요소로 전파(기본값)
- 캡쳐링(capturing): 자식요소에서 발생한 이벤트가 부모 요소로부터 시작해 안쪽 자식 요소까지 도달

**참고**

- https://ko.javascript.info/bubbling-and-capturing
- https://medium.com/hcleedev/web-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%B2%84%EB%B8%94%EB%A7%81%EA%B3%BC-%EC%BA%A1%EC%B2%98%EB%A7%81-%EC%9C%84%EC%9E%84-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0-f85d9c728561
- https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-%EB%B2%84%EB%B8%94%EB%A7%81-%EC%BA%A1%EC%B3%90%EB%A7%81#1._%EB%85%BC%EB%A6%AC%EC%A0%81%EC%9D%B8_%EC%9D%B4%EC%9C%A0
