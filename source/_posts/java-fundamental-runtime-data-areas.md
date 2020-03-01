---
title: java-fundamental-runtime-data-areas
date: 2020-02-27 11:40:06
tags:
---

![](/images/java/fundamental/runtime_data_areas/runtime_data_areas.png)

## Runtime Data Areas
- PC Registers
    - CPU에서 명령어(Instruction)을 수행하는 과정에서 필요한 정보를 레지스터(Register)라고 하는 CPU내 기억장치를 사용한다.
    - Java의 PC Registers는 Register-Base로 구동되는 방식이 아니라 Stack-Base로 작동한다.
    - JVM은 CPU에 직접 명령어를 수행하지 않고 Stack에서 Operand를 뽑아내 이를 별도의 메모리 공간에 저장하는 방식을 취하고 있다.
    - 플랫폼 독립적인 설계를 위해 버퍼공간으로 만든 메모리 공간을 PC Registers라고 한다.
    
- Java Virtual Machine Stacks
    - Thread의 수행 정보를 기록하는 Frame을 저장하는 공간
    - Thread별로 하나씩 존재하며 Thread가 시작할 때 생성된다.
    - Stack Frame 이라고 하는 것들로 구성이 되는데 JVM은 Stack Frame을 push,pop 작업만 수행한다.
    - 그래서 이 Stack Frame들의 정보를 Stack Trace또는 Stack Dump를 얻어내어 분석을 하게 된다.
    - 현재 수행하고 있는 Method의 Strack Frame을 Current Frame이라고 한다.
    - 현재 수행하고 있는 Method의 Class를 Current Class이라고 한다.
    
- Method Area
    - 메모리 영역들이 각 Thread마다 할당되는 배타적인 공간인데 반해 Method Area는 모든 thread들이 공유하는 메모리 영역이다.
    - Load된 Type(Class, Interface)을 저장하는 논리적 메모리 공간
    - Type의 ByteCode 뿐만 아니라, 모든 변수, 상수, Reference, Method Data등이 포함된다.
    - Class Variable과 Method와 생성자의 정보도 포함된다. 이 정보들은 ClassLoader에게 넘겨받은 Class File에서 Type관련 정보를
      추출하여 저장하게 된다.
    - Method Area는 JVM이 기동할때 생성이 되고 GC대상이며, 이는 벤더마다 구현이 다르다.