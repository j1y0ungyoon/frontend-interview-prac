# Base64 인코딩이란 무엇인가요?

## 인코딩

정보의 형태나 형식을 다른 형태나 형식으로 변환하는 방식.
왜 굳이 다른 형태나 형식으로 변환함? -> 표준화, 보안, 처리 속도 향상, 저장 공간 절약 등의 이유로 인해서 변환함

## Base64 encoding

- 바이너리데이터(이진데이터)(ex: image/audio)를 플랫폼이나 시스템에 영향을 받지 않는 공통 ASCII(아스키) 영역의 문자열로만 이루어진 텍스트로 바꾸는 인코딩을 뜻함
- Base64는 64진법의 의미를 가져서 2의 6제곱(2^6=64)인 64로 아스키 문자들을 표시할 수 있는 가장 큰 진법임 -> 데이터 교환에 많이 사용됨
- 웹에서 이미지를 표현하거나 전자 메일을 통한 바이너리 데이터를 전송하는 등의 역할로 사용됨
- Base64 인코딩을 하게 되면 전송해야 할 데이터의 용량이 33% 정도 늘어남 -> 왜냐하면 6bit당 2bit의 오버헤드가 발생하기 때문임

## Base64 사용하는 이유

33%나 용량이 커짐에도 사용하는 이유는 아래와 같음

ASCII는 7 bit encoding 이고 나머지 1bit를 처리하는 방식이 시스템 별로 다름. 그렇기 때문에 ASCII로 바이너리 데이터(image, audio)를 전송할 때 오류가 발생할 수 있음. 따라서 다른 시스템 간의 데이터 전송에 영향을 받지 않고 오류 없이 데이터를 전송하기 위해 ASCII 중 제어문자와 특수문자를 제외한 64개의 기본문자를 사용해 base64 인코딩을 함

```
ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/=
```

## Base64 인코딩 순서

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F25352636534CDDCB1C" width="50%" height="50%">
// 아스키 테이블

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F273BDB35534CDE1F16" width="50%" height="50%">
// base64 테이블

- 각 문자를 아스키 테이블과 매핑해 각 문자의 ASCII 값 찾기
- 2진수 변환
- 6bit 단위로 잘라 4개의 블록으로 만듦
- 6bit 단위 블록을 10진수 변환해 정수로 만듦
- Base64 테이블 매핑
- 패딩 연산(총 비트수를 3으로 나눴을 때 나머지가 0이면 =을 안 붙이고, 나머지가 1이면 =을, 2이면 ==을 붙임)

예를 들면 'Sun'이라는 문자가 있다면 과정은 아래와 같음

1. 각 문자의 ASCII 값 찾기
   S, u, n의 아스키 값은 아스키 테이블의 값과 매핑해보면 각각 83, 117, 110임

2. 2진수 변환
   'S'는 01010011, 'u'는 01110101,'n'는 01101110이 됨

3. 6bit 단위로 잘라 4개의 블록으로 만듦
   6bit씩 자르면 010100, 110111, 010101, 101110이 됨
   여기서 남는 비트가 있다면 0을 채우고 패딩(=)을 표시함

4. 6bit 단위 블록을 10진수 변환해 정수로 만들고 base64 테이블과 매핑해 해당하는 문자열을 찾음
   첫 번째 블록인 010100은 정수로 변환하면 20, Base64 테이블에서 인덱스가 "20"인 문자는 "U"
   두 번째 블록인 110111은 정수로 변환하면 "27", 해당하는 Base64 문자는 "b"
   세 번째 블록인 010101을 정수로 변환하면 "21", 해당하는 Base64 문자는 "V"
   마지막 블록인 101110을 정수로 변환하면 "46", 해당하는 Base64 문자는 "+"

=> 'Sun'을 인코딩한 결과는 'U3Vu'임

## 자바스크립트에서의 Base64 인코딩 & 디코딩 -> 별로 중요하지 않음 걍 넣었음

- atob() : Base64로 인코딩된 문자열을 디코딩함
- btoa() : 바이너리 데이터 문자열레서 Base64로 인코딩안 ASCII 문자열 생성

```
let word = 'Hello';

let encodingString = btoa(word)
console.log(encodingString) // 'SGVsbG8='

let decodingString = atob(encodingString)
console.log(decodingString) // 'Hello'

```

## 프론트엔드에서의 Base64

1. 이미지 인코딩
   웹 어플리케이션에서 이미지를 base64 문자열로 인코딩해 HTML 또는 css 내부에 직접 삽입할 수 있음. 이런 방법을 사용하면 이미지 파일을 따로 서버에서 가져올 필요없이 페이지가 로드될 때 이미지도 함께 로드되므로 HTTP 요청의 수를 줄일 수 있음(근데 많이 사용되는 방법은 아닌듯)

```
<img src="data:image/jpeg;base64,/9j/4AAAQSkZ....."/>
```

2. 데이터 전송
   클라이언트와 서버 간의 바이너리 데이터를 안전하게 전송해야하는 경우 해당 데이터를 base64 형식으로 인코딩하여 전송할 수 있음

3. 로컬 스토리지에 복잡한 데이터 저장
   로컬스토리지에는 오직 문자열만 저장이 가능함. 그래서 예를 들면 사용자가 작성 중인 내용을 임시 저장할 경우 base64로 인코딩된 형태로 로컬스토리지에 보관할 수 있음. 또한 사용자가 업로드한 이미지나 파일을 미리보기 할 때 임시로 메모리 상에 보관해야하기 때문에 이럴 때도 바이너리 데이터인 파일을 base64 형식으로 변환해 처리하기 때문임

## 주의점

Base64는 암호화 방법이 아니기 떄문에 보안 목적으로 사용하고자 한다면 다른 방법을 찾자

## 결론

Base64는 HTML 또는 이메일과 같이 문자를 위한 미디어에 바이너리 데이터를 포함해야할 필요가 있을 때 포함된 바이너리 데이터가 시스템에 영향을 받지 않고 동일하게 전송 또는 저장되는 걸 보장하기 위해 사용됨

**참고**

- https://kr.moyens.net/android/226464/
- https://developer.mozilla.org/en-US/docs/Glossary/Base64
- https://itstory.tk/entry/%EC%9D%B8%EC%BD%94%EB%94%A9%EC%9D%B4%EB%9E%80-ASCII-URL-HTML-Base64-MS-Script-%EC%9D%B8%EC%BD%94%EB%94%A9
- https://melonicedlatte.com/2023/01/24/193600.html
