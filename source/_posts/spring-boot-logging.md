---
title: spring-boot-logging
date: 2020-02-18 21:23:27
tags:
---

스프링 부트에서는 로깅 설정을 자동적으로 지원한다.
SLF4J를 사용한 로깅 파사드를 통해 구현체를 사용하게 되며 스프링부트의 기본은 logback으로 되어 있다.
Spring Boot는 기존 Spring에서의 사용법과는 다르게 logback-spring.xml를 사용하도록 권장하고 있다.

기존 logback과 다른점은 logback file 안에서 Spring context를 접근할 수 있다는 것이다.
기존 logback은 파일이름으로 프로파일을 지정해 사용했던 반면, spring boot의 logback은 
내부에서 프로파일에 대한 if문 처리를 해 파일 하나로 간단하게 관리할 수 있게 도와준다.

https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-logging
상단 링크에 적혀있는 글이 매우 도움이 될 것이다.

현재 프로젝트에서는 Spring-boot-starter-web을 기준으로 프로젝트를 진행하고 있다.
https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web/2.2.4.RELEASE

해당 spring-boot-starter-web은 내부에 spring-boot-starter를 참조하고 있고, 
spring-boot-starter는 spring-boot-starter-logging을 참조 하고 있어서 logback 이외에 다른 구현체를 사용하고 싶으면
spring-boot-starter-logging을 exclude하고 다른 spring-boot-starter를 선언해주면 된다.

