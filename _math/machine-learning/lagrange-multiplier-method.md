---
layout: sidebar
title: 라그랑주 승수법
collection_name: notes
nav_order: 4999
---

Lagrange Multiplier Method. 이름부터 벌써 "나 어려운거니까 함부로 보지 마세요"를 말하는 것 같다.  


제약 조건이 있다는 것은 정의역의 바운더리가 한정된다는 뜻이며, 그 바운더리 안에 찾은 최적점은 전역 최적해와 다를 확률이 매우 높다.  

라그랑주 함수는 전역 최댓값이나 전역 최솟값을 가지지 않는다.  $\lambda$ 의 부호나 크기를 마음대로 바꿀 수 있기 때문에, 이 함수는 특정 방향으로는 무한히 솟아오르거나 다른 방향으로 무한히 추락하는 형태를 띈다.  
제약 조건을 목적 함수로 올렸다는 것은 "전 구간에서 새로운 함수의 가장 높은 최적점을 찾겠다"는 것이 아니라, 변수를 하나( $\lambda$ ) 더 추가하여, 확장된 공간에서 모든 미분값이 0이 되는 안장점을 찾는 문제로 변환한 것이다.  
입력( $\boldsymbol{v}$ )에 대해 미분해서 0이 되는 조건 ( $\nabla\_{\boldsymbol{v}} \mathcal{L} = \boldsymbol{0}$ )은 두 그래디언트( $\nabla\_{\boldsymbol{v}} \mathcal{f}, \nabla\_{\boldsymbol{v}} \mathcal{g}$ )가 평행해야 함을 뜻한다.  


예시는 이 [글]({{ site.baseurl }}/notes/machine-learning/variance-of-multidimensional-random-variable/)에서 이어진다.
