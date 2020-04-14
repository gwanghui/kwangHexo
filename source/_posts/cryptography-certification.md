---
title: cryptography-certification
date: 2020-04-13 18:32:33
tags:
- PKI
- certification
categories:
- cryptography
---

## 인증서 
- 공개키 인증서(public-key certificate: PKC)
    - 이름이나 소속, 메일 주소 등의 개인 정보
    - 당사자의 공개 키가 기재
    - 인증기관(CA; certification authority)의 개인 키로 디지털 서명
    
### 인증서를 사용하는 시나리오
![](/images/cryptography/certification/public_key_certification_senario.png)

### 공인 인증서 종류
- 전자서명용 인증서(Digital Signature Certificate)
    - 거래 상대방에 대한 신원확인(인증), 전자문서의 위.변조여부의 검출(무결성), 전자문서 송.수신자간의 송.수신사실 여부에 대한 ⑨부인방지(부인봉쇄) 목적으로 사용
- 암호화용 인증서(Encryption Certificate)
    - 적법한 송.수신자를 제외한 제3자가 전송중인 메시지를 보지 못하도록 하는데 사용(비밀성)
- 클라언트 SSL 인증서(Client SSL Certificate)
    - 클라이언트와 서버가 안전한 통신을 하고자 할 때, 서버가 클라이언트의 신원확인을 위해 사용 
    - Form Signing, SSO(Single Sign On)에서 사용
- 서버 SSL 인증서(Sever SSL Certificate)
    - 클라이언트와 서버가 안전한 통신을 하고자 할 때, 클라이언트가 서버의 신원확인을 위해 사용
- ⓓS/MIME 인증서(S/MIME Certificate)
    - 전자메일에 전자서명 하거나 전자메일의 암호화를 위해서 사용
- 소프트웨어 배포용 인증서(Code-Signing Certificate)
    - 인터넷과 같은 안전하지 않은 통신망을 통해 소프트웨어를 안전하게 배포할 목적으로 사용하는 인증서로서 Java code, Javascript 등
        소프트웨어 코드에 그 제작자가 전자서명을 함으로써, 제작자의 신원확인과 전송과정에서의 소프트웨어의 위.변조를 확인할 용도로 사용합니다.
- 인증기관용 인증서(CA Certificate)
    - CA의 확인을 위해 사용 
    - 클라이언트와 서버의 S/W에서 다른 인증서의 신뢰여부를 검증할 때 사용

### Verisign 무료 서비스
- 개인을 위한 인증서(digital ID) 를 60일간 무료 시험판으로 만들어서 제공
- 웹 브라우저만 있으면 온라인에서 바로 발행할 수 있으며 본인 인증은 메일이 도착했는지 여부만으로 확인
- Https로 보호된 웹 사이트에서 이름, 메일 주소, 패스워드를 입력하고 인증서 작성
    
     ```   
         • Organization = KECA, Inc.
         • Organizational Unit = CrossCert Class 1 Consumer Individual Subscriber CA
         • Organizational Unit = Terms of use at www.crosscert.com/rpa (c)01
         • Organizational Unit = Authenticated by CrossCert
         • Organizational Unit = Member, VeriSign Trust Network
         • Organizational Unit = Persona Not Validated
         • Organizational Unit = Digital ID Class 1 - Netscape
         • Common Name = Gil Dong Hong
         • Email Address = gildong@gmail.com 
     ```
  
### 인증서의 표준 규격 X.509
- X.509
    - 가장 널리 사용
    - ITU, ISO에서 규정한 규격으로 인증서의 생성 교환을 수행할 때 사용
    
- X.509 v3 디지털 인증서의 구조
- Certificate
    - Version 인증서의 버전을 나타냄
    - Serial Number CA가 할당한 정수로 된 고유 번호
    - Signature 서명 알고리즘 식별자
    - Issuer 발행자
    - Validity 유효기간
        - Not Before 유효기간 시작 날짜
        - Not After 유효기간 끝나는 날짜
    - Subject 소유자
    - Subject Public Key Info 소유자 공개 키 정보
        - Public Key Algorithm 공개 키 알고리즘
        - Subject Public Key
    - Issuer Unique Identifier (Optional) 발행자 고유 식별자
    - Subject Unique Identifier (Optional) 소유자 고유 식별자
    - Extensions (Optional) 확장
- Certificate Signature Algorithm
- Certificate Signature

- 인증서 파일 확장
.CER - CER 암호화 된 인증서. 복수의 인자증서도 가능.
.DER - DER 암호화 된 인증서.
.PEM - (Privacy Enhanced Mail) Base64로 인코딩 된 인증서. "-----BEGIN CERTIFICATE-----"와 "-----END CERTIFICATE-----" 가운데에 들어간다.
.P7B - .p7c 참조.
.P7C - PKCS#7 서명 자료 구조(자료는 제외), 인증서이거나 CRL(복수도 가능).
.PFX - .p12 참조.
.P12 - PKCS#12, 공개 인증서와 암호로 보호되는 개인 키를 가질 수 있다(복수도 가능).

