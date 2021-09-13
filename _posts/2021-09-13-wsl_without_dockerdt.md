---
layout: post
toc: false
comments: true
title:  WSL without Docker Desktop
description: Docker Desktop 없이 콘테이트 쓰기 
categories: [coding]
---

# From Docker to Podman 

도커 데스크탑의 유료화 정책이 발표되었다. 아마도 대부분의 사용자나 회사에게는 해당 사항이 없겠지만, 오픈소스로 시작한 회사의 정책이라고 보기에는 뭔가 께름칙하다. 대안이 있는데도 굳이 고집할 필요가 있다. 여러 프로덕트가 복잡하게 의존하는, 즉 발목이 단단히 잡힌 상황이 아니라면 다른 길을 찾는게 맞다. 

평소의 작업 환경이 WSL 내에서 컨테이너를 돌리고 이를 윈도우에서 웹브라우저로 끌어다 쓰고 있던 터라서 대안을 한번 시도해보기로 했다. WSL 내에서 도커를 쓰는 것이 약간 '가짜'인 듯한 느낌이 없진 않았다. 도커 데스크탑은 WSL을 거의 완벽하게 지원한다. 그런데 구조를 약간 뜯어보면 석연치 않은 점이 있다. 우선 도커 데스크탑이 자체로 도커를 위한 컨테이너를 두개 만들고 이 컨테이너와 WSL을 연결한다. WSL 내 OS에서 돌고 있지 않다는 뜻이다. 뭔가 원도를 통한 지원을 완벽하게 구현하기 위해서 꼼수를 사용한 듯한 느낌이 없지 않았다. 

현재 대안으로 제시되고 있는 podman은 OCI(Open Container Initiative)의 표준을 준수하면서도 docker와 거의 모든 면에서 호환된다. docker의 명령어 뿐 아니라 기존에 사용하던 이미지도 그대로 올릴 수 있다. 비유를 하자면 컨테이너를 운반하는 배가 배(동물?)이 바뀔 뿐 컨테이너 자체는 그대로 운용된다. podman을 쓰면 Docker Desktop처럼 복잡한 구조를 걸치지 않아도 된다. WSL 내에 설치된 OS에서만 운용되기 때문이다. 

그리고 무엇보다도 docker의 다른 요소들과 호환성도 뛰어납니다. 내 경우 작업 환경을 유지하는데 docker-compose가 핵심적이었는데, 이 역시 podman에 그냥 붙여서 쓸 수 있었다. 그럼 각설하고 내용으로 바로 들어가보자. 

# Problems 

## Assumptions 

- WSL 2가 잘 깔려 있고 설정되어 있다. 
- Ubuntu 20.04가 깔여 있고 업데이트도 잘 되어 있다. 
- Docker Desktop이 깨끗하게 언인스톨되어 있다.

위 가정에 관해서는 별도로 설명하지 않겠다. 구글 검색을 하시면 좋은 포스팅이 많이 나올 것이다. MS의 공식 가이드도 더할 나위 없이 잘 되어 있다. 

내가 풀고 싶은 문제는 세 가지다. 

1. docker를 podman으로 대체하기 
2. podman으로 nvidia gpu 부리기 
3. podman과 함께 docker-composer 쓰기 

1, 2의 해결책은 Ubuntu 20.04에서 그대로 활용할 수 있다. 3의 경우 WSL의 특성 때문에 몇 가지 추가적인 작업이 필요하지만 비교적 잘 해결된다. 

# Enter the podman 

출처를 먼저 적어 두었으니 바로 보셔도 좋겠다. 

## References 

