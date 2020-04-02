---
title: spring-security-jose
date: 2020-03-31 15:42:00
tags:
---

## JOSE (Javascript Object Signing and Encryption)
https://jose.readthedocs.io/en/latest/

JWT (JSON WEB Token) : JWS or JWE
JWS (JSON Web Signature) : 서버에서 인증을 증거로 인증 정보를 서버의 **Private Key** 로 서명
JWE (Json Web Encryption) : 서버와 클라이언트 간 암호화된 데이터를 Token 화 한것
JWK (Json Web Key) : cryptographic key 표현 형식


## JWT
![](/images/springboot/security/jose/jwt.png)
Base64URL Encode

### Header
```json
{
  "typ": "JWT",
  "alg": "HS256"
}
```

### Payload
- Claim : 정보의 조각을 뜻함
    - Key/Value 의 쌍으로 구성
    - Registerd, public, private 의 type을 가짐



#### Registered Claim
- iss : 토근 발급자 (issuer)
- sub : 토큰 제목 (subject)
- aud : 토큰 대상자 (audience)
- exp : 토큰의 만료시간 (expiration)
- nbf : 토큰의 활성날짜 (Not Before)
    - nbf 이후 토큰이 활성화 되어야 함. 그전까지 처리 안됨
- iat : 토큰이 발급된 시간 (issued at) token age 판단 가능
- jti : JWT 고유 식별자. 중복확인때 사용됨

#### Public Claim
- 충돌 방지된 이름 (collision-resistant)
- Claim 이름을 URI 형식으로 설정함

```json
{
  "https://your.domain/jwt_claims/": true
}
```

#### Private Claim
- 클라이언트 서버 양측간 협의하에 사용되는 클레임 이름

```json
{
    "username": "yours"
}
```

### Signature
- Header의 인코딩 값, Payload의 인코딩 값 을 합치고 *Private Key*로 해쉬를 하여 생성

```text
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret)

```