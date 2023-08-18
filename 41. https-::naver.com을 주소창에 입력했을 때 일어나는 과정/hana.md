# https://naver.com을 주소창에 입력했을 때 일어나는 과정에 대해 아는 만큼 설명해주실 수 있을까요?

<img src="http://www.tcpschool.com/lectures/img_webbasic_10.png" width="50%" height="50%">

**IP 주소**

- 많은 컴퓨터들이 인터넷 상에서 서로를 인식하기 위해 지정받은 식별용 번호.
- 현재는 IPv4 버전(32비트)로 구성되어 있음. 127.0.0.1 같은 형식의 주소를 말함
- IPv4 주소의 부족 -> IPv6가 생김.
  IPv6는 128비트로 구성되어 있기 때문에 IP 주소가 부족하지 않다는 특징이 있음

**도메인 네임**

- ip 주소를 문자로 표현한 주소를 말함
- 사람의 편의성을 위해 만든 주소이기 때문에 실제로는 컴퓨터가 이해할 수 있는 ip 주소로 변환하는 작업 필요함

**DNS(domain name system)**

- 도메인 주소와 ip 주소를 매핑한 서버
- 도메인 네임으로 입력하면 DNS를 사용해 컴퓨터는 ip 주소를 받아 해당하는 주소로 찾아갈 수 있음

## 동작 과정

1. 사용자 입력가 브라우저 주소창에 도메인(www.naver.com)을 입력하고 엔터 누름
   해당 키워드가 url 형식이라면 해당 주소의 페이지를 요청하러 감
   만약 아니라면 사용하고 있는 브라우저의 기본 검색 엔진을 통해 해당 키워드 검색함

2. 웹페이지 url

<img src="https://amunre21.github.io/assets/images/web/url.png">
위와 같은 형식이 바로 url임. 
protocol 부분을 생략하고 바로 도메인을 입력하게 되면 기본적으로 http 요청을 함

3. 도메인 네임 요청
   도메인 네임을 요청하기 전에 우선 캐시\* 된 정보가 있는지 순서대로 아래의 캐시들에서 먼저 찾아봄

\*캐시
자주 쓰이는 데이터들을 저장해놓은 곳. 이를 사용하면 다른 처리 없이 빠른 속도로 정보를 읽을 수 있음.

0. 로컬 PC의 hostFile
   일부 도메인에 대한 ip 주소가 이 파일에 직접 저장되어 있을 수도 있기 때문에 먼저 살펴봄

host file에서 찾지 못했다면 브라우저는 아래의 캐시들을 순서대로 확인함

1.  브라우저 캐시
    예전에 네이버를 방문했다면 브라우저에 해당 도메인에 관한 DNS에 관련된 데이터가 캐시되어 있을 것임

2.  OS 캐시
    브라우저 캐시에서 찾지 못했다면 OS 캐시에서 찾음.
    말 그대로 운영체제 안에 있는 캐시로 systemcall을 통해서 접근 가능

3.  라우터 캐시
    라우터란 집에서 사용하는 공유기라고 생각하면 됨.
    라우터에서도 DNS 내용을 저장하기 때문임

4.  ISP(internet service provider) 캐시
    인터넷을 제공하는 회사

ISP 캐시에도 매핑된 ip 주소를 찾을 수 없다면 ISP의 DNS 서버(그림에서는 DNS)에 DNS 쿼리를 보내야함.
아래의 이미지처럼 순서대로 DNS 서버에서 도메인 주소를 가지고 ip 주소를 찾는데, 이를 recursive search 라고 하고, 이런 일은 ISP의 DNS recursor가 담당하고 다른 DNS에 물어보는 역할을 함

<img src="https://amunre21.github.io/assets/images/web/dns_work.jpg" width="50%" height="50%">

DNS 서버에 도메인 네임을 전달하고 isp DNS 서버 recursor가 루트 DNS 서버로 물어보러 감
루트 DMS 서버에서는 naver.com에서 `.com`을 분류해서 `com DNS 서버`로 연결해줌
왜냐하면 최상위 DNS는 국가 코드(kr, eu)나 net, com 같은 일반 도메인 코드로 나뉘기 때문에 이것만 구별해서 그 다음 하위의 DNS 서버로 연결해줌

<img src="https://amunre21.github.io/assets/images/web/dns_architect.gif">
도메인도 위와 같이 계층화 되어있음

<img src="https://amunre21.github.io/assets/images/web/dns_work.jpg" width="50%" height="50%">
루트 DNS가 연결해준 com DNS 서버로와서 naver.com는 어디로 가야하는지 물어봄
그럼 dom DNS는 가비아 DNS 서버에서 해당 도메인(nver.com)을 가지고 있다고 알려주면서 가비아의 DNS 서버를 연결해줌
그 후 가비아 DNS 서버에 도착해서 naver.com에 대한 정보가 있는지 물어보고 해당 도메인 네임에 관한 ip주소를 얻음

