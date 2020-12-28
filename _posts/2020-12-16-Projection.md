---
layout: post
toc: false
comments: true
title:  Projection and Distance 
description: 프로젝션과 거리 측정에 관해 알아보자.
categories: [math, matrix-theory]

---

## Projection: Scalar and Vector  

### Definition 

다음과 같은 두 개의 벡터, $\mathbf a$, $\mathbf b$를 일단 떠올려보자. 

![enter image description here](https://upload.wikimedia.org/wikipedia/commons/9/98/Projection_and_rejection.png){: style="margin: auto; display: block; border:1.5px solid #021a40;"}{: width="300"}

스칼라 프로젝션 $a_1$의 정의는 다음과 같다. 

$$
a_1 ={\cos \theta}{\lVert \mathbf a \lVert} = \lVert \mathbf a_1 \lVert 
$$

각 $\theta$에 관해서 다음과 같이 정의할 수 있다. 혹시 리마인드가 필요하면 포스팅 [dot product](https://anarinsk.github.io/lostineconomics-v2-1/math/2019/07/18/dot-product.html)를 참고하라. 

$$
\cos \theta = \dfrac{\mathbf a \cdot \mathbf b}{\lVert\mathbf a\lVert \lVert\mathbf b\lVert}
$$

따라서, $\hat\mathbf b = \dfrac{\mathbf b}{\lVert\mathbf b\lVert}$라고 할 때 

$$
a_1 = {\cos \theta}{\lVert \mathbf a \lVert} = \dfrac{\mathbf a \cdot \mathbf b}{\lVert\mathbf b\lVert} = \mathbf a \cdot \hat\mathbf b
$$

쉽게 말해서, $\mathbf a$ 벡터와 정규화된 $\mathbf b$의 닷프로덕트라고 생각하면 된다. 

### Vector projection 

스칼라 프로젝션의 크기로 $\mathbf b$의 벡터를 만든 것이 벡터 프로젝션이다. $\mathbf a$를 $\mathbf b$ 위로 프로젝션한 벡터 $\Pi_{\mathbf b} (\mathbf a)$는 다음과 같다. 

$$
\mathbf a_1 = \Pi_{\mathbf b} (\mathbf a) = a_1 \hat\mathbf b = \dfrac{\mathbf a \cdot \mathbf b}{\lVert\mathbf b\lVert} \dfrac{\mathbf b}{\lVert\mathbf b\lVert}
$$

말로 풀어보자. $\mathbf b$와 같은 방향성을 지니는 벡터를 $\mathbf a$와 $\mathbf b$ 간의 스칼라 프로젝션의 크기로 만들어주는 것이 벡터 프로젝션이다. 

## More Definition 

- $S \subseteq \mathbb R^n$: $S$는 벡터 부분공간이라고 하자. 
- $S^\perp$: $S$와 직교 벡터들의 집합이고 이 역시 벡터 부분공간이다. 
$$
S^\perp = \{ \mathbf w \in \mathbb R^n : w \cdot S = 0 \} 
$$
- $\Pi_S$: 부분공간 $S$ 위로 프로젝션 
- $\Pi_{S^\perp}$: 부분공간 $S^\perp$ 위로 프로젝션

$\Pi_S$는 일종의 함수로서 다음과 같이 정의된다. 

$$
\Pi_S: \mathbb R^n \to S
$$

 $\mathbf x \in \mathbb R^n$의 어떤 벡터가 $\Pi_S$를 거치면 $S$에 속하지 않는 나머지 부분은 사라지게 된다. 즉, $S$라는 화면 위로 자신의 이미지를 투영한다고 보면 된다. 그래서 프로젝션이다. 

몇가지 특징은 다음과 같다. 

- If ${\mathbf v} \in S$, then $\Pi_S(\mathbf v) = \mathbf v$
- If $\mathbf w \in S$, then $\Pi_S(\mathbf w) = \mathbf 0$
- if $\mathbf u = \alpha \mathbf v + \beta \mathbf w$ where $\mathbf v \in S$, $\mathbf w \in S^{\perp}$, then $\Pi_S(\mathbf u) = \alpha \mathbf v$
- $\Pi_S(\mathbf v) (\Pi_S(\mathbf v) ) = \Pi_S(\mathbf v)$ (idempotent) 

### Projection onto line 

![enter image description here](https://github.com/anarinsk/lostineconomics-v2-1/blob/master/images/projection/vector_proj.png?raw=true){: style="margin: auto; display: block; border:1.5px solid #021a40;"}{: width="500"}

그림에서 $\mathbf u$의 $\mathbf v$로의 프로젝션은 앞서 살펴본 벡터 프로젝션이다. $\mathbf v$ 대신 $l$로 표기된 점에 유의하자. 

$$
l: \{ \mathbf v \in \mathbb R^n \lvert \mathbf v = \mathbf 0 + t \mathbf v, t \in \mathbb R \}
$$

$$
\Pi_l(\mathbf u) = \dfrac{\mathbf u \cdot \mathbf v}{\lVert\mathbf v\lVert^2} \mathbf v
$$

이제 여기서 $\Pi_{l^{\perp}}$를 구해보자. 

$$
\mathbf u = \Pi_{l} + \Pi_{l^{\perp}}
$$

따라서 둘 사이의 거리는 $\lVert\Pi_{l^\perp}\lVert$가 된다. 즉, 

$$
\lVert \mathbf u - \dfrac{\mathbf u \cdot \mathbf v}{\lVert\mathbf v\lVert^2} \mathbf v\lVert
$$

### Projection onto plane 

![enter image description here](https://github.com/anarinsk/lostineconomics-v2-1/blob/master/images/projection/vector_proj_2.png?raw=true){: style="margin: auto; display: block; border:1.5px solid #021a40;"}{: width="500"}

이제 평면 위로의 프로젝션을 생각해보자. 기본적인 원리는 동일하다. 이에 앞서 평면을 벡터로 표현하는 방법에 대해 알아보자. 가장 편리한 방법은 노멀 벡터를 활용하는 것이다. 즉, 평면 위의 점 $\mathbf {p_0}$를 지나는 $P$는 다음과 같이 정의할 수 있다. 

$$
P = \{ \mathbf p \in \mathbb R^n : \mathbf n \cdot(\mathbf p - \mathbf p_0) = 0 \}
$$

여기서 노멀 벡터 $\mathbf n$은 프로젝션된 지점에서 평면과 직교하는 성분을 나타낸다. 3차원의 경우 노멜 벡터는 3차원 공간에서 해당 평면이 놓인 모습을 결정한다. 

$$
\mathbf u = \Pi_P(\mathbf u) + \Pi_{P^\perp}(\mathbf u)
$$

그림에서 보듯이 $\mathbf u$에서 $\mathbf n$으로의 프로젝션은 앞서 보았던 벡터에서 벡터로의 프로젝션이다. 따라서 앞서 구한 공식을 그대로 활용하자.  즉,  

$$
\Pi_{P^\perp}(\mathbf u) = \dfrac{\mathbf u \cdot \mathbf n}{\lVert\mathbf n\lVert}\dfrac{\mathbf n}{\lVert \mathbf n \lVert}
$$

이를 그대로 대입하면 $\Pi_{P}$를 쉽게 구할 수 있다. 즉, 

$$
\Pi_P(\mathbf u)  = \mathbf u - \dfrac{\mathbf u \cdot \mathbf n}{\lVert\mathbf n\lVert^2}{\mathbf n}
$$

## Distance in Vector Space 

프로젝션은 거리를 측정할 때 유용하다. 먼저 원점(굳이 원점일 필요는 없다)과 해당 벡터 공간을 지나는 선 사이의 거리를 구해보자. 

$$
l: \{ \mathbf p \in \mathbb R^n | \mathbf p = \mathbf{p}_0 + t \mathbf v, t \in \mathbb R \}
$$

![enter image description here](https://github.com/anarinsk/lostineconomics-v2-1/blob/master/images/projection/line.png?raw=true){: style="margin: auto; display: block; border:1.5px solid #021a40;"}{: width="500"}

위 그림에서 원점과 $l$의 거리는 어떻게 나타낼 수 있을까? $l$이 지나가는 $\mathbf p_0$를 그대로 두고, $l$이 원점을 지나가도록 이동해보자. 즉, 

$$
l_0 = \{  \mathbf p \in \mathbb R^n | \mathbf p = \mathbf{0} + t \mathbf v, t \in \mathbb R \}
$$

이제 이 직선과 $\mathbf p_0$ 사이의 거리를 구하면 된다 .이는 앞서 제시한 벡터 프로젝션의 수직 벡터의 길이와 같다. 즉, 

$$
d(l, \mathbf 0) = d(\mathbf p_0, l_0) = \lVert \mathbf p_0 - \dfrac{\mathbf p_0 \cdot \mathbf v}{\lVert \mathbf v\lVert^2}\mathbf v \lVert
$$

이제 아래와 같은 평면과 원점의 거리를 측정해보자. 

$$
P = \{ \mathbf p \in \mathbb R^n : \mathbf n \cdot(\mathbf p - \mathbf {p}_0) = 0 \}
$$

![enter image description here](https://github.com/anarinsk/lostineconomics-v2-1/blob/master/images/projection/plane.png?raw=true){: style="margin: auto; display: block; border:1.5px solid #021a40;"}{: width="500"}

원점을 지나도록 평면을 이동시키고 평면의 노멀 벡터가 정의된 $p_0$와 원점을 지나는 평면 $P_0$ 사이의 거리를 측정하면 된다. 

$$
P_0 = \{ \mathbf p \in \mathbb R^n : \mathbf n \cdot(\mathbf p - \mathbf {0}) = 0 \}
$$

이 둘 사이의 거리는 $\Pi_{P^\perp}$의 거리와 같다. 즉, 

$$
d(P, \mathbf 0) = d(P_0, \mathbf p_0) = \lVert  \dfrac{\mathbf p_0 \cdot \mathbf n}{\lVert\mathbf n\lVert}\dfrac{\mathbf n}{\lVert \mathbf n \lVert}\lVert = \dfrac{|\mathbf p_0 \cdot \mathbf n |}{\lVert\mathbf n\lVert}
$$

다시 확인해보자. 노멀 벡터 $\mathbf n$은 $\mathbf p_0$와 같은 같은 방향에 있는 벡터다. 따라서 위의 식이 성립한다. 

## Application: Regression Coefficient 

![enter image description here](https://github.com/anarinsk/lie-regression/blob/master/assets/imgs/reg-in-vectorspace.png?raw=true){: style="margin: auto; display: block; border:1.5px solid #021a40;"}{: width="500"}

앞서 소개했던 회귀 분석에 관한 포스팅 [Understanding Regression](https://anarinsk.github.io/lostineconomics-v2-1/math/econometrics/regression/2019/10/25/understanding-regression.html)을 다시 보자. Origin--Observed Y--Fitted $\hat Y$이 만드는 삼각형을 보자. 직각 삼각형이다. 여기서 잔차에 해당하는 $e$는 $X$의 컬럼 스페이스와 항상 직교한다. 즉, $X' e = 0$이 성립한다. 그리고 $\hat Y = X \hat\beta$이므로 

$$
X'(Y - \hat Y)  = X'(Y - X \hat\beta) = 0
$$

이를 노멀 방정식이라고 부른다. 앞서 평면과 직교하는 벡터를 노멀 벡터라고 불렀는데, 둘은 일맥상통한다.

## References 

- Ivan Savov, *No bullshit guide to linear algebra 2nd Edition*, Minireference, 2017 
  
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjM4Mzg3MzY5LC0zMDgzOTUyMjcsLTE2Nz
YwMjM0MywxMTYyMjQ4MjA3LDE1MjMxMDE3OTQsLTQ2NTAzNTQ2
NywtMTA5Mjg2ODUzNiw3MzgyNTM2ODUsLTI5MjMxMjU0NiwtMT
IxMjMxMDMsMjQ3MjIxNzM3LC0xNDYzNDQ0MDY5LC01NjEzNTk0
NzYsMTQxMDIxMjYyMywtMTQ1OTU3Njc4OSwxOTc4NzkwOTcwLD
MwNTM4NzQyNV19
-->