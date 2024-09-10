---
tags:
  - Linux软件
  - 字体编辑
作用: 编辑字体文件，例如ttf
title: FontForge
date created: 2024-08-07
date modified: 2024-08-07
---

官网：[FontForge Open Source Font Editor](https://fontforge.org/en-US/)

>在编辑字体文件前，最好备份一下，以防万一。

简单编辑一个ttf文件，首先导入ttf文件，然后双击某个字符，即可直接打开对应的编辑界面。

>编辑常用的部分操作是，<mark style="background: #FF5582A6;">缩小和放大</mark>，<mark style="background: #FF5582A6;">字符的左右边距</mark>，<mark style="background: #FF5582A6;">设置字符的宽度</mark>等。

打开编辑字符界面后，看到了三条线，中间的是基线，通常排版一行文字时会把各字形的基线对齐。上面两条线间的距离称为上高（详细信息请看：[用FontForge设计字体 | 陈颂光](https://www.chungkwong.cc/font.html)）。

接下来直接进入编辑操作，我想要把某个非等宽字符，修改成等宽字符。

一种可行的方式是，直接鼠标拖动字符上的线条调整。

另一种方式是通过前面讲的操作，C-S-L可以设置字符的整体显示区域宽度，C-\\设置字符本身的缩放操作，然后打开顶部菜单栏上的度量，在弹出的二级菜单中选择：设置两侧边距即可， 接下来C-w关闭当前这个编辑界面，最后C-G导出这个编辑后的字体文件即可，也可以通过C-s保存这个当前的字体模板。
