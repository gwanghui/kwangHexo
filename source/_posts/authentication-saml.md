---
title: authentication-saml
date: 2021-01-05 14:17:04
tags:
---
### SAML (Security Assertion Markup Language)
- 용어
    - Authentication(인증) : 사용자가 자신이 주장하는 사람임을 확인
    - Authorization(권한 부여) : 사용자가 특정 시스템이나 콘텐츠에 접속할 수 있는 권한이 있는지 확인
    - Service Provider(서비스 제공자) : 시스템에서 사용자가 원하는 서비스를 제공하는 제공자
    - Identity Provider(인증 정보 제공자) : 사용자의 크레덴셜(사용자 ID와 패스워드)을 인증하고 SAML Assertion을 발행하는 주체
    - SAML Assertion(SAML 보안 정보) : 사용자 인증을 위해 Identity Provider로 부터 Service Provider로 전달되는 보안정보
                                     사용자명, 권한 등의 내용을 적은 XML 문서로 변조를 막기 위해 전자 서명 되어있다.
    - SAML Request (인증 요청) : Service Provider가 생성ㅁ하는 인증 요청으로 Identity Provider로 인증 위임
    - Circle of Trust : 하나의 Identity Provider를 공유하는 Service Provider 들로 구성
    - Metadata : SSO를 활성화 하는 Service Provider 및 Identity Provider가 생성하는 XML 파일
      웹 어플리케이션의 경우 프로비저닝시에 Service Provider와 Identity Provider간 메타데이터(Entity Id, 암호화 키, Endpoint)의 교환으로 신뢰 관계 설립
    - Assertion Consumer Service URL : Identity Provider가 특정 URL로 최종 SAML 응답을 포스트 하도록 요구한다.  
      
