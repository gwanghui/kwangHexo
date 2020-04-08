---
title: cryptography-signatures
date: 2020-04-07 09:23:21
tags:
- PKI
categories:
- cryptography
---

## Signatures
### Electronic Signature
- 계약 또는 기타 기록에 첨부된 서명으로 서명하려는 사람이 실행하거나 채택한 전자 신호,심볼 또는 프로세스로 정의 된다.
- 기본적으로 전자 서명은 손으로 쓴 서명을 디지털화 한 것과 동일하며 문서 내의 내용 또는 특정 문서의 용어를 확인하는 데 사용할 수 있다.

### Digital Signature
- 문서에 서명 한 사람이 본인이 맞는가?
- 서명이 유효하고 위조되지 않았는지 어떻게 확인할 수 있는가?
- 문서가 변경되지 않았는지 어떻게 확인할 수 있을까?
- Digital Signature의 경우 Certification Authority(CA)으로 알려진 신뢰할 수 있는 제 3자가 신원 확인 측면에서 공증인 역할을 한다.
- 인증 기관은 신원을 PKI (Public Key Infrastructure) 기반 디지털 인증서에 바인딩해서 문서 및 클라우드 기반 서명 플랫폼에 디지털 서명을 적용 할 수 있다.

#### 보장 범위
- 문서는 정통이며 검증된 출처에서 제공된다.
- 서명이 변경되면 서명이 유효하지 않은 것으로 표시되어 디지털 서명 된 이후 문서가 변경되지 않았다.
- 사용자의 신원이 신뢰할 수 있는 조직에 의해 확인되었다.
- Certified Signature 
- Approval Signature
- Signature Line

#### PKI (Public Key Infrastructure)
- RFC 2459 : Internet X.509 Public Key Infrastructure
    - Message Digest (Hash Function)
    - Symmetric Key Algorithm
    - Asymmetric Key Algorithm

- Cryptographic Hash Function
    - Message Digest
    - MD5(128bit), SHA-1(160bit)

- Symmetric Key Algorithm
    - One key (대칭키)
    - Encryption, Decryption
    - 3DES, AES

- Asymmetric Key Algorithm
    - Two Key (비 대칭키)
    - RSA (Rivest, Shmir, Adelman)

- 공개키 : 평문을 공개키로 *암호화*한후 개인키로 *복호화*해 Encrypted Text를 만든다.
- 개인키 : 개인키로 *암호화*한 후 공개키로 *복호화*해 Encrypted Text를 만든다. 개인이 보낸 내용을 증명할 때 많이 사용하며 이러한 경우를
        개인키 '전자서명', 공개키 '서명검증' 이라고 한다.




참조 : https://crazia.tistory.com/entry/PKI-PKI-%EC%9D%98-%EA%B8%B0%EB%B3%B8-%EA%B0%9C%EB%85%90-%EA%B0%84%EB%8B%A8-%EC%84%A4%EB%AA%85