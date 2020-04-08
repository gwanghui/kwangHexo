---
title: cryptography-message-authentication-code
date: 2020-04-08 11:00:48
tags:
- PKI
categories:
- cryptography
---
### 메세지 인증
- Message Authentication : 메시지가 올바른 송신자로 부터 온것이다

![](/images/cryptography/mac/alice-bob.png)

- Message Authentication Code (MAC) 
    - 무결성
    - 메세지 인증
    - 입력 : 메세지, 공유하는 키
    - 출력 : 고정 비트 길이의 코드

![](/images/cryptography/mac/messagecodevshash.png)

### 메시지 인증 순서
1. 앨리스와 수신자 A은행 키(K) 공유
2. 앨리스 : 송금 의뢰 메세지(M) 작성 MAC 값 (MAC(M)) 계산
3. 앨리스 : 수신자 A 은행으로 메세지와 MAC 값을 전송
4. 수신자 A은행 : 수신한 송금 의뢰 메세지를 기초로 해서 MAC 값을 계산
5. 수신자 A은행 : 앨리스로부터 수신한 MAC 값과 4에서 계산한 MAC 값을 비교
6. 수신자 A은행 : MAC 값이 같다면 인증 성공, 다르다면 인증 실패(앨리스로부터 온 것이 아니라고 판단)

![](/images/cryptography/mac/message_authentication_code_flow.png)

### 메시지 인증 코드의 키 배송 문제
- 키 배송 문제 해결
    - 공개키 암호
    - Diffie-Hellman 키 교환
    - 키 배포 센터
    - 키를 안전한 방법으로 별도로 보내기
    
### 메세지 인증 코드 구현
#### Hash
- HMAC

### Block
- triple DES, AES
    - 블록 암호 키를 메세지 인증 코드의 공유키로 사용
    - CBC 모드로 메세지 전체를 암호화
    - 메세지 인증 코드에서는 복호화를 할 필요가 없으므로 최종 블록 이외는 폐기
    - 최종 블록을 MAC 값으로 이용
    
#### GCM
- Galois/Counter Mode
- Padding 불필요
- 인증 기능 제공
- 병렬 처리가 가능해서 암/복호화 속도가 매우 빠름, SSL/TLS 에서 많이 사용

![](/images/cryptography/block/AES/GCM.png)

- Encrypt-then-MAC
    - 평문을 대칭 암호로 암호화 한 후 암호문의 MAC 값을 계싼
    - 메세지 인증 코드 입력에 암호문을 부여
    - 선택 암호문 공격을 막을 수 있다.
    - ex) IPsec
    
![](/images/cryptography/mac/encrypt-then-mac.png)
    
- Encrypt-and-MAC
    - 평문을 대칭 암호로 암호화 한 후 그와는 별도로 평문의 MAC 값을 얻는 방법
    - ex) SSH

![](/images/cryptography/mac/encrypt-and-mac.png)

- MAC-then-Encrypt
    - 미리 평문의 MAC 값을 얻고, 평문과 MAC 값 양쪽을 정리하여 대칭 암호로 암호화 하는 방법
    - ex) SSL
  
![](/images/cryptography/mac/encrypt-and-mac.png)


