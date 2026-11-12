---
layout: sidebar
title: Exercises
collection_name: mml
nav_order: 101212
date: 2026-04-03
---

연습문제  

2.1 우리는  $(\mathbb{R}\setminus{-1}, \star)$ 을 고려한다. 여기서 연산은 다음과 같이 정의된다:  

<div class="math-container">
$$  
a \star b := ab + a + b, \quad a, b \in \mathbb{R}\setminus\{-1\} \quad (2.134)  
$$
</div>


a.  $(\mathbb{R}\setminus{-1}, \star)$ 이 아벨 군(Abelian group)임을 보여라.  
b. 아벨 군  $(\mathbb{R}\setminus{-1}, \star)$ 에서 다음 식을 풀어라. 여기서  $\star$ 는 (2.134)에 정의된 것이다.  

<div class="math-container">
$$  
3 \star x \star x = 15  
$$
</div>


풀이  
a. 아벨 군임을 증명  
아벨 군이 되기 위해서는 5가지 성질(닫혀있음, 결합법칙, 항등원, 역원, 교환법칙)을 만족해야 합니다.  
1. 닫혀있음 (Closure):
     $a, b \neq -1$ 일 때  $a \star b \neq -1$ 임을 보여야 합니다.  
    만약  $a \star b = -1$ 이라고 가정하면,  
     $ab + a + b = -1$   
     $ab + a + b + 1 = 0$   
     $(a+1)(b+1) = 0$   
    이 식이 성립하려면  $a=-1$  또는  $b=-1$ 이어야 하는데, 이는 전제 조건( $a, b \in \mathbb{R}\setminus\{-1\}$ )에 모순됩니다. 따라서  $a \star b \neq -1$ 이며 연산은 닫혀있습니다.  
    
2. 결합법칙 (Associativity):  
     $(a \star b) \star c = a \star (b \star c)$ 임을 확인합니다.  
    좌변:  $(ab+a+b) \star c = (ab+a+b)c + (ab+a+b) + c = abc + ac + bc + ab + a + b + c$   
    우변:  $a \star (bc+b+c) = a(bc+b+c) + a + (bc+b+c) = abc + ab + ac + a + bc + b + c$   
    좌변과 우변이 같으므로 성립합니다.  
    
3. 항등원 (Neutral Element):  
     $a \star e = a$ 를 만족하는  $e$ 를 찾습니다.  
     $ae + a + e = a$   
     $ae + e = 0$   
     $e(a+1) = 0$   
     $a \neq -1$ 이므로  $a+1 \neq 0$ 입니다. 따라서  $e=0$ 이어야 합니다.  $0$ 은 집합  $\mathbb{R}\setminus{-1}$ 에 포함되므로 항등원은  $0$ 입니다.  
    
4. 역원 (Inverse Element):  
     $a \star b = e = 0$ 을 만족하는  $b$ 를 찾습니다.  
     $ab + a + b = 0$   
     $b(a+1) = -a$   
     $b = -\frac{a}{a+1}$   
    여기서  $a \neq -1$ 이므로 분모는 0이 아닙니다. 또한  $b = -1$ 인지 확인해보면,  $-a = -(a+1) \implies -a = -a-1 \implies 0 = -1$  (모순)이므로  $b \neq -1$ 입니다. 따라서 역원이 존재합니다.  
    
5. 교환법칙 (Commutativity):  
     $a \star b = ab + a + b$ 이고  $b \star a = ba + b + a$ 입니다.  
    실수의 덧셈과 곱셈은 교환법칙이 성립하므로 두 식은 같습니다.  

결론적으로, 이 집합과 연산은 아벨 군입니다.  

b. 방정식 풀이  
주어진 식:  $3 \star x \star x = 15$   
먼저 연산의 순서대로 계산합니다.  
1.  $3 \star x$  계산:
     $3 \star x = 3x + 3 + x = 4x + 3$   
    
2.  $(3 \star x) \star x$  계산:  
     $(4x + 3) \star x = (4x+3)x + (4x+3) + x$   
     $= 4x^2 + 3x + 4x + 3 + x$   
     $= 4x^2 + 8x + 3$   
    
3. 방정식 풀기:  
     $4x^2 + 8x + 3 = 15$   
     $4x^2 + 8x - 12 = 0$   
    양변을 4로 나누면,  
     $x^2 + 2x - 3 = 0$   
     $(x+3)(x-1) = 0$   
    따라서  $x = 1$  또는  $x = -3$ .  
    두 값 모두  $-1$ 이 아니므로 집합에 포함됩니다.  

답:  $x = 1$  또는  $x = -3$   


---  

2.2  $n \in \mathbb{N}\setminus\{0\}$ 이라 하자.  $k, x$ 는 정수( $\mathbb{Z}$ )이다. 우리는 정수  $k$ 의 합동류(congruence class)  $\bar{k}$ 를 다음과 같은 집합으로 정의한다:  

