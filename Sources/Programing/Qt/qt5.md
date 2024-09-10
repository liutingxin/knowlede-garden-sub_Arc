---
date created: 2024-06-13
title: Qt课程
---

---

### One

Qwidget 是 所有窗口基类

---

### Two

信号槽

---

### Three

groupBox: 组件组合

checkBox and radioButton

plainTextEdit

组建窗口的文字属性

水平布局、垂直布局

图形化创建信号槽，这种适合简单的信号处理，如确定退出等。

代码创建信号槽 ---> go to slot. 这种方式会在对应窗口基类下的 **private slot** 创建一个函数，然后转到函数可看到实际的代码

也可以手动在**private slot**创建信号槽函数

通过QObject::connect(btn, SIGNAL(clicked()), this, SLOT(setTextFontColor())) 将信号与槽关联

QFont获取qt中的字体

QPalete获取qt中的配色

---

### Four

纯代码编写，需要包含对应的头文件，QCheckBox, QRadioButton, QPushButton…….

没有generate form： 没有ui文件，没有ui_xxx.h文件，类中没有定义ui的指针。

正确显示中文： QString::fronlocal8bit("中文");

tr函数用户国际化翻译

---

### Five

res资源文件，放置图标

setCentraWidget(ui->textEdit);
