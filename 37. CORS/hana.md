# CORS란 무엇이고 어떻게 허용할 수 있나요?

브라우저는 다른 출처의 HTTP 요청을 제한함.
즉, 브라우저는 원래 동일한 출처(origin)의 리소스만 불러올 수 있음

여기서 출처란 아래와 같음

## 출처(origin)

<img src="https://media.geeksforgeeks.org/wp-content/uploads/20210625160610/urldiag.PNG">

- protocol(scheme) : http, https
- Host(subDomain + domain + top level domain) : 사이트 도메인
- port number : 포트 번호
- path : 사이트 내부 경로
- query string : 요청의 key, value 값
- fragment : 해시태그

여기서 origin(출처)은 protocol + Host + port 를 말함.
즉, 프로토콜과 호스트, 포트넘버까지 함친 url을 의미함

## 동일 출처 정책(SOP, same origin policy)

말 그대로 동일한 출처에 대한 정책을 의미함.
즉, 동일한 출처에서만 리소스를 공유할 수 있고 다른 출처 서버(cross-origin)에 있는 리소스는 상호작용이 불가능함

SOP가 필요한 이유는 만약 출처에 대한 제약이 없다면 제 3자가 CSRF나 XSS 등의 방법을 통해 개인 정보 등을 해킹하는 등의 경우가 발생할 수 있기 때문임.

따라서 다른 출처의 스크립트가 실행되지 않도록 브라우저 단에서 미리 방지하는 것임

## 출처의 구분 기준

같은 origin인지 아닌지는 위에서 말했다싶이 protocol + Host + port 가 동일하다면 그 뒤의 다른 요소들이 달라도 같은 origin으로 판단함. 하지만 하나라도 다를 경우 정책상 바로 차단함

=> 하지만 인처넷 환경이 커지고 웹페이지에서 다른 출처에 있는 리소스를 가져와 사용하는 일이 많아짐 -> 예외 조항을 두고 다른 origin의 리소스 요청이라도 해당 조항을 만족하는 경우 허용하기로 함(CORS)

## CORS(cross origin resource sharing)

다른 출처의 리소스 공유에 대한 정책임
CORS 정책에 해당하는 요청은 다른 origin이어도 리소스 사용을 허용함.

## CORS를 허용하려면 어떻게 해야할까?

1. 서버에서 Access-Control-Allow-Origin 헤더 셋팅

`Access-Control-Allow-Origin : https://www.myWebSite.com`

CORS를 허용하려면 서버에서 위처럼 Access-Control-Allow-Origin
헤더에 허용할 출처를 기재에 클라이언트에 응답하면 됨.

**CORS와 관련된 HTTP 헤더값**

```
# 헤더에 작성된 출처만 브라우저가 리소스를 접근할 수 있도록 허용함.
# * 이면 모든 곳에 공개되어 있음을 의미한다.
Access-Control-Allow-Origin : https://naver.com

# 리소스 접근을 허용하는 HTTP 메서드를 지정해 주는 헤더
Access-Control-Request-Methods : GET, POST, PUT, DELETE

# 요청을 허용하는 해더.
Access-Control-Allow-Headers : Origin,Accept,X-Requested-With,Content-Type,Access-Control-Request-Method,Access-Control-Request-Headers,Authorization

# 클라이언트에서 preflight 의 요청 결과를 저장할 기간을 지정
# 60초 동안 preflight 요청을 캐시하는 설정으로, 첫 요청 이후 60초 동안은 OPTIONS 메소드를 사용하는 예비 요청을 보내지 않음.
Access-Control-Max-Age : 60

# 클라이언트 요청이 쿠키를 통해서 자격 증명을 해야 하는 경우에 true.
# 자바스크립트 요청에서 credentials가 include일 때 요청에 대한 응답을 할 수 있는지를 나타냄
Access-Control-Allow-Credentials : true

# 기본적으로 브라우저에게 노출이 되지 않지만, 브라우저 측에서 접근할 수 있게 허용해주는 헤더를 지정
Access-Control-Expose-Headers : Content-Length
```

단, Access-Control-Allow-Origin 헤더 값으로 \*을 사용한다면 모든 출처의 요청을 허용한다는 의미이므로 보안이 취약해질 수 있음. 따라서 origin을 직접 명시하는게 좋음

2. 프록시
   만약 프론트 단에서 서버에 응답을 보냈는데 CORS에러가 발생했음. 그런데 서버에서 당장 설정하기 힘들다면 **프록시**를 사용하면 됨.
   라이브러리나 간단하게 설정하는 방법이 많으니 구글링 ㄱㄱ

**참고**

- https://inpa.tistory.com/entry/WEB-%F0%9F%93%9A-CORS-%F0%9F%92%AF-%EC%A0%95%EB%A6%AC-%ED%95%B4%EA%B2%B0-%EB%B0%A9%EB%B2%95-%F0%9F%91%8F
- https://www.geeksforgeeks.org/components-of-a-url/
- https://hannut91.github.io/blogs/infra/cors
