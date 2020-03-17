---
layout: post
toc: false
comments: true
title: Vanilla Python with Virtual ENV
description: 파이썬을 순정으로 써볼까? 
categories: [coding-tool, python]

---

## Why

아나콘다 사가 개발한 conda는 무척 훌륭한 도구다. 하지만 바닐라, 이른바 순정에 로망이 있지 않을까? 단지 로망만은 아니다. 때로는 여러가지 미묘한 의존성 때문에 순정을 이용해야 할 때가 있다. 이번 포스트에서는 python을 수정으로 설치한 후 가상 환경을 이용하는 방법을 간단하게 소개하겠다.[^1]

[^1]: conda가 여러모로 편하다. 하지만 conda를 써야겠다면, conda 전체를 깔지 말고 minconda를 써보시기를 권한다. conda는 쓸 데 없는 것을 너무 많이 깔아버리거든... 

### Assumption 

* 당신은 윈도 이용자다. 
* "[this-is-desc]"와 같이 표현된 것은 각자의 컴퓨팅 환경을 의미한다. 
* git bash와 같이 bash 환경을 쓸 경우 슬래시(`/`)를 그냥 윈도 커맨드를 쓸 경우 역슬래시(`\`)를 써야 한다. 아래에서는 git bash 환경을 기본으로 둔다. 

## Install 

[https://www.python.org/](https://www.python.org/) 

- 취향대로 필요대로 깔도록 하자. 통상적인 윈도 환경이라면 64비트 설치하자. 보통 디폴트로 깔면 32비트가 깔린다. 

## Virtual Environment 

- 가상 환경을 설정해보자. 보통 conda, virtualenv 같은 패키지를 써왔을 것이다. 
- python 3.3 이후 버전부터 venv가 python 안에 포함되었다. 
- python 가상 환경을 만들기 전에 일단 작업 중인 가상 환경을 모아둘 특정 디렉토리(폴더)를 지정하자. 

### How to make venv

- 내 경우 적당한 디렉토리에 `.pyenv` 라는 디렉토리를 만들었다.
- 해당 디렉토리 안에서  가상 환경을 생성한다. 

```bash 
$ [your-directory]/python.exe -m venv [your-environmet]
```

- 가상 환경으로 진입한다. 

```bash
$ [your-environment]/Scripts/activate 
```

- Powershell을 쓴다면, `Activate.ps1`을 실행하면 된다. 
- 이제 실행창 앞에 (`[name-of-your-environmet]`)이 떠 있는 것을 볼 수 있다. 

### Update your venv 

- 가상 환경을 만드는 이유는 무엇인가? 특정한 환경에서 안정적인 작업환경을 만들기 위해서다. 또한 뭔가 문제가 생겼을 때 가상 환경만 지우면 그만이다. 
- 가상 환경은 이제 막 설치된 상태이므로 반드시 필요한 업데이트들을 해줘야 한다. 
	- `pip`
	- 사용할 패키지 
- pip 업데이트다. 
	- python 이용자라면 pip 업데이트를 굳이 설명할 필요가 없겠지! 

```bash
$ python -m pip install --upgrade pip
```  

- 반드시 깔 필요는 없지만 `conda`에 준하게 패키지 관리를 하고 싶다면, 아래 패키지를 깔자. 

```bash
$ pip3 install pip-review # pip-review 설치 
$ py -3 -m pip_review --local --interactive # 설치된 패키지 업데이트 체크 
```
### How to choose python version 

- 가상 환경에서 python 자체를 선택하고 싶다면 어떻게 해야 할까? 
- 아래 명령처럼 하면 된다. 원하는 버전에 python을 직접 호출해서 가상 환경을 만든다. 

```bash 
$ [your-directory]> C:\Python34\python.exe -m venv [your-venv]
```

### With VS Code 

- VS code에서 해당 가상 환경을 어떻게 인식시킬 수 있을까? 
- VS Code의 메뉴에서 
	 - <kbd>File</kbd> &rarr; <kbd>Preference</kbd> &rarr; <kbd>settings</kbd>
	 - "python:Pythonpath"로 검색한 후 아래를 브라우징하다보면, 
	 - 가상 환경이 이름이 `.venv`라면 적당한 디렉토리와 함께 아래와 같이 심어주면 되겠다. 

![]({{ site.baseurl }}/images/vanilla-python/vscode.png){: style="textalign:center; " width="500"}


## Trouble Shooting

### For git bash 

- 원도우 환경에 git bash를 깔아서 쓰고 있다면 몇가지 곤란한 점이 있다. 
	- 우선 python 명령어가 먹지 않는다. 
	- 가상 환경 활성화를 할 수 없다. 

- 이 문제는 쉽게 해결이 가능하다. 

```bash
$ winpty python.exe
Python 3.7.6 (default, Oct  2 2018, 09:18:58)
Type "help", "copyright", "credits" or "license" for more information.
>>>
```
- 원하는 버전의 python을 실행하려면 이를 응용하면 되겠다. 

```bash
$ winpty [your-python-directory]/python.exe
```

- 매번 이렇게 실행할 수 없으니 git bash 환경에 넣어두자. 

```bash
$ echo "alias python='winpty python.exe'" >> ~/.bashrc
```

### Directory changed

- 디렉토리를 만들면 경로나 이름을 바꾸지 않는 게 좋다. 바꾸느냐 차라리 그냥 새로 만드는 게 낫다. 꼭 바꿔야 한다면, 아래를 바꾸면 된다. 
	- `activate.bat`, `activate.ps1` 파일을 열고 path 부분을 바꿔주어야 한다.






<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3MjYzMjIyOTAsLTMwNDg4NDcyNCwtMT
Q5ODYwNDc3OCwtMTAxMjE5ODE0LC0xMjQxMTc4ODk1XX0=
-->