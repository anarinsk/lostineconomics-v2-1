---
layout: post
toc: false
comments: true
title: Setting python venv in Ubuntu 18.04
description:  파이썬 가상 환경 설정하기
categories: [coding-tool, python, venv]

---

## Assumptions 

- `| |`은 각자의 환경을 나타낸다. 
- `.pyvenv`는 파이썬 가상 환경을 모아둔 임의의 디렉토리를 의미한다. 

## Use venv 

* venv 환경으로 쓸 디렉토리를 만든다. 

```shell
~$ mkdir /.pyvenv 
```

* 디렉토리 만들 때 `sudo` 명령어를 쓰지 말라. 그러면 디렉토리의 권한이 root에 귀속된다. 
* 해당 디렉토리로 이동한다. 

```shell
~$ cd ~/.pyvenv 
```

* 여기서 venv 환경을 만든다. 

```shell
~$ [sudo] python -m venv |your-venv|
```

![]({{ site.baseurl }}/images/python-venv/fig_1.png){: style="textalign:center; " width="800"}

- 위 그림을 보자. 만일 `sudo` 없이 가상 환경을 만들었다면 해당 폴더는 현재 로그인한 사용자에게 귀속된다. 그런데 `sudo`로 만들었다면 이는 root에 귀속된다. 이에 따라서 생성된 가상 환경의 사용 조건 및 운용이 달라지게 된다. 그러니까 `sudo`는 자제하자.  왜?

- VS Code같은 외부 앱과 연동해서 사용할 때 해당 디렉토리가 root에 귀결되면 문제가 발생할 수 있다. VS Code가 자신이 필요한 python 패키지들을 설치할 수 없게 된다. 특별한 이유가 없다면, 아래와 같이 폴더 권한을 유저에게 두자.[^1]

- 혹시 `ls -la`로 조회했을 때 위 그림처럼 root가 소유자라면 다음과 같이 이를 돌리도록 하자. 

```shell
~$ sudo chown |user|:|user| -R |your-venv|
```

그림의 예라면 아래와 같이 실행하여 소유자를 root에서 user로 바꾸자. 

```shell
~$ sudo chown anarinsk:anarinsk -R venv-with-sudo 
``` 

[^1]: 권한 관리에 관해서는 [여기](https://eunguru.tistory.com/93)를 참고하라. 

### Without sudo

- 이제부터는 표준을 따르면 된다.

-  먼저 필요한  venv 환경를 활성화한다. venv 환경 내 `bin` 디렉토리의 activate를 활성화한다. 

```shell
~$ source ~/.pyvenv/|your-venv|/bin/activate 
```
- venv가 활성화된 상태에서 pip를 업그레이드하자. 이때 업그레이드되는 pip는 venv 환경 내의 pip다.  

```shell
~$ (|your-venv|) pip install --upgrade pip
~$ (|your-venv|) pip --version # 업그레이드 확인 용도 
```

* 이후 패키지 설치 등 필요한 작업을 수행한다. 

### With sudo

- `sudo`로 가상 환경을 깔았다면 환경의 권한이 root에 귀결된다. 이 녀석을 다시 user 돌려놓도록 하자. 

```shell
~$ sudo chown |user|:|user| -R |your-venv|
```

* 이제 `sudo` 없이 깔았을 때처럼 사용하자. 

## sudo if you insist

* 만일 여러가지 이유 때문에 디렉토리의 소유권을 변경하지 않으려 한다면, 가상 환경에서 작동하는 pip를 특정해서 업그레이드해야 한다.

```shell
~$ ~/.pyvenv/yellow/bin/pip3 install --upgrade pip
```

- sudo로  절대 pip 업그레이드하지 말 것!
	- 기본적으로 가상환경 내의 pip를 활용해서 업그레이드하는 것이다. 
	- `pip --version`으로제대로 깔렸는지 확인해보자.   


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4MzUzNDM2MDQsMTQwMjIzMTQyLDEwMj
A4ODM3NTAsMTA2OTM1OTY2OSwxMTU3MjcwMjM0LDU3ODc4OTgw
OSwtODU0MTg0MzA1LC01ODc0OTY4NzcsMjA2NTE0NjkwN119
-->