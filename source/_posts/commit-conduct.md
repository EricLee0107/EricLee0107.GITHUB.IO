---
title: commit_conduct
copyright: true
toc: true
date: 2018-05-03 12:01:06
tags:
---

SVN 和 git提交模板配置
<!-- more-->

#### 通过TortoiseSVN设置svn commit模板，步骤如下（转）：

1. 在SVN所在的文件夹即项目(网络上是全体的，本地是只针对自己)，右键TortoiseSVN，选择Properties(属性)
2. 在弹出的界面中，选择new...(新建...)，然后选择Other
3. 在弹出的界面中，Property name项选择tsvn:logtemplate，然后在Property Value中填入模板
【提交类型】:BUG/新功能/需求修改/版本制作/代码整理/解决编译不过/阶段性提交/追加提交
【问题描述】:该单的描述，从devtrack中复制过来或从功能性对本次修改的描述
【程序描述】:无(原因分析或者是对修改的技术性描述)
【修改内容】:
【相关单号】:无
【需要测试】:是/否
4. 确定即可


#### 通过TortoiseGit设置git commit模板(转)

右键项目—>TortoiseGit—>Setttings；在做菜单中选择Git—>Edit global .gitconfig
![打开设置](http://p6sqos6o3.bkt.clouddn.com/TorGitSetting.png-lzw)
添加相应的模板路径，具体格式如下图 
![模板路径设置](http://p6sqos6o3.bkt.clouddn.com/gitcommitglobalconfig.png-lzw)
<font color=red> 注：路径应为两个反斜杠或斜杠进行分割。</font>


