---
layout: post
toc: false
comments: true
title: Setting python venv 
description:  파이썬 가상 환경 설정하기
categories: [coding-tool, python, venv]

---

## Local Ubuntu 

* venv 디렉토리를 만든다. 

```shell
mkdir /.pyvenv 
```

* `sudo` 명령어를 쓰지 말라. 그러면 디렉토리의 권한이 root에 귀속된다. 
* venv 디렉토리로 이동 

```shell
cd ~/.pyvenv 
```

* 해당 디렉토리에서 환경을 만든다. 

```shell
[sudo] python -m venv |your-venv|
```

![]({{ site.baseurl }}/images/python-venv/fig_1.png){: style="textalign:center; " width="800"}

- 여기서 중요한 점! 위 그림을 보자. 만일 `sudo` 없이 가상 환경을 만들었다면 해당 폴더는 현재 로그인한 사용자에게 귀속된다. 그런데 `sudo`로 만들었다면 이는 root에 귀속된다. 이에 따라서 생성된 가상 환경의 사용 조건 및 운용이 달라지게 된다. `sudo`는 자제하자. 

- VS Code같은 외부 앱과 연동해서 사용하려면 root에 귀결되면 문제가 생긴다. VS Code가 자신이 필요한 python 패키지들을 설치할 수 없게 된다. 특별한 이유가 없다면, 아래와 같이 폴더 권한을 유저에게 돌려 두자.[^1]

[^1]: 권한 관리에 관해서는 [여기](https://eunguru.tistory.com/93)를 참고하라. 

```shell
sudo chown |user|:|user| -R |your-venv|
```

* 해당 venv를 활성화한다. 

```shell
source ~/.pyvenv/|your-venv|/bin/activate 
```

* 앞에서 해당 가상 환경 폴더의 소유권을 바꾸었다면 그냥 일반적인 방법으로 `pip`를 업그레이드하면 된다. 

```shell
(|your-venv|) pip install --upgrade pip 
```

* 소유권을 변경하지 않았다면 가상 환경에서 작동하는 pip를 업그레이드해야 한다.

```shell
~/.pyvenv/yellow/bin/pip3 install --upgrade pip
```

- sudo로  절대 pip 업그레이드하지 말 것!
	- 기본적으로 가상환경 내의 pip를 활용해서 업그레이드하는 것이다. 
	- `pip --version`으로제대로 깔렸는지 확인해보자.   

- 여기까지 잘 되었으면 기본적으로 `pip`를 쓰는 데 제한이 없다. 

## Server-side 

AWS 같은 서버를 이용하는 경우는 조금 이야기가 다르다. user 계정을 쓰지 않다면, `sudo`를 기준으로삼아서 명령을 실행해주면 된다. 
 
<!--stackedit_data:
eyJoaXN0b3J5IjpbNDQzNzQwNTYzLC01ODc0OTY4NzcsMjA2NT
E0NjkwN119
-->