### 공개 키 기반 구조 (PKI)
- 공개 키 기반(public key infrastructure)
    - 공개 키를 효과적으로 운용하기 위해 정한 규격이나 선택사양의 총칭
    - PKCS(Public-Key Cryptography Standards) : RSA사가 정하고 있는 규격의 집합
    - RFC(Requests for Comments) 중에서도 PKI에 관련된 문서
    - X.509
    - API 사양서

- PKI 구성 요소
    - 이용자 : PKI를 이용하는 사람
    - 인증기관 : 인증서를 발행 하는 사람
    - 저장소 : 인증서를 보관하고 있는 데이터 베이스
    
![](/images/cryptography/certification/PKI_info.png)

- 이용자
    - PKI를 사용해서 자신의 공개 키를 등록하고 싶어 하는 사람과 등록되어 있는 공개 키를 사용하고 싶어 하는 사람
- 이용자가 하는일
    - 키 쌍을 작성한다
    - 인증 기관에 공개 키를 등록한다
    - 인증 기관으로부터 인증서를 발행 받는다
    - 수신한 암호문을 복호화 한다.
    - 메세지에 디지털 서명을 한다.
- 공개키 사용자가 하는  
    - 메시지를 암호화해서 수신자에게 송신한다
    - 디지털 서명을 검증한다.일
    
- 인증 기관 (CA; certification authority)
    - 인증서의 관리를 행하는 기관
    - 키 쌍을 작성한다
    - 공개키 등록 때 본인을 인증한다
    - 인증서 작성 발행, 폐지
    
- 등록 기관 (RA; registration authority)
    - 인증 기관의 일중 공개키의 등록과 본인에 대한 인증을 대행 하는 기관

![](/images/cryptography/certification/ds_management_korea.png)

한국 공인 인증 기관
|공인인증기관|웹페이지|
|:--:|:--:|
|힌국정보인증(주)|http://www.signgate.com|
|(주)코스콤|http://www.signkorea.com|
|금융결제원|http://www.yessign.com|
|한국전자인증(주)|http://www.crosscert.com|
|한국무역정보통신|http://tradesign.net|

- 저장소 (repository)
    - 인증서를 보존해 두고, PKI의 이용자가 인증서를 입수할 수 있도록 한 데이터 베이스
    - 인증서 디렉토리

- 인증 기관의 역할
    - 키 쌍의 작성
        - PKI의 이용자가 작성하거나
        - 인증기관이 이용자의 키 쌍을 생성할 경우 개인 키를 이용자에게 보내는 추가 업무를 해야한다.
            - [PKCS](https://en.wikipedia.org/wiki/PKCS) #12 (Personal Information Exchange Syntax Standard)
        
    - 인증서 등록
        - 이용자는 인증 기관에 인증서 작성을 의뢰
            - 규격 : [PKCS](https://en.wikipedia.org/wiki/PKCS) #10 (Certification Request Syntax Standard)
        - 운용 규격 (CPS; certification practice statement)에 근거해서 이용자를 인증하고, 인증서를 작성
            - 인증서 형식 : [PKCS](https://en.wikipedia.org/wiki/PKCS) #6 (Extended-Certificate Syntax Standard)나 X.509로 정의

    - 인증서의 폐지
        - 이용자가 개인키에 대한 권한을 잃거나 개인키를 분실 혹은 도난 당했을 경우 인증기관은 인증서를 폐지(revoke)해서 무효화 해야한다.
        - 인증서를 폐지하는 경우 인증기관은 인증서 폐지 목록(CRL) 을 작성한다.
        
    - CRL(Certification revocation list)관리
        - 인증기관이 폐지한 인증서의 일련번호의 목록에 대해 인증기관이 디지털 서명을 붙인 것이다.
        - *인증기관의 최신 CRL을 조사해서 그 인증서가 유효한지 아닌지를 확실히 확인 할 필요가 있다*

### 계층 구조를 갖는 인증서
- 루트 CA
    - 최상위 인증기관
- 셀프 서명(Self-signature)
    - 자기 자신의 공개 키에 대해서 자신의 개인 키로 서명하는 디지털 서명

![](/images/cryptography/certification/hierarchy_structure_CA.png)
![](/images/cryptography/certification/flow.png)

- 한국의 PKI
    - 한국인터넷진흥원 전자서명인증관리센터(http://rootca.kisa.or.kr)
    
### 공인 인증서 대체 기술
- 핸드폰 개통할 때 계좌 인증, 카드 인증 사용
|구분|주요내용|
|:-:|:-:|
|휴대폰 본인 확인|본인 확인 기관으로 지정된 통신 3사가 휴대폰 개통 과정에서 수집된 개인정보를 활용해 본인 여부 확인|
|계좌 인증|신분 확인을 통해 개설된 은행계좌에 소액입금과 함께 임의적 문자를 송부해 본인 여부 확인|
|카드 인증| 신분확인을 통해 발그 ㅂ된 신용카드의 정보입력을 통해 본인 여부 확인|

### 공공 민간 전자 서명
||공인인증기관|본인확인기관|
|:-:|:-:|:-:|
|정의|공인인증업무를 제공 하기 위한 기관|주민등록번호 외 본인 확인 업무를 제공하는 기관|
|업체|한국정보인증,코스콤,금융결제원 등|6개 공인인증 기관,3사 통신사, 카드사 등|