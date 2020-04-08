---
title: cryptography-block-cipher
date: 2020-04-08 13:15:22
tags:
- PKI
categories:
- cryptography
---

## block cipher
- DES : 보안 취약점 발견으로 이제 안씀

## AES 
- 전 세계 공모를 통해 선정 벨기에의 암호학자 2명이 제출한 Rijndael 
- AES is a block cipher with a fixed block size of 128 bit (16 byte)

### Padding
- 입력 데이터가 블록 사이즈의 배수가 아니라면 블록 사임ㅇㄹ호즈에 맞추기 위한 방식이 Padding
- 부족한 size 만큼 바이트 값을 추가하는 PKCS7 Padding을 많이 사용
- 3Byte가 부족할 경우 03d을 3개 패딩

![](/images/cryptography/block/AES/PKCS7_Padding.png)

### Operation Mode
- 입력데이터가 블록보다 크면 암/복호화시 여러 개의 블록이 생성됨
- 각 블록간의 관계를 처리하는게 운영 모드
- ECB, CBC, CFR, GCM 등의 모드가 있음

#### ECB
- Electronic Code Book
- 동일한 내용을 갖는 평문 블록은 이에 대응 되면 동일한 암호문 블록으로 변환되고 1:1 대응표를 갖게 된다.
- 대칭키 암호화시 ECB를 사용하면 안됨

![](/images/cryptography/block/AES/ECB.png)

#### CBC
- Cipher Block Chaining
- Message Authentication Code 에 사용 불
- 암호화
- 직전 블록은 다음 블록의 입력으로 사용하여 안정성이 증대됨
- 초기 블록이 유추가 어렵도록 Initial Vector 사용
- 1단계 전에 수행되어 결과로 출력된 암호문 블록에 평문 블록을 XOR 하고 나서 암호화를 수행 
    그결과 생성되는 각각의 암호문 블록은 단지 현재 평문 블록뿐만 아니라 그 이전의 평문 블록들의 영향
    
![](/images/cryptography/block/AES/CBC_encrypt.png)

- 복호화
- 최초 암호문 블록을 XOR 하고 나서 복호화의 결과와 IV를 이용해 최초 평문 블록 P1을 만들고
- 나머지는 복호화 XOR을 통해 최초 평문 블록을 만든다. 
- 암호문 블록이 손상되면 이후 블록들이 전부 손상된다.

![](/images/cryptography/block/AES/CBC_decrypt_1.png)
![](/images/cryptography/block/AES/CBC_decrypt_2.png)
    
#### GCM
- Galois/Counter Mode
- Padding 불필요
- 인증 기능 제공
- 병렬 처리가 가능해서 암/복호화 속도가 매우 빠름, SSL/TLS 에서 많이 사용

![](/images/cryptography/block/AES/GCM.png)

    
#### Block Size : 128/192/256

#### CFB
- Cipher FeedBack 
    - 1단계 앞의 *암호문* 블록을 암호 알고리즘의 입력으로 사용
    - 피드백 : 암호화의 입력으로 사용
    - 한 단계앞의 암호문 블록을 암호화 한 후 평문 블록과 XOR 연산 
    - 최초의 암호문 블록을 만들어낼 때는 1단계 앞의 출력이 존재하지 않으므로 대신에 초기화 벡터(Initial Vector) 사용
   - replay attack 가능
   
#### OFB
- Output FeedBack
    - 암호 알고리즘의 출력을 암호 알고리즘의 입력으로 피드백
    - 평문 블록은 암호 알고리즘에 의해 직접 암호화 되고 있는 것은 아님
    
#### CTR
- CounTeR 모드
    - 1씩 증가해 가는 카운터를 암호화해서 키 스트림을 만들어 내는 스트림 암호
    - CTR 모드에서는 블록을 암호화 할 때 마다 1씩 증가해 가는 카운터를 암호화해서 키 스트림을 만듬
    - 카운터를 암호화 한 비트열과 평문 블록과 XOR을 취한 결과가 암호문 블록이 됨
    - 카운터의 초기값은 암호화 때마다 다른 값(nonce, 비표)을 기초로 해서 만든다

![](/images/cryptography/block/AES/nonce.png)

- CTR 암호화
![](/images/cryptography/block/AES/CTR_AES_encrypt.png)

- CTR 복호화
![](/images/cryptography/block/AES/CTR_AES_decrypt.png)

- CTR 모드의 암호화와 복호화는 완전히 같은 구조
- CTR 모드는 블록을 임의의 순서로 암호화, 복호화 할 수 있다.
- 프로그램으로 구현하는 것이 매우 간단
- 병렬 처리가 가능한 시스템에서는 CTR 모드를 이용하여 자료를 고속 처리
- CTR Counter size 16byte
- AES128/AES192/AES256 암호화 키 사이즈가 각각의 bit

#### CBC, CTR을 중점으로 보자
