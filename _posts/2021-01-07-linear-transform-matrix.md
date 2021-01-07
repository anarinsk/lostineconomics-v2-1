---
layout: post
toc: false
comments: true
title:  Matrix as Linear Transformation
description: 함수로써의 매트릭스 
categories: [math, matrix-theory]

---

![enter image description here](https://github.com/anarinsk/lostineconomics-v2-1/blob/master/images/linear-transform/matrix_func.png?raw=true){: style="margin: auto; display: block; border:1.5px solid #021a40;"}{: width="500"}

위의 내용을 이해할 수 있다면, 더 할 것이 없다. 일단 잘 봐두도록 하자. 나중에 와서 다시 음미하면 의미가 와 닿을 것이다. 

선형 변환은 특별한 형태의 함수로 이해할 수 있다. 다만 투입과 산출이 다양한 차원(벡터)을 취할 수 있다. 그리고 이 선형 변환이 매트릭스로 표현될 수 있다. 왜 매트릭스 표현이 이렇게 강력할 수 있는지가, 이 대목에 있다. 선형 변환 함수를 구체적으로 표현해주는 것이 매트릭스다. 

## Linear Transformation 

### Concepts 

- $V$: $T$의 인풋 
- $W$: $T$의 아웃풋 
- $T: V \to W$: $V$에서 $W$로의 선형 변환 
	- $T(\vec v) = \vec w$. 즉, $\vec v \in V$를 $\vec w \in W$로 변환하는 것을 나타낸다. 

![enter image description here](https://github.com/anarinsk/lostineconomics-v2-1/blob/master/images/linear-transform/matrix_func_fig.png?raw=true){: style="margin: auto; display: block; border:1.5px solid #021a40;"}{: width="500"}

함수와 마찬가지로 위의 선형 변환에서 치역(Im($T$))와 커널(스칼라 함수에서는 $f(x) = 0$의 해)가 정의된다. 

$$
{\rm Im}(T) \overset{\rm def}{=} \{ \vec w \in W | \vec w = T(\vec v) \text{ for some } \vec v \} \subseteq W
$$

$$
{\rm Ker}(T) \overset{\rm def}{=} \{ \vec v \in V | T(\vec v) = \vec 0 \} \subseteq V
$$

## Matrix Representation 

인풋, 아웃풋의 기저 벡터를 다음과 같이 두자. 

$$
B_V = \{ \vec e_1, \dotsc, \vec e_n \}
$$

$$
B_W = \{ \vec b_1, \dotsc, \vec b_m \}
$$

- $M_T \in \mathbb R^{m \times n}$은 선형 변환 $T$의 매트릭스 표현이다. 
- 보다 정확하게 표현해보자. 

$$
\phantom{}_{B_W}[M]_{B_V}
$$

즉, $V$의 기저로 표현되는 인풋을 $W$의 기저로 표현되는 아웃풋으로 바꿔준다. 

## Linearity 

선형의 의미는 무엇일까? 기하적으로 선을 다룬다는 뜻이 아니다. 선형의 의미는 함수적인 의미다. 아래 그림을 보자. 

![enter image description here](https://github.com/anarinsk/lostineconomics-v2-1/blob/master/images/linear-transform/linearity.png?raw=true){: style="margin: auto; display: block; border:1.5px solid #021a40;"}{: width="500"}

즉, 

$$
T(\alpha_1 \vec v_1 + \alpha_2 \vec v_2) = \alpha_1 T(\vec v_1) + \alpha_2 T(\vec v_2) = \alpha_1 \vec w_1 + \alpha_2 \vec w_2 
$$ 

## Matrix as Linear Transformation

왜 매트릭스가 선형 변환을 나타낼 수 있는지를 좀 더 들여다보자. 

$$
\begin{aligned}
T(\vec v)  &=  T(v_1{\hat e_1} + \dotsb + v_n{\hat e_n} ) \\
& =  v_1 T(\hat e_1) + \dotsb+ v_n T(\hat e_n)  \\
& = 
\begin{bmatrix}
\vert & \vert & \vert \\
T(\hat e_1) & \cdots & T(\hat e_n) \\
\vert & \vert & \vert
\end{bmatrix} \vec v \\
& = M_T \vec v
\end{aligned}
$$

## Take This!  

### Mapping Spaces 

선형 변환을 다시 적어보자. $T: V \to W$ where $V \in \mathbb R^n$, $W \in \mathbb R^m$. 즉 이 변환은 $n$ 차원의 벡터를 $m$ 차원으로 바꿔주는 것이다. 따라서 변환의 투입의 차원을 생각해보자. $n$ 차원은 로우 공간이 생성하는 $\mathcal R (M_T)$와 널 공간으로 가는 $\mathcal N(M_T)$으로 나뉘게 된다. 그리고 이 공간은 서로 direct sum 관계다. 이를 요약하면 다음과 같다. 

$$
T: \mathcal R(M_T) \to \mathcal C(M_T),~  
T: \mathcal N(M_T) \to \{ \vec 0 \}
$$

즉 인풋 $\vec v \in \mathcal R(M_T)$이 하나씩 $\vec w \in \mathcal C(M_T)$로 일대일로 대응된다. 한편, $\vec v \in \mathcal N(M_T)$는 $\vec 0 \in W$으로 대응된다. 

### Surjective and Injective 

![enter image description here](https://github.com/anarinsk/lostineconomics-v2-1/blob/master/images/linear-transform/sur_inj.png?raw=true){: style="margin: auto; display: block; border:1.5px solid #021a40;"}{: width="500"}

함수에서 전사 함수와 단사 함수의 개념을 그대로 적용할 수 있다. 다만 단사 함수의 판단은 쉽다. 만일, $\vec v_1 \neq \vec v_2$이고 $\vec v_1, \vec v_2 \in \mathcal R(M_T)$라면 이는 선형 변환에 따라서 서로 다른 $\vec w$로 매핑된다. 따라서 만일 단사가 되려면, $\mathcal N(M_T) = \{ \vec 0 \}$가 성립하면 된다. 전사 함수의 정의는 통상적인 정의와 같다. 즉, ${\rm Im} (T) = W$이면 된다. 

이를 매트릭스의 맥락에서 다시 음미해보자. 만일 전사(surjective) 변환이 되려면 $n \geq m$이 성립해야 한다. 반면 단사(injective) 변환이 되려면 $n \leq m$이 되어야 한다. 따라서 전단사 변환이 되기 위한 조건은 $m=n$이다. 함수에서 역함수가 존재하려면 전단사 함수여야 한다. 선형 변환도 마찬가지다. 역행렬이 존재하기 위한 조건은 $m=n$이다. 
<!--stackedit_data:
eyJoaXN0b3J5IjpbNTY4MTU4OTcsLTM5NTY2MTQ2MV19
-->