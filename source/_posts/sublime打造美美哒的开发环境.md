title: sublime打造美美哒的开发环境
date: 2016-02-26 11:09:13
tags: sublime
categories: 工具
thumbnail: http://7xn0vc.com1.z0.glb.clouddn.com/sublime_icon.jpg
---
最近折腾了下sublime,同事说很漂亮，就分享一下。
先上美图镇楼：
 {% asset_img 1.png 高清美图一%}
 <!-- more -->
 {% asset_img 2.png 高清美图二%}
 {% asset_img 3.png 高清美图三%}

美美哒的sublime Text主题再加上半透明效果，看起来真是赏心悦目有木有.
好了，废话少说，先看用到的插件：

- `Material Theme` 传送门:[https://github.com/equinusocio/material-theme]
- `SublimeText Transparent` 传送门:[https://github.com/vhanla/SublimeTextTrans]

`Material Theme`可以直接在sublimeText `install package`里安装,装完如果效果不理想，请在`Preferences`->`Setting User`里添加相应配置代码，我的如下：
```
{
	"color_scheme": "Packages/Material Theme/schemes/Material-Theme.tmTheme",
	"font-face": "monaco",
	"font_size": 12,
	"ignored_packages":
	[
		"Vintage"
	],
	"material_theme_accent_orange": true,
	"theme": "Material-Theme.sublime-theme"
}

```

`SublimeText Transparent` 貌似需要`git clone`下来,然后`Preferences`->`Brower Packages...`打开`Packages`目录,拷贝进去sublimeText就半透明了。

这样配置不仅比较好看，而且配合`live load`，可以把网页或者模拟器之类的直接放在IDE下面，实时调试，头都不用扭啊。尤其是在那种一台显示器显示着设计稿，文档，一台放IDE,还要预览的情况下。

比如这个样子：
{% asset_img 2.gif %}
或者这个样子:
{% asset_img 3.gif %}
