---
title: probability-statistics-basic
date: 2020-05-18 15:33:03
tags:
---

## 확률
- 확률
    확률 = 어떤 사건이 발생할 수 있는 경우의 가짓수 / 모든 경우의 가짓 수
    
- 여사건
    - 사건 A가 발생하지 않을 사건
    - $P(\overline{A}) = 1 - P$

- 합사건
    - 사건 A와 사건 B중에서 어느 한쪽이 발생할 사건
    - $P(A \cup B) = P(A) + P(B) - P(A \cap B)$
    
- 곱사건
    - 사건 A와 B가 동시에 일어나는 사건
    - $P(A \cap B) = P(A)P(B)$
    
## 확률 변수와 확률 분포
- 확률 변수
    - 어떤 변수 X를 P(X)의 확률로 나오게 할 수 있다면, 이 X는 확률 변수라고 말할 수 있다.
    - 이산 확률 변수
        - 어떤 사건의 이산확률 변수가 X일 때, 그에 대한 확률 P는 이산확률분포 f(x)를 따른다
        - P(X) = f(x)
        - histogram
    - 연속 확률 변수
        - 어떤 사건의 연속확률 변수가 X일 때, 그에 대한 확률 P는 연속확률분포 f(x)를 지정한 X의 구간 안에서 적분한 값과 같다.
        - $P\big(a \leq X \leq b \big) = \int_a^b f(x)\,dx$
        
## 결합 확률과 조건부 확률
- 결합 확률
    - 사건 A와 사건 B가 서로 독립된 사건일 때, 두 사건의 결합확률은 다음과 같다.
    - $P(A \cup B) = P(A) + P(B)$
- 조건부 확률
    - 사건 B가 일어났을 때, 사건 A가 일어날 조건부 확률은 다음과 같다.
    - $P(A | B) = \dfrac {P(A\cap B)} {P(B)}$

## 기댓값
- 기댓값 : X가 확률 변수이고 확률 P(X)인 사건이 존재할 때, 예상할수 있는 결과값이 기댓값이다.
- $E(X) = \sum P(X) \cdotp X$
- X와 Y가 서로 *독립된* 확률변수이고 k는 상수라고 할 때 다음 식이 성립한다.
    - $E(k) = k$ (상수의 기대값은 상수가 된다)
    - $E(kX) = kE(X)$ (확률변수를 상수 배 하면 기댓값도 상수 배가 된다)
    - $E(X+Y) = E(X) + E(Y)$ (확률 변수들 합의 기대값은 각 기대값의 합과 같다)
    - X와 Y가 서로 독립일 때 $E(XY) = E(X) \cdotp E(Y)$ (독립적인 확률변수의 곱에 대한 기댓값은 각 기댓값의 곱과 같다)

## 평균과 분산 그리고 공분산
- 평균
    - n개의 확률변수가 각각 $x_1, x_2, \cdotp\cdotp\cdotp, x_n$ 이라는 값을 가질 때 평균값 $\overline x$는 다음과 같다.
    $$\overline x =\sum_{k=1}^{n}{\frac 1 n}\cdotp x_k = {\frac 1 n}\sum_{k=1}^{n}x_k $$
- 분산
    - n개의 확률변수가 각각 $x_1, x_2, \cdotp\cdotp\cdotp, x_n$ 이라는 값을 가지고 평균값이 $\overline x$일때 분산 $\sigma^2$는 다음과 같다.
    $$\sigma^2 = {\frac 1 n}\sum_{k=1}^{n}{(x_k - \overline x)}^2$$
    $$\sigma = \sqrt{\sigma^2} = \sqrt{ {\frac 1 n}\sum_{k=1}^{n}{(x_k - \overline x)}^2 }$$
