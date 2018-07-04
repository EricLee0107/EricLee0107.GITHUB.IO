---
title: shell_1 在终端中进行彩色输出
author: 李志伟
tags: [shell]
copyright: true
categories:
  - shell学习笔记
toc: true
date: 2018-05-19 16:56:12
---

摘要：终端彩色打印

<!-- more-->

## 终端打印

echo中转移换行（制表符）：`echo -e "1\t2\n3"`,添加`-e`参数可以转义字符。

### 在终端中进行彩色输出

输出颜色是根据“\e[”（转义左中括号）和“m”之间的数值来定义各种效果的。 在一个效果之后的字符串会以当前设置的效果输出，所以一般最后需要`\e[0m`重置回来。

彩色字体颜色码：
    
- 重置：0
- 黑色：30
- 红色：31
- 绿色：32
- 黄色：33
- 蓝色：34
- 洋红：35
- 青色：36
- 白色：37

彩色字体输出：`echo -e"This is \e[31mred\e[0m text"`

输出结果![shell_text_color](http://p6sqos6o3.bkt.clouddn.com/shell_text_color.jpg-lzw)：

彩色背景颜色码：

- 重置：0
- 黑色：40
- 红色：41
- 绿色：42
- 黄色：44
- 蓝色：44
- 洋红：45
- 青色：46
- 白色：47

彩色背景输出：`echo -e "This is \e[42mgreen\e[0m background"`

输出结果:![shell_bg_color](http://p6sqos6o3.bkt.clouddn.com/shell_bg_color.jpg-lzw)


也可以混合使用

`echo -e "This is \e[42m green \e[31mred\e[0m background"`
![shell_green_red_1](http://p6sqos6o3.bkt.clouddn.com/shell_green_red_1.jpg-lzw)

`echo -e "This is \e[42;31mred\e[0m background"`
![shell_green_red_2](http://p6sqos6o3.bkt.clouddn.com/shell_green_red_2.jpg-lzw)