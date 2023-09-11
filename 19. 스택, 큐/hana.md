# 스택, 큐에 대해 설명해주실 수 있을까요?

## 스택(Stack)

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FQLmgD%2FbtriadO71zj%2Fy3IaQwptHsuFd4dJjKOQBK%2Fimg.png" width="50%" height="50%">

- 나중에 들어간 데이터가 가장 먼저 나오는 방식(후입선출, LIFO)을 사용하는 자료구조 방식임.
- 그림과 같이 아래에서 위로 쌓이는 형식이며 최근에 들어온 자료를 top이라 부름.
- 자료의 삽입과 삭제는 top에서만 이루어지게 됨.
- 스택이 비어있을 떄 자료를 꺼내려고 하면 스택 언더플로우(stack underflow) 발생
- 스택이 꽉 차있을 떄 자료를 넣으려고 하면 스택 오버플로우(stack overflow) 발생

### 사용 예시

주로 함수 호출과 관련된 작업에서 많이 사용됨.

- 컴퓨터 가상 메모리의 스택 영역에서 사용되는데, 함수가 호출되면서 다시 복귀할 주소를 저장하거나 지역변수, 매개변수 등을 임시로 저장하는 데 쓰임.
- 브라우저에서 앞으로 가기 혹은 뒤로 가기 기능.
- ctrl + z : 가장 나중에 수정한 내역부터 되돌리기
- 재귀적 알고리즘
- 후위 표기법 계산

## 큐(Queue)

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FuXE7T%2Fbtrh94R1tqR%2FxQ5Ig2jbUlaEx0AZyVKTZK%2Fimg.png" width="50%" height="50%">

- 먼저 들어온 데이터를 먼저 내보내는 방식(선입선출, FIFO)을 사용하는 리스트임.
  예를 들어 티켓팅 사이트에 사용자들이 많이 접속함 -> 대기열 발생 -> 먼저 접속한 사람부터 차례대로 사이트에 접속시켜야함 -> 큐 사용
- 위의 그림과 같이 rear 부분에서 자료의 삽입, front 부분에서 자료의 삭제가 이루어짐

### 사용 예시

작업처리, 대기열 처리 등 데이터를 순차적으로 처리해야 하는 경우 적합함.

- 프린터 인쇄 대기열 : 가장 먼저 대기열에 오른 프린트가 먼저 출력
- 컴퓨터 운영체제의 task scheduling

**참고**

- https://cocoon1787.tistory.com/691
- https://coduking.com/entry/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%EB%B0%B0%EC%97%B4-%EC%97%B0%EA%B2%B0%EB%A6%AC%EC%8A%A4%ED%8A%B8-%EC%8A%A4%ED%83%9D-%ED%81%90
- [책] 기초부터 시작하는 코딩 생활
