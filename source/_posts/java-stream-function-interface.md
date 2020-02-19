---
title: java-stream-function-interface
date: 2020-02-19 21:41:18
tags:
---

| 함수형 인터페이스 | Descriptor | Method명| 
| ------------ | ----------- | ------ |
| Predicate<T> | T -> boolean| test() |
| Consumer<T>  | T -> void   | accept()|
| Supplier<T>  | () -> T     | accept()|
| Function<T,R> | T -> R     | apply() |
| UnaryOperator<T> | T -> T  | identity()|