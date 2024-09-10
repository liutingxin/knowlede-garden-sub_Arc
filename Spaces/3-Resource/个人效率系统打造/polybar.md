---
date created: 2024-09-09
date modified: 2024-09-09
title: 状态栏
---

以我给出的配置为例，首先查看launch.sh文件，会发现代码中执行的第一个文件是config-top.ini

config-top.ini 是一个具体的配置框架，将其他的一些已有的配件整合在一起

include-module.ini 包含的是导入的文件配置

fonts.ini 包含的是 polybar 状态栏中，使用到的所有字体

module目录下，包含的是具体的模块，同名的py文件，是通过py文件执行相关的代码程序，从而将相关的信息输出到这个模块中，注意若想使用对应的模块功能，可能需要安装一些python软件包

其他详细信息，可以查看polybar官方仓库下的WIKI
