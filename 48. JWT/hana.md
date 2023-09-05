# JWT가 무엇인지 설명해주세요

# JWT가 무엇인지 설명해주세요

## JWT(Json web token)란?

JSON 웹 토큰은 일반적으로 클라이언트(프론트엔드)와 서버(백엔드) 등 두 엔터티 간에 정보를 공유하는 데 사용되는 개방형 표준을 말함.

여기에는 공유해야 하는 정보가 담긴 JSON 개체가 포함되어 있음. 또한 각 JWT는 클라이언트나 악의적인 공격자가(해커 등) JSON 콘텐츠(JWT 클레임이라고도 함)를 변경할 수 없도록 암호화(해싱)를 사용하여 서명됨(비밀키를 사용해 HMAC 알고리즘으로 서명되거나, RSA 또는 ECDSA를 사용해 공개/비공개 키-쌍으로 서명될 수 있음)

## JWT는 왜 필요할까?

인증 서버가 정보를 일반 JSON 객체로 보낸다면 클라이언트쪽 API에서 수신받는 JSON 콘텐츠가 올바른지 확인할 수 없음. 예를 들어 해커가 사용자 마음대로 JSON 콘텐츠 안에 있는 사용자 ID를 변경해도 클라이언트쪽 API는 이러한 변경이 생겼는지 알 수 없음.
이런 보안 문제로 인해 인증 서버는 클라이언트쪽에서도 확인할 수 있는 방식으로 정보를 전송해야하고 여기서 토큰\* 이라는 개념이 등장함

\*토큰

안전하게 확인할 수 있는 일부 정보가 포함된 문자열을 의미함.
ex: 데이터 베이스 ID를 가리키는 임의의 영문과 숫자를 합친 문자열의 집합
ex: 클라이언트에서 확인할 수 있는 인코딩된 JSON(JWT)

## JWT는 주로 언제 사용할까?

- 인증
  JWT를 일반적으로 사용하는 경우임.
  웹사이트에 로그인한 사용자가 다른 페이지로 이동하거나 특정 기능을 요청할 때마다 서버는 사용자가 누구인지 그리고 어떤 권한을 가지고 있는지 확인해야 함. 이때 JWT를 사용하면 효율적으로 이를 수행할 수 있음. 예를 들어 사용자가 로그인하면 서버는 해당 사용자의 정보와 권한 등을 담은 JWT를 생성하여 사용자에게 반환함. 이후 사용자의 모든 요청에는 이 JWT가 포함되어 서버로 전송되며, 서버는 이 토큰을 검증하여 해당 요청이 유효한지 확인함. 따라서 매번 데이터베이스 등을 조회하여 인증 정보를 확인하는 대신, 한 번 발급된 JWT로 빠르게 인증 작업을 수행할 수 있음.

- 정보 교환
  JWT는 당사자 간에 안전하게 정보를 전송하는 방법임.
  예를 들어 A 회사와 B 회사가 파트너 관계라고 가정함. A 회사에서 B 회사로 중요한 데이터를 안전하게 전송해야 하는 상황이 발생함. 여기서 A 회사는 공개/비공개 키 쌍 중 비공개 키로 JWT에 서명하고, B 회사에게 전달함. B 회사는 A회사의 공개키를 알고 있으므로 받은 JWT의 서명을 검증할 수 있음. 만약 JWT의 내용이 변경되었다면 공개키로 검증 시 실패하기 때문에 데이터 변조 여부도 체크할 수 있음. 따라서 양 사이에서 안전하게 정보를 주고받을 수 있으며 보내는 사람(A회사)의 신원도 보장받을 수 있음

## JWT의 구조

JWT는 아래의 세 부분으로 구성되며, 각 부분은 점(.)으로 분리됨

- header
- payload
- signature

```
header.payload.signature
```

<img src="https://supertokens.com/static/b0172cabbcd583dd4ed222bdb83fc51a/78612/jwt-structure.png" width="50%" height="50%">

1. header

JWT에서 사용할 타입과 해시 알고리즘 방식(signature 및 토큰 검증에 사용)이 담겨있음

2. payload
   토큰에서 사용할 정보의 조각들인 클레임(claim)이 담겨있음
   (클레임이란 key-value 형식으로 이루어진 한 쌍의 정보를 의미함)

