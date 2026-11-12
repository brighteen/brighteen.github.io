---
layout: sidebar
title: SBM을 이해하기 위한 개념들
collection_name: genai
nav_order: 8999
date: 2026-05-16
---

### 1. 확률 미분 방정식 (Stochastic Differential Equation, SDE)

데이터에 시간 연속적으로 노이즈를 주입하고 다시 복원하는 과정의 뼈대이다. 기존 상미분 방정식(ODE)에 확률적 변동 항을 추가하여 입자의 궤적을 묘사한다.  

- 기반 개념: 상미분 방정식 기초, 브라운 운동(Brownian Motion) 또는 위너 과정(Wiener Process).
    

### 2. 스코어 함수와 디노이징 스코어 매칭 (Score Function & Denoising Score Matching)

신경망이 역과정의 SDE를 풀기 위해 학습해야 하는 유일한 미지수이자, 목적 함수를 연산하는 방법론이다.  

- 기반 개념: 다변수 미적분학(발산, 그래디언트), 부분 적분(Integration by parts), 베이즈 정리.
    

### 3. 포커-플랑크 방정식 (Fokker-Planck Equation)

SDE를 따르는 미시적인 입자 하나하나의 궤적이 아니라, 전체 입자들의 거시적인 확률 밀도 함수가 시간에 따라 어떻게 변화하는지 묘사하는 편미분 방정식이다.  

- 기반 개념: 편미분 방정식 기초, 연속 방정식(Continuity Equation), 확률의 보존 법칙.
    

### 4. 랑주뱅 동역학 (Langevin Dynamics)

스코어 방향으로의 결정론적 표류(Drift)와 가우시안 노이즈에 의한 확률적 섭동(Perturbation)을 결합하여, 목표 확률 분포에서 샘플을 추출하는 구체적인 물리적 알고리즘이다.  

- 기반 개념: 경사 상승법(Gradient Ascent), 통계 역학의 에너지 기반 모델.
    

### 5. 이토 미적분학 (Itô Calculus)과 앤더슨 정리 (Anderson's Theorem)

순방향 SDE를 역시간(Reverse-time) SDE로 변환하는 논문의 핵심 수식을 유도하기 위한 수학적 규칙이다. 확률 과정은 미분 불가능한 성질을 가지므로 일반적인 테일러 전개나 체인 룰(Chain rule)이 성립하지 않아 독립적인 미적분 체계가 필요하다.  

- 기반 개념: 확률 과정론, 마틴게일(Martingale), 이토 보조정리(Itô's Lemma).
    

### 6. 마르코프 연쇄 몬테카를로 (MCMC - Markov Chain Monte Carlo)

복잡한 확률 분포에서 직접 적분이나 샘플링이 불가능할 때 사용하는 근사 알고리즘의 총칭이다. 논문에서 제안하는 Predictor-Corrector 샘플러 중 Corrector의 구조를 이해하는 데 필요하다.  

- 기반 개념: 마르코프 연쇄의 전이 확률, 상세 균형 조건(Detailed Balance), 메트로폴리스-헤이스팅스(Metropolis-Hastings) 알고리즘.
    

### 7. 확률 흐름 상미분 방정식 (Probability Flow ODE)과 신경망 상미분 방정식 (Neural ODE)

SDE 기반의 역과정과 완전히 동일한 확률 분포 궤적을 가지면서도, 무작위성이 배제된 결정론적 경로를 유도하는 기법이다. 정확한 로그 우도(Exact Log-likelihood)를 계산하기 위해 사용된다.  

- 기반 개념: 연속 시간 정규화 흐름(Continuous Normalizing Flow), 야코비안 행렬(Jacobian), 허친슨 대각합 추정(Hutchinson Trace Estimator).
    

### 8. 수치적 미분 방정식 해법 (Numerical SDE/ODE Solvers)

연속적인 SDE와 ODE를 컴퓨터에서 계산하기 위해 이산적인 시간 단위로 쪼개어 연산하는 방법론이다. 논문의 Predictor 구조를 설계하는 데 쓰인다.  

- 기반 개념: 오일러 방법(Euler Method), 오일러-마루야마 방법(Euler-Maruyama Method), 테일러 급수 전개.
    

### 9. 변분 추론 (Variational Inference)과 증거 하한 (ELBO)

기존 DDPM의 이산적 훈련 목적 함수가 스코어 매칭과 동일하다는 동치성을 증명할 때 사용되는 통계학적 개념이다.  

- 기반 개념: 쿨백-라이블러 발산(KL Divergence), 젠슨 부등식(Jensen's Inequality).
    

### 10. 확률 변수의 선형 변환과 가우시안 분포의 성질

시간  $t$ 에 따른 다중 전이 확률을 매번 순차적으로 계산하지 않고, 주변 확률 분포  $p(x\_t\vert x\_0)$ 를 닫힌 형태(Closed-form)의 가우시안으로 한 번에 계산하기 위한 대수적 기법이다.  

- 기반 개념: 재매개변수화 트릭(Reparameterization Trick), 두 가우시안 분포의 합성 및 합성곱.
