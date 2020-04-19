---
layout: post
toc: false
comments: true
title:  Metrics for Binary Classification 
description:  이항 분류 지표, 함 정리하고 가즈아~ 
categories: [machine-learning, basics]

---

## Mission 

- 자꾸 까먹어서 기회가 될 때 한번 정리해두고자 한다. 

## Basic to Remember 

- 로(행)는 예측치를 컬럼(열)은 실제 속성을 나타낸다. 
- 이에 따라서 4개의 컨퓨전 매트릭스 행렬이 생긴다. 


## Basic Three 

![]({{ site.baseurl }}/images/clf-metrics/fig_1.png){: style="textalign:center; " width="400"}

![]({{ site.baseurl }}/images/clf-metrics/fig_2.png){: style="textalign:center; " width="400"}

![]({{ site.baseurl }}/images/clf-metrics/fig_3.png){: style="textalign:center; " width="400"}

## Term 

헛갈리니 용어를 정리하고 가보자. 

<STYLE TYPE="text/css">  
table {
	font-size: 110%;
	width: 85%; 
	margin: auto; 
	}  
</STYLE>  
 
|True Positive Rate (TPR) | False Positive Rate (FPR)|
|:--:|:--:|
|Recall | |
|Sensitivity | (1 - Specificity) | 
|$\frac{\rm\bold{TP}}{\rm\bold{TP + FN}}$| $\frac{\rm\bold{FP}}{\rm\bold{FP + TN}}$|
|$1-\alpha$ | $\beta$ |
| (1 - type I error) | type II error |

* Specificity는 True Negative Rate를 나타내는 용어다. 

## ROC curve 

- ROC 커브에 이름에 신경쓰지 말자. 어차피 이상한 이름이니까. 
- ROC 커브는 무엇에 쓰는 것일까? binary classification에서는 추정된 확률에 대해서 임계 수준을 어디에 둘지에 따라서 해당 결과에 관한 판정을 다르게 내릴 수 있다. 이를 TPR와 FPR를 $x-y$ 평면 위에서 표시한 것이다. 이때 TPR는 1이 가장 좋고 여기 가까울수록 좋다. 반대로 FPR는 0에 가까울수록 좋다. 따라서, $x$ 축에 FPR를, $y$ 축에 TPR를 둔다면, $(0,1)$로 갈수록 좋고, $(1,0)$으로 갈수록 나쁜 점이 된다. 

![]({{ site.baseurl }}/images/clf-metrics/fig_4.png){: style="textalign:center; " width="600"}

- 경제학에서 쓰는 dominance 개념과 비슷하다. 만일 모든 TPR-FPR 조합에서 어떰 모델의 커브가 다른 모델의 커브보다 $(0,1)$에 근접해 있다면 해당 모델이 우월하다. 만일 둘이 겹치는 영역이 있다면 어떨까? 
- 이때 비교를 위해 개발된 지표가 AUC(Area Under Curve)이다. 즉, ROC의 아래 면적의 크기를 구하는 것이다. 설명력이 없는 경우 즉, $(0,0)-(1,1)$ 선의 경우 ROC는 1/2이다. 설명력이 높을수록 1에 가까운 값을 갖게 된다. 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQ0MDI5MjQ1OSwtMTQ2NzEyMjI4OCwtNz
IyNjkxNDI5LC0xOTUzNjU2Njg1LC02ODI2OTAwODgsODA5NTc3
ODMyLC05Nzg1MjQ3MSwtMTk4NjE3MDQ5NCw2NjQ2MDg2NTIsLT
EzNTc1MTcxNjhdfQ==
-->