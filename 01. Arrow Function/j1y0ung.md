# Arrow Function 이란 무엇인지 설명해주실 수 있을까요?

ES6 에 나온 자바스크립트를 함수로 만들 수 있는 문법으로 함수의 본연의 기능을 잘 표현한 문법이다.

함수의 본연의 기능이란, 여러가지 기능을 하는 코드를 하나의 단어로 묶는 기능, 입출력 기능 을 가능이다. 화살표 함수는 함수 본연의 입출력 기능을 직관적으로 잘 표현해준다.

화살표함수의 다른 점은 파라미터가 하나라면 소괄호가 생략 가능하다는 점과 중괄호 생갹이 가능하다는 점이 있다( 중괄호 안의 return이 한줄이라면 중괄호와 return 모두 생략이 가능하다).

화살표 함수를 사용하는 핵심 이유는 화살표 함수를 쓰면 내부에서 this 값을 쓸 때 밖에 있던 this값을 그대로 사용한다.

- 함수를 쓸 때 함수가 쓰인 위치에 따라 내부의 this값이 변하는데 화살표 함수는 내부의 this 값을 변화시키지 않는다. (바깥에 있던 this의 의미를 그대로 내부에서도 사용한다.)
