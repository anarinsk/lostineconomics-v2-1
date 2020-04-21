---
layout: post
toc: false
comments: true
title:  wsl 2 + docker + rstudio 
description:  도커로 가볍게 rstudio를 돌려볼까? 
categories: [coding, rstat, docker]

---

![](https://www.codemotion.com/magazine/wp-content/uploads/2020/01/31518965950_460ff828ba_b_2f62655c94d0d2f5d51a75899b6f9280_2000-896x504.jpg){: style="textalign:center; " width="800"}


## rstat! 

R이 그리울 때가 있다. 직관적인 문법 그리고 무엇보다도 `ggplot`의 환상적인 구조는 한번 체험하면 잊기 힘들다. 그런데 요즘 wsl2를 쓰면서 RStudio를 켜는 일이 부쩍 줄어들었다. 

장인은 도구를 안가리지만, 나같은 범인은 도구가 중요해! wsl 2에 어떻게 하면 R을 편하게 얹어서 쓰지? 고민이 시작되었다. 
물론 wsl 2에 linux용 R을 깔아쓰면 될 일이다. 하지만 뭔가 깔끔하지 않다. 도커를 써볼까? 

## Here Comes the rocker! 

문득 예전에 클라우드에 linux + Rstudio server를 쓰던 생각이 났다. 클라우드에 RStudio를 세팅하면 웹브라우저를 통해 RStudio의 기능을 100% 쓸 수 있다. 

같은 방식을 그냥 wsl 2에 적용하면 되잖아? 그렇다! 그리고 이러한 용처를 예상했는지 R 버전, RStudio, tidyverse, 그리고 퍼브리싱 도구까지 필요에 따라 단계별로 묶어둔 docker image가 있더라. 이름하여 `rocker`!

[https://www.rocker-project.org/](https://www.rocker-project.org/)

정말 환상적인 프로젝트인 것이다! 

## Script 

- wsl 2를 가동한다. 
- docker edge edition 2.3이 설치되었다고 가정한다. 
- `rocker`의 버전은 다음과 같다. 

[https://github.com/rocker-org/rocker-versioned](https://github.com/rocker-org/rocker-versioned)

- `r-ver`: R 프로그램과 핵심 유틸 
- `rstudio`: `r-ver` + RStudio 
- `tidyverse`: `rstudio` + tidyverse 
- `verse`: `tidyverse` + 퍼브리싱 툴 

적당한 이미지를 끌어다가 올리면 되겠다. 

### Best practice 

```shell
sudo docker run -d --rm -p 8787:8787 -v /mnt/|your-local|:/home/rstudio -e DISABLE_AUTH=true rocker/tidyverse
```

- 위 페이지에 보면, 도커의 여러가지 실행 옵션이 있다. 필요대로 참고하시면 되겠다. 
- 위 스크립트는 로그인을 생략한 버전이다. 공용 PC가 아니라면, 로컬에서 돌리는 데 로그인이 필요하지는 않을 듯 싶다. 
- 위 스크립트는 `tidyverse`를 태우는데 wsl 2가 마운트한 로컬 폴더 중 rocker와 연결시킬 폴더를 지정한다. 이 윈도 폴더가 콘테이너 내에 `/home/rstudio`로 매핑된다. 
- 브라우저에서 `localhost:8787`을 치면 Rstudio가 돌아간다. 

## Good Enough? 

솔직히 윈도에서 쓰는 R + RStudio 조합보다 좋더라. 

- 인코딩 문제 없다! 
- 패키지 깔 때 권한 이슈 없다. 
- 퍼포먼스가 원도보다 낫더라. 
- 윈도의 로컬 폴더와 연결되어 있으니, 코드는 따로 저장하고 관리하면 된다.
- 패키지 깔다가 문제가 생기면 그냥 도커 내리면 그만이다.  

wsl 2는 찐이다. 마소 짱! 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQxNzQ3ODY2OCwxNjIwNjgzMTcwLDc5ND
kxNjQ3OCwtMTY4NTkyMDY1NCwtMTU0NzQxMDc2MCwxMjYwODgw
NzY2XX0=
-->