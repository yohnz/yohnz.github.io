title: hexo NEXT主题打开慢的问题
date: 2016-02-17 19:07:24
tags: Hexo
categories: 前端
---

今天把博客部署到coding.net上，想加快国内访问速度，部署之后发现依然很慢，F12了一下，发现了这货：
{% asset_img 111.png 图一%}
原来引用了google字体库，21秒。。。卡的就是你啊。。
<!-- more -->
于是全局搜索`fonts.googleapis.com`，找到如下引用代码：

```
{% if theme.use_font_lato %}
  <link href="//fonts.googleapis.com/css?family=Lato:300,400,700,400italic&subset=latin,latin-ext" rel="stylesheet" type="text/css">
{% endif %}
```
这里我们把google节点改成国内的CDN，即把`fonts.googleapis.com`修改成`fonts.useso.com`
```
{% if theme.use_font_lato %}
  <link href="//fonts.useso.com/css?family=Lato:300,400,700,400italic&subset=latin,latin-ext" rel="stylesheet" type="text/css">
{% endif %}
```
更新完之后，再打开博客，爽的飞起有木有：
{% asset_img 222.png 图二%}

