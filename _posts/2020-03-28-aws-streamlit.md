---
layout: post
toc: false
comments: true
title: AWS + streamlit 
description:  AWS에 스트림라이트를 깔아보자. 
categories: [coding-tool, python, AWS]

---

## tl;dr 

- 몹시 불친절하다.  개인적인 비망록의 성격이 강하다. 
- streamlit의 원래 목적은 공유다. AWS를 이용해 streamlit 앱을 깔아보자. 

## AWS 계정 만들기 

- 대략의 과정은 [여기](https://towardsdatascience.com/how-to-deploy-a-streamlit-app-using-an-amazon-free-ec2-instance-416a41f69dc3)를 참고하자. 

	- Seoul Region에 만든다. 
	- EC2 instane를 만든다. 
	- Unbuntu 18.04 이미지 선택 

- Security Group
	- 기존에 적용되어 있는 룰을 고치자.  
		- 인바운드 
		- 아웃바운드 
		- 모든 트래픽을 허용해도 된다. 다만, inbound에서 8501번 포트를 꼭 열도록 하자. 

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

해당 파일이 있는 디렉토리에서 아래 명령어로 AWS로 접속한다. 

```shell
ssh -i |Your-pem-File| ubuntu@|Your-AWS-IP|
```

### Standard Procedure for Newly Installed Ubuntu 

```shell
~$ sudo apt update # 업데이트 목록 갱신
~$ sudo ap upgrade # 현재 패키지 업그레이드
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

`python3.7 --version`으로 버전을 확인해보자. 

- 아래와 같이 깔려 있는 파이썬이 있는지 확인해보자. 

```
$ sudo update-alternatives --config python
update-alternatives: error: no alternatives for python
```

- 기존 버전이 없을 경우는 아래와 같이 한다. 

```
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.7 1
```

- 기존 버전이 있다면, 뒤 번호를 바꿔가며 어러 버전의 파이썬을 세팅한다. 자세한 것은 [여기](https://anarinsk.github.io/lostineconomics-v2-1/coding-tool/python/wsl/2020/03/19/WSL_Cmder.html)를 참고하자.  



### Installing `venv`

```
~$ sudo apt install python3.7-venv
#(가상환경을 모아둘 폴더로 이동. 없다면 하나 만들어라.)
~$ sudo python -m venv |your-venv|
~$ source |your-venv|/bin/activate 
```

#### Trouble shooting 

- 가상 환경에서 pip가 권한 문제로 업그레이드가 되지 않을 수 있다. 

```
sudo -H |Your-venv-dir|/pip3 install -U pip 
```
- 로그아웃 후 다시 로그인 한다. 


## Installing streamlit

### 가상환경에서 streamlit 띄우기 

- `.pyenv`: 가상환경이름 


```shell 
source ~/.pyvenv/|name-of-venv|/bin/activate
```
- 이 상태에서 streamlit를 깔고 서비스를 띄우면 된다. 

```shell 
(|your-venv|) pip install streamlit
```

### tmux not to kill service 

- 세션에 접속이 끊어지만 스트림라이트 서비스도 같이 끊어진다. 이를 막기 위해서 `tmux`를 쓰도록 하자. 

```shell
tmux new -s |name-of-stream-session|
```

- 이렇게 하면 별도의 세션 윈도가 생성된다. 
- 여기서 스트림라이트를 띄우도록 하자. 
- 세선에서 나올 때는 <kbd>CTRL-B</kbd> &rarr; D 를 누르면 된다. 

- 다시 세션에 들아가고 싶다면,

```shell
tmux attach -t StreamSession
```

- 기본적인 사용법은 [여기](https://gist.github.com/LeoHeo/70d191eb629b7e3e3084278e19a73e38)를 참고하라. 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEzMjU2MTIyODYsMTkyNjAwOTEyNCw4OD
kzMTI0NzBdfQ==
-->