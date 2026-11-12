---
layout: sidebar
title: Score based models
collection_name: genai
nav_order: 8999
date: 2026-04-29
---

Score의 정의는 데이터 확률 밀도 함수에 로그를 취한 후 데이터 공간에 대해 미분한 벡터 장, 즉  $\nabla\_{\mathbf{x}} \log p(\mathbf{x})$ 이다. 이는 데이터 공간에서 확률 밀도가 가장 가파르게 증가하는 방향을 지시한다. DDPM은 주입된 노이즈  $\boldsymbol{\epsilon}\_\theta$ 를 예측하도록 신경망을 학습시킨다. 반면 양송의 SBM은 이 스코어를 직접 추정하도록 신경망을 학습시킨다. DDPM 논문의 수식 전개에 따르면 신경망이 예측한 노이즈에 상수를 곱한 값은 해당 시점의 스코어와 동치이다. 따라서 노이즈를 예측하는 행위는 암묵적으로 스코어 매칭을 수행하는 것과 같다.  



<div class="obsidian-callout" markdown="1">
스코어와 그래디언트의 차이는 무엇인가?

정의의 차이:

그래디언트는 임의의 다변수 스칼라 함수  $f(\mathbf{x})$ 를 벡터  $\mathbf{x}$ 에 대해 편미분한 벡터이다.  $\nabla\_{\mathbf{x}} f(\mathbf{x})$ 로 표기하며, 해당 공간상에서 함수값이 가파르게 증가하는 방향을 지시한다.

스코어는 확률 밀도 함수  $p(\mathbf{x})$ 에 자연로그를 취한 함수를 데이터 변수  $\mathbf{x}$ 에 대해 편미분한 벡터이다.  $\nabla\_{\mathbf{x}} \log p(\mathbf{x})$ 로 표기하며, 데이터 공간상에서 확률 밀도가 가파르게 증가하는 방향을 지시한다.

기계학습 적용 맥락에서의 차이:

미분 변수: 일반적인 모델 학습 과정에서 사용하는 그래디언트는  $\nabla\_\theta L(\theta)$ 이다. 미분 변수는 신경망의 가중치 파라미터  $\theta$ 이다. 반면 스코어는  $\nabla\_{\mathbf{x}} \log p(\mathbf{x})$ 이다. 미분 변수는 신경망의 가중치가 아닌 데이터 공간의 상태 변수  $\mathbf{x}$ (픽셀 좌표 값 등)이다.

대상 공간: 그래디언트는 매개변수를 최적화하기 위해 손실 함수  $L$  공간을 탐색한다. 스코어는 데이터를 생성하기 위해 확률 밀도 함수  $p$  공간을 탐색한다.

스코어는 데이터 매니폴드를 정의하는 지표이다. 랑주뱅 동역학의 이산화된 수식은 다음과 같다.

<div class="math-container">
$$
\mathbf{x}_{i+1} = \mathbf{x}_i + c \nabla_{\mathbf{x}} \log p(\mathbf{x}_i) + \sqrt{2c} \mathbf{z}_i
$$  
</div>


이 수식에서  $\nabla\_{\mathbf{x}} \log p(\mathbf{x}\_i)$ 가 스코어이다. 스코어는 현재 상태  $\mathbf{x}\_i$ 를 확률 밀도가 높은 영역, 즉 실제 관측 데이터가 밀집하여 존재하는 매니폴드 방향으로 견인하는 벡터장으로 작용한다.
</div>



스코어는  $\nabla\_{\mathbf{x}} \log p(\mathbf{x})$ 로 정의된다. 임의의 목적 함수  $f(\mathbf{x})$ 의 최댓값을 찾기 위해 기울기 방향으로 이동하는 경사 상승법의 업데이트 규칙은  $\mathbf{x} \leftarrow \mathbf{x} + \alpha \nabla\_{\mathbf{x}} f(\mathbf{x})$ 이다. 여기서 최적화할 목적 함수  $f(\mathbf{x})$ 를 데이터의 로그 확률 밀도 함수  $\log p(\mathbf{x})$ 로 치환하면, 업데이트 수식은  $\mathbf{x} \leftarrow \mathbf{x} + \alpha \nabla\_{\mathbf{x}} \log p(\mathbf{x})$ 가 된다. 이는 스코어 벡터의 방향으로 이동하는 것과 같다  



<div class="obsidian-callout" markdown="1">
스코어를 따라가는 것은 기하학적으로 경사상승법과 동일하다.
</div>





<div class="obsidian-callout" markdown="1">
왜 스코어라는 이름이 붙었는가?

