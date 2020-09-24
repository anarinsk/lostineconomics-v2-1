---
layout: post
toc: false
comments: true
title:  Docker 컨테이너 + 한글 폰트 
description:  컨테이너 안에서도 한글을 써보자 
categories: [docker, data-science]

---


## 왜 필요한가?

Jupyter 등에서 matplotlib을 쓰고, 라벨이 한글일 경우 대부분 한글 출력에 문제를 겪게 된다. ㅁㅁㅁ형태로 출력되는데, 파이썬 환경에서 폰트가 인식되지 않게 때문이다. 일반적인 환경이라면 웹 검색으로 찾을 수 있는 [가이드](https://financedata.github.io/posts/matplotlib-hangul-for-windows-anaconda.html)를 통해 해결할 수 있다.  

그런데 내가 도커 환경을 쓴다면 이런 가이드를 적용할 수 있다. 도커는 아래 그림과 같이 'host os - 도커 - 컨테이너'의 구조로 돌아간다. 컨테이너(App x)가 각기 독자적인 os를 지니고 있기 때문에 해당 os에서 한글을 인식시켜줘야 도커 환경에서 제대로 한글을 쓸 수 있는 것이다.  

![](https://www.docker.com/sites/default/files/d8/styles/large/public/2018-11/container-what-is-container.png?itok=vle7kjDj)

## Simple Solution 

사실 방법은 간단하다. 컨테이너 내의 os에 한글을 설치해주면 된다. 해당 os 내의 bash 혹은 상응하는 CLI에 들어가서 필요한 명령어들을 실행하면 된다. 아예 해당 명령어를 별도의 bash 스크립트로 만들고 필요한 경우 녀석을 한방에 돌리면 보다 편리할 것이다. 아래는 나눔 계열 폰트를 설치하는 스크립트다. 파일 이름을  `install_nanum.sh`로 하자. 

```shell
#!/bin/sh

sudo sed -i 's/archive.ubuntu.com/ftp.daumkakao.com/g' /etc/apt/sources.list
sudo apt-get update 
sudo apt-get install -y fonts-nanum*
sudo fc-cache -fv
```

나머지는 대체로 자명한 명령어다. 첫행의 sed는 ftp 주소를 국내로 바꿔주는 것이다. 업데이트 시 걸리는 시간을 단축하기 위해 사용했다. 

이제 도커 컨테이너 안의 Jupyter 에서 다음과 같이 실행하면, 나눔고딕 폰트를 쓸 수 있다. 

```python
import matplotlib.pyplot as plt
import matplotlib as mpl
from matplotlib import rc, font_manager
font_fname = '/usr/share/fonts/truetype/nanum/NanumGothic.ttf'
prop = font_manager.FontProperties(fname=font_fname)
mpl.rcParams['font.family'] = 'NanumGothic'
font_manager._rebuild()
```

혹시 잘 보이지 않으면, 커널을 한번 리프레시 해주면 된다. 다른 폰트를 설정하고 싶다면 비슷하게 응용해 활용하면 되겠다. 
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjE0ODEyMzExLC00ODk0MjQzNjZdfQ==
-->