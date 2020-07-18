---
title: spring-boot-proxy
date: 2020-07-19 00:55:53
tags: Springboot
---

### Spring Proxy
- Proxy pattern
    - 실제 기능을 수행하는 객체(Real Object)대신 가장의 객체(Proxy Object)를 사용해 로직의 흐름을 제어하는 디자인 패턴
    - 원래 하려던 기능 외에 다른 것을 추가하고 싶으나 해당 코드 자체를 변경 하지 않고 싶을때, 많이 사용한다.
    - Real Object, Proxy Object는 동일한 인터페이스를 구현하고 Proxy Object는 메서드 수행시 Real Object의 메소드 실행을 가로챈다.
        - Virtual Proxy(Lazy initialisation 등), Protection Proxy

- Byte Code Generation Library
    - cglib : https://github.com/cglib/cglib
    - byteBuddy : https://bytebuddy.net

- ASM : Java bytecode manipulation and analysis framework
    - https://asm.ow2.io/
    