스코어라는 용어는 영국의 통계학자 로널드 피셔가 1925년에 고안한 피셔 정보와 스코어 검정에서 처음 등장했다.

통계학에서 스코어 함수  $V(\theta)$ 는 로그 우도 함수를 '데이터  $\mathbf{x}$ 가 아니라 매개변수  $\theta$ 에 대해 편미분한 값으로 정의된다.

<div class="math-container">
$$
V(\theta) = \nabla_{\theta} \log p(\mathbf{x}; \theta)
$$  
</div>


통계학자들은 주어진 데이터  $\mathbf{x}$ 를 바탕으로 가장 타당한 확률 분포의 매개변수  $\theta$ 를 찾고 싶어 했다(MLE). 이때  $\nabla\_{\theta} \log p(\mathbf{x}; \theta)$ 를 계산하면, 현재 설정된  $\theta$ 값에서 "이 매개변수를 미세하게 변경했을 때, 현재 데이터가 설명될 확률(우도)이 얼마나 가파르게 상승하는가?"를 알 수 있다. 즉, 이 미분값은 현재 매개변수  $\theta$ 가 최적값에 얼마나 가까운지를 평가하고 채점(scoring)하는 지표로 사용되었다. 만약 스코어가 0이라면, 현재  $\theta$ 가 확률을 극대화하는 최적의 매개변수라는 뜻이다.

2005년, 핀란드의 컴퓨터 과학자 아포 히바리넨이 "Estimation of Non-Normalized Statistical Models by Score Matching"이라는 논문을 발표하며 이 용어를 기계학습으로 가져온다. 히바리넨은 피셔의 수식에서 미분하는 대상(변수)을  $\theta$ 에서 데이터  $\mathbf{x}$ 로 바꾸었다. 따라서 기계학습에서 스코어 함수는 로그 확률밀도함수를 데이터  $\mathbf{x}$ 에 대해 편미분한 벡터장이다.

<div class="math-container">
$$
\text{Score} = \nabla_{\mathbf{x}} \log p(\mathbf{x})
$$  
</div>


피셔의 스코어가 매개변수  $\theta$ 의 타당성을 평가하는 점수라면, 디퓨전 모델이나 스코어 기반 모델에서 말하는  $\nabla\_{\mathbf{x}} \log p(\mathbf{x})$ 는 데이터  $\mathbf{x}$  자체의 현실성을 평가하고 유도하는 점수로 해석할 수 있다.

로그 확률 밀도  $\log p(\mathbf{x})$ 는 특정 이미지  $\mathbf{x}$ 가 실제(데이터셋)로 존재할 법한 확률, 즉 얼마나 진짜 같은지 나타내는 절대적인 평가 점수이다.(고양이 사진은 점수가 높고, 백색 잡음은 점수가 낮음.)

그래디언트  $\nabla\_{\mathbf{x}}$ 는 그 점수가 가장 가파르게 올라가는 방향과 크기이다.

따라서 생성모델에서의 스코어는 현재 노이즈 낀 이미지  $\mathbf{x}$ 의 픽셀 값들을 어느 방향으로 움직여야 진짜 데이터로서의 점수(확률 밀도)를 가장 빠르게 높일 수 있는가를 알려주는 역할을 한다. 결론적으로 미분 변수가 다르지만 확률 분포의 최대점을 찾기 위한 지표라는 본질은 동일하다.
</div>





<div class="obsidian-callout" markdown="1">
스코어가 0이라는 것의 의미는 무엇인가?

다변수 미적분학의 임계점: 어떤 다변수 함수에서 그래디언트가 0이라는 것은 해당 좌표에서 함수의 1차 미분 변화율이 0임을 나타낸다. 스코어  $\nabla\_{\mathbf{x}} \log p(\mathbf{x}) = \mathbf{0}$ 은 현재 데이터 상태  $\mathbf{x}$ 의 아주 미세한 인접 영역에서 확률 밀도 값의 변화가 없는 지점임을 뜻한다. 기계학습이 다루는 확률 밀도 함수가 위로 볼록한 형태를 띤다고 전제할 때, 이는 해당 데이터 공간 내에서 확률 밀도가 가장 높은 극대점에 위치해 있음을 증명한다.

