---
title: event-publishing-run-listener
date: 2019-12-19 15:44:23
tags: Spring Boot Listener
---

스프링부트가 기동 될때, 기본적으로 등록하는 Run Listener

![](/images/springboot/eventpublishinglistener/eventpublisingrunlistener.png)

내부에 SpringApplication을 가지고 있고, 
SimpleApplicationEventMulticaster에 ApplicationListener들을 등록한다.