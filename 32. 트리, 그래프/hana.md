# 트리, 그래프를 비교하여 설명해주실 수 있을까요?

## 그래프

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbAlVNh%2FbtreKxkxoN3%2FH47hLGhJOVKlQs8cBD7DM1%2Fimg.png" width="50%" height="50%">

노드(하나의 점)와 노드 간을 연결하는 간선으로 구성된 자료구조를 의미함
이를 통해 연결된 노드 간의 관계를 표현할 수 있음

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FP1Nld%2FbtrdqjmmZA7%2F0ew4riVr56lBekTHsf9uhK%2Fimg.png">

참고로 그래프에서는 하나의 점을 정점(vertex)라고 표현하고 하나의 선은 간선(edge)라 함

### 특징

- 그래프는 순환 또는 비순환 구조를 가짐
- 그래프는 방향이 있는 그래프와 방향이 없는 그래프가 있음
- 루트 노드의 개념이 X -> 부모 자식 관계라는 개념 X
- 2개 이상의 경로가 가능함(무방향, 단방향, 양방향)
- 그래프는 네트워크 모델임

### 관련 용어

- 비가중치 그래프 : 간선이 두 정점을 이어주고 있다는 사실만 알려주고 그 외의 정보는 포함하고 있지 않은 그래프
- 가중치 그래프 : 간선에 더 자세한 정보를 포함하고 있는 그래프
- 무방향 그래프 : 정점 간 이동이 양쪽으로 다 가능한 그래프
- 단방향 그래프 : 한 쪽으로만 이동이 가능한 그래프
- 진입차수(in-degree), 진출 지수(out-degree) : 한 정점에 진입(들어오는 간선)과 진출(나가는 간선)하는 간선이 몇 개인지를 나타냄
- 인접 : 두 정점 간에 간선이 직접 이어져 있다면 이 두 정점은 인접한 정점임
- self loop : 정점에서 진출하는 간선이 곧바로 자기 자신에게 진입하는 경우 self loop를 가졌다고 함. 다른 점점을 거치지 않는 다는 것이 특징임
- 사이클 : 한 정점에서 출발해 다시 해당 정점으로 졸아갈 수 있다면 사이클이 있다고 표현함
- 인접 행렬 : 서로 다른 정점들이 인접한 상태인지를 표시한 행렬로, 2차원 배열의 형태로 나타냄

### 실사용 예시

- 포탈사이트의 검색 엔진
- SNS 팔로우
- 네비게이션 길 찾기 등
  [여기](https://hyojin96.tistory.com/entry/%EA%B7%B8%EB%9E%98%ED%94%84%EC%99%80-%ED%8A%B8%EB%A6%AC%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90) 참조하기

## 트리

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FlOMaH%2Fbtrdr4Wjnaq%2Fr4IfAJlxkfhS3fCFMaaRZk%2Fimg.png" width="50%" height="50%">

그래프와 같이 노드와 노드 간을 연결하는 간선으로 구성된 자료구조임
그러나 트리는 두 개의 노드 사이에 반드시 1개의 경로만을 가지며, 사이클이 존재하지 않는 방향 그래프임 -> 이러한 특성 때문에 `최소 연결 트리`로 부르기도 함
두 개의 노드가 상하계층으로 연결되면 부모 자식 관계가 성립하기 때문에 `계층형 모델`로 부르기도 함
자식이 없는 노드는 리프 노드(leaf node)임

## 특징

- 부모-자식 관계가 존재하기 때문에 최상위에 Root노드가 존재함
- 노드가 N개 이면 간선은 N-1개, 각 레벨 k에 존재하는 노드는 2^k개(완전 이진 트리의 경우에)
- 방향성이 존재하고 사이클은 존재하지 않음(비순환)
- 트리의 순회는 전위순회, 중위순회, 후위순회 3가지가 존재함

### 관련 용어

- BST(binary search tree, 이진 탐색 트리)
  이진 트리는(binary tree)는 자식 노드가 최대 두 개인 노드들로 구성된 트리임. 이 두개의 자식 노드는 왼쪽 자식 노드와 오른쪽 자식 노드로 나눌 수 있음. 이진 탐색 트리는 모든 왼쪽 자식의 값이 루트나 부모보다 작고, 모든 오른쪽 자식의 값이 루트나 부모보다 큰 값을 가지는 특징이 있음

- BFS(breadth-first-search)
  너비를 우선적으로 탐색하는 방법. 모든 정점(노드)들을 한 번씩 방문(탐색)하는 것이 목적임. 주로 두 정점 사이의 최단 경로를 찾을 때 사용. 코드 상에서 queue와 단짝

- DFS(depth-first-search)
  깊이를 우선적으로 탐색하는 방법. 모든 정점(노드)들을 한 번씩 방문(탐색)하는 것이 목적임. 하나의 경로를 끝까지 탐색한 후, 찾는 것이 아니라면 다음 경로로 넘어가 탐색. 하나의 노선을 끝까지 들어가서 확인하고 다음으로 넘어가기 때문에 운이 좋다면 몇 번 만에 경로를 찾을 수 있음.

=> DFS와 BFS는 모든 정점을 한 번만 방문(탐색)한다는 공통점을 가지고 있지만 탐색 순서가 다르기 때문에 해당 상황에 맞는 것을 선택해 사용해야 함

### 실사용 예시

- 컴퓨터 디렉토리 구조, 토너먼트 대진표, 가계도, 조직도 등
  [여기](https://hyojin96.tistory.com/entry/%EA%B7%B8%EB%9E%98%ED%94%84%EC%99%80-%ED%8A%B8%EB%A6%AC%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90) 참조하기

**참고**

- https://tobegood.tistory.com/entry/day29
- https://hyojin96.tistory.com/entry/%EA%B7%B8%EB%9E%98%ED%94%84%EC%99%80-%ED%8A%B8%EB%A6%AC%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90
- https://bigsong.tistory.com/33
- https://gusrb3164.github.io/computer-science/2021/04/16/graph,tree/
- [책] 기초부터 시작하는 코딩 생활
