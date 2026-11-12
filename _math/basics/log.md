---
layout: sidebar
title: 로그
collection_name: notes
nav_order: 999
---

로그는 지수 함수의 역함수이다. 어떤 수를 나타내기 위해 고정된 밑을 몇 번 거듭제곱해야 하는지를 나타낸다.  
정의는 다음과 같다.  

<div class="math-container">
$$  
\log_a(b) = c \iff a^c = b  
$$
</div>


밑이  $a > 0$ ,  $a \neq 1$ 이고, 진수가  $x > 0$ ,  $y > 0$ 일 때,  $\log\_a(xy) = \log\_a(x) + \log\_a(y)$ 가 성립함을 지수 법칙을 통해 증명가능하다.  



<div class="obsidian-callout" markdown="1">
 $\to$  로그는 지수함수  $f(x) = a^x$ 의 역함수인데 만약  $a < 0$ 일 경우, 지수  $x$ 가 특정한 실수(예: 분모가 짝수인 분수)일 때 함숫값으로 허수가 유도된다.

<div class="math-container">
$$
(-2)^{1/2} = \sqrt{-2} = \sqrt{2}i
$$  
</div>


지수함수가 실수 전체의 정의역에서 연속적인 실수 함숫값을 갖도록 보장하기 위해 밑을 양수로 제한한다.

 $\to$   $a = 1$ 일 경우, 지수함수는  $f(x) = 1^x = 1$  형태의 상수함수가 된다. 상수함수는 일대일 대응 함수가 아니므로 역함수가 존재하지 않는다. 로그함수는 지수함수의 역함수로서 정의되므로, 역함수 성립 요건을 충족하기 위해 밑을  $a \neq 1$  로 전제한다.
</div>





<div class="obsidian-callout" markdown="1">
밑이 양수인  $a$ 를 임의의 실수  $m$ 번 거듭제곱한 결과는 항상 양수이므로 진수는 항상 0보다 크다.
</div>



두 개의 로그를 각각  $m$ 과  $n$ 으로 치환한다.  

<div class="math-container">
$$  
m = \log_a(x)  
$$
</div>



<div class="math-container">
$$  
n = \log_a(y)  
$$
</div>


두 식은 로그의 정의에 따라 지수 형태로 변환 가능하다.  

<div class="math-container">
$$  
x = a^m  
$$
</div>



<div class="math-container">
$$  
y = a^n  
$$
</div>


두 변수를 곱하면  

<div class="math-container">
$$  
xy = a^m \cdot a^n  
$$
</div>



<div class="math-container">
$$  
= a^{m+n}  
$$
</div>


이를 다시 로그 형태로 변환하면  

<div class="math-container">
$$  
\log_a(xy) = m + n  
$$
</div>



<div class="math-container">
$$  
\log_a(xy) = \log_a(x) + \log_a(y)  
$$
</div>
