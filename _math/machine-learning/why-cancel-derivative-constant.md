---
layout: sidebar
title: 미분 상수를 상쇄하는 이유
collection_name: notes
nav_order: 4999
---

## 수학적 편의성 (상수 1/2의 도입)

### 스칼라 방정식의 관점

손실 함수를  $L = (f - y)^2$  이라고 정의했을 때, 이를 예측값  $f$  로 편미분하면 합성함수의 미분법에 의해  $2(f-y)$  가 도출된다. 이 앞에 붙은 상수  $2$  는 이후 역전파 과정에서 모든 레이어의 그래디언트 수식에 계속 곱해진다.  

반면, 애초에 목적 함수를  $L = \frac{1}{2}(f - y)^2$  으로 정의해 두면, 미분 값이  $(f - y)$  라는 순수한 오차항 자체로만 떨어지게 되어, 논문에서 수식을 전개하거나 실제 코드를 구현할 때 불필요한 상수를 매번 추적하고 연산해야 하는 대수적 번거로움을 덜어준다.  

### 다변수 이차형식(Quadratic Form)의 관점

이러한 수학적 편의성은 단일 변수를 넘어, 기계 학습과 볼록 최적화에서 자주 등장하는 다차원 행렬 연산인 '이차형식'에서 더욱 극대화된다.  

다수의 파라미터를 벡터  $\boldsymbol{x}$  로, 가중치 관계를 나타내는 정방 대칭 행렬을  $\boldsymbol{Q}$  로 두었을 때, 일반적인 이차 목적 함수는  $L(\boldsymbol{x}) = \boldsymbol{x}^\top \boldsymbol{Q}\boldsymbol{x}$  의 형태를 띤다.  

이 함수를 파라미터 벡터  $\boldsymbol{x}$  에 대해 미분(그래디언트)하면 다음과 같은 결과가 도출된다.  

<div class="math-container">
$$  
\nabla_{\boldsymbol{x}} (\boldsymbol{x}^\top \boldsymbol{Q}\boldsymbol{x}) = (\boldsymbol{Q} + \boldsymbol{Q}^\top)\boldsymbol{x}  
$$
</div>


여기서 최적화 문제의 행렬  $\boldsymbol{Q}$  는 대칭 행렬( $\boldsymbol{Q} = \boldsymbol{Q}^\top$ )로 가정하는 것이 일반적이므로, 미분 결과는  $2\boldsymbol{Q}\boldsymbol{x}$  가 된다.  

따라서 스칼라 방정식과 마찬가지로, 애초에 목적 함수를  $L(\boldsymbol{x}) = \frac{1}{2}\boldsymbol{x}^\top \boldsymbol{Q}\boldsymbol{x}$  로 정의해 두면, 그래디언트가  $\boldsymbol{Q}\boldsymbol{x}$  로, 2차 미분인 헤시안(Hessian) 행렬이 순수한  $\boldsymbol{Q}$  로 깔끔하게 떨어지게 된다. 이는 수만 번의 반복 연산을 수행하는 최적화 알고리즘에서 불필요한 상수 곱셈 비용(Overhead)을 완전히 제거해 주는 핵심적인 대수학적 설계다.  

## 결과의 차이가 있을까?

목적 함수에 상수를 곱하거나 더하는 것이 최적해를 바꾸지 않을까 생각해볼 수 있다. 결론부터 말하면, 상수를 조작했을 때와 하지 않았을 때 알고리즘이 찾아내는 최종 파라미터 값(최적화 결과)은 수학적으로 완벽하게 동일하다.  

기계 학습의 최적화 목표는 손실 함수  $L(\theta)$  의 값을 최소로 만드는 파라미터  $\theta$  의 위치를 찾는 것, 즉  $\arg\min\_{\theta} L(\theta)$  를 구하는 것이다.  

**상수를 곱하는 경우:**  

임의의 양의 상수  $c$  에 대하여 함수  $c \cdot L(\theta)$  를 고려해보자. 모든 함숫값에  $c$  를 곱하면 다차원 공간에서 손실 곡면(Loss landscape)의 전체적인 높낮이(Scale)만 가파르거나 완만하게 변할 뿐, 극소점이 위치한 밑면의 좌표  $\theta$  자체는 기하학적으로 절대 이동하지 않는다 (함수  $x^2$  의 계수가 변해도 극소점의 위치는 원점으로 고정되어 있는 것과 같다). 따라서 목적 함수를 스케일링하는 것은 최적점의 위치에 아무런 영향을 주지 않는다는 논리가 성립한다.  