랑주뱅 동역학에서 결정론적 표류항(drift term)의 소실: 샘플링을 위한 랑주뱅 동역학 점화식  $\mathbf{x}\_{i+1} = \mathbf{x}\_i + c \nabla\_{\mathbf{x}} \log p(\mathbf{x}\_i) + \sqrt{2c} \mathbf{z}\_i$ 에서 스코어 항이 0이 되면 결정론적인 이동 수식( $c \nabla$ )이 완전히 소실된다. 이는 시스템이 현재의 상태  $\mathbf{x}\_i$ 를 관측 데이터 분포 내에서 확률 밀도가 가장 높은 온전한 데이터로 판정하였음을 의미한다. 알고리즘은 데이터를 특정 방향으로 수정하는 연산을 멈추며, 이후 입자는 오직 확률적 섭동 항( $\mathbf{z}\_i$ )에 의해서만 해당 극대점 주변을 무작위로 맴돌게 된다.

피셔 스코어에서의 최대 우도 추정(MLE) 완료: 피셔의 스코어인  $V(\theta) = \nabla\_{\theta} \log p(\mathbf{x}; \theta) = \mathbf{0}$ 의 경우는 매개변수  $\theta$  공간에서 우도(Likelihood)가 극대화되었음을 뜻한다. 이는 주어진 데이터  $\mathbf{x}$ 의 분포를 가장 타당하게 설명할 수 있는 최적의 매개변수  $\theta$ 를 찾는 편미분 방정식의 해가 도출되었음을 의미한다.
</div>





<div class="obsidian-callout" markdown="1">
그럼 피셔의 스코어는 일반적인 기계학습에서 말하는 최대 우도추정(MLE)에서  $p(x\vert theta)$ 를 최대화하는 매개변수( $\theta$ )를 추정하기 위함이고, 기계학습에서의 스코어는 어떤 무작위 데이터가 관측데이터 방향(밀도가 높은)으로 가기 위한 나침반인가? O

피셔의 스코어는 고정된 관측 데이터  $\mathbf{x}$ 를 가장 잘 설명하는 최적의 매개변수를 찾는 것이다.

<div class="math-container">
$$
\theta \leftarrow \theta + \alpha \nabla_{\theta} \log p(\mathbf{x}; \theta)
$$  
</div>


일반적인 경사하강법은 오차(Loss) 함수  $L(\theta)$ 를 '최소화'하는 것이 목적이므로, 그래디언트의 반대 방향( $-$ )으로 매개변수를 갱신한다.

<div class="math-container">
$$
\theta \leftarrow \theta - \alpha \nabla_{\theta} L(\theta)
$$  
</div>


만약 손실함수를 음의 로그 우도로 정의한다면, 경사하강법의 업데이트 연산은 피셔 스코어를 빼는 형태로 동일하다.
</div>





<div class="obsidian-callout" markdown="1">
피셔 스코어가 일반적인 그래디언트와 구분되는 성질은 현재 모델의 매개변수  $\theta$ 가 실제 데이터 분포를 완벽히 모사하고 있다면, 피셔 스코어의 기댓값은 정확히 0이 된다.

<div class="math-container">
$$
\mathbb{E}_{p(\mathbf{x};\theta)} [\nabla_{\theta} \log p(\mathbf{x}; \theta)] = 0
$$  
</div>


Q. 매개변수  $\theta$ 가 실제 데이터 분포를 완벽히 모사함을 어떻게 확인하는가? 이는 개념적인 정의인가?
</div>





<div class="obsidian-callout" markdown="1">
일반적인 경사하강법은 파라미터 공간이 유클리드 공간이라고 가정하고 그래디언트를 따라간다. 하지만 확률 분포의 파라미터 공간은 곡률이 존재하는 비유클리드 기하학(정보 기하학)의 성질을 띤다.

피셔 스코어 값들을 제곱하여 기댓값(분산)을 구하면, 이를 피셔 정보 행렬(fisher information matrix, fim)이라고 부른다. 이 행렬은 파라미터 공간의 곡률을 나타낸다.

단순히 피셔 스코어(그래디언트)만 따라가는 1차 미분 최적화를 넘어, 이 피셔 정보 행렬의 역행렬을 그래디언트에 곱해주면 확률 공간의 기하학적 구조까지 반영하여 훨씬 더 빠르게 최적값을 찾는 2차 미분 최적화 알고리즘이 된다. 이를 자연 경사 하강법, 피셔 스코어링 알고리즘이라고 부른다.

Q. 연산량이 많을거같은데.
</div>






<div class="obsidian-callout" markdown="1">
 $\mathbf{x}\_{i+1} = \mathbf{x}\_i + c \nabla\_{\mathbf{x}} \log p(\mathbf{x}\_i) + \sqrt{2c} \mathbf{z}\_i$ 에서 상수  $c$ 는 경사 상승법의 학습률 혹은 스텝 사이즈와 동일한 역할을 한다.

