---
title: JPA
tags: jpa, hibernate
date: 2020-04-08 13:15:22
---

JPA ( Java Persistent API )와 ORM ( Object Relational Mapping )

JPA란 자바 ORM 기술에 대한 API 표준 명세를 의미.

JPA는 ORM을 사용하기 위한 인터페이스를 모아둔 것이다. 
JPA는 Java Persistence API의 약자로, 자바 어플리케이션에서 관계형 데이터베이스를 사용하는 방식을 정의한 인터페이스이다. 
일반적인 백엔드 API가 클라이언트가 어떻게 서버를 사용해야 하는지를 정의한 것처럼, 
JPA 역시 자바 어플리케이션에서 관계형 데이터베이스를 어떻게 사용해야 하는지를 정의하는 한 방법일 뿐이다.

JPA는 단순히 명세이기 때문에 구현이 없다. JPA를 정의한 javax.persistence 패키지의 
대부분은 interface, enum, Exception, 그리고 각종 Annotation으로 이루어져 있다. 
예를 들어, JPA의 핵심이 되는 EntityManager는 아래와 같이 javax.persistence.EntityManager 라는 파일에 interface로 정의되어 있다.


``` java
package javax.persistence;

import ...

public interface EntityManager {

    public void persist(Object entity);

    public <T> T merge(T entity);

    public void remove(Object entity);

    public <T> T find(Class<T> entityClass, Object primaryKey);

    // More interface methods...
}
```


JPA를 사용하기 위해서는 JPA를 구현한 Hibernate, EclipseLink, DataNucleus 같은 ORM 프레임워크를 사용해야 한다.

![](/images/jpa_hibernate_relationship.png)

위와 같이 Interface의 구현체이며, EntityManagerFactory, EntityManager, EntityTransaction을  SessionFactory, Session, Transaction으로 상속받고 이를 Impl Class로 구현한다.

Spring Data JPA    
