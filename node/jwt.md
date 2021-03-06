## JWT

# JSON Web Token
  - Json 포맷을 이용한 web token
  - Claim based token
  - 회원 인증, 정보 전달에 주로 사용
  - Header, Payload, Signature로 구성

# 세션인증
서버에서 세션을 생성하여 세션정보를  통한 req/res

# 세션의 문제점
  1. 서버가 스케일 아웃되어 여러 개가 생기면 각 서버마다 세션 정보가 저장되 세션 정보의 동기화 문제 발생
  2. 세션을 서버,DB에 저장시 과부하가 발생함
  3. 웹/앱의 간의 쿠키-세션 처리 방법이 다름

# JWT
  1. Header : JWT 웹 토큰의 헤더 정보(typ:토큰타입, alg:해싱 알고리즘)
  2. Payload : 실제 토큰으로 사용하려는 데이터가 담기는 부분. 각 데이터를 Claim이라 한다.
    - Reserved claims : 이미 예약된 Claim. iss(토큰 발행자 정보), exp(만료일), sub(제목), aud(audience)
    - Public claims : 사용자 정의 Claim. 공개용 정보로 url포맷을 이용해 저장.
    - Private claims : 사용자 정의 Claim. 사용자가 임의로 정한 정보.
  3. Signature : header와 payload의 데이터 무결성과 변조 방지를 위한 서명.

최종적으로 base64 인코딩 후 합치며 .을 이용한다. 토큰은 http통신 간 이용되며, Authorization이라는 key의 value로서 사용된다.

# 인증과정
  POST/login -> 토큰 생성 -> 토큰 리턴 -> 헤더에 토큰을 실어서 요청 -> 토큰 확인 -> Response

# 단점
  - 정보가 많아질수록 토큰의 길이가 늘어나 네트워크에 부하를 줄 수 있다.
  - payload자체는 암호화 되지 않고 base64로 인코딩한 데이터이기 때문에 중간에 payload를 탈취하면 디코딩을 통해 데이터를 볼 수 있다. JWE를 통해 암호화해야한다.
  - 토큰을 한번 만들면 임의로 삭제할 수 없다.
  - 토큰을 클라이언트에서 관리해야한다.
