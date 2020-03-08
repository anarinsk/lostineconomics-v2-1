---
layout: post
toc: false
comments: true
title: Free Your Blog with Github
description: github에서 무료 블로그를! 
categories: [coding-tool, web-tool]

---

## tl; dr 

* 깃헙에서 가장 쉽게 블로그를 빌드할 수 있는 방법을 알아보자. 
* 디자인은 포기하시라, 마. (지금 만으로도 너무 훌륭하니까!)

## Blog with Github 

깃헙을 통해 static web 기반의 블로그 / 홈페이지를 운영할 수 있다. 많은 분들이 그렇게 하고 있다. 블로그를 운영하기 위해서는 Hugo, Jekyll의 도구를 사용하면 전문적인 블로그 운영도 가능하다. 이 툴들의 경우 관리를 위해서는 약간의 번거로움을 감수해야 한다. 

Hugo의 경우 R의 [BlogDown](https://bookdown.org/yihui/blogdown/) 패키지를 활용할 수 있다. Jekyll의 경우 Ruby 기반으로 제작되어서 블로그 관리를 위해서는 local에서 해줘야 하는 작업이 꽤 많다.[^1]

[^1]: 자세한 내용은 [여기](https://help.github.com/en/github/working-with-github-pages/setting-up-a-github-pages-site-with-jekyll)을 참고하라.  

이상적인 형태이 블로그 툴이랄 무엇일까? 내 생각은 이렇다. 

1. 최초의 설정 및 초기 디자인 요소를 제외하면 코드를 수정할 필요가 없어야 한다.
2. html을 되도록 쓰지 않고, md 형태로 작성한 후 바로 반영이 되어야 한다. 
3.  무료로 제한 없이 호스팅이 가능해야 한다. 

3은 깃헙으로 가볍게 해결된다. 1, 2는 사실 좀 어려운 부분이었다. 깃헙을 블로그 툴로 쓰기 위한 트레이드오프랄까... 

이 글에서 소개할 `fastpages`는 1,2를 구현해주는 이상적인 서비스다. 게다가 `fastpages`는 ipynb 형태의 노트북, word 파일도 알아서 블로그 페이지로 바꿔준다. 문송한 나로서는 참으로 반가운 서비스다.

## fastpages by fast.ai 

일단 별도의 인스톨 과정 필요 없다. 사전에 준비해야 할 것은 깃헙 계정 뿐이다. 

### 페이지 생성 

[https://github.com/fastai/fastpages/generate](https://github.com/fastai/fastpages/generate)

![]({{ site.baseurl }}/images/fastpages/fig_1.png){: style="textalign:center; " width="700"}


깃헙에 로그인한 상태에서 이 곳에 접속해 페이지를 생성한다. 페이지를 생성한 후 내 깃헙 계정에서 조금 기다리면 "Pull Request"(PR) 메시지가 날아온다. 

![]({{ site.baseurl }}/images/fastpages/fig_2.png){: style="textalign:center; " width="700"}

### PR

PR을 하기 전에 SSH 키를 생성하는 작업을 해줘야 한다. 메시지에 친절하게 설명이 되어 있으니 그대로 따라하면 된다. 

![]({{ site.baseurl }}/images/fastpages/fig_3.png){: style="textalign:center; " width="700"}

1. 링크를 눌러 웹 페이지에서 private key와 public 키를 생성한다. RSA, 4069를 선택하도록 하다. 
2. 두번째의 링크를 눌러 새 sceret을 생성하고 private key를 넣어준다. 이름은 반드시 `SSH_DEPLOY_KEY`로 해야 한다. 
3. 세번째의 링크를 눌러 deploy key를 생성하고 여기에 public key를 넣어주면 된다. 이름은 임의로 지정하면 된다. 

마지막으로 PR를 수락하고 이를 merge하면 준비가 완료된다. 깃헙이 작업을 하는 동안 잠시 기다리면 된다. 작업 상태가 궁금하면 github actions 탭을 확인하면 된다. 설치가 끝나면 아래와 같이 생성된 페이지를 확인할 수 있다. 

![]({{ site.baseurl }}/images/fastpages/fig_4.png){: style="textalign:center; " width="700"}

[https://anarinsk.github.io/test-fastpages/](https://anarinsk.github.io/test-fastpages/)

## Customize yours 

이제 각자의 취향에 맞게 몇 가지 커스터마이즈를 거치면 된다. 대체로 `md`를 고치면 되기 때문에 크게 어려운 대목은 없다. 

-  home
	- repo root의 `index.md`를 적당히 수정하면 된다. 

- about
	- `[repo root]/_pages/about.md` 를 수정하면 된다. 
 
- comments 
	- utterances 앱을 깔면, github의 issue 기능을 활용해 코멘트를 관리할 수 있다. 
	- [인스톨 페이지](ttps://github.com/apps/utterances)의 안내대로 하면 된다. 
	- 별도로 뭘 까는 것은 아니고 github 계정의 해당 repo를 설정하면 끝. 

- favicon 
	- `[repo root]/images`의 favicon.ico를 고치면 된다. 
 
- 그림 관리 
	- `[repo root]/images`에 적절한 디렉토리를 만들어 관리하면 된다. 
	- 그림 링크는 `![]({{ site.baseurl }}/images/git-cmder/fig_3.png)` 식으로 `{{ site.baseurl }}`로 시작하면 된다. 

- css 
	- css를 다룰 줄 안다면 블로그의 많은 것은 커스터마이즈할 수 있다. 
	- `[repo root]/_sass/minima`안에 보면 `custom-styles.scss`와 `fastpages-styles.scss` 두 개가 있다. `custom-styles.scss`에 부가 내용을 수정하면 스타일을 바꿀 수 있다. 
	- 예를 들어 기본 텍스트의 크기를 바꾸고 싶다면, 
	
```css
.post-content  p, .post-content  li {
    font-size: 17px; # 원래 값은 20px
	color: #515151;
	}
```
- google analytics 
	- 먼저 google analytics id가 필요하다. 알아서 발급 받으시라.  
	- 구글 페이지에도 안내가 되어 있지만, `gs` 모듈을 활용하기 위해서 `[repo root]/_include`에 `google_analytics.html`과 같은 파일을 만들어 아래의 내용을 넣는다. 물론 자신의 analytics id로 바꿔 넣어야 한다. 

```html
<!-- Global site tag (gtag.js) - Google Analytics -->
<script  async  src="https://www.googletagmanager.com/gtag/js?id=[your-ga-id]"></script>
<script>
	window.dataLayer = window.dataLayer || [];
	function  gtag(){dataLayer.push(arguments);}
	gtag('js', new  Date());
	gtag('config', '[your-ga-id]');
	gtag('set', {'user_id':  'USER_ID'}); // 로그인한 User-ID를 사용하여 User-ID를 설정합니다.
</script>
```

- `[repo root]/_include`의 `head.html`의 적당한 줄에 `{%- include google_analytics.html -%}`를 넣어준다. 

- google analytics의 실시간 항목에서 작동 여부를 확인할 수 있다. 

## 활용 

### Post 작성 

통상적인 `md` 파일로 작성하면 된다. 두 가지만 주의하면 된다. 우선 파일 이름을 `년도-월-일-이름.md` 형태로 넣어야 한다. 그래야 fastpages가 컴파일을 할 수 있다. 예를 들어 이 포스팅을 `2020-03-7-blogging-with-fastpages.md`다. 

글 앞에 yml 명령어를 넣어 해당 포스트에 관한 정보를 넣어주면 된다. 

```yml 
---
layout: post # 글의 레이아웃, 보통이라면 post로 두면 된다. 
toc: false # 목차 출력 여부 
comments: true # 코멘트 기능 사용 여부 
title: Free Your Blog with Github # 제목 
description: github에서 무료 블로그를! # 제목 아래 부제목 
categories: [coding-tool, web-tool] # tag 혹은 카테고리 
---
```

### fastpage의 몇가지 장점 

* 보통 github 기반의 블로그를 만들면, `"자기ID".github.io`만을 주소로 가지게 된다. fastpages를 쓰면 repo 수준의 홈페이지를 운영할 수 있다. 예를 들어, 이 블로그의 주소는 `anarinsk.github.io/lostineconomics-v2-1`다. 
* 강력한 장점은 `ipynb` 확장자의 노트북 파일을 그대로 포스팅으로 바꿔준다는 것이다. `[repo root]/_notebooks`에 파일을 넣어주면 된다.  
* 디자인은 포기하라. css나 html을 잘 안다면 커스터마이즈할 여지가 있지만, 그럴 수 있는 사람이라면 Hugo나 Jekyll을 직접 쓰는 편이 나을 수도 있겠다. 
* 매번 markdown을 에디터에 올려쓰는 것이 불편하다면 웹 에디터를 활용할 수 있다. 
	* [stackedit](https://stackedit.io/app#)의 경우 github 저장을 지원하기 때문에 해당 repo의 `_posts/` 아래의 md 문서를 동기화해두면 웹에서 수정 후 동기화하는 것만으로도 포스트의 수정을 쉽게 할 수 있다. 
	* fastpages의 경우 commit이 발생하면 자동으로 블로그의 빌드에 들어간다. 
	* 따라서 웹 에디터에서 글을 수정한 후 적절한 주소를 지정해주고 동기화를 하면, 즉 커밋을 하면 바뀐 내용을 반영해 블로그가 다시 빌드 된다.



<!--stackedit_data:
eyJoaXN0b3J5IjpbMTY5MzY3NjE3NSw2NDIyMTQxODksMTQyOT
AwNzcxLC0yMzQzNDY1OTYsLTIwMTE1NTI3OTYsLTQzNTkyNjI0
MCwtMTk0MjkxODE5NCwxNjc3NzM0MDc1LC04ODA3NjQ1ODYsLT
E0ODI3NTk5NzAsLTYxOTk5MTcxNCwyMDU4MjQ2NzEyLDE0NDUw
ODAyOTQsLTQ1MjU5OTY2MCwxMjUyMjAwNzM3XX0=
-->