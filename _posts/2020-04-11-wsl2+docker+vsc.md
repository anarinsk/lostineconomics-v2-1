---
layout: post
toc: false
comments: true
title:  wsl 2 + docker + vsc 
description:  커널은 리눅스에 맡기시고, 코딩은 윈도에서 편안하게
categories: [wsl, visual-studio-code, docker]

---


## Assumptions 

- tensorflow = tf 
- visual studio code = vsc 
- windows subsystem for linux version 2 = wsl 2 

## Mission 

- 한마디로 이중 원격 접속이다. 즉 wsl 2 안에 설치한 리눅스(1단계 원격 접속) 안에서 돌아가는 도커 컨테이너(2단계 원격 접속)에 vsc로 접근하기. 


## First Things First 

- wsl 2를 세팅하자. 
- docker desktop edge 버전 2.0 이상을 깔자. [여기](https://docs.docker.com/docker-for-windows/wsl-tech-preview/)를 참고하자. 
- [vsc insider](https://code.visualstudio.com/insiders/)를 깔자. 

앞 두 개는 [여기](https://anarinsk.github.io/lostineconomics-v2-1/wsl/2020/04/09/wsl2-walkthru.html)를 참고하자. vsc insider의 경우 기존에 stable 버전이 깔려 있더라도 상관 없다. 아이콘에서 알 수 있듯, 둘은 다른 앱이다. 

## Pulling docker image 

- 이제 wsl 2를 통해 Ubuntu로 진입한다. 
- 도커가 제대로 설치되어 있다면 이미지들을 Ubuntu로 끌어올 수 있을 것이다. 시험 삼아서 tensorflow의 이미지를 끌어와보자. 

```shell
~$ docker run -it --rm tensorflow/tensorflow:latest-py3
```
- 이제 `wsl 2` - Ubuntu - docker" 체제 아래 tf 컨테이너를 돌리는 데 성공했다! 사실 tensorflow 설정이 조금 까다로울 수 있기 때문에 이렇게 쓰는 편이 편할 수도 있다. 

## Extensions for vsc 

- 우리의 목적은 wsl 2 아래 설치된 Linux 안에서 도는 docker container에 vsc로 접근해 편안한 환경에서 코딩하는 것이다. 그런데 생각해보면 곤란한 점이 있다. 
- vsc는 윈도에서 돌고 있다. 따라서 wsl 2를 통해 그 안에 깔린 Ubuntu에 '원격'으로 접속하는 개념이다. 그래서 관련된 vsc 익스텐션 이름도 "Remote WSL"이다. 그런데 그 곳에 설치된 도커 안에 있는 컨테이너에 접속하려면 원격-원격 접속이 되는 셈이다. 이게 돼? 
- 이 포스팅을 작성하는 시점에서 vsc 정식(stable) 버전은 이러한 이중 원격 접속을 허용하지 않았다. 혹시 해서 insider 버전, 즉 실험적인 기능을 구현한 둔 버전을 깔았더니 이 이중 원격 접속이 잘 되더라.  

![]({{ site.baseurl }}/images/wsldockervsc/fig_1.png){: style="textalign:center; " width="800"}

-  Extensions에서 remote를 검색하면 `Remote Development`를 찾을 수 있다. 이 녀석은 `Remote WSL`, `Remote SSH`, `Remote Container` 세 개가 깔린다. 셋 다 유용한 extension이니 설치하도록 하자. 
- docker extension은 깔아도 좋고 안 깔아도 좋지만 docker를 쓸 거라면 깔아두도록 하자. 
- Extensions 중에서 remote explorer로 간 후 "container"를 선택하면 현재 돌아가고 있는 docker container를 확인할 수 있다.

![]({{ site.baseurl }}/images/wsldockervsc/fig_2.png){: style="textalign:center; " width="400"}
 
- 이 컨테이너를 vsc에 붙이면(attach) 끝이다! 허무하게 간단하다. 

![]({{ site.baseurl }}/images/wsldockervsc/fig_3.png){: style="textalign:center; " width="400"}
 
- 부착과 함께 별도의 창이 뜨면서 아래 그림처럼 윈도 앱 창 하단에서  컨테이너로의 이중 원격 접속이 잘 이루어졌음을 확인할 수 있다. 

![]({{ site.baseurl }}/images/wsldockervsc/fig_4.png){: style="textalign:center; " width="400"}

## For what? 

- docker의 용도가 그렇지만 필요한 컨테이너를 끌어다가 마음껏 활용하고 문제가 생기면 버리면 된다. 도커를 쓴다면 사실 python 가상 환경조차 필요 없다. 필요한 패키지가 깔린 파이썬 환경을 만들어두고 도커로 끌어와 쓰면 그만이다. 
- vsc가 윈도에서 돌기 때문에 작업중인 파일 등은 docker container 내에 두지 않아도 된다. 유연하다!
- 재현 가능성은 연구에서만 문제가 되는 것이 아니다. 소프트웨어 개발에서 상호 의존성 때문에 구현된 환경을 관리하는 것이 못지 않게 중요하다. 도커는 이런 점에서 거의 완벽한 솔루션이다.[^1]

[^1]: 사족을 달자면 데이터 사이언스 그리고 컴퓨터 작업이 중요해지는 분에서 도커의 활용은 학술과 산업 모두에서 필수적이라고 생각한다.  

## Making Pandas Docker 

- 이제까지 살펴본 내용을 바탕으로 아래의 요소를 지닌 도커를 만들어 보자. 
	- ubuntu 18.04 
	- python 3.7 
	- panas 1.0.3
- 먼저 기본이 될만한 컨테이너를 끌어와야 한다. 필요한 컨테이너는 Docker Hub에서 찾을 수 있다. 그리고 자신의 이미지를 관리하고 활용하고 싶다면, Docker Hub에도 가입해두도록 하자. 

### Pull ubuntu 18.04 

[여기](https://hub.docker.com/_/ubuntu?tab=tags)를 참고해서 필요한 컨테이너 이미지를 끌어오자. 내 경우는 18.04를 가져왔다. 

```shell
~$ docker pull ubuntu:18.04
```

이렇게 끌어온 이미지를 컨테이너에 태워 실행시켜보도록 하자. 

```shell
~$ docker run -it |image-id-of-container|
```

이제 ubuntu 18.04 위에 ubuntu 18.04가 올라갔다! 재미있는 일이 아닌가! 

- Ubuntu를 깔았을 때 필요한 기본적인 작업을 하고, 파이썬을 깔고 필요한 패키지를 깐다. 기본적으로 local ubuntu에서 했던 일을 동일하게 해주면 된다. 단 `sudo`는 붙일 필요가 없다. ubuntu 이미지 자체가 root로 태워져 있으므로 모든 명령어는 기본적으로 sudo가 붙어 있다고 보면 되겠다. 

### How to Commit / Tag / Push Image 

- 이렇게 별도의 작업을 거친 컨테이너는 애초에 끌고 왔던 이미지와는 달라졌을 것이다. 그런데 도커의 속성상 콘테이너를 내리게 되면 변경했던 내용들은 다 사라진다. 이렇게 변경된 콘테이너를 저장할 수 있어야 한다. 

```shell
~$ docker ps 
```

![]({{ site.baseurl }}/images/wsldockervsc/fig_5.png){: style="textalign:center; " width="700"}

```shell
~$ docker commit |container-name or id-of-container| |name-of-image|
```

- 이제 저장된 이미지를 확인해보자. 

```shell
~$ docker images 
```

- 이렇게 확인된 이미지 중에서 docker hub로 보내고 싶은 이미지에 태그를 걸어준다. 

```shell
~$ docker tag |image-id| |id-of-docker-hub/name-of-docker-hub-image| 
```

- 다시 이미지를 조회해보면 `|id-of-docker-hub/name-of-docker-hub-image|` 이미지가 생성되었을 것이다. 이제 이 이미지를 도커 허브로 푸시하면 된다. 

```shell
~$ docker push |id-of-docker-hub/name-of-docker-hub-image|
```

- 이미지의 상태가 지저분하다고 느끼면 다 지우고 다시 pull 하면 된다. 이미지를 모두 삭제하려면, 아래와 같이 실행하자. 

```shell
~$ docker rmi -f $(docker images -a -q)

```

- 기본적인 Ubuntu update와 python3.7 그리고 pandas, matplotlib가 설치된 이미지를 당겨와 실행해보자. 

```shell
~$ docker run -it --rm anarinsk/py37-pandas 
```

- 위에서 설명한대로 vsc를 통해 작업하면 된다. 안에  testcode.py라는 간략한 실험 코드를 넣어 두었다. 

![]({{ site.baseurl }}/images/wsldockervsc/fig_6.png){: style="textalign:center; " width="700"}
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEwMzkxMjY3ODYsMTc4OTQwNzc0NCwtMT
gyMDI4Mjc0Niw3Nzc1Nzk1OTYsNjUzNTczMjIwLDU5MTU5ODU0
OCwtMTI5NDkxNTQ5NCwxMjA2MDEyNzk2LDM5ODI1NDc3NiwtNT
YzMzU5MjQxLDE5MzM2OTM3NTIsLTY0NTY2ODM3NCwtMjk0NTc0
Mzk0LDE5NTY0ODM3MjMsNjcwNzcyNDIxLDE1NDc5ODU1ODUsLT
cxMDY1Mzc3NywxNTkzMjIwNzUyLC0yMTYzNzI4MjBdfQ==
-->