페이로드는 크게 3가지 타입으로 나눌 수 있음

- registered claims(등록된 클레임)

  - iss(issuer: 발행자 )
  - exp(expiration time: 만료 시간)
  - sub(subject: 제목)
  - iot(issued at: 발행 시간)
  - jti(JWI ID)

  등의 정보를 담고 있음

- public claims(공개 클레임)
  사용자가 정의할 수 있는 클레임. 공개용 정보 전달을 위해 사용. 충돌 방지를 위해 URI 포맷을 사용함

  ```
  {
    "https://example.com": true
  }
  ```

- private claims(비공개 클레임)
  해당하는 당사자들 간에 정보를 공유하기 위해 만들어진 사용자 지정 클레임. 해당 유저를 특정할 수 있는 정보들을 담음

3. signature
   토큰을 인코딩하거나 유효성 검증을 할 때 사용하는 유니크한 암호화 코드임
   서명은 위에서 만든 헤더와 페이로드의 값을 각각 Base64 URL Encode하고, 인코딩한 값을 시크릿키와 합친 것을 헤더에서 정의한 알고리즘으로 암호화(해싱)함

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fc3oKza%2FbtqArSHlIuP%2FQQU4oljS5KxyNgv4651Nk1%2Fimg.png" width="50%" height="50%">

생성된 토큰은 HTTP 통신을 할 때 Authorization이라는 key의 value로 사용됨. 일반적으로 value에는 Bearer이 앞에 붙음

```
"Authorization": "Bearer {생성된 토큰 값}"
```

## JWT를 사용한 인증 과정

1. 사용자가 아이디와 패스워드를 입력해 서버에 로그인 인증을 요청함
2. 서버가 클라이언트로부터 인증 요청을 받으면 header, payload, signature를 정의함
   header, payload, signature를 각각 Base64 URL로 암호화하여 JWT를 생성하고 이를 쿠키에 담아 클라이언트에게 발급함
3. 클라이언트는 서버로부터 받은 JWT를 로컬스토리지에 저장함(쿠키나 다른 곳에 저장할 수도 있음)
4. 클라이언트에서 API를 서버에 요청할 때 AUthorization header에 Access Token을 담아 보냄
5. 서버에서는 클라이언트가 header에 담아 보낸 JWT가 내 서버에서 발행한 토큰인지 일치 여부를 확인함. 인증이 통과되었다면 payload에 들어있는 유저의 정보들을 선택해서 클라이언트에 보냄
6. 클라이언트가 서버에 요청을 했는데 만약 엑세스 토큰의 시간이 만료되었다면, 클라이언트는 리프레쉬 토큰을 서버에 보내 서버로부터 새로운 액세스 토큰을 발급받음

## JWT의 엑세스 토큰과 리프레쉬 토큰

JWT도 제 3자에게 토큰 탈취의 위험성이 있기 때문에 그대로 사용하는 것이 아닌 Access Token과 Refresh Token 이렇게 이중으로 나누어 인증하는 방식을 사용함

Access Token과 Refresh Token은 둘다 JWT임. 다만 토큰이 어디에 저장되고 관리되느냐에 따른 사용의 차이일 뿐임

- Access Token
  클라이언트가 가지고 있는 실제 유저 정보가 담긴 토큰.
  클라이언트에서 요청이 오면 서버에서 해당 토큰에 있는 정보를 활용해 사용자 정보에 맞게 응답을 진행

- Refresh Token
  새로운 Access Token을 발급해주기 위해 사용되는 토큰.
  리프레쉬 토큰은 보통 데이터 베이스에 유저정보와 같이 기록함

=> Access Token은 접근에 관여하는 토큰, Refresh Token은 재발급에 관려하는 토큰의 역할로 사용되는 JWT

**참고**

- https://jwt.io
- https://supertokens.com/blog/what-is-jwt#
- https://inpa.tistory.com/entry/WEB-%F0%9F%93%9A-JWTjson-web-token-%EB%9E%80-%F0%9F%92%AF-%EC%A0%95%EB%A6%AC#jwt_json_web_token_%EC%9D%B4%EB%9E%80
