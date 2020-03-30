---
title: java-class-clazz
date: 2020-03-30 09:50:55
tags:
---
간단하게 instanceof는 특정 Object가 어떤 클래스/인터페이스를 상속/구현했는지를 체크하며
Class.isAssignableFrom()은 특정 Class가 어떤 클래스/인터페이스를 상속/구현했는지 체크합니다.

```java
// instanceof
MacPro obj = new MacPro();
if (obj instanceof Computer) {
  ...
}

// Class.isAssignableFrom()
if (Computer.class.isAssignableFrom(MacPro.class)) {
  ...
}
```

특정 클래스가 다른 인터페이스를 구현했거나 상속받았는지를 체크하기 위해서는 어떻게 하면 될까? 상위클래스들을 모조리(java.lang.Object 가 될때까지) 찾아다니면서 구현한 Interface들을 확인하면 될것도 같은데 왠지 세련되지 않은 것 같다.
isAssignableFrom 이라는 메소드는 위 문제를 쉽게 해결해준다.
내가 원하는 것은 사용자로부터 입력받은 클래스가 java.util.Collection 인터페이스를 (implements)구현한 클래스인지 체크하는 것이었다.