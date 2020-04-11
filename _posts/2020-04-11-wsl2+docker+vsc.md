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

앞 두 개는 [여기](https://anarinsk.github.io/lostineconomics-v2-1/wsl/2020/04/09/wsl2-walkthru.html)를 참고하고, vsc insider는 [여기](https://code.visualstudio.com/insiders/)서 다운받아서 설치하자. 

## Pulling docker image 

- 이제 wsl 2를 통해 Ubuntu로 진입한다. 
- 도커가 제대로 설치되어 있다면 이미지들을 Ubuntu로 끌어올 수 있을 것이다. 시험 삼아서 tensorflow의 이미지를 끌어와보자. 

```shell
~$ docker run -it --rm tensorflow/tensorflow:latest-py3
```
- 이제 wsl 2 - Ubuntu - docker 체제 아래 tf 컨테이너를 돌리는 데 성공했다!

## Extensions

- 우리의 목적은 wsl 2 아래에서 도는 docker container에 vsc로 접근해 편안한 환경에서 코딩하는 것이다. 그런데 생각해보면 곤란한 점이 있다. 
- vsc는 윈도에서 돌고 있다. 따라서 wsl 2를 통해 그 안에 깔린 Ubuntu에 원격으로 접속하는 개념이다. 그런데 그 위에 깔린 도커 안에 있는 컨테이너에 접속하려면 원격-원격 접속이 되는 셈이다. 이게 될까? 
- vsc 정식(stable) 버전에서는 이 원격-원격 접속이 구현되지 않는다. 혹시 해서 insder 버전, 즉 실험적인 기능을 구현한 둔 버전을 깔아 보니 잘 되더라.  

![]({{ site.baseurl }}/images/wsldockervsc/fig_1.png){: style="textalign:center; " width="800"}

-  Extensions에서 remote를 검색하면 `Remote Development`를 찾을 수 있다. 이 녀석은 `Remote WSL`, `Remote SSH`, `Remote Container` 세 개가 깔린다. 셋 다 유용한 extension이니 설치하도록 하자. 
- docker extension은 깔아도 좋고 안 깔아도 좋지만 docker를 쓸 거라면 깔아두도록 하자. 
- Extensions 중에서 remote explorer로 간 후 "container"를 선택하면 현재 돌아가고 있는 docker container를 확인할 수 있다.

![]({{ site.baseurl }}/images/wsldockervsc/fig_2.png){: style="textalign:center; " width="400"}
 
- 이 컨테이너를 vsc에 붙이면(attach) 끝이다. 

![]({{ site.baseurl }}/images/wsldockervsc/fig_3.png){: style="textalign:center; " width="400"}
 
- 어태치와 함께 별도의 창이 뜨면서 아래 그림처럼 컨테이너로서의 원격 접속이 잘 이루어졌음을 확인할 수 있다. 

![]({{ site.baseurl }}/images/wsldockervsc/fig_4.png){: style="textalign:center; " width="400"}

## For what? 

- docker의 용도가 그렇지만 필요한 컨테이너를 끌어다가 마음껏 활용하고 문제가 생기면 버리면 된다. 이렇게 쓴다면 python 가상 환경조차도 필요 없게 된다. 필요한 패키지가 깔린 파이썬 환경을 만들어두고 도커로 끌어와 쓰면 그만인 셈이다. 
- vsc가 윈도에서 돌기 때문에 작업중인 파일 등은 docker container 내에 두지 않아도 된다. 유연하다! 

## Making Pandas Docker 

- 이제까지 살펴본 내용을 바탕으로 아래의 요소를 지닌 도커를 만들어 보자. 
	- ubuntu 18.04 
	- python 3.7 
	- panas
- 먼저 기본이 될만한 컨테이너를 끌어와야 한다. 필요한 컨테이너는 Docker Hub에서 찾을 수 있다. 그리고 자신의 독자적인 이미지를 관리하고 싶다면 Docker Hub에도 가입해두도록 하자. 

### Pull ubuntu 18.04 

[여기](https://hub.docker.com/_/ubuntu?tab=tags)를 참고해서 필요한 컨테이너 이미지를 끌어오자. 내 경우는 18.04를 가져왔다. 

```shell
~$ docker pull ubuntu:18.04
```

이렇게 끌어온 이미지를 컨테이너에 태워 실행시켜보도록 하자. 

```shell
~$ docker run -it [image-id-of-container]
```

이제 ubuntu 18.04 위에 ubuntu 18.04가 올라갔다! 재미있는 일이 아닌가! 

- Ubuntu를 깔았을 때 필요한 기본적인 작업을 하고, 파이썬을 깔고 필요한 패키지를 깐다. 기본적으로 local ubuntu에서 했던 일을 동일하게 해주면 된다. 단 `sudo`는 붙일 필요가 없다. ubuntu 이미지 자체가 root로 태워져 있으므로 모든 명령어는 기본적으로 sudo가 붙어 있다고 보면 되겠다. 



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTI5NDU3NDM5NCwxOTU2NDgzNzIzLDY3MD
c3MjQyMSwxNTQ3OTg1NTg1LC03MTA2NTM3NzcsMTU5MzIyMDc1
MiwtMjE2MzcyODIwXX0=
-->