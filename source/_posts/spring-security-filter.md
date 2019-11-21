---
title: spring_security_filter
date: 2019-11-21 16:07:05
tags:
---

|제목|내용|
|---|---|
|SecurityContextPersistence Filter|Security Context Repository에서 SecurityContext를 로드하고 저장|
|Logout Filter|로그아웃 URL로 지정된 가상 URL에 대한 요청 감시, 매칭되면 로그아웃|
|UsernamePasswordAuthenticationFilter|사용자명과 비밀번호로 이뤄진 폼 기반 인증 URL 확인 및 인증 진행|
|DefaultLoginPageGeneratingFilter|폼 or OpenID 기반 인증에 사용하는 URL에 대한 요청을 감시하고 로그인을 수행하는 필요한 HTML 생성|
|BasicAuthenticationFilter|HTTP 기반 인증 헤더를 감시하고 이를 처리함|
|RequestCacheAwareFilter|로그인 성공 이후 인증 요청에 의해 가로 채어진 사용자의 원래 요청을 재구성|
|AnonymousAuthenticationFilter|이 필터가 호출되는 시점까지 사용자가 아직 인증을 받지 못했다면 요청 관련 인증 토큰에서 사용자가 익명 사용자로 나타남|
|SessionManagementFilter|인증된 주체를 바탕으로 세션 트래킹을 처리해 단일 주체와 관련된 모든 세션 트래킹|
|ExceptionTranslationFilter|발생하는 예외 처리 담당|
|FilterSecurityFilter|권한부여 등 여러 결정을 AccessDecisionManager에게 위임해 최종 제어|

하단은 타 블로그에서 참조

SecurityContextPersistenceFilter

SecurityContextRepository 에서 SecurityContext 를 로딩하거나 SecurityContextRepository 로 SecurityContext 를 저장하는 역할을 한다.SecurityContext 란 사용자의 보호및 인증된 세션을 의미한다.



LogoutFilter

로그아웃 URL(디폴트 값 : /j_spring_security_logout) 로의 요청을 감시하여 해당 사용자를 로그아웃 시킨다.



UsernamePasswordAuthenticationFilter

username 과 password 를 사용하는 폼기반 인증 요청 URL(디폴트 값: /j_spring_security_check) 을 감시하여 사용자를 인증하는 역할을 한다. 



DefaultLoginPageGeneratingFilter

폼또는 OpenID 기반 인증을 위한 로그인폼 URL(디폴트 값: /spring_security_login)을 감시하여 로그인폼을 생성한다.



BasicAuthenticationFilter

 HTTP 기본 인증 헤더를 감시하여 처리한다.



RequestCacheAwareFilter

로그인 성공 후, 원래 요청 정보를 재구성하기 위해 사용됨



SecurityContextHolderAwareRequestFilter

HttpServletRequestWrapper 를 상속한 SecurityContextHolderAwareRequestWapper 클래스로 HttpServletRequest 정보를 감싼다. SecurityContextHolderAwareRequestWrapper 클래스는 필터 체인상의 다음 필터들에게 추가 정보를 제공한다.



AnonymousAuthenticationFilter

이 필터가 호출되는 시점까지 사용자 정보가 인증되지 않았다면 사용자가 익명이라는 것 나타내는 인증토큰이 요청과 관련지어 진다.



SessionManagementFilter

이 필터는 하나의 인증된 사용자와 관련된 모든 세션을 추적하고, 인증된 사용자 정보를 기반으로 세션을 추적을 처리한다.



ExceptionTranslationFilter

이 필터는 보호된 요청을 처리하는 중에 발생하는 예상된 예외를 위임하거나 전달하는 역할을 한다.



FilterSecurityInterceptor

이 필터는 AccessDecisionManager 로 인증에 대한 결정권을 위임함으로써 인증허가 및  접근제어 결정을 용이하게 한다.
