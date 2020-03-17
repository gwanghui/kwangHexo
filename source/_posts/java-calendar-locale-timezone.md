---
title: java-calendar-locale-timezone
date: 2020-03-16 18:19:33
tags:
---
## Java Date & Calendar
Locale Extension : buddhist, japanese, gregory 형태의 달력 선택 가능 
- BuddhistCalendar (불멸기원 : 석가모니가 입멸한 해를 기준으로 삼는 연대 표기)
- JapaneseImperialCalendar (일본 연호 : 일본에서는 새 천황이 즉위하거나 나라에 자연재해가 계속될 때 연호를 바꾼다. )
```java
  public static final int BEFORE_MEIJI = 0;
  public static final int MEIJI = 1; (1868년 ~ 1912년)
  public static final int TAISHO = 2; 
  public static final int SHOWA = 3;
  public static final int HEISEI = 4;
  private static final int REIWA = 5;
```

- GregorianCalendar 
    - 율리우스력의 바탕이 된 이집트력을 기본으로 하고 1년의 길이를 365.2425일로 정하는 역법체계. 윤년을 포함하는 양력을 말한다.
    - 율리우스력의 오차를 수정한 달력이며, 기본 구조는 율리우스력을 그대로 따르되 윤년을 정하는 규칙을 추가했다.
    
## Java Locale
- A Locale object represents a specific geographical, political, or cultural region. 
- Language, Script, Country
- [Country Code & Language Code](https://docs.oracle.com/cd/E13214_01/wli/docs92/xref/xqisocodes.html)

||Language Code|Script Code| Country|
|:--:|:--:|:--:|:--:|
|한국어|ko|Kore|KR|
|일본어|ja||JP|
|영어|en||US|

- Locale 은 Display 속성과 Format 속성을 따로 관리한다.
- Display 속성은 언어를 표현하는 단어를 어울러서 가지고 있다.
- Format 속성은 Locale 에 해당하는 숫자, 연도 포멧을 가지고 있다.

