---
title: java-fundamental-method-stack
date: 2020-02-27 11:23:55
tags:
---
![](/images/java/fundamental/method_stack_memory/method_stacks.png)

## Java Virtual Machine Stacks
- Local Variable Section
    - Method Parameters음
        - float size 1
        - long, double size 2
        - char, byte, boolean, short, int 전체가 int형으로 선언되어 있음
    - Local variable

- Operand Stack
    - Work Space in JVM 
        - JVM 작업 공간, JVM 이 프로그램을 수행하면서 연산을 위해 사용되는 데이터 및 결과를 Operand Stack에 집어넣고 처리함

![](/images/java/fundamental/method_stack_memory/operand_stack.png)


- Frame Data
    - Constant pool Resolution
    - Normal Method Return
    - Exception Dispatch

## Native Method Stacks
- Native Code C ==> C Stack
- Native Code C++ ==> C++ Stack

![](/images/java/fundamental/method_stack_memory/native_method.png)
