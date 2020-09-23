---
layout: post
toc: false
comments: true
title:  Docker 소박한 사용법 
description:  문과생이 도커를 쓰는 법 
categories: [docker, data-science]

---

## 사전 전제 

- 도커에 관해서 대충 알고 있다. 
- 도커를 써보기는 했다. 하지만... (막상 쓰려면...)

## 내가 도커를 쓰는 이유 

- (사실 있어 보이고 싶으니까) 
- 개발 환경의 격리 및 환경 공유를 위해서 
- 가끔 리눅스 환경에서만 구현할 수 있는 파이썬 패키지들이 있다. 
- 윈도에서 한번은 마주하게 되는 한글 인코딩 이슈 

##  원칙 

본격적으로 사용을 소개하기 전에 도커를 쓰는 원칙 몇 가지를 전제하도록 하자. 

본인에게 특화된 이미지를 빌드하지 말고 그냥 official build를 당겨와서 쓰자. 

도커의 장점이 개발환경의 개별화라고 말했다. 그렇다면 자신에게 적합한 환경을 세팅해두고 이 녀석을 끌어와 쓰면 좋지 않을까? 약간의 취향과 사용성의 차이를 감안하고 이야기하면, 그러지 않는 게 좋다. 도커 허브에서는 공식적으로 관리되고 업데이트되는 도커 이미지들이 많이 올라와 있다. 되도록 초반에는 이런 이미지들을 취사선택해서 활용하는 편을 권장한다. 

예를 들어보자. 파이썬으로 코딩을 한다면 그리고 주피터 환경을 써야 한다면 어떻게 하는 것이 좋을까? 주피터에서 공식적으로 운영하는 도커 이미지가 있다. 도커로 R을 쓰고 싶다면? 역시 rocker라는 공식적인 프로젝트가 있다. 개인이 제조한 사설 이미지보다는 이런 녀석들을 끌어 쓰는 편을 권한다. 

이하에서는 내가 기본으로 사용하는 두 개의 환경, 파이썬+주피터, RStudio을 어떻게 구축하는지 설명하겠다. 

## 미리 해야 할 것들 

### 원도10 2004 Build 이후 WSL 2 환경 

WSL 2가 깔리는 윈도10 버전으로 업데이트하고 이를 설치하도록 하자. 시중에 가이드가 많으니 구글 검색 후 참고하면 되겠다. 

