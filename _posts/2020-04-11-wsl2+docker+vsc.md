---
layout: post
toc: false
comments: true
title:  wsl 2 + docker + vsc 
description:  코딩은 편안하게 해야겠죠. 
categories: [wsl, visual-studio-code, docker]

---


## Assumptions 

- tensorflow = tf 
- visual studio code = vsc 
- windows subsystem for linux version 2 = wsl 2 

## Mission 

- tf를 wsl 2에서 linux로 돌려보자. 
- tf를 docker로 돌려보자. 
- tf를 visual studio code(vsc) 위에서 돌려보자. 

## First Things First 

- wsl 2를 세팅하자. 
- docker desktop edge 버전 2.0 이상이 깔려 있어야 한다.
- vsc insider를 깔자. 

## Pulling docker image 

- 이제 wsl 2를 통해 Ubuntu로 진입한다. 
- 도커가 제대로 설치되어 있다면 이미지들을 Ubuntu로 끌어올 수 있을 것이다. 시험 삼아서 tensorflow의 이미지를 끌어와보자. 

```shell
~$ docker run -it --rm tensorflow/tensorflow:latest-py3
```
- 이제 wsl 2 - Ubuntu - docker 체제 아래 tf 컨테이너를 돌리는 데 성공했다!

## Extensions

- 최종 목적은 태워진 docker container에 vsc로 접근해 편안한 환경에서 코딩하는 것이다. 그런데 생각해보면 곤란한 점이 있다. vsc는 윈도에서 돌고 있다. 따라서 wsl 2를 통해 그 안에 깔린 Ubuntu에 원격으로 접속하는 개념이다. 그런데 그 위에 깔린 도커 안에 있는 컨테이너에 접속하려면 원격-원격 접속이 되는 셈이다. 이게 될까? 
- vsc 정식 버전에서는 되지 않았다. 혹시나해서 insder 버전, 즉 실험적인 기능을 조금 더 구현해 둔 버전을 깔아 보니 잘 되더라. 

![]({{ site.baseurl }}/images/wsldockervsc/fig_1.png){: style="textalign:center; " width="600"}

-  Extensions에서 remote를 검색하면 `Remote Development`를 찾을 수 있다. 이 녀석은 `Remote WSL`, `Remote SSH`, `Remote Container` 세 개가 깔린다. 셋 다 유용한 extension이니 설치하도록 하자. 
	- docker extension은 깔아도 좋고 안 깔아도 좋지만 docker를 쓸 거라면 깔아두도록 하자. 

- Extensions 중에서 remote explorer로 간 후 "container"를 선택하면 현재 돌아가고 있는 docker container를 확인할 수 있다.

![]({{ site.baseurl }}/images/wsldockervsc/fig_1.png){: style="textalign:center; " width="400"}
 
- 이를 vsc에 어태치하면 끝이다. 

![]({{ site.baseurl }}/images/wsldockervsc/fig_3.png){: style="textalign:center; " width="400"}
 
- 어태치와 함께 별도의 창이 뜨면서 아래 그림처럼 컨테이너로서의 원격 접속이 잘 이루어졌음을 확인할 수 있다. 

![]({{ site.baseurl }}/images/wsldockervsc/fig_4.png){: style="textalign:center; " width="400"}

## For what? 

- docker의 용도가 그렇지만 필요한 컨테이너를 끌어다가 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3MzE0OTEzNDUsLTIxNjM3MjgyMF19
-->