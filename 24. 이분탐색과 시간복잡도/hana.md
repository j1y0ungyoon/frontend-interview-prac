# 이분탐색이 무엇이고 시간복잡도는 어떻게 되며 그 이유는 무엇인가요?

## 이분 탐색(binary search, 이진탐색이라고도 함)

<img src="https://blog.penjee.com/wp-content/uploads/2015/04/binary-and-linear-search-animations.gif" width="50%" height="50%">

- 정렬되어 있는 리스트에서 탐색 범위를 절반씩 좁혀가며 데이터를 탐색하는 방법을 말함.
- 이분 탐색을 사용하기 위해서는 배열 내부의 데이터가 오름차순이나 내림차순으로 정렬되어있어야 함(일정한 규칙으로 정렬되어있는 데이터일때만 가능)
- 찾으려는 데이터와 중간에 있는 데이터를 반복적으로 비교해 원하는 데이터를 찾음
- 시간 복잡도는 O(logN)임
  단계마다 탐색 범위를 절반씩 줄이고 O(logN)의 시간 복잡도를 가짐.
  ex: 데이터 개수 32개 -> 1단계 탐색을 거치면 16개가 남음 -> 2번째 탐색을 거지면 8개 -> 3단계는 4개의 데이터가 남음

## 이분 탐색(이진 탐색) 코드 구현

```
// 반복분

const FindNum = (arr, target) => {
    let sortedArr = arr.sort((x,y) => x - y)

    let currentMin = 0;
    let currentMax = newArr.length - 1

    while(currentMin <= currentMax) {
      let mid = Math.floor((currentMin + currentMax) / 2 )
        if(newArr[mid] === target) return true
        else if(newArr[mid] < target) {
            currentMin = mid + 1
        } else {
            currentMax = mid - 1
        }

    }
    return false;
}

console.log(findNumber([0, 3, 5, 6, 1, 2, 4], 2));

```

```
// 재귀 함수

const findNum = (arr, target, min = 0; max = arr.length - 1) => {

if(min > max) return false
// 해당 요소를 찾지 못한 경우

const mid = Math.floor((min + max) / 2)

if(arr[mid] === target) return true;
else if(arr[mid] < target) {
    return fundNum(arr, target, mid + 1, max)
} else {
    return findNum(arr, target, min, mid - 1)
}


}

console.log(findNumber([1,2,3,4,5,6,7,11,54,76,100], 7));
```

**참고**
- https://yoongrammer.tistory.com/75
- https://velog.io/@kimdukbae/%EC%9D%B4%EB%B6%84-%ED%83%90%EC%83%89-%EC%9D%B4%EC%A7%84-%ED%83%90%EC%83%89-Binary-Search
- https://cjh5414.github.io/binary-search/
- 스파르타 알고리즘 강의