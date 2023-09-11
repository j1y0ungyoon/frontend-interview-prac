# 배열, 링크드리스트를 비교하여 설명해주실 수 있을까요?

## 링크드 리스트(linked list=list)

선형적으로 마치 체인처럼 연결된 연속된 노드(node)의 연결체를 의미함

노드란 링크드 리스트의 각 요소를 부르는 단어임. 노드는 실제 저장되는 값인 데이터 필드와 다음 노드를 가리키는 링크 필드로 구성되어있음.
링크드 리스트의 맨 처음 노드를 head, 마지막 노드를 tail이라 부름. 이 때 마지막 노드는 가리킬 노드가 없기 때문에 null을 가리킴

<img src="https://res.cloudinary.com/dvj2hbywq/image/upload/v1590572188/Group_14_5_bvpwu0.png" width="50%" height="50%">

## 배열과 링크드 리스트 특징 비교

### 데이터 탐색 속도

배열은 인덱스를 사용해 데이터 탐색을 효율적으로 빠르게 실행할 수 있음. O(1)의 시간복잡도 가짐
링크드 리스트는 노드들이 서로 연결되어 있기 때문에 처음부터 순차적으로 탐색해야함. 따라서 데이터 탐색이 느릴 수 있고, 최대 O(n)의 시간복잡도 가짐

### 메모리 사용

배열은 요소의 크기가 고정되어있고 메모리 오버헤드가 적음
링크드 리스트는 각 노드에 다음 데이터를 가리키는 링크 필드를 가지고 있기 때문에 배열보다 더 많은 메모리를 사용함

### 삽입 및 삭제

배열의 경우 요소를 삽입 혹은 삭제할 때 해당 인덱스 이후 모든 요소를 이동시켜야 하므로 비용이 높음
링크드 리스트는 배열과는 달리 구조의 큰 틀을 바꾸지 않고 노드를 추가하거나 삭제하기 쉬움.
(노드를 추가하려면 링크를 변경하면 되고 삭제하려면 이전 노드의 링크만 변경하면 되기 때문)

## linked list의 종류

### 단일 연결 리스트(singly linked list)

<img src="https://res.cloudinary.com/dvj2hbywq/image/upload/v1590572188/Group_14_5_bvpwu0.png" width="50%" height="50%">

기본적인 링크드 리스트를 말함

### 이중 연결 리스트(doubly linked list)

<img src="https://lamarr.dev/images/algorithm/linkedlist02.jpg" width="50%" height="50%">

단일 연결 리스크는 현재 노드에서 다음 노드로 갈 수 있지만 이전 노드로는 갈 수 없음. 이러한 점을 해결하기 위해 각 노드마다 다음 노드값과 이전 노드값이 무엇인지 저장된 두 개의 링크필드를 가지고 있는 이중 연결 리스트가 나옴. 앞뒤로 탐색이 빠르지만 두 개의 링크 필드로 인해 데이터 구조와 흐름이 복잡해질 수 있음

### 원형 연결 리스트(circular linked list)

<img src="https://lamarr.dev/images/algorithm/linkedlist03.jpg" width="50%" height="50%">

테일의 링크 필드가 헤드의 데이터 필드를 가리키고 있는 링크드 리스트를 말함. 리스트의 끝이 존재하지 않음

## 사용 예시

배열 : 데이터를 연속적으로 저장하고 빠른 접근이 필요한 경우 적합함
ex) 정렬된 데이터에 추가적인 데이터 저장, 특정 데이터 검색하는 작업

링크드 리스트 : 데이터의 추가, 삭제가 많이 일어나는 경우에 적합함
ex) 실시간으로 발생하는 로그 데이터를 저장할 떄

## 코드 예시 - javascript

```
// array
let arr = [1, 2, 3, 4, 5];
console.log(arr[2]); // 인덱스를 사용한 데이터 접근

arr.push(6); // 배열에 요소 추가
arr.pop(); // 배열에서 마지막 요소 삭제

// linked list
class Node {
  constructor(data) {
    this.data = data;
    this.next = null;
  }
}

class LinkedList {
  constructor() {
    this.head = null;
  }

  append(data) {
    const newNode = new Node(data);
    if (!this.head) {
      this.head = newNode;
    } else {
      let current = this.head;
      while (current.next) {
        current = current.next;
      }
      current.next = newNode;
    }
  }

  print() {
    let current = this.head;
    while (current) {
      console.log(current.data);
      current = current.next;
    }
  }
}

const myList = new LinkedList();
myList.append(1);
myList.append(2);
myList.append(3);
myList.print(); // 링크드 리스트의 데이터 출력

```

**참고**

- https://daniel-any.medium.com/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EB%A7%81%ED%81%AC%EB%93%9C-%EB%A6%AC%EC%8A%A4%ED%8A%B8-javascript-linked-list-809428f42e19
- https://www.freecodecamp.org/korean/news/implementing-a-linked-list-in-javascript/
- https://www.geeksforgeeks.org/data-structures/linked-list/
- [책] 기초부터 시작하는 코딩 생활
