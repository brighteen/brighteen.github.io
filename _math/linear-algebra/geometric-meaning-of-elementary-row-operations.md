---
layout: sidebar
title: 기본 행 연산의 기하학적 의미
collection_name: notes
nav_order: 1999
---

## 1. 기본행렬(Elementary Matrix)의 정의

항등행렬  $I$ 에 단 한 번의 기본 행 연산(Elementary Row Operation)을 적용하여 도출되는 정사각행렬을 기본행렬이라 정의한다. 기본 행 연산은 다음 세 가지로 분류되며, 각 연산에 대응하는 기본행렬이 존재한다.  

1. 행 교환: 두 행의 위치를 바꾼다.
    
2. 행 스케일링: 한 행에 0이 아닌 상수를 곱한다.  
    
3. 행 덧셈: 한 행에 다른 행의 상수배를 더한다.  
    

임의의 행렬  $A$ 에 기본행렬  $E$ 를 좌곱( $EA$ )하는 연산은 행렬  $A$ 에 해당 기본 행 연산을 직접 수행하는 것과 대수적으로 동치이다.  

세 가지 기본 행 연산은 모두 수학적으로 역연산이 존재한다. 교환된 행은 다시 교환하여 복구할 수 있고, 상수를 곱한 행은 역수를 곱하여 복구할 수 있으며, 더해진 행은 동일한 상수배를 빼서 복구할 수 있다. 따라서 모든 기본행렬은 가역적(Invertible)이며, 그 역행렬 역시 기본행렬이다.  

## 2. 기본행렬을 통한 역행렬 유도

가우스 소거법의 각 기본 행 연산은 대응하는 기본행렬  $E$ 의 곱으로 환원된다. 행렬  $A$ 를 기약 행 사다리꼴(RREF)인 항등행렬  $I$ 로 변환하는 과정은 다음과 같이 기본행렬들의 연속적인 좌곱으로 나타낼 수 있다.  

<div class="math-container">
$$  
A = \begin{bmatrix} 1 & 0 & 2 & 0 \\ 1 & 1 & 0 & 0 \\ 1 & 2 & 0 & 1 \\ 1 & 1 & 1 & 1 \end{bmatrix}  
$$
</div>


**1단계: 1열 소거 (** $E\_1$ **)**  

1행을 기준으로 2, 3, 4행의 1열 성분을 0으로 소거한다. (연산:  $R\_2 - R\_1, R\_3 - R\_1, R\_4 - R\_1$ )  

<div class="math-container">
$$  
E_1 = \begin{bmatrix} 1 & 0 & 0 & 0 \\ -1 & 1 & 0 & 0 \\ -1 & 0 & 1 & 0 \\ -1 & 0 & 0 & 1 \end{bmatrix}, \quad A_1 = E_1 A = \begin{bmatrix} 1 & 0 & 2 & 0 \\ 0 & 1 & -2 & 0 \\ 0 & 2 & -2 & 1 \\ 0 & 1 & -1 & 1 \end{bmatrix}  
$$
</div>


**2단계: 2열 소거 (** $E\_2$ **)**  

2행을 기준으로 3, 4행의 2열 성분을 0으로 소거한다. (연산:  $R\_3 - 2R\_2, R\_4 - R\_2$ )  

