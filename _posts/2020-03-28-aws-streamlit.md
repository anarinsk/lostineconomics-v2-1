---
layout: post
toc: false
comments: true
title: AWS + streamlit 
description:  AWS에 스트림라이트를 깔아보자. 
categories: [coding-tool, python, AWS]

---

## tl;dr 

- 몹시 불친절하다.  개인적인 활용을 위한 비망록이다. 
- streamlit의 원래 목적은 공유다. AWS를 이용해 streamlit 앱을 깔아보자. 

## Assumptions 

- `| |`  안의 내용은 각자 맞게 설정하시라. 

## Preparing AWS 

- 스트림라이트의 원래 목적은 자신이 지닌 데이터 혹은 데이터 모델을 남들과 공유하는 것이다. 굳이 웹이 필요한 것은 이러한 사연이다. 앞서 Heroku를 통해 streamlit를 공유하는 [방법](https://anarinsk.github.io/lostineconomics-v2-1/coding-tool/python/web-tool/2020/03/09/Streamlit-Heroku.html)을 안내했다. 해당 방법이 코드만 공유하는 방법이라면 여기 소개하는 방법은 AWS 위에 우분투 서버를 깔고 이를 streamlit 서버로 활용하는 방법이다. 이것이 보다 안정적이고 본격적인 방법이다. 

### Creating AWS server

- 대략의 과정은 [여기](https://towardsdatascience.com/how-to-deploy-a-streamlit-app-using-an-amazon-free-ec2-instance-416a41f69dc3)를 참고하자. 

	- Seoul Region에 만든다. 
	- EC2 instane를 만든다. 
	- Unbuntu 18.04 이미지 선택 

- Security Group
	- 기존에 적용되어 있는 룰을 고치거나 새로 세팅을 해야 한다. 
		- 인바운드 
		- 아웃바운드 
		- 모든 트래픽을 허용해도 된다. 
		- 스트림라이트의 기본 설정을 고치지 않는다면, inbound에서 8501번 포트를 여는 것은 필수! 

## Setup for Ubuntu 18.04

- 준비물 
	- pem 파일 
	- 서버 IP 주소 
	- 터미널 앱 

### Log in to AWS 

- 다운로드 받은 pem 파일의 권한을 바꿔야 한다. 

```shell
~$ chmod 400 |Your-pem-File|
```

- 해당 파일이 있는 디렉토리에서 아래 명령어로 AWS로 접속한다. 

```shell
ssh -i |Your-pem-File| ubuntu@|Your-AWS-IP|
```

### Standard procedure for newly installed Ubuntu 

```shell
~$ sudo apt update # 업데이트 목록 갱신
~$ sudo apt upgrade # 현재 패키지 업그레이드
~$ sudo apt dist-upgrade # 신규 업데이트 설치 (필요 없을 수도 있다)
```
 
## Installing python 

```shell
~$ sudo apt update
~$ sudo apt install software-properties-common
~$ sudo add-apt-repository ppa:deadsnakes/ppa
~$ sudo apt update
~$ sudo apt install python3.7
```

- `python3.7 --version`으로 버전을 확인해보자. 

- 아래와 같이 깔려 있는 파이썬이 있는지 확인해보자. 

```shell
~$ sudo update-alternatives --config python
update-alternatives: error: no alternatives for python
```

- 기존 버전이 없을 경우는 아래와 같이 한다. 설치한 파이썬 버전이 3.7이라고 가정하자. 

```shell
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.7 1
```

- 만일 깔려 있는 버전이 있다면 아래의 예처럼 각각 버전에 번호를 부여하고 이를 전환해서 쓰면 된다. 자세한 것은 [여기](https://anarinsk.github.io/lostineconomics-v2-1/coding-tool/python/wsl/2020/03/19/WSL_Cmder.html)를 참고하자.  



### Installing `venv`

```shell
~$ sudo apt install python3.7-venv
```
* 가상 환경을 모아둔 디렉토리로 이동하자. 없다면 하나 만들자. 

```shell
~$ mkdir ~/.pyvenv
```

* `ls -la` 명령어로 소유자가 유저인지를 확인하자. 이제 이 가상환경 디렉토리 안에서 다음 작업을 수행하자. 

```shell
~$ python -m venv |your-venv|
~$ source |your-venv|/bin/activate 
```
```shell
~$ install --upgrade pip 
```

- 참고로 가상환경이든 그 안의 `pip`든 `sudo`로 깔지 말자.[^1]    
- 저 명령어는 특정한 디렉토리, 즉 생성된 환경 내에 있는 `pip`를 활용해서 pip를 업그레이드하라는 의미로 이해하면 된다. 상세한 내용은 [여기](https://anarinsk.github.io/lostineconomics-v2-1/coding-tool/python/venv/2020/04/04/python-venv.html)를 참고하라.  

[^1]: [여기](https://medium.com/@chullino/sudo-%EC%A0%88%EB%8C%80-%EC%93%B0%EC%A7%80-%EB%A7%88%EC%84%B8%EC%9A%94-8544aa3fb0e7)를 참고하라. 

## Installing streamlit

###  streamlit + venv 

- `.pyenv`이 가상 환경의 이름이라고 하자. 아래와 같이 가상 환경을 활성화할 수 있다. 

```shell 
~$ source ~/.pyvenv/|name-of-venv|/bin/activate
```
- 해당 디렉토리에 진입해 `source activate`를 실행해도 된다. 가상 환경 해제는 그냥 `deactivate` 명령을 쓰면 된다. 

- 이 상태에서 streamlit를 깔고 서비스를 띄우도록 하자. 

```shell 
~$ (|your-venv|) pip install streamlit
~$ streamlit run |name-of-your-py-code|
```

- 만일 venv 환경 내 폴더의 소유자를 root로 꼭 두어야 곘다면, 아래와 같은 방식으로 설치할 수 있다. (권장하지 않는다) 

```shell 
(|your-venv|) sudo -H |location-of-your-venv|/pip3 install streamlit
```

- `pip`를 업데이트했던 것과 동일한 방식으로 처리한다. 

### Go tmux not to kill service! 

- 세션에 접속이 끊어지면 스트림라이트 서비스도 같이 끊어진다. 이를 막고 싶다면 `tmux`를 쓰도록 하자. 

```shell
tmux new -s |name-of-stream-session|
```

- 이렇게 하면 별도의 세션 윈도가 생성된다. 
- 여기서 스트림라이트를 띄우도록 하자. 
- 세선에서 나올 때는 <kbd>CTRL-B</kbd> &rarr; D 를 누르면 된다. 

- 다시 해당 세션에 들아가고 싶다면,

```shell
tmux attach -t |name-of-stream-session|
```

- tmux의 기본적인 사용법은 [여기](https://gist.github.com/LeoHeo/70d191eb629b7e3e3084278e19a73e38)를 참고하라. 

## Protips 

- streamlit 서버를 운용할 때 관련한 데이터 혹은 코드를 꼭 서버 내에 둘 필요는 없다. 스트림라이트는 url을 받아서 실행할 수도 있다. 반갑게도 github의 코드 역시 실행이 가능하다. [여기](https://github.com/streamlit/demo-uber-nyc-pickups)의 예를 살펴보자. 

```shell
streamlit run https://raw.githubusercontent.com/streamlit/demo-uber-nyc-pickups/master/app.py
```

### Possibly ideal workflow 

1.  local에서 개발 
2. github를 통해 코드, 데이터 연동 
3. aws의 서버가 gihub의 코드를 받아서 스트림라이트 실행 (업그레이드 이슈가 없는 이상 계속 실행해도 상관 없겠다)

이렇게 해두면, 1과 같이 로컬에서 파이썬 코드로 작업하고, 깃헙으로 push하면 자동으로 AWS 서비스에 반영된다. 굳이 AWS에서 많은 디스크 자원을 쓸 필요가 없다. 


<!--stackedit_data:
eyJoaXN0b3J5IjpbNzYyOTYzOTYxLDEwMjM0MjkzOTAsODEwND
Q4OTIwLC00MzgyMDI4OTQsLTE2NDg3OTkxNjAsMTk0MzU5Mjk0
NCw0ODc3ODA5MjMsLTEyMzQ5NzcxMTIsMzI3Mzc2NDI4LC03ND
QyNDIyMDIsMTM2Nzk1NTczNCwtMTY2MDUyMjkzNSw2ODI0OTMx
NDEsLTkyNTc3MTk4NywtNTE2OTg0OTExLC0xNjQ2ODAwNDAsLT
EzMzYzNzY5MjcsMTgyMzY4NTQ0NCwtMTA4ODA3MTMyMSwxNjQw
Njc5ODk1XX0=
-->