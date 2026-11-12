---
layout: sidebar
title: 이토 적분
collection_name: genai
nav_order: 8999
date: 2026-05-16
---

# 1. 문제의 정의: 노이즈를 적분한다는 것

<div class="math-container">
$$  
I(t) = \int_0^T f(t, W_t) dW_t  
$$
</div>


-  $W\_t$ : 위너 과정(브라운 운동)
-  $f(t, W\_t)$ : 피적분 함수(ex. 현재의 주가, 데이터의 상태 등)
-  $dW\_t$ : 노이즈의 변화량

일반 미적분(리만 적분)처럼 이를 합의 극한으로 표현. 구간  $[0, T]$ 를 N개로 ( $0 = t\_0 < t\_1 < \dots t\_N = T$ ).  

<div class="math-container">
$$  
I(t) \approx \sum_{i=0}^{N-1} f(t_i^*, W_{t_i^*}) (W_{t_{i+1}} - W_{t_i})  
$$
</div>


여기서 어느 시점( $t\_i^\*$ )의 함숫값을 높이로 쓸 것인가?가 결정적인 문제가 됨.  

# 2. 이토 적분의 정의: "왼쪽 끝점을 선택"
이토 적분은 적분 시 직사각형의 높이를 구간의 왼쪽 끝점( $t\_i$ )에서 잰다고 정의.  

<div class="math-container">
$$  
t_i^* = t_i \quad (\text{Left Endpoint})  
$$
</div>


이토 적분의 정의:  

<div class="math-container">
$$  
\int_0^T f(t) dW_t = \lim_{N \to \infty} \sum_{i=0}^{N-1} f(t_i) (W_{t_{i+1}} - W_{t_i})  
$$
</div>


왜 하필 왼쪽인가에 대한 답변으로 인과율과 Non-Anticipating(미래를 미리 보지 않음)성질을 만족하기 때문임.  
예시 상황: 주식( $W\_t$ )에 투자하는 전략( $f$ )을 짜는데 이때 구간은  $t\_i$ 에서  $t\_{i+1}$ 사이임.  $t\_i$ 시점에 의사결정을 내릴 때, 나는  $t\_i$ 시점의 정보( $W\_{t\_i}$ )만 알 수 있음.  $t\_{i+1}$ 에 주가가 어떻게 변할지  $W\_{t\_{i+1}}$ 는 아직 모름. 따라서 변화량  $(W\_{t\_{i+1}} - W\_{t\_i})$ 와 곱해지는 값은 현재 시점( $t\_i$ )의 값이어야 함. 만약 오른쪽 점을 쓴다면 미래의 값을 보고 현재 투자를 결정하는 셈이 되어 수학적으로 공정한 게임이 되지 않음.  

# 3. 이토의 보조 정리: SDE의 연쇄 법칙
이토 적분을 매번 리만 합의 극한으로 계산하는 것은 너무 어려움. 일반 미적분학에서 미분과 적분의 관계를 통해 쉽게 계산하듯이, SDE에서도 함수의 변화량( $df$ )을 구하는 공식이 필요.  
이것이 SDE 분야에서  $E = mc^2$ 이라 불리는 이토의 보조 정리(Itô's Lemma)임.  
어떤 함수  $F(x, t)$ 가 있고,  $x$ 가 위너 과정  $W\_t$ 를 따른다고 할 때, 일반적인 전미분(Total Derivative)은 다음과 같음.(테일러 급수 1차 근사):  

<div class="math-container">
$$  
dF = \frac{\partial F}{\partial t}dt + \frac{\partial F}{\partial x}dx  
$$
</div>


하지만 이토 미적분에서는 2차항을 무시할 수 없음. 따라서 테일러 급수를 2차항 이상까지 전개해보면:  

<div class="math-container">
$$  
dF = \frac{\partial F}{\partial t}dt + \frac{\partial F}{\partial x}dx + \frac{1}{2}\frac{\partial^2 F}{\partial x^2}(dx)^2 + \dots  
$$
</div>


여기  $dx$ 자리에 SDE인  $dx = fdt + gdW$ 를 대입함.(편의상 간단히  $dx \approx gdW$ 라고 생각.  $dt$ 는  $dW$ 에 비해 너무 작으니깐.)  
이때  $(dx)^2$ 은 어떻게 될까?  

<div class="math-container">
$$  
(dx)^2 \approx (g dW)^2 = g^2 (dW)^2  
$$
</div>


이때  $(dW)^2 = dt$ 를 대입해보면:  

<div class="math-container">
$$  
(dx)^2 = g^2 dt  
$$
</div>


이걸 다시 위 테일러 전개식에 집어넣으면 이토 공식이 탄생함.  

<div class="math-container">
$$  
dF = \left( \frac{\partial F}{\partial t} + \frac{\partial F}{\partial x}f + \frac{1}{2}g^2 \frac{\partial^2 F}{\partial x^2} \right) dt + \left( \frac{\partial F}{\partial x}g \right) dW  
$$
</div>


( $\frac{1}{2}g^2 \frac{\partial^2 F}{\partial x^2} dt$ )가 일반 미적분학에는 없는 이토 보정항임. 노이즈의 변동성(Variance)이 함수의 평균적인 흐름(Drift)에 영향을 미치는 현상임.  

# 4.  $\int\_0^T W\_t dW\_t$  계산
일반 미적분학에서는  $\int x dx = \frac{1}{2}x^2$ 이므로,  $\int W dW = \frac{1}{2}W^2$ 이 될 것 같지만, 이토 공식은 전혀 다른 결과가 유도됨.  
함수  $F(W\_t) = \frac{1}{2}W\_t^2$ 의 변화량( $dF$ )을 구해서 적분하려 함.  
-  $F(x) = \frac{1}{2}x^2$ 
- 1계 미분:  $F'(x) = x$ 
- 2계 미분:  $F''(x) = 1$ 
이토 공식( $dF = F' dW + \frac{1}{2}F'' (dW)^2$ )에 대입:  

<div class="math-container">
$$  
d(\frac{1}{2}W_t^2) = W_t dW_t + \frac{1}{2}(1)(dt)  
$$
</div>


양변을 0에서 T까지 적분:  

<div class="math-container">
$$  
\int_0^T d(\frac{1}{2}W_t^2) = \int_0^T W_t dW_t + \int_0^T \frac{1}{2} dt  
$$
</div>


좌변은 미분의 적분이므로 값의 차이임.( $W\_0 = 0$  가정)  

<div class="math-container">
$$  
\frac{1}{2}W_T^2 - \frac{1}{2}W_0^2 = \int_0^T W_t dW_t + \frac{1}{2}T  
$$
</div>


위 식을 구하려는 적분( $\int W\_t dW\_t$ )에 대해 정리:  

<div class="math-container">
$$  
\int_0^T W_t dW_t = \frac{1}{2}W_T^2 - \frac{1}{2}T  
$$
</div>


일반 미적분학의 결과와 다르게  $-\frac{1}{2}T$ 라는 값이 추가됨. 이 항은 시간(t)이 지날수록(T가 커질수록), 적분값이 예상보다 작아짐을 의미함. 이것이 노이즈의 불확실성이 누적되어 생긴 편향임.  

이토 미적분  
- 변동성:  $dW$ 는  $\sqrt{dt}$ 만큼 크다.
- 제곱: 따라서  $(dW)^2$ 은  $dt$ 가 되어 무시할 수 없음.
- 결과: 미분을 할 때 2차 미분항( $\frac{1}{2}F''$ )이 살아남아, 식에 추가적인 항(Drift term)을 만들어냄.
