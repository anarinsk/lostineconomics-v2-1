---
layout: post
toc: false
comments: true
title:  Docker + Jupyter + 한글 폰트  
description: 빌드할 때 Jupyter의 한글 문제도 같이 해결하자!
categories: [coding, docker, jupyter]

---

## Build도 어렵지 않다! 

이 글은 [이 포스팅]()의 연장선상이다. 시작은 별도의 `sh` 스크립트를 실행하지 않고 한큐에 해결할 수 없을까, 였다. 역시 방법이 있더라. 논리는 간단하다. 사실 이 글을 쓰게된 이유는 갑자기 `matplotlib`의 font_manager 안에 `_rebuild`가 사라졌기 때문이다. 이 글을 쓰는 시점에서 나는 그랬다. 

`docker-compose`로 필요한 이미지를 끌어올 때 단순히 이미지를 마운트하지 않고 적당하게 build를 하면 된다. 

앞서 build를 하는 옵션을 택하지 않았다. 빌드가 꽤 중후장대하게 느껴졌기 때문이다. 일단 필요한 ubuntu 버전을 끌어오고 여기에 Python, Jupyter를 깔고... 이 빌드 과정이 꽤 험난하고 불필요해보였기 때문이다. Jupyter의 경우 데이터사이언스를 위한 도커 이미지가 따로 있었고, 이를 그대로 쓰면 된다고 생각했다. 

그런데 문득 떠올랐다. 도커를 빌드할 때 이 이미지에 기반하고 여기에 필요한 몇 가지 명령만 수행하면 많은 문제를 해결할 수 있지 않을까? 확인한 사항은 다음과 같다. 

- `docker-compose`를 쓸 때 이미지를 지정하는 `image` 부분을 제외한 나머지는 거의 그대로 재사용이 가능하다. 
- 당연한 말이지만 image를 끌어오는 시간이나 몇 가지 명령어를 넣어서 build를 하는 시간이 크게 차이가 없더라. 

## 실행 방법 

본론이다. 

1. docker-compose를 위한 file이 있는 디렉토리 아래 `dockerfiles`와 같은 디렉토리를 하나 더 만든다. 여기에 docker build를 위한 파일을 넣어어둔다. 
2. docker-compose용 파일을 약간 수정한다. 

### 실행 환경 

1. Windows 10 WSL 2 + Ubuntu 20.04 
2. Docker for Desktop (Windows)

### dockerfiles 

데이터 사이언스를 위한 Jupyter 파일을 예시로 들겠다. 

```shell
FROM jupyter/datascience-notebook:latest
# Declare root as user 
USER root
# Update Ubuntu 
RUN sed -i 's/archive.ubuntu.com/mirror.kakao.com/g' /etc/apt/sources.list && sed -i 's/security.ubuntu.com/mirror.kakao.com/g' /etc/apt/sources.list
# Install Nanum for Korean Font 
RUN apt-get update && apt-get -y upgrade && apt-get install -y fonts-nanum* && fc-cache -fv && rm -fr ~/.cache/matplotlib
```

+ `FROM jupyter/datascience-notebook:latest` 끌어올 이미지를 지정한다. 
+ `USER root` 이미지 내에서 root 권한을 부여한다. 이후 sudo는 안 써도 된다. 
+ `RUN sed -i...` 끌어온 우분투 이미지가 미국 기준이기 때문에 업데이트 서버 주소 역시 미국이다. 이걸 국내에서 가장 안정적이고 빠른 카카오 서버로 바꾼다. 
+ `RUN apt-get update && apt-get -y upgrade...` 
    + 우분투 배포판의 업데이트 및 업그레이드를 단행한다. 
    + 나눔 폰트를 깔아준다. 
    + docker 이미지 배포판의 폰트 캐시를 지운다. 
    + Jupyter의 폰트 캐시를 지운다. 

### docker-compose.yml 

docker-compose를 위한 yml을 예시한다. 각자 환경에 맞게 조금씩 변형해서 쓰면 되겠다. 파일 이름을 `docker-jupyter.yml`이라고 하자. 

```shell
version: '3'
#
services:
#
    jupyter-ds:
      build:
        context: .
        dockerfile: ./dockerfiles/dockerfile-jupyter
      user: root
      environment:
        - GRANT_SUDO=yes
        - JUPYTER_ENABLE_LAB=yes
        - JUPYTER_TOKEN={YOUR-PASSWORD}
      volumes:
        - /mnt/c/Users/{YOUR-DIR}:/home/jovyan/github-anari
      ports:
        - "8888:8888"
      container_name: "jupyter-ds"
# End of yml
```

+ 위의 yml 파일에서 `{}`로 처리된 부분은 각자 채워 넣으면 된다. 
+ image 대신 build 명령어를 사용했다. 앞서 지정한 dockerfiles 디렉토리 내의 도커 명령어를 통해 빌드를 수행한다. 이렇게 빌드된 이미지는 시작부터 한글 구현에 아무런 문제가 없다. 

```shell
> sudo docker-compose -f /mnt/{YOUR DIR}/docker-jupyter.yml -p "jupyter-ds" up -d
```

실행 옵션은 다음과 같다. 역시 `{}`는 각자 채워 넣으시라. 

+ `-f` 도커 콤포즈를 특정 파일로 실행하기 위한 옵션이다. 만일 이를 안쓰려면 yml 파일의 이름을 `docker-compose.yml`로 두고 해당 디렉토리 안에서 실행하면 된다. 
+ `-p` 콤포즈 안에 묶인 서비스의 이름을 나타낸다. 같은 네트워크로 묶이며 이 이름을 네트워크 이름으로 갖는다. 
+ `up` `yml` 내에 있는 콘테이너를 올린다. 
+ `-d` 디태치 모드를 뜻한다. 