- 공분산
    - 두 가지 데이터에 대한 n조의 확률변수 (X,Y) = $\{(x_1,y_1),(x_2,y_2),...,(x_n, y_n\}$ 이 있다고 가정한다.
    - x의 평균이 $\mu_x$이고 Y의 평균이 $\mu_y$라고 할 때 공분산 Cov(X,Y)는 다음과 같다.
    $$COV(X,Y) = {\frac 1 n} \sum_{k=1}^{n}{(x_k - \mu_x)(y_k - \mu_y)}$$
    - 다른 두 데이터 간의 관계를 표현하는 지표이기 때문에 공분산을 계산할 때는 단위에 대해 신경쓸 필요가 없다.
    - 공분산이 양의 값을 가질 때, 두가지 데이터는 양의 관계가 있다고 하고, 공분산이 음의 값을 가질 때 두가지 데이터는 음의 관계가 있다고 한다.
    
## 상관계수
- 상관계수
    - 확률 변수 X와 Y가 분산이 양수이고 각각의 표준편차가 $\sigma_x$, $\sigma_y$, 공분산이 $\sigma_xy$라고 할 때의 상관게수 $(-1 \le \rho \le 1)$ 는 다음과 같다.
    - $$\rho = \frac {\sigma_xy} {\sigma_x \sigma_y}$$
    - 상관계수 $\rho$는 +1에 가까울 수록 양의 관계가 강하고, -1에 가까울수록 음의 관계가 강합니다. 상관관계가 0에 가까울수록 상관관계가 약하다고 보는데,
    일반적으로 상관계수의 절대값이 0.7보다 클 때 상관계수가 강하다고 말한다.
    
## 최대 가능도 추정
- 최대 가능도 추정이란 '가장 그럴듯 하게 추정' 한다는 의미로, 영어로는 가능도를 'likelihood'라고 표현합니다.
- 최대 가능도를 추정한 다는 말은 곧, 파라미터 $\theta$에 대한 가능도함수 $L(\theta)$를 최대화 할 수 있는 $\theta$값을 구하는 것을 의미한다.
- 1차 미분을 했을 때 ${\frac {dL(\theta)} {d\theta}} =0$ 이 되는 지점을 의미한다.
- 극값은 그래프에서 값이 변곡되는 곳에서 발생하기 때문에 0이 되는 지점은 최소값 혹은 최대값이 될 수 있다.

## 로그 가능도 함수
- 가능도 함수에 자연로그를 붙여 로그 가능도 함수 ${\frac {d} {d\theta}}\log_eL(\theta) = 0$를 만들면 된다.
- 일반적인 이산확률분포의 식은 확률의 곱으로 표현되는 일이 많다 보니 미분 자체가 어려운 경우가 많다.
- 로그를 사용하면 곱셈을 덧셈으로 만들고 수식의 차원을 낮춰 계산을 용이하게 하는 장점이 있다.
- 가능도 함수 $L(\theta_1,\theta_2,...,\theta_m)$을 최대로 하는 $\theta_1,\theta_2,...,\theta_m$는 다음 방정식을 만족한다.  
  $${\frac {\partial}{\partial\theta_1}}L(\theta_1,\theta_2,...,\theta_m) = 0$$
  $${\frac {\partial}{\partial\theta_2}}L(\theta_1,\theta_2,...,\theta_m) = 0$$
  $${\frac {\partial}{\partial\theta_m}}L(\theta_1,\theta_2,...,\theta_m) = 0$$
  
## 선형 회귀 모델
- 회귀 모델이란 하나의 목적변수(종속변수)를 하나 이상의 설명변수(독립변수)로 기술한 관계식이다.
- $\omega_0, \omega_1,... \omega_l$을 계수(가중치)라고 하고, $x_1,x_2,...,x_l$을 설명 변수라고 할 때 목적변수 y의 선형회귀 모델은 다음과 같이 기술 할 수 있다.
    $$y = \omega_0 + \sum_{k=1}^{l}{\omega_kx_k}$$
    $$y = \omega_0 + \omega_1x_1 + \omega_2x_2 + \omega_3x_3 + \cdots + \omega_lx_l$$
    $$\begin {bmatrix}
       y1 \\ y2 \\ {\vdots} \\ y_n 
       \end{bmatrix} = 
       \begin {bmatrix}
       1 & x_11 & x_12 & {\cdots} & x_1l \\
       1 & x_21 & x_22 & {\cdots} & x_1l \\ 
       {\vdots} & {\vdots} & {\vdots} & {\vdots} & {\vdots} \\ 
       1 & x_n1 & x_n2 & {\cdotp\cdotp\cdotp} & x_nl \\
       \end{bmatrix} 
       \begin {bmatrix}
       \omega_0 \\
       \omega_1 \\
       {\vdots} \\
       \omega_l \\
       \end{bmatrix}
     $$
     
## 최소 제곱법
- 모델식에 가장 잘 들어맞는 가중치를 찾는 것이 중요한데, 이때 사용할 수 있는 근사법으로 *최소제곱법*이라는 것이 있다.
- 최소제곱법에서는 데이터 세트와 같은 수치 데이터들을 1차 함수와 같은 특정 함수를 사용하여 근사적으로 표현할 수 있다고 가정한다.
- 목적변수를 Y, 설명변수를 $X_1, X_2, X_3,... ,X_l$ 그리고 모델식을 $f(X_1, X_2,... ,X_l)$이라고 할때, 
  최소 제곱법을 적용 하는 과정은 오차의 제곱합 D를 최소화 하는 $f(X_1, X_2,... ,X_l)$를 구하는 것이다.
- $D = \sum_{k=1}^{n}({y_k - f(x_k1, x_k2, \cdots, x_kl}))^2$
- $f(x_k1, x_k2, \cdots, x_kl) = \sum_{m=1}^{l}w_mx_km + \omega_0$
- 각 변수의 편미분을 진행하면
  ${\frac {\partial D}{\partial\omega_0}} = 0$,  ${\frac {\partial D}{\partial\omega_1}} = 0$ , $\cdots$, $  ${\frac {\partial D}{\partial\omega_l}} = 0$
- 구하고자 하는 변수와 연립방정식이 l+1개가 있으므로 $\omega_0, \omega_1, \cdots, \omega_l$의 값을 구하는 것은 어렵지 않다.
- 선형 회귀 모델에서는 최소제곱법을 사용해서 가중치의 값을 결정한다.



