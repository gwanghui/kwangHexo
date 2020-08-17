---
title: protocol-smtp
date: 2020-07-01 10:46:20
tags:
---
### SMTP (Simple Mail Transfer Protocol)
- SMTP : 두 메일시스템이 전자우편을 교환할 수 있게 하는 메세지 전송용 프로토콜
- 통신 포트 : 25
- 모든 처리는 항상 하나의 *TCP* 연결을 통해 실행됨
- 메일 서버는 데몬으로 동작하며, 메일 클라이언트로부터 요청에 항상 준비

#### 주요 구성 요소
- UA(User Agent) : Outlook, Zmail등
- MTA(Message Transfer Agent) : 메일을 중계, 전달하는 기능을 수행
- MAA(Message Access Agent) : 메일 엑세스용 프로토콜

![](/images/protocol/smtp/mta_maa.png)

#### MTA 클라이언트 및 MTA 서버 간 주요 명령 및 응답
- SMTP 주요 명령
    - HELLO : 클라이언트 자신이 누구인지 밝힘 (Hello : kwang.co.kr)
    - MAIL FROM : 클라이언트가 메일 송신자가 누구인지
    - RCPT TO : 수신처
    - DATA : 전체 메일 메세지 전송을 송신측에서 준비됨
    - QUIT 등

#### Mail Message Format
- 메세지 구성
    - Header 
        - Return-Path : 하나 이상의 SMTP 서버 경유시
        - Received : 하나 이상의 SMTP 서버 경유시
        - From: 발신자
        - To : 수신자
        - Subject : 제목
        - Date : 날짜
        - MIME 확장 : Body에 멀티미디어 데이터가 포함되는 경우에 MIME(Multi-purpose Internet Mail Extension Type)
            - MIME-version
            - Content-Transfer-Encoding
            - Content-Type:
            
        |Content-Type|설명|    
        |--|--|
        |Text / Plain               | 포맷되지 않는 텍스트 (특정한 문자세트로 구성됨)|
        |Text / Richtext            | 볼드, 이탤릭체, 밑줄 등 간단한 포맷을 가진 텍스트|
        |Application / Octet-Stream | Binary 데이타|
        |Application / Postscript   | PostScript 프로그램|
        |Message / RFC 822          | 또다른 우편 메시지 RFC 822| 
        |Image / JPEG               | 정지화상 데이타, ISO 10918 포맷|
        |Image / GIF                | 정지화상 데이타, CompuServe의 Graphic interchange 포맷|
        |Video / MPEG               | 동화상 데이타, ISO 11172 포맷|
        |Audio / Basic              | 오디오 데이타, 8비트 ISDN mu-law 포맷을 사용한 인코드|
    - Body
        - 각 줄은 단일점. 으로 끝남
        - 멀티 미디어용 데이터인 경우에는 헤더부분에 MIME 선언이 있음
        
``` Text
telnet 172.0.0.1 25
HELLO kwang.co.kr
MAIL FROM:kwang@kwang.co.kr
RCPT TO:kwang@kwang.co.kr
DATA
SUBJECT: Test subject
test content
. 
```                          

RFC-2045 : Multipurpose Internet Mail Extensions (MIME) Part One: Format of Internet Message Bodies. N. Freed, N. Borenstein. November 1996. (Status: DRAFT STANDARD)
RFC-2046 : Multipurpose Internet Mail Extensions (MIME) Part Two: Media Types. N. Freed, N. Borenstein. November 1996. (Status: DRAFT STANDARD)
RFC-2047 : MIME (Multipurpose Internet Mail Extensions) Part Three: Message Header Extensions for Non-ASCII Text. K. Moore. November 1996. (Status: DRAFT STANDARD)
RFC-2048 : Multipurpose Internet Mail Extensions (MIME) Part Four: Registration Procedures. N. Freed, J. Klensin, J. Postel. November 1996. (Status: BEST CURRENT PRACTICE)
RFC-2049 : Multipurpose Internet Mail Extensions (MIME) Part Five: Conformance Criteria and Examples. N. Freed, N. Borenstein. November 1996. (Status: DRAFT STANDARD)