<div class="math-container">
$$  
\bar{k} = \{ x \in \mathbb{Z} \vert  x - k = 0 \pmod n \}  
$$
</div>



<div class="math-container">
$$  
= \{ x \in \mathbb{Z} \vert  \exists a \in \mathbb{Z} : (x - k = n \cdot a) \}  
$$
</div>


우리는 이제  $\mathbb{Z}/n\mathbb{Z}$  (때때로  $\mathbb{Z}\_n$ 으로 표기됨)를 법(modulo)  $n$ 에 대한 모든 합동류의 집합으로 정의한다. 유클리드 나눗셈은 이 집합이  $n$ 개의 원소를 포함하는 유한 집합임을 암시한다:  

<div class="math-container">
$$  
\mathbb{Z}_n = \{ \bar{0}, \bar{1}, \dots, \overline{n-1} \}  
$$
</div>


모든  $\bar{a}, \bar{b} \in \mathbb{Z}\_n$ 에 대하여, 우리는 다음을 정의한다:  

<div class="math-container">
$$  
\bar{a} \oplus \bar{b} := \overline{a+b}  
$$
</div>


a.  $(\mathbb{Z}\_n, \oplus)$ 가 군임을 보여라. 이것은 아벨 군인가?  
b. 우리는 이제  $\mathbb{Z}\_n$ 의 모든  $\bar{a}$ 와  $\bar{b}$ 에 대하여 또 다른 연산  $\otimes$ 를 정의한다:  

<div class="math-container">
$$  
\bar{a} \otimes \bar{b} := \overline{a \times b} \quad (2.135)  
$$
</div>


여기서  $a \times b$ 는  $\mathbb{Z}$ 에서의 일반적인 곱셈을 나타낸다.  
 $n=5$ 라 하자.  $\mathbb{Z}\_5\setminus\{\bar{0}\}$  원소들의  $\otimes$  연산에 대한 곱셈표(times table)를 그려라. 즉,  $\mathbb{Z}\_5\setminus{\bar{0}}$ 의 모든  $\bar{a}$ 와  $\bar{b}$ 에 대해 곱  $\bar{a} \otimes \bar{b}$ 를 계산하라.  
이를 통해,  $\mathbb{Z}\_5\setminus{\bar{0}}$ 가  $\otimes$ 에 대해 닫혀 있으며  $\otimes$ 에 대한 항등원을 가짐을 보여라.  $\otimes$ 에 대한  $\mathbb{Z}\_5\setminus{\bar{0}}$ 의 모든 원소의 역원을 보여라.  $(\mathbb{Z}\_5\setminus{\bar{0}}, \otimes)$ 가 아벨 군이라고 결론지어라.  
c.  $(\mathbb{Z}\_8\setminus{\bar{0}}, \otimes)$ 가 군이 아님을 보여라.  
d. 베주 항등식(Bézout theorem)은 두 정수  $a$ 와  $b$ 가 서로소(즉,  $\gcd(a, b)=1$ )일 필요충분조건은  $au + bv = 1$ 이 되는 두 정수  $u$ 와  $v$ 가 존재하는 것이라고 말한다.  $(\mathbb{Z}\_n\setminus{\bar{0}}, \otimes)$ 가 군이 될 필요충분조건은  $n \in \mathbb{N}\setminus{0}$ 이 소수(prime)인 경우임을 보여라.  

풀이  
a. 덧셈군 증명  
1. 닫혀있음: 정수의 합은 정수이며, 이를  $n$ 으로 나눈 나머지는 다시  $\mathbb{Z}\_n$  안에 있다.
2. 결합법칙: 정수의 덧셈은 결합법칙이 성립하므로, 합동류의 덧셈도 성립한다.  $(\bar{a} \oplus \bar{b}) \oplus \bar{c} = \overline{(a+b)+c} = \overline{a+(b+c)} = \bar{a} \oplus (\bar{b} \oplus \bar{c})$ .  
3. 항등원:  $\bar{a} \oplus \bar{0} = \overline{a+0} = \bar{a}$ . 따라서 항등원은  $\bar{0}$ 이다.  
4. 역원: 각  $\bar{a}$ 에 대해,  $\bar{a} \oplus \overline{n-a} = \overline{a+n-a} = \bar{n} = \bar{0}$ . 따라서 역원은  $\overline{n-a}$ 이다. (만약  $a=0$ 이면 역원은  $\bar{0}$ ).  
5. 아벨 군 여부: 정수의 덧셈은 교환 가능하므로  $\bar{a} \oplus \bar{b} = \overline{a+b} = \overline{b+a} = \bar{b} \oplus \bar{a}$ 이다.  
    결론:  $(\mathbb{Z}\_n, \oplus)$ 는 아벨 군이다.  

b.  $n=5$ 일 때 곱셈군 확인**  
집합:  $\{\bar{1}, \bar{2}, \bar{3}, \bar{4}\}$   
곱셈표:  

