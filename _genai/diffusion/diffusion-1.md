---
layout: sidebar
title: Diffusion 1
collection_name: genai
nav_order: 9999
date: 2026-04-29
---

## Abstract
비평형 열역학(nonequilibrium thermodynamics)의 원리에서 영감을 받은 잠재 변수 모델의 일종인 확산 확률 모델(diffusion probabilistic models)을 사용하여 고품질의 이미지 합성 결과를 제시한다. 본 논문에서 달성한 최고의 성과는 확산 확률 모델과 '랑주뱅 동역학(Langevin dynamics)을 활용한 디노이징 스코어 매칭(denoising score matching)' 사이의 새로운 연결 고리를 바탕으로 설계된, 가중치가 부여된 변분 하한(weighted variational bound)을 학습함으로써 얻어졌다. 또한, 본 모델은 자기회귀 디코딩(autoregressive decoding)의 일반화된 형태로 해석될 수 있는 점진적인 손실 압축 해제(progressive lossy decompression) 방식을 자연스럽게 지원한다. 조건 없는(unconditional) CIFAR10 데이터셋에서 9.46의 인셉션 스코어(Inception score)와 당시 최고 성능(SOTA)인 3.17의 FID 스코어를 달성했다. 256x256 해상도의 LSUN 데이터셋에서는 ProgressiveGAN과 유사한 샘플 품질을 얻었다. 구현 코드는 [https://github.com/hojonathanho/diffusion](https://github.com/hojonathanho/diffusion) 에서 확인할 수 있다.  



<div class="obsidian-callout" markdown="1">
[비평형 열역학]({{ site.baseurl }}/genai/diffusion/non-equilibrium-thermodynamics/)

물에 잉크 방울을 떨어뜨리면 시간이 지남에 따라 잉크가 퍼져나가며 결국 완전히 흐려지는 현상(엔트로피 증가)을 의미한다. 디퓨전 모델은 이 자연계의 확산 과정을 수학적인 마르코프 연쇄로 모델링하여 원본데이터에 점진적으로 노이즈를 주입하는 forward process의 근간으로 삼았다.
</div>





<div class="obsidian-callout" markdown="1">
새로운 연결 고리와 가중치가 부여된 변분 하한

통계학에서 VAE를 풀기 위해 사용한 증거 하한(ELBO, 변분 하한) 수식을 전개해보니, 그 결과가 물리학의 랑주뱅 동역학 기반의 스코어 매칭 목적함수와 동일한 형태가 됨을 밝혀낸 것이다. 여기서 '가중치가 부여된'이란 말은, 수학적으로 엄밀하게 유도된 원래의 ELBO 수식에서 특정 시간대(t)의 가중치 항들을 의도적으로 날리고 단순화한 목적 함수를 사용했더니 오히려 이미지 생성 품질이 훨씬 좋아졌다는 발견을 의미한다.
</div>





<div class="obsidian-callout" markdown="1">
점진적 손실 압축 해제

순수한 노이즈( $\mathbf{x}\_T$ )에서 시작해 원본 이미지( $\mathbf{x}\_0$ )를 만들어가는 1000번의 디노이징 과정을 정보 이론의 관점에서 해석한다. 픽셀을 한 번에 하나씩 칠하는 기존의 자기회귀 모델과 달리, 디퓨전은 거대한 윤곽(저주파)부터 시작해 미세한 디테일(고주파)까지 점진적으로 복원해나간다. 이는 아주 강하게 압축된 파일(노이즈)의 압축을 서서히 풀면서 이미지를 선명하게 만들어가는 과정과 동치이다.
</div>



## 1. Introduction
최근 모든 종류의 심층 생성 모델들이 다양한 데이터 모달리티에서 고품질의 샘플을 보여주고 있다. 생성적 적대 신경망(GAN), 자기회귀 모델(autoregressive models), 정상화 흐름(flows), 그리고 변분 자동인코더(VAE)는 놀라운 이미지 및 오디오 샘플을 합성해냈으며 [14, 27, 3, 58, 38, 25, 10, 32, 44, 57, 26, 33, 45], 에너지 기반 모델링(energy-based modeling)과 스코어 매칭(score matching) 분야에서도 GAN에 필적하는 이미지를 생성하는 괄목할 만한 발전이 있었다 [11, 55].  

본 논문은 확산 확률 모델(diffusion probabilistic models) [53]의 발전을 제시한다. 확산 확률 모델(간결하게 "확산 모델"이라 부르겠다)은 유한한 시간 후에 데이터와 일치하는 샘플을 생성하기 위해 변분 추론(variational inference)을 사용하여 학습된 매개변수화된 마르코프 연쇄(parameterized Markov chain)이다. 이 연쇄의 전이(transitions)는 샘플링의 역방향으로 신호가 완전히 파괴될 때까지 점진적으로 데이터에 노이즈를 추가하는 마르코프 연쇄인 '확산 과정(diffusion process)'을 되돌리도록 학습된다. 확산이 적은 양의 가우시안 노이즈로 구성될 때, 샘플링 연쇄의 전이 또한 조건부 가우시안(conditional Gaussians)으로 설정하는 것으로 충분하며, 이는 특히 단순한 신경망 매개변수화를 가능하게 한다.  



<div class="obsidian-callout" markdown="1">
변분 추론을 사용하여 학습된 마르코프 연쇄

디퓨전 모델이 1000개의 계층을 가지는 마르코프 연쇄이며, 이를 학습할 때 VAE에서 사용했던 증거 하한 수식을 가져와 최적화한다는 것을 밝힌다.
</div>





<div class="obsidian-callout" markdown="1">
신호가 파괴될 때까지 노이즈를 추가하는 확산 과정

forward process의 정의이다. 데이터에 노이즈를 계속 더해서 원점( $\mathcal{N}(\mathbf{0}, \mathbf{I})$ )으로 유도하여 원본 이미지의 정보(신호)를 완전히 없애버리는 과정을 의미한다.
</div>





<div class="obsidian-callout" markdown="1">
연쇄의 전이는 확산 과정을 되돌리도록 학습된다

reverse process(디노이징)의 정의이다. 망가진 노이즈 상태에서 출발하여 원래의 데이터 분포로 되돌아오는 복원력(스코어)을 학습한다는 뜻이다.
</div>





<div class="obsidian-callout" markdown="1">
단순한 신경망 매개변수화

매 스텝마다 추가되는 노이즈의 양이 매우 작다면, 그 방향(디노이징)으로 돌아가는 확률 분포 역시 수학적으로 가우시안 분포가 됨이 통계적으로 증명되어 있다. 역방향이 가우시안 분포라는 것은, 딥러닝 모델(u-net)이 복잡한 확률 분포 전체를 어렵게 예측할필요 없이 오직 가우시안의 평균(결국 노이즈 혹은 스코어) 단 하나만 예측하도록 단순하게 설계할 수 있다는 이점을 설명한다.
</div>



확산 모델은 정의하기 직관적이고 학습하기 효율적이지만, 우리가 아는 한 고품질 샘플을 생성할 수 있다는 입증은 아직 없었다. 우리는 확산 모델이 실제로 고품질 샘플을 생성할 수 있으며, 때로는 다른 유형의 생성 모델에서 발표된 결과보다 더 나은 결과를 보여준다는 것을 입증한다(섹션 4). 또한, 확산 모델의 특정 매개변수화(parameterization)가 학습 중에는 여러 노이즈 수준에 걸친 디노이징 스코어 매칭(denoising score matching)과, 샘플링 중에는 어닐링된 랑주뱅 동역학(annealed Langevin dynamics)과 동치(equivalence)를 이룬다는 것을 보여준다(섹션 3.2) [55, 61]. 우리는 이 매개변수화를 사용하여 최고의 샘플 품질 결과를 얻었으므로(섹션 4.2), 이 동치성을 밝혀낸 것을 본 논문의 주요 기여 중 하나로 간주한다.  



<div class="obsidian-callout" markdown="1">
디퓨전 모델 자체는 2015년에 처음 제안되었지만, 당시에는 생성된 이미지의 품질이 좋지 않았다. 본 논문의 저자들은 u-net이 원본 이미지( $\mathbf{x}\_0$ )나 평균( $\tilde{\mu}\_t$ )을 예측하게 하지 않고, 추가된 가우시안 노이즈( $\epsilon$ )를 예측하도록 수식을 우회하였다(특정 매개변수화). 이 우회하는 과정이 랑주뱅 동역학의 복원력(스코어 함수)을 계산하는 식과 동치인 것이다.
</div>



샘플 품질이 뛰어남에도 불구하고, 우리 모델은 다른 우도 기반(likelihood-based) 모델과 비교했을 때 경쟁력 있는 로그 우도(log likelihoods)를 가지지는 못한다 (그러나 우리 모델은 에너지 기반 모델과 스코어 매칭을 위해 어닐링된 중요도 샘플링이 생성한다고 보고된 큰 추정치보다는 더 나은 로그 우도를 가진다 [11, 55]). 우리는 우리 모델의 무손실 코드 길이(lossless codelengths)의 대부분이 눈에 보이지 않는(imperceptible) 이미지 세부 사항을 묘사하는 데 소비된다는 것을 발견했다(섹션 4.3). 우리는 손실 압축(lossy compression)의 언어로 이 현상에 대한 더 정제된 분석을 제시하며, 확산 모델의 샘플링 절차가 일반적인 자기회귀(autoregressive) 모델에서 가능한 것을 크게 일반화한 비트 순서(bit ordering)에 따른 점진적 디코딩(progressive decoding)의 일종임을 보여준다.  



<div class="obsidian-callout" markdown="1">
우도(likelihood)란 이 모델이 실제 데이터의 분포를 얼마나 근사하는가를 측정하는 지표이다. VAE나 flow모델들은 이 점수가 아주 높지만, DDPM은 화질이 좋음에도 불구하고 이 점수는 상대적으로 낮았다.
</div>





<div class="obsidian-callout" markdown="1">
무손실 코드 길이와 눈에 보이지 않는 디테일(이미지 세부 사항)

우도가 낮게 나온 이유를 분석해 보니, 모델의 정보 처리 용량의 절반 이상을 사람 눈에는 보이지도 않는 픽셀 단위의 미세한 노이즈(고주파 패턴)를 복원하는 데 낭비하고 있었기 때문이다. 때문에, 모델은 사람이 이미지를 인식하는 방식처럼 거대한 측징(저주파)부터 먼저 복원하고, 작은 디테일은 나중에 채워 넣는 디코더(압축 해제기)의 역할(점진적 손실 압축 해제)을 유도한다.
</div>



## 2. Background
확산 모델(Diffusion models) [53]은  $p\_\theta(\mathbf{x}\_0) := \int p\_\theta(\mathbf{x}\_{0:T}) d\mathbf{x}\_{1:T}$  형태의 잠재 변수 모델(latent variable models)이며, 여기서  $\mathbf{x}\_1, \dots, \mathbf{x}\_T$ 는 데이터  $\mathbf{x}\_0 \sim q(\mathbf{x}\_0)$ 와 동일한 차원을 가지는 잠재 변수들이다. 결합 분포(joint distribution)  $p\_\theta(\mathbf{x}\_{0:T})$ 는 역과정(reverse process)이라 불리며,  $p(\mathbf{x}\_T) = \mathcal{N}(\mathbf{x}\_T; \mathbf{0}, \mathbf{I})$ 에서 시작하여 학습된 가우시안 전이(learned Gaussian transitions)를 가지는 마르코프 연쇄(Markov chain)로 정의된다:  



<div class="obsidian-callout" markdown="1">
실제 데이터가 샘플링된 분포  $q(\mathbf{x}\_0)$ 를 알 수 없으므로, 신경망( $p\_\theta(\mathbf{x}\_0) := \int p\_\theta(\mathbf{x}\_{0:T}) d\mathbf{x}\_{1:T}$ )을 통해 근사한다. 하지만 이 분포는 복잡하여 한 번에 정의할 수 없으므로,  $\mathbf{x}\_1$ 부터  $\mathbf{x}\_T$ 까지 총  $T$ 개(논문에서는 1000개)의 잠재 변수를 도입한다. 전체 결합 확률  $p\_\theta(\mathbf{x}\_{0:T})$ 에서 잠재 변수들( $\mathbf{x}\_1 \sim \mathbf{x}\_T$ )을 적분해서 없애면(경우의 수를 다 더하면), 진짜 데이터의 확률 $p\_\theta(\mathbf{x}\_0)$ 만 남는다는 뜻이다.
</div>





<div class="obsidian-callout" markdown="1">
VAE 같은 기존 모델들은 원본 이미지( $\mathbf{x}\_0$ )를 저차원의 잠재 벡터로 압축한다. 반면 디퓨전 모델은  $256 \times 256$  해상도라면,  $\mathbf{x}\_1$  도  $256 \times 256$  이고, 1000번째 노이즈  $\mathbf{x}\_{1000}$  도 완벽하게 똑같은  $256 \times 256$ 해상도이다. 차원이 똑같다는 것은, 1000개의 단계가 모드 이미지 공간 그 자체라는 뜻이며, 이를 통해 이미지의 세밀한 구조가 파괴되지 않고 유지된다.
</div>

<div class="math-container">
$$  
p_\theta(\mathbf{x}_{0:T}) := p(\mathbf{x}_T) \prod_{t=1}^T p_\theta(\mathbf{x}_{t-1} \mid \mathbf{x}_t), \quad p_\theta(\mathbf{x}_{t-1} \mid \mathbf{x}_t) := \mathcal{N}(\mathbf{x}_{t-1}; \boldsymbol{\mu}_\theta(\mathbf{x}_t, t), \boldsymbol{\Sigma}_\theta(\mathbf{x}_t, t)) \quad (1)  
$$
</div>


<div class="obsidian-callout" markdown="1">
(디노이징, reverse) 출발점 ( $p(\mathbf{x}\_T)$ ): 먼저  $p(\mathbf{x}\_T) = \mathcal{N}(\mathbf{x}\_T; \mathbf{0}, \mathbf{I})$ 라는 사전 분포(완전 노이즈)에서 출발한다.

마르코프 연쇄 ( $\prod$ ): 결합 분포  $p\_\theta(\mathbf{x}\_{0:T})$ 를 단순히 이전 단계에서 다음 단계를 조건부로 예측하는 확률( $p\_\theta(\mathbf{x}\_{t-1} \mid \mathbf{x}\_t)$ )들의 계속된 곱셈( $\prod$ )으로 표현했다. 즉,  $\mathbf{x}\_{999}$ 를 만들 때는 오직  $\mathbf{x}\_{1000}$ 만 보고 만들지,  $\mathbf{x}\_{1000}$ 이전의 상태는 고려하지 않겠다는 의미이다.

가우시안 전이 ( $\mathcal{N}$ ): 또한 각 스텝에서 노이즈( $\mathbf{x}\_t$ )를 살짝 덜어낸 이전 상태( $\mathbf{x}\_{t-1}$ )를 예측할 때, 그 확률 분포가 정규분포를 따른다고 강제한다.

신경망의 역할 ( $\boldsymbol{\mu}\_\theta, \boldsymbol{\Sigma}\_\theta$ ):  $\theta$ 는 딥러닝 모델(U-Net)의 가중치를 의미한다. 즉, u-net이 해야할 일은 이미지를 직접 그리는 것이 아니라, 특정 시간  $t$ 에 노이즈 낀 이미지  $\mathbf{x}\_t$ 가 들어왔을 때, 바로 직전 단계( $\mathbf{x}\_{t-1}$ )의 가우시안 분포는 평균이  $\boldsymbol{\mu}$  이고 분산이  $\boldsymbol{\Sigma}$ 일 것을 예측하는 회귀 문제를 푸는 것이다.
</div>





<div class="obsidian-callout" markdown="1">
그 이전 상태를 고려하지 않는다는 것은 무슨의미인가? 이전의 정보들을 가지고 있으면 좋지않나?

디퓨전 모델은 입자가 무작위로 퍼져나가는 물리학의 [브라운 운동]({{ site.baseurl }}/genai/diffusion/brownian-motion/)에서 영감을 받았다.

당구대 위에서 굴러가는 당구공을 상상해보자. 당구공이 1초 뒤에 어디로 갈지 예측하기 위해, 우리는 공의 현재 위치와 현재 속도만 알면 된다. 이 공이 5초 전에 쿠션을 몇 번 맞고 굴러왔는지(과거의 정보)는 전혀 알 필요가 없다. 왜냐하면 그 과거의 모든 사건들이 누적된 결과물이 바로 현재 상태이기 때문이다.

디퓨전또한  $t$  시점의 이미지  $\mathbf{x}\_t$  안에는 이미 1000번째부터  $t+1$ 번째까지 가해진 모든 디노이징의 결과가 담겨있다. 즉,  $\mathbf{x}\_t$ 자체가 과거 정보의 요약이므로, 굳이 방대한 과거 사진 스냅샷을 들고 다닐 필요가 없는 것이다.  $\mathbf{x}\_t$  하나가 256x256 해상도의 컬러 이미지(약 20만 개의 숫자)일때, 만약 990번째 이미지를 예측하기 위해 1000번째부터 991번째까지 총 10장의 이미지를 모두 u-net에 입력으로 넣는다면, 연산량이 어마어마할 것이다.

또한 수식 (1)에서 마르코프 연쇄 덕분에 거대한 결합 확률을 단순한 조건부 확률들의 곱셈으로 쪼갤 수 있었다.

<div class="math-container">
$$
p_\theta(\mathbf{x}_{0:T}) = p(\mathbf{x}_T) \times p_\theta(\mathbf{x}_{T-1} \mid \mathbf{x}_T) \times p_\theta(\mathbf{x}_{T-2} \mid \mathbf{x}_{T-1}) \dots
$$  
</div>


만약 마르코프 가정을 빼고 과거를 모두 기억하게 한다면 위 결합 확률은 다음과 같다.

<div class="math-container">
$$
p_\theta(\mathbf{x}_{0:T}) = p(\mathbf{x}_T) \times p_\theta(\mathbf{x}_{T-1} \mid \mathbf{x}_T) \times p_\theta(\mathbf{x}_{T-2} \mid \mathbf{x}_{T-1}, \mathbf{x}_T) \dots
$$  
</div>


뒤로 갈 수록 조건부가 무한히 길어질 것이며, ELBO를 분수 형태로 약분할 수도 없고, KL발산 형태로 수식을 정리하는 것 자체가 불가능하다.

마르코프 가정을 깨고(비(Non)-마르코프 과정), 과거의 정보(원본 이미지로 향하는 방향 등)를 우회해서 활용한 것이 DDIM(Denoising Diffusion Implicit Models)이다. 현재 Stable Diffusion 등의 상용화 모델들은 모두 이 DDIM의 비마르코프 방식을 채택하였다.
</div>



확산 모델을 다른 유형의 잠재 변수 모델들과 구별 짓는 특징은, 순방향 과정(forward process) 또는 확산 과정(diffusion process)이라 불리는 근사 사후 분포(approximate posterior)  $q(\mathbf{x}\_{1:T} \mid \mathbf{x}\_0)$ 가, 분산 스케줄(variance schedule)  $\beta\_1, \dots, \beta\_T$ 에 따라 점진적으로 데이터에 가우시안 노이즈를 추가하는 고정된 마르코프 연쇄로 설정되어 있다는 점이다:  

<div class="math-container">
$$  
q(\mathbf{x}_{1:T} \mid \mathbf{x}_0) := \prod_{t=1}^T q(\mathbf{x}_t \mid \mathbf{x}_{t-1}), \quad q(\mathbf{x}_t \mid \mathbf{x}_{t-1}) := \mathcal{N}(\mathbf{x}_t; \sqrt{1 - \beta_t} \mathbf{x}_{t-1}, \beta_t \mathbf{I}) \quad (2)  
$$
</div>


<div class="obsidian-callout" markdown="1">
(노이징, forward) VAE는 원본 이미지를 잠재 벡터로 압축하는 인코더를 학습시켜야 하는 반면, 디퓨전은 forward에서 학습이라는 과정이 없다. 전체 노이즈 주입 과정은, 바로 직전 단계  $\mathbf{x}\_{t-1}$ 에 노이즈를 한 번 더 추가해서 현재 단계  $\mathbf{x}\_t$ 를 만드는 단순한 작업의  $T$ 번(=1,000) 반복( $\prod$ )일 뿐이다.
</div>





<div class="obsidian-callout" markdown="1">
논문에서는 총 스텝 수를  $T=1000$ 으로 고정하고, 분산 스케줄은  $\beta\_1 = 0.0001$  (매우 작은 노이즈)에서 시작하여,  $\beta\_{1000} = 0.02$  (상대적으로 큰 노이즈)까지 선형적으로 서서히 증가하도록 스케줄을 짜두었다.
</div>





<div class="obsidian-callout" markdown="1">
수식 (2)의 의미는  $t=1$ 일 때( $q(\mathbf{x}\_1 \mid \mathbf{x}\_0) = \mathcal{N}(\mathbf{x}\_1; \sqrt{1 - \beta\_1} \mathbf{x}\_0, \beta\_1 \mathbf{I})$ ),  $\mathbf{x}\_1$ 가 평균이  $\sqrt{1 - \beta\_1} \mathbf{x}\_0$ 이고 (공)분산이  $\beta\_1 \mathbf{I}$ 인 가우시안을 따른다는 뜻이다. 실제로  $\mathbf{x}\_1$ 를 샘플링하려면 재매개변수화 트릭을 통해 다음 연산을 수행해야 한다.

<div class="math-container">
$$
\mathbf{x}_1 = \underbrace{\sqrt{1 - \beta_1} \mathbf{x}_0}_{\text{평균 (스케일 다운)}} + \underbrace{\sqrt{\beta_1} \boldsymbol{\epsilon}}_{\text{실제 더해지는 노이즈}}
$$  
</div>


(공분산이 대각행렬이라는 것은 변수들 간에 상관관계가 없다는 뜻이다. 즉, 이미지 왼쪽 위에 추가된 노이즈와, 오른쪽 아래에 추가된 노이즈는 서로 아무런 연관성없이 독립적으로 무작위하게 뽑힌다.)

원본 이미지와 같은 고차원 벡터인 노이즈 벡터  $\boldsymbol{\epsilon}$ 의 각 원소는  $\mathcal{N}(0, 1)$ 에서 무작위로 뽑힌 난수이다.
</div>





<div class="obsidian-callout" markdown="1">
 $\mathbf{x}\_{t-1}$ 가 가우시안 분포를 따르는 이유는 가우시안 확률 변수( $\boldsymbol{\epsilon} \sim \mathcal{N}(\mathbf{0}, \mathbf{I})$ )에 선형 결합(또는 아핀 변환)의 결과는 가우시안이기 때문이다. 위 재매개변수화 트릭(아핀 변환,  $Y = aX + c$ )으로 만들어진  $\mathbf{x}\_{t-1}$ 의 평균과 기댓값은 다음과 같이 구할 수 있다.

<div class="math-container">
$$
E[\mathbf{x}_{t}] = \sqrt{\beta_t} E[\boldsymbol{\epsilon}] + \sqrt{1 - \beta_t} \mathbf{x}_{t-1}
$$  
</div>


 $\boldsymbol{\epsilon}$ 의 평균이  $\mathbf{0}$ 이므로,  $E[\mathbf{x}\_t \mid \mathbf{x}\_{t-1}] = \sqrt{1 - \beta\_t} \mathbf{x}\_{t-1}$ 이다.

<div class="math-container">
$$
Var(\mathbf{x}_t) = (\sqrt{\beta_t})^2 Var(\boldsymbol{\epsilon}) + 0
$$  
</div>


 $Var(\boldsymbol{\epsilon}) = \mathbf{I}$ 이므로  $Var(\mathbf{x}\_t \mid \mathbf{x}\_{t-1}) = \beta\_t \mathbf{I}$ 이다.
</div>





<div class="obsidian-callout" markdown="1">
Q. 분산 스케줄을 파라미터로 두면 어떨까?

"The forward process variances  $\beta\_t$  can be learned by reparameterization..."에서 "순방향 과정의 분산  $\beta\_t$ 는 재매개변수화를 통해 학습되거나 하이퍼파라미터로 상수로 유지될 수 있다."고 말한다. 그러나 사전에 선형 스케줄로 고정했음에도 불구하고 당시 좋은 품질을 가질 수 있었다.

추후 연구에서 코사인 스케줄을 제안해 샘플링 속도를 1,000번에서 50번으로 줄이거나(Improved DDPM, 2021), 노이즈 스케줄  $\beta\_t$  자체를 신경망으로 두고 학습하게 만들었다(Variational Diffusion models, 2021). 그 결과 DDPM의 낮았던 로그 우도 점수를 최고 수준으로 끌어올리게 된다.
</div>



학습은 음의 로그 우도(negative log likelihood)에 대한 일반적인 변분 하한(variational bound)을 최적화함으로써 수행된다:  

<div class="math-container">
$$  
\mathbb{E}[-\log p_\theta(\mathbf{x}_0)] \le \mathbb{E}_q \left[ -\log \frac{p_\theta(\mathbf{x}_{0:T})}{q(\mathbf{x}_{1:T} \mid \mathbf{x}_0)} \right] = \mathbb{E}_q \left[ -\log p(\mathbf{x}_T) - \sum_{t \ge 1} \log \frac{p_\theta(\mathbf{x}_{t-1} \mid \mathbf{x}_t)}{q(\mathbf{x}_t \mid \mathbf{x}_{t-1})} \right] =: L \quad (3)  
$$
</div>


<div class="obsidian-callout" markdown="1">
목표는  $p\_\theta(\mathbf{x}\_0)$ 을 최대화하는  $\theta$ 를 찾는 것이다. 여기에 로그를 씌워도 최적해는 바뀌지 않으며(로그가 단조증가함수이므로), 음수를 취해 minimize문제로 바꾼다.

그러나 이 값을 구하려면 1000개의 모든 노이즈 단계( $\mathbf{x}\_1 \dots \mathbf{x}\_T$ )의 경우의 수를 전부 적분해야 한다(intractable).

<div class="math-container">
$$
\min_{\theta} \quad -\log p_\theta(\mathbf{x}_0)
$$  
</div>


우리가 알고 싶은 것은  $p\_\theta(\mathbf{x}\_0)$ 이지만, 디퓨전 모델은  $\mathbf{x}\_1$ 부터  $\mathbf{x}\_T$ 까지의 잠재 변수를 포함하는 결합 확률 분포  $p\_\theta(\mathbf{x}\_{0:T})$ 로 정의되어 있다. marginalization에 의해  $p\_\theta(\mathbf{x}\_0)$ 는 다음과 같다.

<div class="math-container">
$$
p_\theta(\mathbf{x}_0) = \int p_\theta(\mathbf{x}_{0:T}) d\mathbf{x}_{1:T}
$$  
</div>


이때 순방향 과정을 나타내는 확률 밀도 함수  $q(\mathbf{x}\_{1:T} \mid \mathbf{x}\_0)$ 를 분모와 분자에 곱하는 트릭을 쓴다(1을 곱하는 것과 같으므로 등식은 유지됨.).

<div class="math-container">
$$
p_\theta(\mathbf{x}_0) = \int q(\mathbf{x}_{1:T} \mid \mathbf{x}_0) \frac{p_\theta(\mathbf{x}_{0:T})}{q(\mathbf{x}_{1:T} \mid \mathbf{x}_0)} d\mathbf{x}_{1:T}
$$  
</div>


어떤 함수  $f(X)$ 에 대해 확률 밀도 함수  $q(X)$ 를 곱하여 적분한 것은,  $q$  분포에 대한  $f(X)$ 의 기댓값의 정의와 같다.

<div class="math-container">
$$
p_\theta(\mathbf{x}_0) = \mathbb{E}_{q(\mathbf{x}_{1:T} \mid \mathbf{x}_0)} \left[ \frac{p_\theta(\mathbf{x}_{0:T})}{q(\mathbf{x}_{1:T} \mid \mathbf{x}_0)} \right]
$$  
</div>


양변에  $-\log$ 를 취하면

<div class="math-container">
$$
-\log p_\theta(\mathbf{x}_0) = -\log \left( \mathbb{E}_{q(\mathbf{x}_{1:T} \mid \mathbf{x}_0)} \left[ \frac{p_\theta(\mathbf{x}_{0:T})}{q(\mathbf{x}_{1:T} \mid \mathbf{x}_0)} \right] \right)
$$  
</div>


 $-\log(x)$ 는 볼록함수이므로 [젠슨 부등식]({{ site.baseurl }}/genai/diffusion/jensens-inequality/)( $f(\mathbb{E}[x]) \le \mathbb{E}[f(x)]$ )에 의해 다음이 성립한다.

<div class="math-container">
$$
-\log p_\theta(\mathbf{x}_0) = -\log \left( \mathbb{E}_{q(\mathbf{x}_{1:T} \mid \mathbf{x}_0)} \left[ \frac{p_\theta(\mathbf{x}_{0:T})}{q(\mathbf{x}_{1:T} \mid \mathbf{x}_0)} \right] \right) \le \mathbb{E}_{q(\mathbf{x}_{1:T} \mid \mathbf{x}_0)} \left[ -\log \frac{p_\theta(\mathbf{x}_{0:T})}{q(\mathbf{x}_{1:T} \mid \mathbf{x}_0)} \right]
$$  
</div>


좌변의 상한(upper bound)가 우변이므로, 우변을 최소화하는  $\theta$ 를 찾는 것은 좌변의 상한을 최소화하는  $\theta$ 를 찾는것과 동일하다.
</div>





<div class="obsidian-callout" markdown="1">
위 upper bound의 분수에서 분자(역과정)는  $p\_\theta(\mathbf{x}\_{0:T}) = p(\mathbf{x}\_T) \prod\_{t=1}^T p\_\theta(\mathbf{x}\_{t-1} \mid \mathbf{x}\_t)$ , 분모(순방향)  $q(\mathbf{x}\_{1:T} \mid \mathbf{x}\_0) = \prod\_{t=1}^T q(\mathbf{x}\_t \mid \mathbf{x}\_{t-1})$ 이므로

<div class="math-container">
$$
-\log \left(\frac{p_\theta(\mathbf{x}_{0:T})}{q(\mathbf{x}_{1:T} \mid \mathbf{x}_0)} \right) = -\log \left( \frac{p(\mathbf{x}_T) \prod_{t=1}^T p_\theta(\mathbf{x}_{t-1} \mid \mathbf{x}_t)}{\prod_{t=1}^T q(\mathbf{x}_t \mid \mathbf{x}_{t-1})} \right)
$$  
</div>


로그의 기본 성질  $\log(AB) = \log A + \log B$  와  $\log(A/B) = \log A - \log B$ , 그리고  $\log(\prod A\_i) = \sum \log A\_i$ 를 이용하여 식을 전개한다.

<div class="math-container">
$$
= -\log p(\mathbf{x}_T) - \log \left( \frac{\prod_{t=1}^T p_\theta(\mathbf{x}_{t-1} \mid \mathbf{x}_t)}{\prod_{t=1}^T q(\mathbf{x}_t \mid \mathbf{x}_{t-1})} \right)
$$  
</div>



<div class="math-container">
$$
= -\log p(\mathbf{x}_T) - \log \left( \prod_{t=1}^T \frac{p_\theta(\mathbf{x}_{t-1} \mid \mathbf{x}_t)}{q(\mathbf{x}_t \mid \mathbf{x}_{t-1})} \right)
$$  
</div>



<div class="math-container">
$$
= -\log p(\mathbf{x}_T) - \sum_{t=1}^T \log \frac{p_\theta(\mathbf{x}_{t-1} \mid \mathbf{x}_t)}{q(\mathbf{x}_t \mid \mathbf{x}_{t-1})}
$$  
</div>


이 전체 항을 기댓값  $\mathbb{E}\_q$ 로 묶으면 식 (3)이 도출된다.
</div>



순방향 과정의 분산  $\beta\_t$ 는 재매개변수화(reparameterization) [33]를 통해 학습되거나 하이퍼파라미터로 상수로 유지될 수 있다. 역과정의 표현력(expressiveness)은 부분적으로  $p\_\theta(\mathbf{x}\_{t-1} \mid \mathbf{x}\_t)$ 에서 가우시안 조건부를 선택함으로써 보장되는데, 왜냐하면  $\beta\_t$ 가 작을 때 두 과정은 동일한 함수 형태를 가지기 때문이다 [53]. 순방향 과정의 주목할 만한 특성은 임의의 시간 단계  $t$ 에서  $\mathbf{x}\_t$ 를 닫힌 형태(closed form)로 샘플링할 수 있다는 것이다:  $\alpha\_t := 1 - \beta\_t$  이고  $\bar{\alpha}\_t := \prod\_{s=1}^t \alpha\_s$  라는 표기법을 사용하면, 다음과 같다:  

<div class="math-container">
$$  
q(\mathbf{x}_t \mid \mathbf{x}_0) = \mathcal{N}(\mathbf{x}_t; \sqrt{\bar{\alpha}_t} \mathbf{x}_0, (1 - \bar{\alpha}_t)\mathbf{I}) \quad (4)  
$$
</div>


<div class="obsidian-callout" markdown="1">
스케줄은 학습될 수 있으며, 노이즈 주입량( $\beta\_t$ , 순방향)이 충분히 작다면, 역방향 과정의 확률 분포 또한 가우시안이다.
</div>





<div class="obsidian-callout" markdown="1">
Q. 노이즈 주입량이 작은 것이 역방향또한 가우시안임을 어떻게 보장하는가?

역과정 확률인  $q(\mathbf{x}\_{t-1} \mid \mathbf{x}\_t)$ 를 베이즈 정리로 전개하면 다음과 같다.

<div class="math-container">
$$
q(\mathbf{x}_{t-1} \mid \mathbf{x}_t) = q(\mathbf{x}_t \mid \mathbf{x}_{t-1}) \frac{q(\mathbf{x}_{t-1})}{q(\mathbf{x}_t)}
$$  
</div>


 $q(\mathbf{x}\_t \mid \mathbf{x}\_{t-1})$ 는 순방향 과정에서  $\mathcal{N}(\sqrt{1-\beta\_t}\mathbf{x}\_{t-1}, \beta\_t \mathbf{I})$ 로 정의한 가우시안 분포이며,  $q(\mathbf{x}\_{t-1})$  및  $q(\mathbf{x}\_t)$ 는 전체 데이터 매니폴드가 시간  $t$ 에 따라 어떻게 분포하는지를 나타내는 확률 밀도 함수이다. 데이터셋 전체의 확률 분포이므로, 매우 복잡한 다봉형의 비선형 함수이다.

만약  $\beta\_t$ 가 크다면(큰 스텝)  $\mathbf{x}\_t$ 와  $\mathbf{x}\_{t-1}$  사이의 거리가 멀어진다. 가우시안 함수인  $q(\mathbf{x}\_t \mid \mathbf{x}\_{t-1})$ 에 매우 복잡하고 굴곡진  $q(\mathbf{x}\_{t-1})$ 를 곱하게 되므로, 그 결과물인 좌변의  $q(\mathbf{x}\_{t-1} \mid \mathbf{x}\_t)$ 는 찌그러진 비대칭 형태가 되어 가우시안 분포를 유지할 수 없다.

만약  $\beta\_t$ 가 극도로 작다면( $\beta\_t \to 0$ )  $\mathbf{x}\_{t-1}$ 은  $\mathbf{x}\_t$ 의 극소 주변에만 존재하게 된다. 미적분학의 원리에 따라, 복잡한 비선형함수(데이터의 원래 확률 분포  $q$ )라도 극소 영역만 보면 선형 함수로 근사할 수 있다(1차 테일러 전개).

극소 영역에서  $\log q(\mathbf{x}\_{t-1})$ 는 선형 근사에 의해 상수 벡터  $\mathbf{c}$ 에 대하여  $\mathbf{c}^T \mathbf{x}\_{t-1}$ 로 표현될 수 있으며, 따라서  $q(\mathbf{x}\_{t-1}) \approx e^{\mathbf{c}^T \mathbf{x}\_{t-1}}$  형태의 단순한 지수 함수로 근사된다.

가우시안 함수에 지수함수( $e^{\mathbf{c}^T \mathbf{x}}$ )를 곱하면, 그 결과는 분산은 그대로고 평균만 c방향으로 평행이동한 가우시안 함수가 된다. 따라서  $\beta\_t$ 가 충분히 작을 경우

<div class="math-container">
$$
[\text{가우시안 분포}] \times [\text{극소 영역의 지수 함수 근사}] = [\text{평균이 이동된 가우시안 분포}]
$$  
</div>


라는 연산이 성립한다.

(아직 이해가 부족. 테일러 전개, 선형 근사)
</div>





<div class="obsidian-callout" markdown="1">
 $\beta\_t$ 를  $\alpha\_t := 1 - \beta\_t$ 로 표기함으로써 매 스텝 추가되는 가우시안 노이즈의 분산(노이즈 주입률,  $\beta\_t$ )을 전체 에너지 1 중에서 노이즈  $\beta\_t$ 를 뺀 나머지, 즉  $\mathbf{x}\_{t-1}$ 의 정보가 다음 스텝  $\mathbf{x}\_t$ 로 얼마나 보존되었는지(비율, 신호 보존율)로 해석할 수 있다. 식 (4)에서  $\sqrt{\bar{\alpha}\_t}$ 는 1단계부터  $t$ 단계까지 거치며 살아남은 원본 신호( $\mathbf{x}\_0$ )의 총 비율이며,  $(1 - \bar{\alpha}\_t)$ 는 1단계부터  $t$ 단계까지의 누적된 노이즈의 총분산이다.
</div>



따라서 확률적 경사 하강법(stochastic gradient descent)으로  $L$ 의 무작위 항(random terms)을 최적화함으로써 효율적인 학습이 가능하다.  $L$  (수식 3)을 다음과 같이 다시 작성하여 분산(variance)을 감소시킴으로써 추가적인 개선을 얻을 수 있다:  

<div class="math-container">
$$  
\mathbb{E}_q \left[ \underbrace{D_{\text{KL}}(q(\mathbf{x}_T \mid \mathbf{x}_0) \parallel p(\mathbf{x}_T))}_{L_T} + \sum_{t>1} \underbrace{D_{\text{KL}}(q(\mathbf{x}_{t-1} \mid \mathbf{x}_t, \mathbf{x}_0) \parallel p_\theta(\mathbf{x}_{t-1} \mid \mathbf{x}_t))}_{L_{t-1}} \underbrace{- \log p_\theta(\mathbf{x}_0 \mid \mathbf{x}_1)}_{L_0} \right] \quad (5)  
$$
</div>


<div class="obsidian-callout" markdown="1">
식 (5)의 유도(부록 A)는 아래에 있다.
</div>





<div class="obsidian-callout" markdown="1">
 $L\_T$ :  $D\_{\text{KL}}(q(\mathbf{x}\_T \mid \mathbf{x}\_0) \parallel p(\mathbf{x}\_T))$ , 순방향 과정이 끝나고 도달한 노이즈( $q$ )가, 사전에 가정한(prior) 가우시안 분포( $p$ )와 얼마나 똑같은지를 측정한다. 이는  $\theta$ 와 무관한 상수이다.

 $L\_{t-1}$ :  $D\_{\text{KL}}(q(\mathbf{x}\_{t-1} \mid \mathbf{x}\_t, \mathbf{x}\_0) \parallel p\_\theta(\mathbf{x}\_{t-1} \mid \mathbf{x}\_t))$ , 정답 분포  $q$ 와 모델이 예측한 가우시안 분포  $p\_{\theta}$ 가 얼마나 비슷한지를 측정하며, 이 항이 모델이 학습할(오차를 줄일) 대상이다. 이때  $q$ 쪽에 원본 이미지  $\mathbf{x}\_0$ 가 들어와 있는데, 이는 아래 식 (6)에서 설명한다.

 $L\_0$ :  $-\log p\_\theta(\mathbf{x}\_0 \mid \mathbf{x}\_1)$ ,  $t=1$ 에서 마지막으로 노이즈를 한 번 더 걷어내어 최종 원본 이미지( $\mathbf{x}\_0$ )를 만들어낼 때의 오차(재구성 오차)이다. KL발산이 아닌 우도형태이며, 생성된 실수값(연속형 픽셀 값)을 0~255 사이의 정수값(이산형)으로 매핑하는 디코더를 거친다.
</div>





<div class="obsidian-callout" markdown="1">
 $L\_0 = -\log p\_\theta(\mathbf{x}\_0 \mid \mathbf{x}\_1)$ 이 재구성 오차인 이유.

디퓨전 모델은 계층이 1,000개인 특수한 계층적 VAE이다. VAE의 목적 함수를 떠올려보자.

<div class="math-container">
$$
L_{\text{VAE}} = \underbrace{D_{\text{KL}}(q(z \mid x) \parallel p(z))}_{\text{정규화 항 (Regularization)}} - \underbrace{\mathbb{E}_q [\log p_\theta(x \mid z)]}_{\text{재구성 오차 항 (Reconstruction)}}
$$  
</div>


VAE에서 잠재 변수  $z$ 를 입력받아 최종 원본 데이터  $x$ 를 복원해 내는 디코더의 확률값이 바로  $p\_\theta(x \mid z)$ 입니다. 이 확률에 음의 로그( $-\log$ )를 취한 것은 정보 이론에서 크로스 엔트로피를 의미하며, 이는 생성된 결과물과 실제 데이터 사이의 오차(loss)를 나타낸다.

디퓨전 수식에서  $\mathbf{x}\_1$ 은 원본 데이터  $\mathbf{x}\_0$ 에 닿기 직전의 마지막 잠재 변수입니다. 즉,  $\mathbf{x}\_1$ 이 VAE의  $z$  역할을 하고,  $\mathbf{x}\_0$ 가  $x$  역할을 한다. 따라서  $-\log p\_\theta(\mathbf{x}\_0 \mid \mathbf{x}\_1)$ 는 마지막 잠재 변수에서 원본 데이터를 재구성할 때 발생하는 오차를 측정하는 항이다.

다른  $L\_{t-1}$ 항들과 구조적으로도 차이가 있는데,  $t > 1$  일 때의  $L\_{t-1}$  항들은 모두 가우시안 분포끼리의 비교이다. 잠재 공간에서 확률 분포 거리를 조정하는 작업이다. 반면  $t=1$ 에서  $t=0$ 으로 넘어가는 순간  $\mathbf{x}\_0$ 은 관측된(결정론적인) 실제 데이터이다. 따라서 두 확률 분포 사이의 거리를 재는 KL 발산 대신 주어진  $\mathbf{x}\_1$  상태에서 진짜 데이터  $\mathbf{x}\_0$ 가 등장할 확률(likelihood)을 측정하는  $-\log p$ 형태를 취해야 한다.

섹션 3.3에서 언급되지만, 모델이 예측한 연속적인 가우시안 분포와 관측된 이산형 데이터를 연결하기 위해 독립적인 이산 디코더를 도입하였다. 연속적인 가우시안 분포의 확률 밀도 함수를 특정 픽셀 값의 아주 좁은 구간에 대해 적분하여 이산적인 확률값으로 반환한다.
</div>



(자세한 내용은 부록 A를 참조하라. 항에 붙은 레이블은 섹션 3에서 사용된다.) 수식 (5)는 KL 발산(KL divergence)을 사용하여  $p\_\theta(\mathbf{x}\_{t-1} \mid \mathbf{x}\_t)$ 를 순방향 과정의 사후 분포(posteriors)와 직접 비교하는데, 이 사후 분포는  $\mathbf{x}\_0$ 를 조건으로 할 때 다루기 용이하다(tractable):  

<div class="math-container">
$$  
q(\mathbf{x}_{t-1} \mid \mathbf{x}_t, \mathbf{x}_0) = \mathcal{N}(\mathbf{x}_{t-1}; \tilde{\boldsymbol{\mu}}_t(\mathbf{x}_t, \mathbf{x}_0), \tilde{\beta}_t \mathbf{I}), \quad (6)  
$$
</div>



<div class="math-container">
$$  
\text{where} \quad \tilde{\boldsymbol{\mu}}_t(\mathbf{x}_t, \mathbf{x}_0) := \frac{\sqrt{\bar{\alpha}_{t-1}} \beta_t}{1 - \bar{\alpha}_t} \mathbf{x}_0 + \frac{\sqrt{\alpha_t}(1 - \bar{\alpha}_{t-1})}{1 - \bar{\alpha}_t} \mathbf{x}_t \quad \text{and} \quad \tilde{\beta}_t := \frac{1 - \bar{\alpha}_{t-1}}{1 - \bar{\alpha}_t} \beta_t \quad (7)  
$$
</div>


결과적으로, 수식 (5)에 있는 모든 KL 발산은 가우시안들 간의 비교가 되므로, 분산이 큰 몬테카를로 추정(Monte Carlo estimates) 대신 닫힌 형태의 수식을 가진 라오-블랙웰라이즈(Rao-Blackwellized) 방식으로 계산할 수 있다.  



<div class="obsidian-callout" markdown="1">
 $\mathbf{x}\_0$ 를 조건으로 한다는 것은, 편향된 추정이 아닌가? X

조건은 q에만 있음. 모델이 추정하는  $p\_{\theta}$ 에는 없음.
</div>



## 부록 A
(조건부 베이즈 정리와 망원 급수 전개 과정을 추가하였다.)  

<div class="math-container">
$$  
L = \mathbb{E}_q \left[ -\log \frac{p_\theta(\mathbf{x}_{0:T})}{q(\mathbf{x}_{1:T} \mid \mathbf{x}_0)} \right] \quad (17)  
$$
</div>



<div class="math-container">
$$  
= \mathbb{E}_q \left[ -\log p(\mathbf{x}_T) - \sum_{t \ge 1} \log \frac{p_\theta(\mathbf{x}_{t-1} \mid \mathbf{x}_t)}{q(\mathbf{x}_t \mid \mathbf{x}_{t-1})} \right] \quad (18)  
$$
</div>


마르코프 연쇄 정의에 의해 분자의 역과정  $p\_\theta(\mathbf{x}\_{0:T})$ 는  $p(\mathbf{x}\_T) \prod\_{t=1}^T p\_\theta(\mathbf{x}\_{t-1} \mid \mathbf{x}\_t)$ 로, 분모의 순방향 과정  $q(\mathbf{x}\_{1:T} \mid \mathbf{x}\_0)$ 는  $\prod\_{t=1}^T q(\mathbf{x}\_t \mid \mathbf{x}\_{t-1})$ 로 분해됩니다. 대입 후 로그의 성질( $\log(AB) = \log A + \log B$ ,  $\log(\prod) = \sum \log$ )을 적용하여 곱셈을 덧셈 기호( $\sum$ )로 변환한 형태이다. 식 (3)과 동일하다.  

-  $t=1$  항의 분리

<div class="math-container">
$$  
= \mathbb{E}_q \left[ -\log p(\mathbf{x}_T) - \sum_{t>1} \log \frac{p_\theta(\mathbf{x}_{t-1} \mid \mathbf{x}_t)}{q(\mathbf{x}_t \mid \mathbf{x}_{t-1})} - \log \frac{p_\theta(\mathbf{x}_0 \mid \mathbf{x}_1)}{q(\mathbf{x}_1 \mid \mathbf{x}_0)} \right] \quad (19)  
$$
</div>


시그마( $\sum\_{t \ge 1}$ )에서  $t=1$ 일 때의 항을 바깥으로 빼낸 것이다. 이는 이후 망원 급수에서 소거될 항을 맞추기 위한 밑작업이다.  

- 조건부 베이즈 정리 적용
결합 확률  $q(\mathbf{x}\_t, \mathbf{x}\_{t-1} \mid \mathbf{x}\_0)$ 를 두 가지 방식으로 전개할 수 있다.  
- A:  $q(\mathbf{x}\_t \mid \mathbf{x}\_{t-1}, \mathbf{x}\_0) q(\mathbf{x}\_{t-1} \mid \mathbf{x}\_0)$ 
- B:  $q(\mathbf{x}\_{t-1} \mid \mathbf{x}\_t, \mathbf{x}\_0) q(\mathbf{x}\_t \mid \mathbf{x}\_0)$ 
순방향 과정은 마르코프 연쇄이므로, 현재 상태  $\mathbf{x}\_t$ 는 오직 직전 상태  $\mathbf{x}\_{t-1}$ 에만 의존한다. 따라서  $\mathbf{x}\_0$  조건은 무시될 수 있다.  

<div class="math-container">
$$  
q(\mathbf{x}_t \mid \mathbf{x}_{t-1}, \mathbf{x}_0) = q(\mathbf{x}_t \mid \mathbf{x}_{t-1})  
$$
</div>


위 성질을 A에 대입하고, A와 B가 같다는 등식을 세우면  

<div class="math-container">
$$  
q(\mathbf{x}_t \mid \mathbf{x}_{t-1}) q(\mathbf{x}_{t-1} \mid \mathbf{x}_0) = q(\mathbf{x}_{t-1} \mid \mathbf{x}_t, \mathbf{x}_0) q(\mathbf{x}_t \mid \mathbf{x}_0)  
$$
</div>


이를  $q(\mathbf{x}\_t \mid \mathbf{x}\_{t-1})$ 에 대해 정리하면  

<div class="math-container">
$$  
q(\mathbf{x}_t \mid \mathbf{x}_{t-1}) = \frac{q(\mathbf{x}_{t-1} \mid \mathbf{x}_t, \mathbf{x}_0) q(\mathbf{x}_t \mid \mathbf{x}_0)}{q(\mathbf{x}_{t-1} \mid \mathbf{x}_0)}  
$$
</div>


이를 식 (19)의 시그마 내부 분모에 대입하면 식 (20)을 유도할 수 있다.  

<div class="math-container">
$$  
= \mathbb{E}_q \left[ -\log p(\mathbf{x}_T) - \sum_{t>1} \log \left( \frac{p_\theta(\mathbf{x}_{t-1} \mid \mathbf{x}_t)}{q(\mathbf{x}_{t-1} \mid \mathbf{x}_t, \mathbf{x}_0)} \cdot \frac{q(\mathbf{x}_{t-1} \mid \mathbf{x}_0)}{q(\mathbf{x}_t \mid \mathbf{x}_0)} \right) - \log \frac{p_\theta(\mathbf{x}_0 \mid \mathbf{x}_1)}{q(\mathbf{x}_1 \mid \mathbf{x}_0)} \right] \quad (20)  
$$
</div>


- 망원 급수를 통한 항 소거
망원급수(telescoping series)란 부분적 항들의 합이 소거 후에 결과적으로 고정된 값만이 남는 수열을 일컫는다.  
식 (20)의 시그마 내부 로그를 분리하면  

<div class="math-container">
$$  
\sum_{t=2}^T \left( \log \frac{p_\theta(\dots)}{q(\dots)} + \log q(\mathbf{x}_{t-1} \mid \mathbf{x}_0) - \log q(\mathbf{x}_t \mid \mathbf{x}_0) \right)  
$$
</div>


뒤쪽 두 항에 대해  $t=2$ 부터  $T$ 까지 나열하면(망원 급수)  

<div class="math-container">
$$  
(\log q(\mathbf{x}_1 \mid \mathbf{x}_0) - \log q(\mathbf{x}_2 \mid \mathbf{x}_0))  
$$
</div>



<div class="math-container">
$$  
+ (\log q(\mathbf{x}_2 \mid \mathbf{x}_0) - \log q(\mathbf{x}_3 \mid \mathbf{x}_0))
$$
</div>



<div class="math-container">
$$  
\dots  
$$
</div>



<div class="math-container">
$$  
+ (\log q(\mathbf{x}_{T-1} \mid \mathbf{x}_0) - \log q(\mathbf{x}_T \mid \mathbf{x}_0))
$$
</div>


이때 인접한 항들이 모두 소거되고 양끝 항만 남는다.  

<div class="math-container">
$$  
= \log q(\mathbf{x}_1 \mid \mathbf{x}_0) - \log q(\mathbf{x}_T \mid \mathbf{x}_0)  
$$
</div>


이때, 시그마 앞에 마이너스가 있으므로  

<div class="math-container">
$$  
- \log p(\mathbf{x}_T) - (\dots \text{시그마 나머지} \dots) - \log q(\mathbf{x}_1 \mid \mathbf{x}_0) + \log q(\mathbf{x}_T \mid \mathbf{x}_0) - \left( \log p_\theta(\mathbf{x}_0 \mid \mathbf{x}_1) - \log q(\mathbf{x}_1 \mid \mathbf{x}_0) \right)
$$
</div>


 $-\log q(\mathbf{x}\_1 \mid \mathbf{x}\_0)$  와 뒤쪽의  $+ \log q(\mathbf{x}\_1 \mid \mathbf{x}\_0)$ 가 소거된다(앞서  $t=1$  항을 분리한 이유).  
 $- \log p(\mathbf{x}\_T)$  와  $+ \log q(\mathbf{x}\_T \mid \mathbf{x}\_0)$  를 병합하여  $-\log \frac{p(\mathbf{x}\_T)}{q(\mathbf{x}\_T \mid \mathbf{x}\_0)}$ 를 만들어 식 (21)을 유도한다.  

<div class="math-container">
$$  
= \mathbb{E}_q \left[ -\log \frac{p(\mathbf{x}_T)}{q(\mathbf{x}_T \mid \mathbf{x}_0)} - \sum_{t>1} \log \frac{p_\theta(\mathbf{x}_{t-1} \mid \mathbf{x}_t)}{q(\mathbf{x}_{t-1} \mid \mathbf{x}_t, \mathbf{x}_0)} - \log p_\theta(\mathbf{x}_0 \mid \mathbf{x}_1) \right] \quad (21)  
$$
</div>


- KL발산으로 치환
KL 발산의 정의:  $D\_{\text{KL}}(P \parallel Q) = \mathbb{E}\_{x \sim P} \left[ \log \frac{P(x)}{Q(x)} \right]$   
식 (21)의 첫번째 항  $\mathbb{E}\_q \left[ -\log \frac{p(\mathbf{x}\_T)}{q(\mathbf{x}\_T \mid \mathbf{x}\_0)} \right]$ 내부에 마이너스 부호를 적용하여 분자와 분모를 뒤집는다.  

<div class="math-container">
$$  
= \mathbb{E}_q \left[ \log \frac{q(\mathbf{x}_T \mid \mathbf{x}_0)}{p(\mathbf{x}_T)} \right] = D_{\text{KL}}(q(\mathbf{x}_T \mid \mathbf{x}_0) \parallel p(\mathbf{x}_T))  
$$
</div>


두번째 항도 마이너스를 적용하여 분자/분모를 뒤집는다.  

<div class="math-container">
$$  
= \mathbb{E}_q \left[ \sum_{t>1} \log \frac{q(\mathbf{x}_{t-1} \mid \mathbf{x}_t, \mathbf{x}_0)}{p_\theta(\mathbf{x}_{t-1} \mid \mathbf{x}_t)} \right] = \sum_{t>1} D_{\text{KL}}(q(\mathbf{x}_{t-1} \mid \mathbf{x}_t, \mathbf{x}_0) \parallel p_\theta(\mathbf{x}_{t-1} \mid \mathbf{x}_t))  
$$
</div>


세 번째 항은 재구성 오차이므로 KL발산으로 묶지 않고 우도 형태 그대로 두어 식 (21)을 유도한다.  

<div class="math-container">
$$  
= \mathbb{E}_q \left[ D_{\text{KL}}(q(\mathbf{x}_T \mid \mathbf{x}_0) \parallel p(\mathbf{x}_T)) + \sum_{t>1} D_{\text{KL}}(q(\mathbf{x}_{t-1} \mid \mathbf{x}_t, \mathbf{x}_0) \parallel p_\theta(\mathbf{x}_{t-1} \mid \mathbf{x}_t)) - \log p_\theta(\mathbf{x}_0 \mid \mathbf{x}_1) \right] \quad (22)  
$$
</div>
