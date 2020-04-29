---
title: cryptography-diffie-hellman-algorithm
date: 2020-04-20 14:23:56
tags:
- key exchange
categories:
- cryptography
---
## Diffe-Hellman Algorithm
- 타인에게 알려져도 상관없는 정보를 두 사람이 교환하는 것만으로 공통의 비밀 값을 만들어내는 방법
- 실제 키 교환이 아닌 공유할 키를 계산하여 만들어 내는 것
- 이산 대수 문제(Discrete Logarithm Problem) 이용
$y = g^x mod P$ 일때 *g와 x와 p를 안다면 y는 구하기 쉽지만* *g와 y와 P를 알땐 x를 구하기는 어렵다*는 방식에 착안하여 만들어진 알고리즘

### 잠깐의 수학시간
- 항등원 : 집합 S의 임의의 원소 a와 원소 e를 연산한 결과가 a가 될 때 e를 연산에 대한 항등원이라고 한다.
    - ex) 10 + 0 = 10 , 10 * 1 = 10
    - 0은 덧셈의 항등원, 1은 곱셈의 항등원
- 역원 : 집합 S의 임의의 원소 a와 x를 연산한 결과가 항등원 e가 될 때 x를 연산에 대한 a의 역원이라고 한다.
    - ex ) 10 - 10 = 0, 10 * $\frac{1}{10}$ = 1
    - 덧셈 10 의 역원은 -10, 곱셈 10의 역원은 $\frac{1}{10}$
- 합동 : 두 정수 a와 b에 대하여 그들의 차 a-b가 m의 배수일 때, a는 b는 m을 법으로 *합동(congruent modulo m)* 이라함.
    - $a \equiv b$ (mod m)
    - ex) $29 \equiv 4$ (mod 5), 1 $\equiv$ -1 (mod 2)
- 잉여계 : 정수를 원소로 가지는 집합 A에 대하여, 어떤 임의의 서로 다른 원소도 m에 대한 합동이 아니면 집합 A는 법 m에 대한 잉여계이다.
    - $n(A) \le m$ 이다
- 완전 잉여계 : C = {x|x는 법 m에 대하여 0,1,2,...(m-1)과 합동인 자연수}
    - 특히, C = {x|x는 법 m에 대하여 0,1,2,...(m-1)} 일때 C는 법 M에 대한 최소 완전 잉여계이다.
    - 집합 {0,1,3,7,9}는 법 5에 대한 완전 잉여계이다.
    - 집합 {0,1,2,3,4,5,6}은 법 7에 대한 최소 완전 잉여계이다.
- 기약 잉여계 : $\text{\textbraceleft}{a1,a2,..., a_m}\text{\textbraceright}$을 법 m에 대한 완전 잉여계라고 할 때, 이들 중 m과 서로소일때 원소만 모은 집합 $\text{\textbraceleft}{a_1',a_2',...,a_m'}\text{\textbraceright}$
- 원시근 : 어떤 기약잉여계의 모든 원소를 원시근의 거듭제곱으로 표현할 수 있을때 이를 원시근이라 한다.

### Discrete logarithm problem
- https://www.youtube.com/watch?v=SL7J8hPKEWY&feature=youtu.be
- https://www.youtube.com/watch?v=M-0qt6tdHzk
- https://docs.oracle.com/javase/7/docs/technotes/guides/security/crypto/CryptoSpec.html#DH2Ex
