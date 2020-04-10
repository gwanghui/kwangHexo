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

- c : Cypher message
- m : message
- $K_E$ : Message를 암호화 할 Key
- $K_I$ : Tag를 암호화 할 Key
- $E(K_E,m)$ : message 암호화
- $S(K_I,c)$ : c로 MAC값 계산
- $E(K_E, m||tag)$

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
  
![](/images/cryptography/mac/MAC-then-Encrypt.png)

#### GMAC 
- Galois/Counter Mode MAC
    - GCM을 메세지 인증 코드 전용으로 사용

#### HMAC
- Hash Message Authentication Code
- 일방향 해시 함수를 이용하여 메시지 인증 코드를 구성
- HMAC의 일방향 해시 함수는 모듈형으로 골라서 사용
- HMAC-SHA1 : SHA-1

- RFC 2104 정의
    $HMAC(K,m) = H\big( ({K'}\oplus opad ) \big) ||  H\big( ({K'}\oplus ipad || m)\big )$
    $K' = \begin{cases} 
    H(K) &\text{K is larger than block size} \\
    K &\text{otherwise}
    \end{cases}$

- HMAC의 순서
    1. 키 패딩 : 일방향 해시 함수 블록 길이
    2. 패딩한 키와 ipad (Inner Padding)의 XOR
    3. 메시지 결합
    4. 해시 값의 계산
    5. 패딩한 키와 opad(Outer Padding)의 XOR
    6. 해시 값과의 결합
    7. 해시 값의 계산
    
![](/images/cryptography/mac/HMAC.png)

- Replay 공격

![](/images/cryptography/mac/replay_attack.png)

- Replay 공격 방어
    - 순서 번호 (Sequence number) : 송신 메세지에 매회 1씩 증가하는 번호 (순서 번호, Sequence number)
        - 마지막 통신시 순서 번호를 저장
    - TimeStamp
        - 송신 메세지에 현재 시각 넣기
        - 송수신자 사이의 동기화 필요
    - 비표(Nonce)
        - 송신자에게 일회용의 랜덤한 값을 전송
        - 메시지와 비표를 합해 MAC 값을 계산
        - 비표 값은 통신 마다 교체
        
- 키 추측 공격
    - Brute Force
    - Birthday Attack

- MAC 값만 획득한 공격자가 키를 추측하지 못하도록 해야 한다
    - 해시 함수의 일방향성
    - 해시 함수의 충돌내성
    - 키 생성에 의사난수 생성기 사용


![](/images/cryptography/mac/mac_algorithm_types.png)
