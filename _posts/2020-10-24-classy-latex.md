---
layout: post
toc: false
comments: true
title:  LaTeX in Modern Ways
description: $\LaTeX$을 다시 써볼까?
categories: [latex, rstat, doc-tech]

---

박사 논문을 $\LaTeX$(이하 그냥 레이텍 혹은 텍이라고 쓰겠다)으로 썼지만... 역시 안 쓰면 잊는다. 갑자기 텍을 다시 써볼까, 라는 생각이 드는 순간 갑갑하더라. 윈도를 쓰건 맥을 쓰건 용량이 적지 않고 관리가 쉽지 않은 텍라이브를 까는 것이 내키는 일은 아니다. 

- 좀 쉽게 쓸 수 있는 방법이 없을까? 
- 가볍게 쓸 수 있는 방법은 없을까? 

이 두 가지를 놓고 해결책을 찾아봤다. 세 가지 정도 해법이 있다. 

## tinytex with R

R의 문서 관련 패키지를 거의 혼자 만들고 있는 谢益辉(사익휘, 시에-이-휘)가 R에서 텍을 컴파일할 수 있도록 아주 가벼운 패키지 [tinytex](https://yihui.org/tinytex/)을 만들었다. R을 쓴다면, RStudio를 쓸 것이고 편집기로서 역할 역시 훌륭하다. 그냥 RStuido 자체를 텍 문서를 만들고 컴파일하는 용도로 써도 크게 부족함은 없을 것이다. 

```R
install.packages('devtools')
devtools::install_github('yihui/tinytex')
tinytex::install_tinytex()
```

- devtools 패키지를 설치한다. 
- 깃헙에서 tinytex이라는 R패키지를 설치한다. 
- tinytex 패키지를 통해서 tinytex을 설치한다. 

간략하게 잘 컴파일 되는지 확인해보자. 

```r
writeLines(c(
	'\\documentclass{article}',
	'\\begin{document}', 'Hello world!', '\\end{document}'
	), 'test.tex')

tinytex::pdflatex('test.tex')
```

- `test.tex`이라는 파일을 쓰고 
- 예를 pdflatex으로 컴파일 한다. 

### Compling `.tex` directly 

이제 `.tex` 파일을 직접 컴파일 해보자. 테스트할 용도의 파일은 다음과 같다. 적당한 이름으로 아래의 tex 파일을 적당한 디렉토리에 생성하자. 

```latex
\documentclass{article}
\usepackage{kotex}
\begin{document}
Hello world! 한글은 어떠함?
\end{document}
```

`.tex` 파일을 RStudio에서 열면 위에 pdf를 생성하는 버튼을 볼 수 있다. 이걸 실행해보도록 하자. 아마도 컴파일하면 에러를 만나게 될 것이다. 한글 패키지가 제대로 설치되어 있지 않기 때문이다. 텍도 마찬가지로 패키지들을 설치해야 할 때가 많다. 몇가지 방법이 있지만, 편리한 방법을 소개하겠다. 

```r
tinytex::parse_install(
	text = "! LaTeX Error: File `kotex.sty' not found."
)
```

R로 돌아와서 발생한 에러 메시지를 `text`의 인자로 넣으면 어떤 패키지가 필요한지 인식해서 texlive에서 끌어와 설치한다. 설치 후 tex 패키지를 업데이트하자. 

```r
tinytex::tlmgr_update()
```

이제 `tinytex::pdflatex('test.tex')`을 실행하면 pdf가 잘 생성될 것이다. 다시 `.tex` 파일로 와서 pdf 버튼을 눌러보자. 아마도 에러가 뜰 것이다. 방법 패키지를 설치했는데 왜 에러가 뜰까? 해법은 간단하다. 메뉴에서 다음과 같은 순서로 찾아들어가자. 

<kbd>Tools</kbd> > <kbd>Global Options</kbd> > <kbd>Sweave</kbd>

"Use tinytex when compling .tex files"을 체크해주자.

## WSL + Tinytex + VS Code 

사이휘는 tinytex을 R에서만 쓰도록 만들지 않았다. 리눅스 인스톨러를 제공한다. 이 블로그를 보신 분은 잘 알겠지만, 나는 WSL의 광팬이다. WSL에 tinytex을 깔고 녀석을 VS Code에 얹어서 쓰면 된다. 물론 맥OS, 윈도 인스톨러도 제공하니 관심이 있으시면 직접 확인을 하시면 되겠다. [홈페이지](https://yihui.org/tinytex/) 인스톨 섹션에서 확인하면 되겠다. 

WSL 터미널에서 다음과 같이 실행하자. 

```shell
wget -qO- "https://yihui.org/tinytex/install-bin-unix.sh" | sh
```

기타 패키지 설치 등은 `tlmgr` 명령을 활용하면 된다. 위 홈페이지를 참고하면 되겠다. 

VS Code에서는 어떤 확장을 쓰면 될까? [Latex Workshop](https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop)이 PC에 깔리는 에디터 못지 않은 확장 기능을 제공한다.  extension의 `settings.json`을 통해 다양한 컴파일 옵션도 제공하니 자세한 것은 위 페이지의 안내를 참고하면 되겠다. 일단 pdflatex을 쓴다고 가정하자. 

VS Code에서의 활용은 아래 섹션을 참고하자. 

## TexLive with Docker! 

본격적으로 텍을 쓰는 사람이라면 어찌해야 하나? Tinytex은 임시 방편이고 많이 부족해 보일 것이다. Texlive를 온전하게 쓰고 싶다면? 역시 OS 별로 텍라이브를 까는 방법 밖에 없나? 이럴 때 손쉽게 동원할 수 있는 게 docker이다. 사실 unix 기본으로 설계되서 텍을 맥OS에 까는 것도 부담스럽고, 윈도는 더하다. 그냥 필요할 때 불러서 쓰고 치워버리는 게 최고다. 도커는 이런 용도로 쓰기에도 아주 적합하다. 

내가 검색해본 바로는 공인 이미지는 없는 것 같다. 다양한 사용자 이미지 중에서 꽤 관리가 잘되는 것을 골라서 쓰면 되겠다. 내가 활용하는 이미지는 다음과 같다. 

https://hub.docker.com/r/thomasweise/docker-pandoc/

녀석을 도커에 마운트하자. WSL에서 도커를 쓰는 법은 [이 글](https://anarinsk.github.io/lostineconomics-v2-1/wsl/visual-studio-code/docker/2020/04/11/wsl2+docker+vsc.html)을 참고하라. 

```shell
​docker run -v /mnt/[YOUR-LOCAL]:/doc/ -t -i thomasweise/docker-pandoc
```

- WSL이 `/mnt` 디렉토리 아래 윈도의 디스크를 마운트한다. 따라서 저 아래 로컬 폴더의 주소를 넣으면 된다. 예를 들어, C의 doc라는 이름이라면, `c/doc`를 넣으면 된다. 이렇게 두면 해당 폴더가 도커의 `/doc` 아래 연결된다. 

초기에 이것저것 깔아주고 초기화해야 하는 것들이 있다. 이 녀석들을 셸스크립트(`sh`)로 만들어두자. `install_kotex.sh` 라는 스크립트는 아래와 같이 만들어보자. 해당 스크립트는 위에 마운트하는 폴더에 넣어두면 좋다. 그러면 도커 이미지가 올라갔을 때 해당 스크립에 쉽게 접근할 수 있다. 

```shell
#!/bin/sh
tlmgr init-usertree
#
tlmgr repository add https://cran.asia/KTUG/texlive/tlnet/ ktug
#tlmgr pinning add ktug "*"
tlmgr install collection-langkorean
updmap -sys
updmap -sys
tlmgr update --all
``` 

상태를 초기화하고 한글을 까는 명령어들을 모아둔 것이다. 실행 중인 도커 내에서 해당 디렉토리로 이동해서 `./install_kotex.sh`로 명령을 실행하자. 

이제 VS Code로 가자. 필요한 Extension은 앞서 살펴본 Latex workshop과 더불어 [Remote-Container](https://code.visualstudio.com/docs/remote/remote-overview)를 설치하자. 이 녀석을 설치하면 WSL 내에서 돌아가는 컨테이너 안에도 VS Code를 통해 바로 접속할 수 있다. 

 ![]({{ site.baseurl }}/images/latex/vscode_1.png){: style="textalign:center; " width="350"}  

- 아래 `><`으로 된 부분을 클릭하면 상단에 메뉴가 뜬다. 

 ![]({{ site.baseurl }}/images/latex/vscode_2.png){: style="textalign:center; " width="550"}  

- "attach to running container"를 실행해서 실행중인 도커 콘테이너를 부착한다. 

 ![]({{ site.baseurl }}/images/latex/vscode_3.png){: style="textalign:center; " width="350"}  

- 부착하면 새 창이 뜨면서 아래 녹색 창이 콘테이너 부착되었음을 알려준다. 
- 콘테이너 내부에 Latex Workshop 확장(extension)을 설치해야 한다. 

 ![]({{ site.baseurl }}/images/latex/vscode_4.png){: style="textalign:center; " width="350"}  

- 설치 후 해당 Extension의 설정을 고쳐주자. 
- 위에 찾기창에서 디폴트로 떠 있는 내용 뒤에 "pdf" 치면 pdf에 관련된 설정만 필터링된다. 

 ![]({{ site.baseurl }}/images/latex/vscode_5.png){: style="textalign:center; " width="550"}  

- 위 항목을 찾아서 적당한 포워딩 포트를 넣어준다. 
- 포트가 잘 포워딩 된다면, 아래와 같이 VS Code에서 PDF 뷰어를 활성화할 수 있다. 
- 포트 포워딩 설정은 Latex Workshop 확장의  미비함 때문에 필요하다. 이후 해당 확장의 업데이트를 통해 해결될 것으로 기대한다. 

### Activating PDF Viewer

확장의 PDF 뷰어의 작동 방식 때문에 포트포워딩 선택을 해줘야 한다. 

#### Port Forward 

일단 확장 설정에서 적당히 포워딩될 포트를 설정해야 한다. 

#### Container Setting 

콘테이너에서 해당 포트가 잘 포워딩되고 있는지 확인하자. 

 ![]({{ site.baseurl }}/images/latex/vscode_7.png){: style="textalign:center; " width="350"}  

- 위 그림처럼 35556 포트를 통해 잘 포워딩되고 있음을 알 수 있다. 
- 옆에 원으로 표시해 둔 곳은 `Remote-Container` 확장으로 바로 가는 단축 아이콘이다. 

이제 VS Code에서 텍 파일을 열고 문서를 작성하고 컴파일하면 된다. 

 ![]({{ site.baseurl }}/images/latex/vscode_6.png){: style="textalign:center; " width="1200"}  

## References 

- [tinytex](https://yihui.org/tinytex/)
- [texlive 도커 이미지](https://hub.docker.com/r/thomasweise/docker-pandoc/)
- [kotex 리눅스 설치](http://wiki.ktug.org/wiki/wiki.php/LinuxInstall2014)
- [VS Code Remote-Cotainer 포트 포워딩](https://code.visualstudio.com/docs/remote/ssh#_temporarily-forwarding-a-port)
- 컴파일에 활용된 파일은 여기를 참고하라. 
	- [https://github.com/anarinsk/test-vscode-latex]( https://github.com/anarinsk/test-vscode-latex)







 













<!--stackedit_data:
eyJoaXN0b3J5IjpbOTEyMTMwMjg2LC03Nzk3NjM1MjgsLTE5MD
gzNjk1NTEsMTkzMTI2NDk1Myw4MTIxNDI3NjMsLTE2OTg1NTg1
NDYsMzA5NzA2ODM1LC00NjkxOTUxNDUsLTEwMzYzMTY5MiwtMT
I1MTk4Mzk0NywtMTQ2NDIyMzAyMywxNDc0Mzk1NjY3LDUyNzE3
NTIzLC0yMDc4Mzg2MDAzLDQyMTc1MjIzN119
-->