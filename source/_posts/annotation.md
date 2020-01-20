---
title: annotation
date: 2020-01-20 19:15:38
tags:
---

### Java Annotation 

- 어노테이션이란 본래 주석이란 뜻
- 해석 되는 시점 정의
- 주석 대체
- 기존 자바 웹 어플리케이션들은 구성과 설정값들을 외부의 XML설정 파일에 명시하는 방법으로 프로그래밍 되었다. 
  변경 될 수 있는 코드가 아닌 외부 설정 파일에 분리하기 때문에 재컴파일 없이도 쉽게 변경사항을 적용 할 수 있었지만, 프로그램 작성을 위해 매번 많은 설정을 작성해야 한다는 불편함이 존재했다.
- 어노테이션을 사용하면 기존 로직과는 별개로 필요한 정보들을 기입 할 수 있고, 런타임에서 Reflection을 통해 해당 정보를 얻어 올 수 있다.
- 문서화 부분은 Javadoc이 존재하기 때문에 많이 사용되지 않으며, 어노테이션의 본질적 목적은 소스 코드에 메타데이터를 표현하는 것이다.

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface CustomAnnotation {
	boolean isCheck() default true;
}
```

### Meta Annotation
- @Retention
    - Java Compiler가 Annotation을 다루는 방법을 기술하며, 어느 시점까지 영향을 미치는 지 결정하는 값
    - RetentionPolicy.SOURCE : 컴파일 전까지 유효 ( 컴파일 이후 클래스 정보에서 삭제)
    - RetentionPolicy.CLASS : 컴파일러가 클래스를 참조하기 전까지 유효
    - RetentionPolicy.RUNTIME : 컴파일 이후에도 JVM에 의해 계속 참조가 가능. (Runtime Code에서 Reflection을 통한 참조가 가능)
    
- @Target
    - Annotation을 선언할 위치를 선택한다.
    - ElementType.PACKAGE : 패키지 선언
    - ElementType.TYPE : 타입 선언
    - ElementType.ANNOTATION_TYPE : 어노테이션 타입 선언
    - ElementType.CONSTRUCTOR : 생성자 선언
    - ElementType.FIELD : 멤버 변수 선언
    - ElementType.LOCAL_VARIABLE : 지역 변수 선언
    - ElementType.METHOD : 메서드 선언
    - ElementType.PARAMETER : 전달인자 선언
    - ElementType.TYPE_PARAMETER : 전달인자 타입 선언
    - ElementType.TYPE_USE : 타입 선언
    
- @Documented
    - 해당 어노테이션을 Javadoc에 포함시킨다.
 
- @Inherited
    - 어노테이션의 상속을 가능하게 한다.
    - 주의 : 어노테이션 끼리의 상속이 아닌 해당 어노테이션을 가지고 있는 클래스를 상속할 경우 자식 클래스도 해당 어노테이션을 가짐을 뜻한다.
    
- @Repeatable
    - 같은 어노테이션을 중복정의 가능한 @Repeatable 어노테이션을 제공
    
```java
  // case 1
  @GreenColor
  @BlueColor
  @RedColor
  public class RGBColor { ... }
  
  // case 2
  @Color(colors={"green", "blue", "red"}
  public class RGBColor { ... }
```

- 아래와 같이 하나의 RGB Color가 Color에 속함을 보이고 
```java
@Color("green")
@Color("blue")
@Color("red")
public class RGBColor { ... }
```
 
- Color 어노테이션과 Colors 어노테이션을 정의해 표현한다.

```java
 @Repeatable(value = Colors.class)
 public @interface Color {}
 
 @Target(ElementType.TYPE)
 @Retention(RetentionPolicy.RUNTIME)
 @Documented
 public @interface Colors {
     Color[] value();  
 }
``` 


- 참조 : 
    - https://asfirstalways.tistory.com/309
    - https://jistol.github.io/java/2018/08/31/annotation-repeatable/
    - https://stackoverflow.com/questions/23973107/how-to-use-inherited-annotation-in-java/23973331