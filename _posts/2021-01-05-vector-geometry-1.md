---
layout: post
toc: false
comments: true
title:  Vector Geometry, part 1 
description: 벡터 공간의 기하적 특성 1
categories: [math, matrix-theory]

---

## Concepts 

편의상 3차원 공간을 예시하도록 한다. $n$ 차원으로 확장은 쉽게 된다. 

- $p = (p_x, p_y, p_z)$: $p \in \mathbb R^3$
- $\vec v = (v_x, v_y, v_z)$: $\vec v \in \mathbb R^3$
- $\hat v = \dfrac{v}{\lVert v \lVert}$: Unit vector 
- $p_0$를 지나는 무한의 1차원 선은 다음과 같이 정의된다. 
	- Parametric equation: 
	$$
	l : \{ (x, y, z) \in \mathbb R^3 | p_0 + t \vec v, t \in \mathbb R\}
	$$
	- Symetric equation: 
	$$
	l: \{  (x, y, z) \in \mathbb R^3 | x - \dfrac{p_{0x}}{v_x} = y - \dfrac{p_{0y}}{v_y}  = z - \dfrac{p_{0z}}{v_z} \}
	$$ 
- $p_0$를 지나는 무한 평면 P는 다음과 같이 정의된다. 
	- General equation
	$$
	P: \{ (x, y, z) \in \mathbb R^3 | Ax + By + Cz = D \}
	$$
	- Parametric equation
	$$
	P : \{ (x, y, z) \in \mathbb R^3 | p_0 + s \vec v + t \vec w, s, t \in \mathbb R \}
	$$
	- Geometric equation
	$$
	P : \{ (x, y, z) \in \mathbb R^3 | \vec n \cdot [(x,y,z) - p_0] = 0 \} \text{with normal vector $\vec n$}
	$$

### Point vs vector 

점과 벡터의 차이는 무엇일까? 점은 좌표계에서 원점을 기준으로 한 위치를 나타낸다. 반면, 벡터는 크기와 방향을 모두 나타낸다. 반면 벡터는 기준이 없다. 벡터를 점으로 나타내려면 어떻게 해야 할까? 두 점의 차이를 구하면 이는 벡터가 된다. 쉽게 생각해보자. 2차원 데카르트 좌표계에서 $(3,3)$이라는 위치를 생각해보자. $(3,3)$을 그냥 나타내면 점이다. 반면, $(3,3) - (0,0)$을 의미하면 $(3,3)$은 벡터가 된다. 그래프 상에서 $(0,0)$에서 $(3,3)$으로 화살표를 그리면 벡터가 된다. 



### Normal vector 

노멀 벡터는 아래 플레인 혹은 벡터과 직교하는 성분의 벡터다. 이 녀석을 어떻게 구할까? 플레인의 파라메트릭 식을 보자. 2차원 플레인을 구성하는 두 벡터 $\vec v$, $\vec w$가 있다. 노멀 벡터는 이 두 성분 모두와 직교하는 성분의 벡터이다. 이는 바로 크로스 프로덕트의 정의를 그대로 따른다. 평면위의 세 점 p, q, r이 임의로 있다고 하자. 이를 $\vec v = q - p$, $\vec w = r - p$라고 하자. 

$$
\vec n  = \vec v \times \vec w = (q-p) \times (r-p)
$$

## Distance 

거리 역시 세 가지로 나누어 이해해보자. 점과 점 사이의 유클리드 거리는 생략하겠다. 벡터와 점의 거리는 어떻게 구할까? 플레인과 점의 거리는 어떻게 구할까? [이 포스팅](https://anarinsk.github.io/lostineconomics-v2-1/math/matrix-theory/2020/12/16/Projection.html)을 참고하라. 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0MDk0NTA3OCwxOTA2NzEwODYwLDY0Mj
Y4MzIxNV19
-->