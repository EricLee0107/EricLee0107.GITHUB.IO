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
3. 在主题中配置Gitment,因为yilia主题已经适配了Gitment，所以只需要进入到主题的`_config.yml`中，然后在Gitment栏填写相应的信息，

```

gitment_owner:       #你的 GitHub ID
gitment_repo:''          # 这里就是你在注册时设置的HomePage URL项目的名字
gitment_oauth:
  client_id: ''           #client ID 注册完之后可以在注册完成界面看到
  client_secret: ''       #client secret
```

Gitment新添加好之后进入博客后会发现没有评论栏，但会有一个评论初始化的按钮，这个按钮只有第一次进入的时候有，点一下就有评论栏了，但如果有很多博客每个都要点一次也是很麻烦的。还好我才刚开始！不过作者也有说要在之后的版本中添加批量初始化的脚本！

2018.03.22添加

#### 设置多端同步

**参考文章**

[如何解决github+Hexo的博客多终端同步问题](http://blog.csdn.net/Monkey_LZL/article/details/60870891)

##### 1.提交源文件

在博客项目上新建分支来保存博客的源码

```git

git init //初始化本地仓库
git add source //将必要的文件依次添加
git commit -m"log"
git branch hexo //新建hexo分支
git checkout hexo //切换到hexo分支
git remote add origin 你的github地址(git@github.com:yourname/youname.github.io.git)youname 换成自己的github用户名
git push origin hexo //push 到项目的hexo分支

```

我同步到hexo分支的内容包括 source文件夹、scaffolds文件夹、themes(这个是主题文件的文件夹，可以只同步自己需要的，注意：如果是从github上git下来的，需要删除掉相应主题文件夹内的.git文件）、`_config.yml`(这个要同步，要不还要重新设置）、packge.json、.npmignore（这个不知道为什么，又知道的可以告诉我^0^）。

提交完成后其实就是你已经把你本地的基础设置和文章的markdown文件保存到了分支上了。后续其他设备只需要从分支上clone下来就行了，日常使用只需要每次更新source文件夹就能保持多段同步了。

##### 2. 其他设备上第一次使用hexo

前提条件：安装了 git， nodejs

1. 先从分支上clone下来hexo分支 
`git clone -b hexo 你的github地址` 
2. 进入到刚刚clone下来的文件夹内,然后安装hexo，这样的好处是不需要再init了
`cd youname.github.io`
`npm install hexo-cli -g`
3. 到此就算完成了，新建个笔记验证一下
`hexo new test`
4. 之后就是需要将新添加的笔记同步到分支上了，因为只是在source中增加了源文件，所以只同步source文件就可以了。
`git add source`
`git commit -m "add test"`
`git push origin hexo //更新分支`

4. 更新文章到博客
`hexo d -g`

中间如果没有其他错误的话，此时博客中应该已经有了这篇文章。



##### 3. 不同设备都配置完成了之后

```
git pull origin hexo //先pull，完成本地与远端的同步
hexo new 文章标题
git commit -m"提交新文章"
git push origin hexo //提交分支
hexo d -g // 更新博客

```



#### Nodejs 安装

linux 建议直接下载编译好的二进制文件，然后自己配置下环境变量将bin目录加到环境变量中。





```






