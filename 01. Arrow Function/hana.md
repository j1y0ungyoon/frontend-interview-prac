# Arrow Function 이란 무엇인지 설명해주실 수 있을까요?

기존의 function 키워드 대신 화살표를 사용해 간단한 방법으로 함수를 선언할 수 있습니다.
화살표 함수는 익명 함수로만 사용할 수 있어서 화살표 함수를 호풀하려면 함수 표현식을 사용해야 합니다

```
// ES5

var sum = function (x) {return x + x};
console.log(sum(10)); // 20

// ES6
const sum = x => x + x;
console.log(sum(10)) // 20

```

기존 화살표 함수와 function 키워드로 생성한 함수와 this에서 가장 큰 차이점을 보입니다

일반 함수의 this는 함수의 호출 방식에 따라 this에 바인딩할 객체가 동적으로 결정됩니다. 자세하게 말하자면 함수를 선언할 때
this에 바인딩할 객체가 정해지는 것이 아니고 함수를 호출할 때 함수가 어떻게 호출되었느냐에 따라서 this에 바인딩할 객체가 동적으로 결정됩니다
아래의 예시를 통해 살펴보겠습니다

```
function Prefixer(prefix) {
    this.prefix = prefix
}
// 이 곳에서의 this는 생성자 함수(Prefixer)가 새로 생성한 객체(이 코드에서는 아래의 pre)


Prefixer.prototype.prefixArray = function (arr) {
    return arr.map(function (x) {
        return this.prefix + ' ' + x
        // 이 곳에서의 this는 생성자 함수가 생성한 객체가 아니라 전역객체인 window임
        // 왜냐하면 생성자 함수와 메소드를 제외한 모든 함수(내부 함수, 콜백함수 등)의 내부의 this는 전역 객체(window)를 가리킴
    })
}

var pre = new Prefixer('hihi')
console.log(pre.prefixArray(['Lee', 'Kim']))
// ['undefined Lee', 'undefined Kim']
```

이러한 문제점을 해결하려면 다음과 같이 고칠 수 있습니다

// 1. that = this

```
function Prefixer(perfix) {
    this.prefix = prefix
}

Prefixer.prototype.prefixArray = function (arr) {
        var that = this // 여기서의 this는 Prefixer 생성자 함수의 인스턴스(이 코드에서는 pre. 즉, preficArrayfmf ghcnfgks wncp)
        return arr.map(function (x) {
            return this.prefix + ' ' + x
    })
}


var pre = new Prefixer('hihi')
console.log(pre.prefixArray(['Lee', 'Kim']))
// ['hihi Lee', 'hihi Kim']

```

// 2. map(func, thisArg)

```
function Prefixer(prefix) {
  this.prefix = prefix;
}

Prefixer.prototype.prefixArray = function (arr) {
  return arr.map(function (x) {
    return this.prefix + ' ' + x;
  }, this); // this: Prefixer 생성자 함수의 인스턴스(즉, prefixArray를 호출한 객체인 pre 인스턴스를 말함)
};

var pre = new Prefixer('Hi');
console.log(pre.prefixArray(['Lee', 'Kim']));

```

// 3. bind(this)

```
// Solution 3: bind(this)
function Prefixer(prefix) {
  this.prefix = prefix;
}

Prefixer.prototype.prefixArray = function (arr) {
  return arr.map(function (x) {
    return this.prefix + ' ' + x;
  }.bind(this)); // this: Prefixer 생성자 함수의 인스턴스
};

var pre = new Prefixer('Hi');
console.log(pre.prefixArray(['Lee', 'Kim']));

```

위와 같이 function 키워드로 선언된 함수는 선언할 때 this에 바인딩할 객체가 결정되는 게 아니라 함수를 호출할 때 함수가 어떻게 호출 되었는지에 따라서
this에 바인딩할 객체가 동적으로 결정됨. 이와 달리 화살표 함수는 선언할 때 this에 바인딩할 객체가 정적으로 결정됨.
화살표 함수의 this는 언제나 상위 스코프의 this를 가리킵니다(lexical this). 그래서 화살표 함수의 this 바인딩 객체의 결정 방식은
렉시컬 스코프와 유사합니다

```
function Prefixer(prefix) {
  this.prefix = prefix;
}

Prefixer.prototype.prefixArray = function (arr) {
  // this는 상위 스코프인 prefixArray 메소드 내의 this를 가리킴
  return arr.map(x => `${this.prefix}  ${x}`);
};

const pre = new Prefixer('Hi');
console.log(pre.prefixArray(['Lee', 'Kim']));
// 'Hi Lee', 'Hi Kim'
```

화살표 함수는 자신만의 this를 가지지 않고 외부 스코프의 this의 값을 그대로 가져다가 사용합니다
이 코드에서 prefixArray 메서드 내부에서 사용된 화살표 함수는 this가 정의된 스코프가 prefixArray 메서드이기 때문에, this는 prefixArray 메서드를 호출한 객체인 pre 인스턴스를 가리키게 됩니다.
