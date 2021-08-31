---
layout: post
toc: false
comments: true
title:  Test of Katex in Fastpages
description: Displaystyle is not working properly 
categories: []
---

## Purpose 

This post serves only to show the problem of Katex in fastpages. If the problem is solved, this post disppaers. 

## Setting 

I'm not touch anything in default setting for latest version of fastpage. Using `displaystyle` by using `$$`. 

```
$$
\begin{aligned}
R^*=\underset{RR^t=I,\det(R)=1}{\operatorname{argmin}}\sum_{i=1}^n|RX_i-Y_i\|^2_2.
\end{aligned}
$$
```

This is rendered with default fastpage option in `_config.yml`; `math_engine: null`.
$$
\begin{aligned}
R^*=\underset{RR^t=I,\det(R)=1}{\operatorname{argmin}}\sum_{i=1}^n|RX_i-Y_i\|^2_2.
\end{aligned}
$$

## Problem 

`_config.yml` is set as

```yml
kramdown:
  math_engine: katex
  input: GFM
  auto_ids: true
  hard_wrap: false
  syntax_highlighter: rouge
```

The result is 

![](https://github.com/anarinsk/lostineconomics-v2-1/blob/master/images/etc/with_katex.png?raw=true)

`_config.yml` is set as

```yml
kramdown:
  math_engine: null
  input: GFM
  auto_ids: true
  hard_wrap: false
  syntax_highlighter: rouge
```

The result is 


![](https://github.com/anarinsk/lostineconomics-v2-1/blob/master/images/etc/with_mathjax.png?raw=true)

The equation with `katex` option is not **Italicized**, and the font size is smaller than the one with `null`. 