| **⊗**         | **1ˉ**    | **2ˉ**    | **3ˉ**    | **4ˉ**    |  
| ------------- | --------- | --------- | --------- | --------- |  
| ** $\bar{1}$ ** |  $\bar{1}$  |  $\bar{2}$  |  $\bar{3}$  |  $\bar{4}$  |  
| ** $\bar{2}$ ** |  $\bar{2}$  |  $\bar{4}$  |  $\bar{1}$  |  $\bar{3}$  |  
| ** $\bar{3}$ ** |  $\bar{3}$  |  $\bar{1}$  |  $\bar{4}$  |  $\bar{2}$  |  
| ** $\bar{4}$ ** |  $\bar{4}$  |  $\bar{3}$  |  $\bar{2}$  |  $\bar{1}$  |  
- 닫혀있음: 표의 모든 결과값이 집합  $\{\bar{1}, \bar{2}, \bar{3}, \bar{4}\}$  안에 존재한다 ( $\bar{0}$ 이 나오지 않음).
- 항등원: 첫 번째 행과 열을 보면  $\bar{1}$ 과 연산했을 때 자기 자신이 나오므로 항등원은  $\bar{1}$ 이다.
- 역원: 결과가  $\bar{1}$ 이 되는 짝을 찾는다.
    -  $\bar{1}$ 의 역원:  $\bar{1}$  ( $1 \times 1 = 1$ )
    -  $\bar{2}$ 의 역원:  $\bar{3}$  ( $2 \times 3 = 6 \equiv 1$ )
    -  $\bar{3}$ 의 역원:  $\bar{2}$  ( $3 \times 2 = 6 \equiv 1$ )
    -  $\bar{4}$ 의 역원:  $\bar{4}$  ( $4 \times 4 = 16 \equiv 1$ )
- 교환법칙: 표가 대각선을 기준으로 대칭이므로 성립한다.

결론:  $(\mathbb{Z}\_5\setminus{\bar{0}}, \otimes)$ 는 아벨 군이다.  
c.  $n=8$ 일 때 반례  
집합은  ${\bar{1}, \dots, \bar{7}}$ 이다.  
 $\bar{2}$ 와  $\bar{4}$ 를 선택하여 연산해본다.  
 $\bar{2} \otimes \bar{4} = \overline{2 \times 4} = \bar{8} = \bar{0}$   
연산의 결과인  $\bar{0}$ 이 집합  $(\mathbb{Z}\_8\setminus{\bar{0}})$ 에 포함되지 않는다.  
따라서 닫혀있지 않으므로(Not closed), 군이 아니다. (이를 영인자(Zero divisor)가 존재한다고 한다.)  

d. 소수 조건 증명  
우리는  $(\mathbb{Z}\_n\setminus{\bar{0}}, \otimes)$ 가 군이 되기 위한 조건이  $n$ 이 소수인 것임을 보여야 한다.  
1. 닫혀있음과 영인자:
    군이 되려면  $\bar{0}$ 이 나오면 안 된다. 즉,  $\bar{a} \otimes \bar{b} = \bar{0}$ 인  $\bar{a}, \bar{b} \neq \bar{0}$ 가 없어야 한다.  
     $ab \equiv 0 \pmod n$ 인데  $a, b$ 가  $n$ 의 배수가 아니라는 뜻은,  $n$ 이 합성수일 경우(예:  $n=ab$ ) 항상 성립한다. 따라서  $n$ 이 소수여야만 두 수의 곱이  $n$ 의 배수가 되는 것을 막을 수 있다(유클리드의 보조정리). 즉,  $n$ 이 소수이면 닫혀있다.  
2. 역원의 존재 (베주 항등식 이용):  
    임의의 원소  $\bar{a} \in \mathbb{Z}\_n\setminus{\bar{0}}$ 에 대해 역원이 존재해야 한다.  
    즉,  $\bar{a} \otimes \bar{x} = \bar{1}$ 인  $\bar{x}$ 가 있어야 한다.  
    이는 합동식  $ax \equiv 1 \pmod n$ 의 해가 존재한다는 뜻이다.  
     $ax - 1 = ny$  (어떤 정수  $y$ 에 대해)로 쓸 수 있으므로,  $ax - ny = 1$ 이다.  
    베주 항등식에 따르면,  $ax + n(-y) = 1$ 이 해를 가지려면  $\gcd(a, n) = 1$ 이어야 한다.  
     $\mathbb{Z}\_n\setminus{\bar{0}}$ 의 모든 원소  $a$  (즉,  $1 \le a < n$ )에 대해  $\gcd(a, n)=1$ 이 성립하려면,  $n$ 은 자신보다 작은 모든 수와 서로소여야 한다. 이것이 바로 소수(Prime)의 정의이다.  

결론: 닫혀있고 역원이 존재하기 위해서는  $n$ 이 소수여야 한다.  $n$ 이 소수이면  $\mathbb{Z}\_p\setminus{\bar{0}}$ 는 아벨 군(이를 곱셈군  $\mathbb{Z}\_p^\times$ 라 부름)이 된다.