**상수를 더하는 경우:**  

함수  $L(\theta)$  에 상수  $C$  를 더하여  $L\_{new}(\theta) = L(\theta) + C$  를 만든다는 것은, 기하학적으로 볼 때 손실 곡면 전체를 손실값 축(z축)을 따라 그대로 수직 평행 이동시키는 것과 같다. 파라미터 공간 전체가 수직으로 이동한다 해서 그 공간의 극소점 좌표가 바뀌지는 않는다.  

미분 관점에서 봐도 파라미터  $\theta$  의 입장에서 상수  $C$  는 아무런 변화율을 가지지 않는다.  

<div class="math-container">
$$  
\nabla_\theta L_{new}(\theta) = \nabla_\theta (L(\theta) + C) = \nabla_\theta L(\theta) + \nabla_\theta C = \nabla_\theta L(\theta) + 0 = \nabla_\theta L(\theta)  
$$
</div>


때문에,  $\theta\_{new} = \theta\_{old} - \eta \nabla L$  로 파라미터를 업데이트해도 수렴하는 최적해의 위치는 변하지 않는다.  

## 그럼 무엇으로 인해 최적해가 바뀔까?

단순한 상수 조작이 아닌, 최적해의 위치 자체를 기하학적으로 변화시키는 대수학적 요인으로는 다음 세 가지를 생각해 볼 수 있다.  

**첫 번째, 파라미터 크기에 비례하는 페널티 항의 추가 (정규화, Regularization)**  

보통 릿지 회귀(Ridge Regression, L2 정규화)에서 사용되는 방식이다.  

<div class="math-container">
$$  
L_{new}(\theta) = L(\theta) + \lambda \Vert \theta\Vert ^2  
$$
</div>


여기서  $\lambda$  는 정규화의 강도를 조절하는 하이퍼파라미터이다. 이 함수를  $\theta$  로 미분하면 다음과 같다.  

<div class="math-container">
$$  
\nabla_\theta L_{new}(\theta) = \nabla_\theta L(\theta) + 2\lambda\theta  
$$
</div>


미분 결과에  $2\lambda\theta$  항이 생겼다. 기존에는  $\nabla\_\theta L(\theta) = 0$  이 되는 곳이 최적해였지만, 이제는  $\nabla\_\theta L(\theta) = -2\lambda\theta$  를 만족하는 새로운 지점으로 최적해  $\theta^\*$  가 강제로 이동한다. 기하학적으로 이는 원래의 손실 계곡을 원점( $\theta=0$ ) 쪽으로 강하게 끌어당기는 것과 같다. 이를 통해 파라미터가 불필요하게 커지는 과적합(Overfitting)을 방지할 수 있다.  

**두 번째, 특정 데이터의 오차에 대한 가중치 부여**  

<div class="math-container">
$$  
L_{new}(\theta) = (y - f(\theta))^\top \boldsymbol{W} (y - f(\theta))  
$$
</div>


대칭 행렬  $\boldsymbol{W}$  가 중간에 삽입됨으로써, 손실 곡면의 기울어짐이 완전히 달라진다.  $\boldsymbol{W}$  의 대각 원소 중 특정 값이 크다면, 모델은 그 특정 데이터 포인트에서 발생하는 오차를 비교적 크게 받아들이게 된다. 따라서 최적해  $\theta^\*$  는 가중치가 높은 데이터를 우선적으로 정답에 맞추는 방향으로 공간상에서 크게 이동하게 된다.  

**세 번째, 오차를 측정하는 거리(Norm) 정의의 변경**  

보통 오차를 L2 Norm(MSE)으로 구하지만, 이를 L1 손실(MAE)로 변경할 수 있다.  

<div class="math-container">
$$  
L_{new}(\theta) = \sum \vert y_i - f(\theta, x_i)\vert   
$$
</div>


MSE는 오차가 클수록 포물선처럼 손실값이 기하급수적으로 커지므로, 최적해가 이상치(Outlier, 튀는 데이터) 쪽으로 크게 끌려간다. 반면 MAE는 오차가 커져도 손실값이 선형적으로만 증가하므로 기울기가 일정( $+1$  또는  $-1$ )하여 이상치에 저항성(Robustness)을 가진다. 목적 함수의 기하학적 형태가 둥근 포물면에서 V자 형태의 평면 결합으로 바뀌면서 극소점의 위치가 근본적으로 변하게 된다.
