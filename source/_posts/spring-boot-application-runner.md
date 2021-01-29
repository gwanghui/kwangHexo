---
title: spring-boot-application-runner
date: 2021-01-29 10:41:35
tags:
- SpringBoot
---

### CommandLineRunner
- 실행되는 코드가 자바 문자열 아규먼트 배열에 접근해야 할 필요가 있는 경우에 사용.
- CommandLineRunner 인터페이스를 구현한 클래스에 @Component 어노테이션을 선언해두고 컴포넌트 스캔 이후 구동 시점에 run 메소드 실행

### ApplicationRunner
- 실행되는 코드가 아규먼트에 접근하고 싶으나 추상화된 ApplicationArguments 타입의 객체를 받고 싶을 때 사용
- CommandLineRunner 인터페이스를 구현한 클래스에 @Component 어노테이션을 선언해두고 컴포넌트 스캔 이후 구동 시점에 run 메소드 실행
