---
layout: post
toc: false
comments: true
title: Following Gilbert Strang 2
description: 매트릭스 곱을 이해하는 4가지 방법 그리고 역행렬 
categories: [math, matrix-theory]

---


## Source 

[3. Multiplication and Inverse Matrices](https://www.youtube.com/watch?v=FX4C-JpTFgY)

## 4 Views of Matrix Multiplication 

### Standard 

$$
\underbrace{A}_{m \times n} \underbrace{B}_{n \times p} = C
$$

- $c_{ij}$는 뭘까? $A$의 $i$ 행 벡터와 $B$의 $j$ 열 벡터의 곱이다. 즉, 

$$
c_{ij} = \sum_{k}^n a_{ik} b_{kj}
$$

![]({{ site.baseurl }}/images/GS-LA/fig_1.png){: style="textalign:center; " width="500"}

## Combination of Columns of A 

$$
[A_1 \dotsc A_n] 
\begin{bmatrix}
b_{11} \\
\vdots \\
b_{n1}
\end{bmatrix} = b_{11} A_1 + \dotsc + b_{n1} A_n = C_1
$$

- 즉, 매트릭스 $A$의 열 벡터 $A_i$가 있을 때 이를 B의 각 열 벡터로 선형결합한 것이 $C$가 된다. 

$$
[A_1 \dotsc A_n] 
\begin{bmatrix}
b_{1i} \\
\vdots \\
b_{ni}
\end{bmatrix} = b_{1i} A_1 + \dotsc + b_{ni} A_n = \text{col } C_i, \text{for } i = 1, \dotsc, p
$$

![]({{ site.baseurl }}/images/GS-LA/fig_2.png){: style="textalign:center; " width="500"}

## Combination of Rows of B 

- 위의 해석과 거의 같다. 다만, $A$와 $B$의 역할을 바꾼 형태다. 즉, 

$$
[a_{i1} \dotsc a_{in}] 
\begin{bmatrix}
B_{1} \\
\vdots \\
B_{n}
\end{bmatrix} = a_{11} B_1 + \dotsc + a_{in} B_n = \text{row } C_i, \text{for } i = 1, \dotsc, p
$$

![]({{ site.baseurl }}/images/GS-LA/fig_3.png){: style="textalign:center; " width="500"}

## Summation of Matices 

- 가장 급진적인 형태? 흥미롭게 생각해볼 수 있는 것은 각 개별 행렬의 rank다. 각각은 모두 1이다. 

$$
[A_1 \dotsc A_n]
\begin{bmatrix}
B_{1} \\
\vdots \\
B_{n}
\end{bmatrix} = A_1 B_1 + \dotsb + A_n B_n
$$

![]({{ site.baseurl }}/images/GS-LA/fig_4.png){: style="textalign:center; " width="500"}

## Block Multiplication 

![]({{ site.baseurl }}/images/GS-LA/fig_5.png){: style="textalign:center; " width="500"}

## Inverse of Matrix 

- 정의상 보면, 정방행렬 $A$에 대해서 역행렬 $A^{-1}$은 

$$
A A^{-1} = I, \text{ also } A^{-1} A = I
$$

- 역행렬이 존재하는 정방행렬을 non-singluar or invertible matrices라고 부른다. 
	- singluar or non-invertible 

- 역행렬을 구하는 과정을 따져보자. 

$$
A [A^{-1}_1, \dotsc A^{-1}_n] = I 
$$

- $A^{-1}_i$는 역행렬 $A^-1$의 컬럼 벡터 
- 역행렬을 구하는 문제는 사실 $n$개의 연립방정식을 푸는 문제와 구조상 동일하다. 

### Another definition 

- $A x = 0$를 만족하는 $0$ 벡터가 아닌 벡터 $x$가 존재하면 singular matrices. 
- 증명은 간단하다. 
	- $A$의 역행렬이 존재하고, $x \neq 0$라고 하자.
	- $A^{-1} A x = A^{-1} 0$ &rarr; $Ix = 0$ 모순 

## Using Gauss Jordan For Inverse Matices 

$[A \vert I]$와 같은 형태의 augmented matrix를 만든 후, $A$를 $I$로 만드는 가우스-조르단 프로세스를 반복하면, $I$ 자리에 $A^{-1}$을 얻게 된다. 

### Example 

$$
[A|I] = 
\begin{bmatrix}
3 &-2 & 4 & \vert & 1 & 0 & 0\\ 
1 & 0 & 2 & \vert & 0 & 1 & 0\\
0 & 1 & 0 & \vert & 0 & 0 & 1\\
\end{bmatrix}
$$

* 첫번째 행과 두번째 행을 바꾼다. 

$$
[A|I] \sim
E_{12} [A|I] = 
\begin{bmatrix}
1 & 0 & 2 & \vert & 0 & 1 & 0 \\
3 &-2 & 4 & \vert & 1 & 0 & 0
\end{bmatrix}
0 & 1 & 0 & \vert & 0 & 0 & 1 \\

$$

* 

$$
[A|I] \sim
E_{23} [A|I] = 
\begin{bmatrix}
3 &-2 & 4 & \vert & 1 & 0 & 0\\ 
0 & 1 & 0 & \vert & 0 & 0 & 1\\
1 & 0 & 2 & \vert & 0 & 1 & 0
\end{bmatrix}
$$

![]({{ site.baseurl }}/images/GS-LA/fig_6.jpg){: style="textalign:center; " width="500"}
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTk4NjA3NDI1MCwtNDQyMjg0ODUyXX0=
-->