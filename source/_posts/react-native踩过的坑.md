title: "'react-native踩过的坑'"
date: 2016-04-15 12:18:37
tags: react-native
categories: 前端
thumbnail: http://7xn0vc.com1.z0.glb.clouddn.com/Rect_icon.jpg
---

### 小米真机运行白屏
这是因为调试的应用没有“显示悬浮窗”的权限，需要手动开启，开启方法：

1. 打开‘安全中心’应用，点击‘授权管理’，点击‘应用权限管理’
2. 在打开的应用列表里找到调试的应用并点击
3. 点击‘显示悬浮窗’，选择‘允许’

{% asset_img 11.png %}

<!-- more -->

### "lodash/find"错误
这种错误常见于真机运行时出现，而模拟器正常,如图：

{% asset_img 2.png %}

google了许久也没发现解决方案，后来无意中发现，问题的原因是由于开启了`JS Dev Mode`，解决方案：
摇晃手机，点击`Dev Settings`打开调试选项，关掉`JS Dev Mode`选项:

{% asset_img 3.png %}