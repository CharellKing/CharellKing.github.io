---
title: 使用gimp制作透明图标
date: 2014-08-28 19:52 +0800
layout: post
permalink: /blog/2014/08/28 make-transparent-icon.html
comments: true
categories:
  - 绘画
tags:
  - gimp
  - 透明图标
---
本博客的圆形图标就是用gimp制作出来的。首先看看作图的最终效果，我这里把我制作的灰太郎的图标贴出来。
<table  border="0" cellpadding="0" cellspacing="0" align="center" bordercolor="#000000">
  <tr>
    <td vertical-align:middle> ![初始图片](/assets/picture/灰太郎1.jpg) </td>
    <td vertical-align:middle> ![结果图片](/assets/picture/灰太郎2.png) </td>
  </tr>
</table>

* 利用gimp的导出功能将图片转化为png格式。用gimp打开图片，点击【文件=》导出】，将文件命名为以png结尾的名字。之所以转化为png格式，是因为png格式支持ALPHA通道，透明的效果才能显示出来。

* 选定去掉的部分。用gimp打开转化好的png图片，用选择工具【椭圆，矩形或者任意选择】选定需要的部分，然后点击反转【选择=》反转】，就反向选择我们要去掉的其他部分。

* 去掉选择的部分。点击以前景颜色填充【编辑=》以前景颜色填充，前景颜色用黑色】，然后点击颜色至ALPHA【图层=》透明=》添加ALPHA通道】中设定0x000000.