<div class="math-container">
$$  
E_2 = \begin{bmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & -2 & 1 & 0 \\ 0 & -1 & 0 & 1 \end{bmatrix}, \quad A_2 = E_2 A_1 = \begin{bmatrix} 1 & 0 & 2 & 0 \\ 0 & 1 & -2 & 0 \\ 0 & 0 & 2 & 1 \\ 0 & 0 & 1 & 1 \end{bmatrix}  
$$
</div>


**3단계: 3행과 4행 교환 (** $E\_3$ **)**  

3행 3열의 피벗(2)과 4행 3열의 성분(1) 위치를 교환하여 피벗을 1로 설정한다. (연산:  $R\_3 \leftrightarrow R\_4$ )  

<div class="math-container">
$$  
E_3 = \begin{bmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 0 & 1 \\ 0 & 0 & 1 & 0 \end{bmatrix}, \quad A_3 = E_3 A_2 = \begin{bmatrix} 1 & 0 & 2 & 0 \\ 0 & 1 & -2 & 0 \\ 0 & 0 & 1 & 1 \\ 0 & 0 & 2 & 1 \end{bmatrix}  
$$
</div>


**4단계: 3열 소거 (** $E\_4$ **)**  

3행을 기준으로 1, 2, 4행의 3열 성분을 0으로 소거한다. (연산:  $R\_1 - 2R\_3, R\_2 + 2R\_3, R\_4 - 2R\_3$ )  

<div class="math-container">
$$  
E_4 = \begin{bmatrix} 1 & 0 & -2 & 0 \\ 0 & 1 & 2 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & -2 & 1 \end{bmatrix}, \quad A_4 = E_4 A_3 = \begin{bmatrix} 1 & 0 & 0 & -2 \\ 0 & 1 & 0 & 2 \\ 0 & 0 & 1 & 1 \\ 0 & 0 & 0 & -1 \end{bmatrix}  
$$
</div>


**5단계: 4행 스케일링 (** $E\_5$ **)**  

4행 4열의 피벗을 1로 설정하기 위해 -1을 곱한다. (연산:  $-1 \times R\_4$ )  

<div class="math-container">
$$  
E_5 = \begin{bmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & -1 \end{bmatrix}, \quad A_5 = E_5 A_4 = \begin{bmatrix} 1 & 0 & 0 & -2 \\ 0 & 1 & 0 & 2 \\ 0 & 0 & 1 & 1 \\ 0 & 0 & 0 & 1 \end{bmatrix}  
$$
</div>


**6단계: 4열 소거 (** $E\_6$ **)**  

4행을 기준으로 1, 2, 3행의 4열 성분을 0으로 소거한다. (연산:  $R\_1 + 2R\_4, R\_2 - 2R\_4, R\_3 - R\_4$ )  

<div class="math-container">
$$  
E_6 = \begin{bmatrix} 1 & 0 & 0 & 2 \\ 0 & 1 & 0 & -2 \\ 0 & 0 & 1 & -1 \\ 0 & 0 & 0 & 1 \end{bmatrix}, \quad I = E_6 A_5 = \begin{bmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix}  
$$
</div>


**결론**  

행렬  $A$ 에 도출된 기본행렬들을 좌곱한 결과는 항등행렬  $I$ 이다.  

<div class="math-container">
$$  
E_6 E_5 E_4 E_3 E_2 E_1 \cdot A = I  
$$
</div>


역행렬의 정의에 따라 행렬  $A$ 에  $A^{-1}$ 을 곱한 결과는  $I$ 이므로, 기본행렬들의 연속적인 곱은  $A$ 의 역행렬과 동치이다.  

<div class="math-container">
$$  
A^{-1} = E_6 E_5 E_4 E_3 E_2 E_1  
$$
</div>


## 3. 기본 행 연산의 기하학적 의미

선형계에 적용되는 기본 행 연산은 기하학적으로 해집합(교점)의 위치를 보존하는 회전 및 스케일 변환이다.  

2차원 공간 상의 선형계를 통해 이 과정을 대수적, 기하학적으로 증명한다.  

임의의 두 직선  $a\_{11}x + a\_{12}y = b\_1$ 과  $a\_{21}x + a\_{22}y = b\_2$ 의 첨가행렬(Augmented Matrix)은 다음과 같다.  

<div class="math-container">
$$  
\begin{bmatrix} a_{11} & a_{12} & \vert & b_1 \\ a_{21} & a_{22} & \vert & b_2 \end{bmatrix}  
$$
</div>


두 직선이 선형독립이고 유일한 교점  $(x\_0, y\_0)$ 를 갖는다고 상정한다. 기본 행 연산을 통해 기약 행 사다리꼴을 도출하면 첨가행렬은 다음 형태로 변환된다.  

<div class="math-container">
$$  
\begin{bmatrix} 1 & 0 & \vert & x_0 \\ 0 & 1 & \vert & y_0 \end{bmatrix}  
$$
</div>


이는 두 직선의 방정식에 대한 선형 결합을 통해, 동일한 교점  $(x\_0, y\_0)$ 을 공유하는 초평면 집합 속에서 법선 벡터가 좌표계의 표준 기저 벡터와 평행해지도록(즉, 직선 자체가 각 좌표축과 직교하도록) 새로운 직선 방정식을 선택하여 해집합을 정렬하는 과정을 의미한다.  

간단한 예시를 들어보자.  

<div class="math-container">
$$  
\begin{aligned}x + y = 1 \\ -2x + y = 1 \end{aligned}  
$$
</div>


![기본행렬 변환1.png]({{ site.baseurl }}/assets/notes/linear-algebra/1.png){: .img-normal width="400" }
해당 선형계의 교점은  $(0, 1)$ 이다. 첨가행렬의 변환 과정을 통해 기하학적 변화를 추적한다.  

<div class="math-container">
$$  
\begin{bmatrix} 1 & 1 & \vert & 1 \\ -2 & 1 & \vert & 1 \end{bmatrix}  
$$
</div>


- **1단계:**  $R\_2 \leftarrow R\_2 + 2R\_1$   

<div class="math-container">
$$  
\begin{bmatrix} 1 & 1 & \vert & 1 \\ 0 & 3 & \vert & 3 \end{bmatrix}  
$$
</div>


    두 번째 방정식이  $3y = 3$ 으로 변환된다. 기하학적으로 첫 번째 직선  $x + y = 1$ 은 불변하며, 두 직선의 선형 결합으로 도출된 두 번째 방정식은 법선 벡터가  $\begin{bmatrix}-2 \ 1\end{bmatrix}$ 에서  $\begin{bmatrix}0 \ 3\end{bmatrix}$ 으로 변환되어 교점  $(0, 1)$ 을 지나는 기울기 0의 수평선이 된다.  
![기본행렬 변환2.png]({{ site.baseurl }}/assets/notes/linear-algebra/2.png){: .img-normal width="400" }
    
- **2단계:**  $R\_2 \leftarrow \frac{1}{3}R\_2$   

<div class="math-container">
$$  
\begin{bmatrix} 1 & 1 & \vert & 1 \\ 0 & 1 & \vert & 1 \end{bmatrix}  
$$
</div>


    두 번째 방정식이  $y = 1$ 로 환원된다. 대수적 계수는 축소되었으나, 기하학적 직선의 위치 집합은 보존된다.  
    
- **3단계:**  $R\_1 \leftarrow R\_1 - R\_2$   

<div class="math-container">
$$  
\begin{bmatrix} 1 & 0 & \vert & 0 \\ 0 & 1 & \vert & 1 \end{bmatrix}  
$$
</div>


    첫 번째 방정식이  $x = 0$ 으로 변환된다. 두 번째 수평선  $y = 1$ 은 불변하며, 선형 결합을 통해 첫 번째 방정식의 법선 벡터가  $\begin{bmatrix}1 \ 0\end{bmatrix}$ 으로 변환되어 교점  $(0, 1)$ 을 지나며 y축과 일치하는 수직선  $x = 0$ 이 도출된다.  
![기본행렬 변환3.png]({{ site.baseurl }}/assets/notes/linear-algebra/3.png){: .img-normal width="400" }
    

결과적으로 기본 행 연산(행 교환, 스케일링, 상수배 덧셈)은 기하학적으로 선형계의 교점 위치 좌표를 보존하며, 초평면 집합 내에서의 선형 결합을 통해 하이퍼플레인(2차원의 경우 직선)들의 법선 벡터 방향을 변환시켜 결과적으로 각 하이퍼플레인이 좌표축에 직교하도록 방정식을 재구성하는 과정이다.
