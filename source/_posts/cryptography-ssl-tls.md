---
title: cryptography-ssl-tls
date: 2020-04-29 10:39:00
tags:
- key exchange
- ssl
- tls
- https
categories:
- cryptography
---

### SSL(Secure socket layer)/TLS(Transport Layer Security)
- 통신 내용을 암호화 해주는 프로토콜
    - SSL/TLS 상에 HTTP을 올리는 것이다.
    - HTTP의 통신(요청과 응답)은 암호화되어 도청을 방지할 수 있다.
    
### SSL/TLS의 역할
- *도청* 당하는 일 없이 통신하고 싶다. ( 기밀성 )
- *조작* 당하는 일 없이 통신하고 싶다. ( 무결성 )
- 통신 상대의 웹 서버가 해당 상대인 것을 확인하고 싶다 ( 상호 인증 )

### Cypher Suite
- 암호 기술의 추천 세트가 SSL/TLS로 규정 되어 있다.

### HTTPS 인증서 유형
- 신원 검증
    - DV (Domain Validated) : DV 인증서는 해당 도메인에 대한 알맞은 공개 키인지를 간단히 확인하며, 브라우저에서는 법적 신원을 보여주지 않는다.
    - EV (Extended Validation) : EV 인증서는 웹 사이트의 법적 신분을 검증한다.
        - 도메인 관리
        - 공인된 사업기록
        - D&B(Dunn and Bradstreet), salesforce, connect.data.com 등에 기재된 정보
        - 인증서의 모든 도메인 이름 검사 ( 와일드카드 안됨)

- 도메인 수 
    - 단일 도메인
    - 다중 도메인(Unified Communications Certificate/SAN)
    - 와일드 카드
    
### HTTPS 구성
- 초기 키 교환 (공개키 알고리즘)
- 신원 인증서 (인증기관에서 발행한 HTTPS 인증서)
- 실제 메세지 암호화 (대칭키 알고리즘)
- MAC (Hash 알고리즘)    

### 참고 자료

https://dokydoky.tistory.com/462
https://dokydoky.tistory.com/463
https://dokydoky.tistory.com/464