- [LINK](https://wbhegedus.me/running-podman-on-wsl2/)
    + 잘 되어 있지만 진행 순서에 살짝 오류가 있어서 아래 수정했다. 
- [LINK](https://podman.io/getting-started/installation#installing-development-versions-of-podman)


## Basics

최신 버전의 podman을 설치하기 위해서는 apt 저장소의 주소를 별도로 업데이트해줘야 한다. 20.10 이후부터는 공식적으로 지원한다고 하니, 다음번 LTS가 나오면 ppa를 추가하는 이슈는 해소될 것 같다. 

```
1$ cat /etc/lsb-release
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=20.04
DISTRIB_CODENAME=focal
DISTRIB_DESCRIPTION="Ubuntu 20.04.2 LTS"

2$ export VERSION_ID="20.04"

echo "deb https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/xUbuntu_${VERSION_ID}/ /" | sudo tee /etc/apt/sources.list.d/devel:kubic:libcontainers:stable.list
curl -L https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/xUbuntu_${VERSION_ID}/Release.key | sudo apt-key add -
sudo apt-get update
sudo apt-get -y upgrade
sudo apt-get -y install podman
```

- 셸 스크립트에서 `숫자$`라고 되어 있는 부분은 설명이 필요한 부분이다. 
    1. `1$`은 현재 설치된 Ubuntu의 버전 등을 알아내는 대목이다. 
    2. 이를 통해 `VERSION_ID` 변수를 설정한다. 
- 나머지 부분은 PPA를 설정하고 패키지 저장소를 업데이트하고 포드맨을 설치하기 위한 작업이다. 

## Setting 

많은 가이드를 보면 아래 두 파일 중 하나의 내용을 수정할 것을 권고하고 있다. 후자의 파일은 없기 때문에 생성하면 된다. 

- `/usr/share/containers/containers.conf` 
- `~/.config/containers/containers.conf`

수정 혹은 생성 내용은 아래와 같다. 

```txt
[engine]
events_logger="file"
cgroup_manager="cgroupfs"
```

- 그런데 굳이 고치지 않아도 컨테이너를 돌리는 데 큰 이슈는 없었다. 

## Testrun 

podman이 잘 깔렸는데 아래와 같이 테스트해보자. 

```shell
podman run hello-world 
```

docker의 hello-world를 테스트로 사용하면 된다. 잘 되었다면 잘 깔린 것이다. 

# Podman + nvidia GPU 

docker desktop에서 제일 좋았던 것이 nvidia GPU에 관한 지원이다. docker에서 `gpu` 옵션이 지원되서 nvidia docker를 별도로 깔지 않아도 되었다. 

왜 이게 중요할까? 컨테이너를 쓰는 이유가 각종 복잡한 설정을 우회해서 바로 올려서 쓰기 위해서다. Ubuntu에서 GPU를 부리기 위한 준비는 간단하지 않다. 각종 드라이버 및 API를 설치하는 수고로움을 피하기 위해서 해당 준비가 갖춰진 컨테이너를 올려 쓰고 싶은 것이다. Podman에서 이게 어렵다면 매력이 떨어지지 않을까? 

다행스럽게도 nvidia에서 컨테이너를 위한 런타임을 제공한다. 이 녀석을 깔면 podman과 nvidia GPU를 어렵지 않게 연결할 수 있다. 

## Reference

- [LINK](https://nvidia.github.io/nvidia-docker/)
- [LINK](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#id8)

## Installation 

컨테이너 툴킷 설치를 위해서 먼저 PPA를 설정하자. 

```shell
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
sudo apt-get update
```

컨테이너 툴킷을 설치한다. 

```shell
sudo apt install -y nvidia-container-toolkit
```

`/usr/share/containers/oci/hooks.d/oci-nvidia-hook.json` 파일이 있는지 확인해본다. 없으면 생성하고 아래의 내용을 담도록 하자. 

```json
{
    "version": "1.0.0",
    "hook": {
        "path": "/usr/bin/nvidia-container-toolkit",
        "args": ["nvidia-container-toolkit", "prestart"],
        "env": [
            "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
        ]
    },
    "when": {
        "always": true,
        "commands": [".*"]
    },
    "stages": ["prestart"]
}
```

`config.toml` 파일을 수정하도록 하자. 아래 코드는 파일에서 필요한 부분을 수정하고 제대로 수정되었는지 조회하도록 했다. `no-cgroups = true`가 설정되어 있으면 제대로 된 것이다. 해당 위치의 파일을 열어 직접 수정해도 된다. 

```
$ sudo sed -i 's/^#no-cgroups = false/no-cgroups = true/;' /etc/nvidia-container-runtime/config.toml
$ cat /etc/nvidia-container-runtime/config.toml
```

부팅을 한번 다시 해주자. WSL에서 부팅이란 그냥 셸을 껐다가 다시 켜면 된다. 

## Testrun 

아래 예는 nvidia-smi 명령을 통해서 OS에 드라이버가 제대로 설정되어 있는지 확인하는 명령어이다. 깨끗하게 설치된 상태라면 WSL-Ubuntu에는 별도로 nvidia driver를 설치하지 않았을 것이다. 컨테이너를 통해서 드라이버가 설정된 이미지를 이렇게 돌릴 수 있는 것이다. 하드웨어와 직접 접촉하는 윈도 드라이버만 WSL을 지원하는 버전으로 잘 깔아주면 된다. 

```shell
podman run --rm --security-opt=label=disable nvidia/cuda:11.0-base nvidia-smi
```

조금 허전하니 nbody problem도 돌려보자. 

```shell
podman run --env NVIDIA_DISABLE_REQUIRE=1 nvcr.io/nvidia/k8s/cuda-sample:nbody nbody -gpu -benchmark
```

# Podman + Docker Compose 

도커의 여러가지 요소가 집약된 데스크탑이 유료화되는 것이지 CE 버전 및 리눅스 배포판에서 쓸 수 있는 각종 요소가 유료화되는 것은 아니다. 라이선스 상 이 개별 요소들이 유료화되긴 힘들 것이다. 

도커에서 편리한 앱 중 하나가 컴포즈다. 여러 개의 컨테이너를 올려 놓고 돌려 쓰기에 딱 좋다. 여러 가지 용도가 있겠지만 내 경우는 Jupyter, RStudio, Tensflow 세 개 정도의 컨테이너를 돌려 놓고 돌려가며 쓴다. 각종 설정을 한방에 잡아주기에는 컴포즈 만큼 편한 게 없다. 

## Reference 

[LINK](https://github.com/arkane-systems/genie)

## Problem 

WSL이 Ubuntu의 중요 요소인 systemd를 활성화하지 않고 docker나 docker의 요소를 데스크탑을 거치지 않고 깔아서 쓸 때 문제가 된다. systemd가 활성화되지 않으면 이를 활성화해주면 된다. 위 링크에 그 절차가 자세하게 소개되어 있다. 

## Solution 

### Runtime 

- 먼저 런타임을 설치해야 한다. 

```
wget https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
rm packages-microsoft-prod.deb
```

### wsl-transdevian 

- 이 녀석을 설치하기 위해서 먼저 lsb를 설치하자. 

```shell
sudo apt-get install lsb 
```

- sudo로 실행되어야 하는 항목에서 이를 타이핑하는 번거로움을 피하기 위해서 `sudo -s`를 실행하자. 이후 아래 스크립트를 실행한다. 

```shell
wget -O /etc/apt/trusted.gpg.d/wsl-transdebian.gpg https://arkane-systems.github.io/wsl-transdebian/apt/wsl-transdebian.gpg
chmod a+r /etc/apt/trusted.gpg.d/wsl-transdebian.gpg

cat << EOF > /etc/apt/sources.list.d/wsl-transdebian.list
deb https://arkane-systems.github.io/wsl-transdebian/apt/ $(lsb_release -cs) main
deb-src https://arkane-systems.github.io/wsl-transdebian/apt/ $(lsb_release -cs) main
EOF

apt update
```

### genie & docker-compose 

이제 마지막으로 genie와 docker-compose를 설치해준다. 

```shell
$ sudo apt update
$ sudo apt install -y systemd-genie
$ sudo apt install docker-compose
```

### Setting 

```shell
1$ systemctl --user start podman.socket
2$ export DOCKER_HOST=unix://$XDG_RUNTIME_DIR/podman/podman.sock
```

1. systemctl 명령을 통해서 podman의 소켓을 개시하는 것이다. 소켓의 상태를 보고 싶다면, `start` &rarr; `status`로 바뀌 실행해보자. 
2. docker를 지웠으므로 이를 대신할 가상화 앱을 지정해야 한다. 이를 포드맨 소켓과 연결한다. 

이 상태에서 `docker-compose`를 실행하면 잘 돌아간다. 

- WSL-Ubuntu 부팅 시 자동으로 준비가 되게 하려면 1,2를 .bashrc에 넣는다. 
    + 단 `wsl genie -s` 상태가 아니라면 systemd 사용이 제한되기 때문에 아래 같은 경고 메시지를 볼 수 있다.  
    + `Failed to connect to bus: No such file or directory`

## Testrun 

- 현재 위치(대체로는 유저 홈)에 아래와 같이 docker-compose.yml 파일을 생성하자.
- 현재 위치에 `data`디렉토리를 생성하자.  

```yml
version: '3'
#
services:
    tf-gpu:
        image: tensorflow/tensorflow:latest-gpu-jupyter
        environment:
            - NVIDIA_DISABLE_REQUIRE=1
            - GRANT_SUDO=yes
            - JUPYTER_ENABLE_LAB=yes
            - JUPYTER_TOKEN=1234
        volumes:
            - "./data:/mnt/space/ml"
        ports:
            - "8888:8888"
        # deploy:
        #     resources:
        #         reservations:
        #             devices:
        #                 - driver: nvidia
        #                   device_ids: ['all']
        #                   capabilities: [gpu]
        container_name: tf_gpu
```

이제 다음을 실행하자. 

```
$ docker-compose up 
```

잘 실행되었다면 주피터 노트북이 생성되었을 것이다. 접속 주소는 웹브라우저에서 localhost:8888을 치면 된다. 토큰은 1234로 설정해두었다. GPU가 잘 돌아가고 있음을 확인할 수 있는데, 노트북을 열고 다음과 같이 쳐보자. 

```python
import tensorflow as tf
tf.config.get_visible_devices(
    device_type=None
)
```

CPU 이외에 GPU가 보이면 잘 설정된 것이다. 컨테이너 안에 담긴 fashion mnist 등의 예제를 시험해보기 바란다. 

위 yml 파일에서 눈치를 챘을지 모르겠지만, deploy 항목은 docker에서만 실행되는 항목이다. 이를 주석처리 하지 않고 podman에서 돌리면 에러가 발생한다. 

# Problems Left 

아직 해결되지 않은 이슈도 있다. 

- nvidia-container-toolkit의 호환성이 조금 떨어지는 경우가 있는 것 같다. 
- 도커 데스크탑에서는 VS Code의 container 접속을 통해서 바로 컨테이너 접속이 가능했다. 도커 데스크탑이 별도의 컨테이너를 운용했기 때문에 가능한 일이었다. 그런데 이제 WSL-Ubuntu 안에서 컨테이너가 돌기 때문에 VS Code에서 바로 접속이 불가능해졌다. 조만간 해결책이 나오지 않을까 기대해본다. 