4. ip 주소 획득
   3번에서 naver.com에 대한 ip 주소(125.209.222.141)을 얻음

5. 브라우저가 TCP/IP 프로토콜을 사용해 서버에 연결
   ip 주소를 알았으니 이제 TCP/IP 프로토콜을 사용해 서버에 연결을 할 차례임.
   TCP 연결을 할 때 3-way-handshake를 사용함

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F225A964D52F1BB6917">

자세한 설명은 3-way-handshake 부분을 참고하셈

6. HTTP response & HTTP request
   TCP 연결이 된 후 브라우저는 HTTP request를 서버로 전송함. (필요한 경우 HTTPS 보안 통신이 진행됨). 서버가 요청을 처리하고 HTTP response를 다시 웹 브라우저로 전송함
7. 웹 브라우저가 전송 받은 HTML문서를 파싱하고 DOM을 생성함
8. 중간에 css를 로드하는 link혹은 style 태그를 만나면 DOM 생성을 중지함
9. CSS를 파싱하고 CSSOM을 생성함
10. 이렇게 만들어진 DOM와 CSSOM은 렌더링(브라우저에 시각적으로 출력하는 것)을 위해 렌더 트리로 결합됨
11. 만약 script 태그를 만나면, css와 동일하게 JS코드를 실행하기 위해 파싱을 중단함
12. 이후 JS엔진을 실행하고 JS코드를 파싱함 -->
    여기서 script태그를 만날때마다 파싱이 중단되는 문제를 script 태그 뒤에 async 혹은 defer를 붙여줌으로써 해결할 수 있다.
    async: HTML 파싱, JS 파일 로드가 동시에 진행
    defer: DOM 생성이 완료된 직후, JS의 파싱과 실행이 진행된다.

---

**~정리~**

1. 사용자가 주소창에 "www.naver.com"을 입력하고 엔터를 누름
2. 브라우저는 URL 형식이 맞는지 확인함. 프로토콜 정보가 생략되어 있다면 기본적으로 "http://"를 사용함
3. 도메인 네임을 찾기 위해 로컬 pc의 hostFile을 확인함 -> 호스트파일에서 찾지 못했다면 브라우저는 로컬 캐시, OS 캐시, 라우터 캐시, ISP 캐시 순서로 확인함. 모든 캐시에서 찾지 못했다면 ISP의 DNS 서버에 요청을 보내고 최종적으로 도메인 네임에 대한 IP 주소를 찾게 됨
4. IP 주소를 얻었다면 브라우저는 TCP/IP 프로토콜을 사용하여 (ex: 125.209.222.141) 해당 서버와 TCP 연결(3-way-handshake)을 수행함
5. 연결이 된 후, 브라우저는 HTTP 요청을 서버로 전송함. 서버는 요청을 처리한 다음 HTML과 같은 응답 데이터를 클라이언트로 다시 전송함
6. 웹 브라우저는 전송받은 HTML 문서를 파싱하여 DOM을 생성함.
   HTML 파싱 도중 CSS 및 링크 태그를 만나면 파싱을 잠시 중단하고 CSS를 파싱하여 CSSOM을 생성함
7. DOM과 CSSOM을 결합하여 렌더 트리를 구성함
8. 만약 script 태그를 만나면, css와 동일하게 JS코드를 실행하기 위해 파싱을 중단함
   이후 JS엔진을 실행하고 JS코드를 파싱함 -->
   여기서 script태그를 만날때마다 파싱이 중단되는 문제를 script 태그 뒤에 async 혹은 defer를 붙여줌으로써 해결할 수 있음

   - async: HTML 파싱, JS 파일 로드가 동시에 진행
   - defer: DOM 생성이 완료된 직후, JS의 파싱과 실행이 진행됨

9. 렌더 트리를 활용해 브라우저 창에 웹페이지를 시각적으로 그림

**참고**

- https://aws.amazon.com/ko/blogs/korea/what-happens-when-you-type-a-url-into-your-browser/
- https://velog.io/@wkahd01/%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C-%EB%A9%B4%EC%A0%91-%EB%AC%B8%EC%A0%9C-%EC%9D%80%ED%96%89-HTML-%EC%A7%88%EB%AC%B8-%EB%8B%B5%EB%B3%80
- https://youtu.be/YahjHM9UNCA
- https://medium.com/sjk5766/dns-%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80%EC%97%90-%EC%9E%85%EB%A0%A5%ED%95%9C-url%EB%A1%9C%EB%B6%80%ED%84%B0-ip%EB%A5%BC-%EA%B0%80%EC%A0%B8%EC%98%A4%EB%8A%94-%EA%B3%BC%EC%A0%95-d31dbfe118da
- https://developer.mozilla.org/ko/docs/Learn/Common_questions/Web_mechanics/What_is_a_URL
- https://youtu.be/0Oqkw8wVY_c
- https://brunch.co.kr/@seungjoonlernnx/100