또, 무작위 노이즈  $\mathbf{z}\_i$ 에 곱해지는 계수는  $\sqrt{2c}$ 인데, 이는 보폭  $c$ 가 결정되면, 이에 비례하여 확률적 흔들림(섭동)의 강도까지 결정됨을 의미한다. 보폭이 클수록 스코어 방향으로 크게 이동함과 동시에 무작위 탐색 반경도 넒어지며, 보폭이 작아지면 극소 영역에서만 정교하게 배회한다.
</div>





<div class="obsidian-callout" markdown="1">
스코어에 음수를 취하여 음의 로그우도를 최소화한다면 어떤 해석을 할 수 있는가?

이는 에너지 기반 모델(energy-based model, EBM) 관점과 정확히 일치한다.

통계 물리학 및 기계학습에서는 음의 로그 우도를 시스템의 에너지로 정의한다( $E(\mathbf{x}) = -\log p(\mathbf{x})$ ).

자연계의 물리 시스템은 에너지가 가장 낮은 안정된 상태로 수렴하려는 성질이 있다. 따라서  $-\nabla\_{\mathbf{x}} \log p(\mathbf{x})$ 를 따라 하강하는 것은, 입자가 에너지가 높은 불안정한 상태(무작위 노이즈)에서 에너지가 가장 낮은 바닥 상태(실제 데이터 매니폴드)를 향해 굴러 떨어지는 과정으로 해석된다. 이는 목적 함수의 부호만 반전되었을 뿐, 고밀도 확률 영역을 찾아간다는 기하학적 결과는 동일하다.
</div>





<div class="obsidian-callout" markdown="1">
스코어를 추정한다는 것은 무슨 뜻인가?

스코어  $\nabla\_{\mathbf{x}} \log p(\mathbf{x})$ 를 수학적으로 완벽하게 계산하려면 실제 True 확률 분포  $p(\mathbf{x})$ 의 수식을 알아야 하지만, 이는 불가능하다.

따라서 '스코어를 추정한다'는 것은 진짜 수식을 직접 미분하는 대신, 임의의 입력 데이터  $\mathbf{x}$ 를 받았을 때 그 위치에서의 스코어 벡터를 계산하여 출력하는 인공신경망  $\mathbf{s}\_\theta(\mathbf{x})$ 를 만드는 것을 의미한다.

즉, 신경망 매개변수  $\theta$ 를 훈련시켜  $\mathbf{s}\_\theta(\mathbf{x}) \approx \nabla\_{\mathbf{x}} \log p(\mathbf{x})$ 가 성립하도록 벡터장의 형태를 근사(Approximation)하는 과정이다.
</div>





<div class="obsidian-callout" markdown="1">
Q. 그럼 데이터가 관측된 부분에서의 밀도값만 아는 것인가? 이는 최대 우도 추정이 아닌가?

관측된 데이터의 좌표에서만 밀도값을 무한히 높이는 현상을 통계학에서 디랙 델타 함수로의 붕괴라고 한다. 모델이 관측된 데이터 좌표에만 밀도를 할당하고 그외 공간에 확률 밀도를 0으로 부여한다면, 모델은 데이터를 암기할 뿐 새로운 데이터를 생성할 수 없다.

최대 우도 추정의 목적 함수가 관측 데이터  $\mathbf{x}\_i$ 에서의 확률  $p(\mathbf{x}\_i)$ 를 최대화하도록 지시하더라도, 이를 근사하는 인공신경망  $p\_\theta$ 는 불연속적인 함수를 만들어낼 수 없다. 신경망을 구성하는 선형변환과 활성화 함수(ReLU, SiLU 등)는 본질적으로 연속 함수이며, 일정 수준의 립시츠 연속성을 가진다.

따라서 신경망이 특정 관측 좌표  $\mathbf{x}\_i$ 의 밀도값을 높이기 위해 매개변수  $\theta$ 를 갱신하면, 수학적 연속성에 의해 해당 좌표 주변의 미관측 공간  $\mathbf{x} + \Delta\mathbf{x}$ 의 밀도값도 함께 상승하게 된다. 이 과정을 통해 신경망은 관측된 점들을 부드럽게 연결하는 매니폴드 표면을 형성하며, 관측되지 않은 데이터 공간에 대해서도 유효한 확률 밀도를 보간해 낸다.

평활화에도 불구하고 고차우너 공간에서 순수 MLE는 여전히 관측 데이터 주변으로만 극단적으로 수렴하려는 과적합 문제를 겪는다. SGM과 DDPM은 관측 데이터 공간  $p\_0(\mathbf{x})$ 를 직접 학습하는 것을포기하고, 수학적으로 의도된 노이즈를 주입하여 문제를 우회한다.

