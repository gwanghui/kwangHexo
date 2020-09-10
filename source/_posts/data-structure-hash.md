---
title: data-structure-hash
date: 2020-09-09 12:31:20
tags:
---

1) 개요
자바에서 제공하는 HashMap과 Hashtable은 Map인터페이스를 상속받아 구현되어 데이터를 키와 값으로 관리하는 자료구조이다. 
큰 특징으로는 키(Key)가 데이터를 추출할 때 구분자로 활용하는 방식을 취하는데 이는 리스트 인터페이스와 같은 자료구조보다 탐색에 있어 더 높은 효율을 기대할 수 있다.

2) 차이점

2.1 - 동기화
이 둘의 차이점으로 동기화(Synchronization)를 들 수 있다. 
HashMap의 경우 동기화를 지원하지 않는다.
반면 다중 스레드 환경에서 Hashtable은 동기화를 지원하기 때문에 실행 환경에 따라 구분하여 사용하면 된다. 
하지만 한 자바 관련 서적에 의하면 Vector의 상위호환(?)개념인 ArrayList의 사용을 권장하듯 새로운 버전인 HashMap을 활용하고 동기화가 필요한 시점에서는 
Java 5부터 제공하는 ConcurrentHashMap을 사용하는 것이 더 좋은 방법이라 표현한다.
추가로 속도적인 측면에서도 구형이라 할 수 있는 Hashtable은 동기화 처리라는 비용때문에 HashMap에 비해 더 느리다고 한다.

2.2 - 반환값
HashMap은 저장된 요소들의 순회를 위해 Fail-Fast Iterator를 반환한다.(ConcurrentHashMap의 경우에는 Fail-Safe Iterator)
Hashtable은 같은 경우 Enumeration을 반환한다.
여기서 Enumeration과 Iterator는 컬렉션에 저장된 요소를 접근하는데 사용되는 인터페이스이다.
Enumeration은 컬렉션 프레임워크 이전에 사용되던 인터페이스로 Iterator의 사용을 권장한다.
Iterator엔 remove() 메소드가 추가되었고 메소드 네이밍이 간략화되었다. 
그리고 다른 스레드(lock된 상황에서)에서 해당 자료에 요소를 수정(삽입, 삭제, 수정 등)이 발생하면 ConcurrentModificationException을 발생시켜 일관성을 보장한다. 
이를 Fail-Fast Iterator라 한다. 
ConcurrentHashMap의 경우 Map의 복사본을 참조하는 Iterator를 반환하며 다시 반환받은 시점에 Map에 수정이 있을 경우 해당 Iterator는 반영되지 않는다. 
고로 ConcurrentModificationException또한 발생하지 않는다. 이는 약한 일관성(Weakly Consistent)를 제공하지만 다중 스레드상황에서 해당 Map의 무결성(?)을 보장한다.
추가로 ListIterator가 있는데 이는 단방향만을 제공하는 Iterator의 기능을 향상시킨 것이다. 