- [MS 공식 가이드](https://docs.microsoft.com/ko-kr/windows/wsl/)

### Docker Desktop (윈도10)

순수 우분투 환경이라면 `apt-get`으로 도커를 깔면 된다. 우리는 윈도10과 리눅스를 오가야 하므로 이를 잘 지원해주는 도커 버전을 까는 편이 정신 건강에 좋다. 도커에서 WSL 2에 특화된 도커를 제공하고 있다. 보다 안정적인 버전인 Stable과 보다 최신 기능을 담은 버전인 Edge 모두 WSL 2 지원한다. 

- [Docker Desktop for WIndows](https://hub.docker.com/editions/community/docker-ce-desktop-windows)

### Windows Terminal 

반드시 써야 하는 것은 아니다. 하지만 윈도10에서 우분투를 써야 한다면 윈도 터미널을 함께 쓰면 좋겠다. 무엇보다도 별다른 조작 없이 우분투 등 WSL 2에 깔린 리눅스를 잘 잡아준다. 다음으로 환경 커스터마이즈가 쉽다. 

- [MS 공식 가이드](https://docs.microsoft.com/ko-kr/windows/terminal/)

## 내 환경 

내가 세팅한 환경을 다음과 같다. 

1. Windows 10 Insider Preview 20190.rs 
2. Docker Desktop Edge 2.3.7.0 
3. Windows Terminal Preview 

### Comments 

1. 윈도우10 20190.rs 버전은 인사이드 프리뷰 버전이다. 프리뷰 버전을 꼭 쓰지 않아도 된다. 
2. 도커 버전은 최신 버전을 유지하는 편을 권한다. Edge가 불안하다면 Stable을 쓰면 된다. 

## Docker Composer 

도커를 써본 사람이라면 개별 컨테이너를 띄울 때 옵션들, 예를 들어 root 권한을 줄 건지, 포트는 어떤 것을 열 것인지 등등, 을 써봤을 것이다. 일상적으로 쓸 때에도 이것들을 매번 지정을 해줘야 할까? 그렇게 해도 되겠지만 불편하다. 한번 정해두고 재사용할 수 있으면 편할 것이다. 이러한 용도에 적합한 것이 도커 콤포저다. 

`.yml` 확장자의 설정 파일을 만들어두고 녀석을 실행해서 매번 동일한 환경을 손쉽게 구축할 수 있다. 내가 쓰는 `.yml` 환경으로 간략하게 소개해보겠다. 

```yml 
version: '3'
services:
    jupyter-ds:
        image: jupyter/datascience-notebook
        user: root
        volumes:
            - /mnt/e/github:/home/jovyan/anari-github
        ports:
            - "8888:8888"
            - "8501-8510:8501-8510"   
        environment:
            - GRANT_SUDO=yes
           - JUPYTER_ENABLE_LAB=yes
           - JUPYTER_TOKEN=[내 비번]
        container_name: jupyter
#
    rstudio:
        image: rocker/verse:latest
        #image: rocker/verse:4.0.2
        volumes:
            - /mnt/e/github:/home/rstudio/anari-github
        ports:
            - "8787:8787"
        environment:
            - ROOT=true
            - PASSWORD=[내 비번]
        container_name: rstudio
# End of yml
```

위 yml에서 `service`  아래 두 개의 항목이 있다. 각각 jupyter-datasicence-notebook과 RStudio 컨테이터 이미지 의미한다. 

- `image`: docker-hub 상의 이미지 이름을 뜻한다. 
- `volume`: WSL 2 환경의 디렉토리와 이미지 내 디렉토리를 매핑한다. 즉, X : Y라고 할 때 X는 윈도우 10의 폴더, Y는 도커 이미지 내의 폴더 이름을 뜻한다. WSL 2는 `/mnt` 내에 윈도 드라이브를 마운트한다. 따라서 윈도10 환경과 연동된다고 보면 얼추 맞다. 
- `ports`: WSL 2에서 인식하는 포트와 이미지 내에서 인식하는 포트를 매핑한다. 
- environment: 각각 이미지에 맞는 환경을 지정할 수 있다. 이 사례에서 주피터 노트북에는 `sudo` 권한을 주고, 주피터랩 버전을 쓰며, 최초 진입시 비번은 [내 비번]을 쓰겠다고 지정한 것이다. RStudio의 경우 root 권한을 주고, 패스워드는 내 비번을 쓰겠다는 의미다. 

## 실사용 

### 어떻게 실행할까? 
주피터든 RStudio든 모두 웹 브라우저에서 돌아간다. 범용 웹브라우저에서 `localhost:8888`을  치면 주피터에 `localhost:8787`을 치면 RStduio에 들어갈 수 있다. 주피터야 원래부터 웹 브라우저 기반이었으니 보통 깔아 쓰는 것과 차이를 느낄 수 없다. RStudio도 그럴까? 그렇다. 원래 RStudio라는 IDE가 웹 기반으로 만들어졌기 때문에 이 역시 차이를 느낄 수 없다. 

![]({{ site.baseurl }}/images/docker-in-use/jupyter.png){: style="textalign:center; " width="400"}  ![]({{ site.baseurl }}/images/docker-in-use/rstudio.png){: style="textalign:center; " width="400"}

해당 페이지 아이콘을 바탕화면 혹은 바로가기에 두고 쓰면 조금 편리하겠다. 

### 특화된 패키지 혹은 설정이 필요하다면? 

그때그때 깔면 된다. 원 도커 이미지에서 웬만해서는 더 깔 것이 없을 것이다. 필요한 경우가 있다면 bash script를 만들어 깔아주도록 하자. 해당 이미지를 멈추지 않는 이상 설치한 상태는 유효하다. 

### 도커 이미지 관리 
이미지를 업데이트하고 싶다면 어떻게 해야 할까? 정석대로 하자면, WSL 2 콘솔창에서 실행중인 콘테이너를 멈추고, 이 콘테이너의 이미지를 지워야 할 것이다. 그렇게 해도 되지만, 윈도용 도커 프로그램에서는 보다 간편한 방법을 제공한다. 

대시보드 상에서 컨테이너를 실행하고 멈출 수 있고 이미지도 관리할 수 있다. 

![]({{ site.baseurl }}/images/docker-in-use/docker_1.png){: style="textalign:center; " width="500"}

아예 데이터 전체를 리셋하는 방법도 있다. 

![]({{ site.baseurl }}/images/docker-in-use/docker_2.png){: style="textalign:center; " width="500"}








<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4MTgwNjUxMTAsLTMyMzgwMzAzNSwtNj
c4MDM0Mjg1LC00NzA5MTYwMjhdfQ==
-->