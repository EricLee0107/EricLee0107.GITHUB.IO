---
title: linux下的压缩和解压
date: 2018-01-17 22:55:06
tags: [linux学习]

---

> 自己总记不住一些文件的解压命令，特备份自己看。

**解压**

`tar -xvf file.tar` 解包tar包
`tar -xzvf file.tar.gz` 解压tar.gz包
`tar -xjvf filt.tar.bz2` 解压tar.bz2包
`tar -xZvf file.tar.Z` 解压tar.Z包
`unrar e file.rar` 解压rar包
`unzip file.zip` 解压zip包
`tar -xJvf file.tar.xz` 解压tar.xz包

<!-- more-->

**压缩**

`tar cvf file.tar DirName` 将DirName文件打包为file.tar
`tar zcvf file.tar.gz DirName` 打包压缩为file.tar.gz
`tar jcvf file.tar.bz2 DirName` 打包压缩为file.tar.bz2


**参数详解**

`-c` 建立一个压缩文件的参数指令 !--create
`-x` 解压一个压缩文件的参数指令 !--extract
`-t` 查看tarfile里的文件
*注意：这三个参数只能存在一个，不可同时存在。

`-z` 是否使用gzip压缩
`-j` 是否使用bzip2压缩
`-v` 显示过程中的详细信息
`-f` 使用文档名，在f之后要立即接文档名（即f需要作为最后一个参数）
`-p` 使用原文件的原来属性
`-P` 使用绝对路径来压缩






[参考博客1](https://www.cnblogs.com/lovevivi/archive/2013/09/04/3300950.html)
[参考博客2](https://www.cnblogs.com/eoiioe/archive/2008/09/20/1294681.html)
[参考博客3](https://www.cnblogs.com/perfy/archive/2012/05/12/2497074.html)