관측 데이터에 분산  $\sigma^2$ 의 가우시안 노이즈를 주입한 분포를  $p\_\sigma(\tilde{\mathbf{x}})$ 라고 할 때, 모델이 학습하는 대상은 고립된 점들이 아니게 된다. 노이즈를 주입하는 연산은 합성곱(convolution)과 같아서, 뾰족했던 관측 데이터의 확률 밀도를 전체 공간으로 넓게 퍼뜨린다(support 확산).

신경망은 점(관측 데이터)이 아니라, 이 퍼져나간 확률 밀도 함수의 기울기인 스코어  $\nabla\_{\mathbf{x}} \log p\_\sigma(\mathbf{x})$ 를 학습한다. 공간 전체에 확률 밀도가 0이 아닌 값을 존재하게 되므로, 모델은 관측 데이터가 없는 텅 빈 공간에서도 "어느 방향으로 가야 관측 데이터가 존재하는 고밀도 매니폴드가 나오는가"를 알려주는 벡터장을 전역적으로 학습할 수 있게 된다.
</div>





<div class="obsidian-callout" markdown="1">
스코어 매칭의 단어 뜻은 무엇인가?

신경망이 예측한 추정 스코어  $\mathbf{s}\_\theta(\mathbf{x})$ 를 실제 데이터의 스코어  $\nabla\_{\mathbf{x}} \log p(\mathbf{x})$ 와 일치(matching)시키는 학습 기법이다.

두 벡터장 사이의 유클리디안 거리(평균 제곱 오차)를 최소화하는 것이 목적이다.

<div class="math-container">
$$
\min_\theta \mathbb{E}_{p(\mathbf{x})} \left[ \frac{1}{2} \Vert  \mathbf{s}_\theta(\mathbf{x}) - \nabla_{\mathbf{x}} \log p(\mathbf{x}) \Vert ^2 \right]
$$  
</div>


실제 스코어  $\nabla\_{\mathbf{x}} \log p(\mathbf{x})$ 를 알 수 없는데 어떻게 오차를 구해 매칭시킬 것인가가 문제이다. 2005년 아포 히바리넨(Aapo Hyvärinen)은 미적분학의 부분 적분(Integration by parts)을 활용하여, 실제  $p(\mathbf{x})$ 를 모르더라도 위 목적 함수를 최소화할 수 있는 완전히 동치인 연산식을 유도해 내었다. 이 수학적 기법 자체를 통칭하여 '스코어 매칭'이라고 부른다. (이후 디퓨전 모델에서는 노이즈를 주입한 분포에 대해 이 연산을 수행하는 '디노이징 스코어 매칭'으로 발전하였다.)
</div>





<div class="obsidian-callout" markdown="1">
벡터장(vector field)이란 무엇인가?

간단하게 정의하면, 공간 상의 모든 점(좌표)마다 벡터(방향과 크기를 가진 화살표)가 하나씩 할당되어있는 공간을 의미한다.

직관적인 예시로 일기 예보의 바람지도를 생각해보자. 지도 위의 모든 지역마다 그 순간 바람이 부는 방향과 바람의 세기가 존재한다. 이 지도 전체가 하나의 2차원 벡터장이다.

중력장과 자기장 또한 벡터장의 한 예시이다.

어떤  $n$ 차원 공간의 좌표를  $\mathbf{x} = (x\_1, x\_2, \dots, x\_n)$ 라고 할 때, 벡터장  $F$ 는 각 좌표  $\mathbf{x}$ 를 입력받아 같은  $n$ 차원의 벡터를 출력하는 함수이다.

<div class="math-container">
$$
F(\mathbf{x}) = (v_1(\mathbf{x}), v_2(\mathbf{x}), \dots, v_n(\mathbf{x}))
$$  
</div>


일반적인 스칼라 함수가 좌표마다 온도나 고도같은 숫자를 출력한다면, 벡터장 함수는 좌표마다 벡터(방향과 크기를 가진 양)를 출력으로 가진다.

스코어와 그래디언트또한 벡터장이다.
</div>





<div class="obsidian-callout" markdown="1">
벡터장 사이의 유클리디안 거리를 구한다는 것은 무슨 의미인가?
</div>





<div class="obsidian-callout" markdown="1">
립시츠 연속성이 무엇인가?
</div>





<div class="obsidian-callout" markdown="1">
신경망이 예측한 노이즈에 상수를 곱하는게 어떻게 해당 시점의 스코어와 동치인가?
</div>





<div class="obsidian-callout" markdown="1">
그래디언트의 역행렬이 존재하는가? 존재한다면 무슨 의미인가?
</div>
