---
title: hexo + github创建个人博客中遇到的一些问题
date: 2017-12-30 22:04:32
tags: 博客搭建

---
关于博客的创建可以参考下面这两篇博客：
[使用hexo+github搭建免费个人博客详细教程](http://blog.haoji.me/build-blog-website-by-hexo-github.html?from=xa)
[史上最详细的Hexo博客搭建图文教程](https://xuanwo.org/2015/03/26/hexo-intor/)
我这里写的只是我自己在搭建过程中遇到的一些问题。
也以此篇博客来开启我的博客航程~
<!-- more-->

#### 前期准备过程

**域名申请**
我是在阿里上申请的域名，虽然很多人说很坑，但目前我只遇到了一个。
在实名认证之后，发现域名中出现出现"注册局设置暂停解析（serverHold)"然后参考下[主册局设置暂停解析](http://blog.csdn.net/xudailong_blog/article/details/78756358)这篇博客最终得以解决。原因是因为通讯地址和身份证上不一致导致的，所以尽量在申请时所有信息和身份证一致。
如果你已经出现了这个问题，那需要通过过户来重新进行实名认证。

**下载node.js**
因为浏览器一直开着翻墙，第一次的下载地址是国外官网，下载速度超级慢，所以一定要在国内地址下载。[国内地址](http://nodejs.cn/download/)

#### 主题设置过程

之前看别人博客时经常遇到yilia这个主题，一直觉得很赞，所以果断选择这个。
换头像的话先进入到主题目录(themes/yilia) ,打开`_config.yml`  找到 `avatar` 关键字，或者看注释就可以找到换头像的位置，在关键字后输入你的头像路径 ， 我的是在主题文件夹下的img里存放的头像图片所以是，`avatar: /img/touxiang.jpg`


#### 添加评论系统
2018-01-15添加

最开始没有添加评论，但觉得没有评论就没有了交流，所以决定添加评论。于是就上网搜了下别人都是用的什么，发下大多数人之前都用的是多说，所以大部分教程也都是关于多说的。然而悲催的是现在多说已经关闭的，再有就是畅言需要域名备案，放弃！网易云人们吐槽比较严重，放弃！最后决定用Disqus，使用了两天之后发现网站访问变慢，而且国内无法看到。就有找了一些用hexo搭建的博客都用的什么评论插件，发现了Gitment。

##### Gitment的配置

1. 首先需要注册一个 OAuth application，注册地址，[点这里跳转](https://github.com/settings/applications/new)
2. 注册时主要填三个东西 `Application Name` 随便填;`HomePage URL`保存评论的项目地址，我是新建了一个项目作为博客评论的项目（也可以先随便填一个，但必须已有的项目，之后还可以改）;**最后一个参数要特别注意`callback URL`这要填自己的博客地址比如:`lizhiwei.net`
3. 在主题中配置Gitment,因为yilia主题已经适配了Gitment，所以只需要进入到主题的_config.yml中，然后在Gitment栏填写相应的信息，

```
#5、Gitment
gitment_owner:       #你的 GitHub ID
gitment_repo:''          # 这里就是你在注册时设置的HomePage URL项目的名字
gitment_oauth:
  client_id: ''           #client ID 注册完之后可以在注册完成界面看到
  client_secret: ''       #client secret
```

Gitment新添加好之后进入博客后会发现没有评论栏，但会有一个评论初始化的按钮，这个按钮只有第一次进入的时候有，点一下就有评论栏了，但如果有很多博客每个都要点一次也是很麻烦的。还好我才刚开始！不过作者也有说要在之后的版本中添加批量初始化的脚本！

