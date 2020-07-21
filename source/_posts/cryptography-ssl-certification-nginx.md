---
title: cryptography-ssl-certification-nginx
date: 2020-07-21 10:01:37
tags:
categories:
- cryptography
---

- SSL 인증서 파일 포멧 종류
    - 확장자는 그 파일의 특성이 무엇인지 쉽게 알 붙이는 것이지만, 바이너리 이진 파일에 .txt 확장자를 붙일수도 있는 것처럼 말그대로 이름일 뿐이다. 
      그래서 확장자 보다는 해당 파일의 실제 형식을 확인하는 것이 중요하다.

- 파일 포멧
    - .pem
        - PEM(Privacy Enhanced Mail)은 Base64 인코딩된 ASCII 텍스트 이다. 파일 구분 확장자로 .pem을 주로 사용한다.
      개인키, 서버 인증서, 루트 인증서, 체인 인증서 및 SSL 발급 요청시 생성하는 CSR 등에 사용되는 포멧이며, 가장 광범위하고 거의 99% 대부분의 시스템에 호환되는 산업 표준이다.
    - .crt
        - 대부분 PEM 포멧이고 유닉스/리눅스 기반 시스템에서 인증서로 구분하기 뒤해 사용된다.
    - .cer
        - 대부분 PEM 포멧이고 윈도우 기반 시스템에서 인증서로 구분하기 뒤해 사용된다.
    - .csr
        - CSR(Cerfiticate Signing Request)는 PEM포멧이며, SSL 발급 신청을 위해서 본 파일 내용을 인증기관 CA에 제출하는 요청서 파일임을 구분하기 위해서 붙이는 확장자 이다.
    - .der
        - DER(Distinguished Encoding Representation)의 약자이며, 바이너리 포멧이다. 최근에는 잘 사용되지 않는다.
    - .key
        - OPENSSL 및 Java에서 개인키 파일임을 구분하기 위해서 사용되는 확장자이다. PEM일수도 있고 DER일수도 있다.
    - .jks
        - Java Key Store의 약자이며, Java 기반의 독자 인증서 바이너리 포멧이다. 개인키,서버 인증서, 루트 인증서, 체인인증서를 모두 담고 있다.
        