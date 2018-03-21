---
title: Centos 7 YCM从头开始安装记录
date: 2018-01-31 21:25:00
tags: [Vim]

---

####Vundle安装
1. 下载Vundle `git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim`
2.  这里先用Vundle提供的配置直接替换自己的.vimrc。（可以直接从[Vundle页面](https://github.com/VundleVim/Vundle.vim)复制）
```vim
set nocompatible              " be iMproved, required
filetype off                  " required

" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
" alternatively, pass a path where Vundle should install plugins
"call vundle#begin('~/some/path/here')

" let Vundle manage Vundle, required
Plugin 'VundleVim/Vundle.vim'

" The following are examples of different formats supported.
" Keep Plugin commands between vundle#begin/end.
" plugin on GitHub repo
Plugin 'tpope/vim-fugitive'
" plugin from http://vim-scripts.org/vim/scripts.html
" Plugin 'L9'
" Git plugin not hosted on GitHub
Plugin 'git://git.wincent.com/command-t.git'
" git repos on your local machine (i.e. when working on your own plugin)
Plugin 'file:///home/gmarik/path/to/plugin'
" The sparkup vim script is in a subdirectory of this repo called vim.
" Pass the path to set the runtimepath properly.
Plugin 'rstacruz/sparkup', {'rtp': 'vim/'}
" Install L9 and avoid a Naming conflict if you've already installed a
" different version somewhere else.
" Plugin 'ascenator/L9', {'name': 'newL9'}

" All of your Plugins must be added before the following line
call vundle#end()            " required
filetype plugin indent on    " required
" To ignore plugin indent changes, instead use:
"filetype plugin on
"
" Brief help
" :PluginList       - lists configured plugins
" :PluginInstall    - installs plugins; append `!` to update or just :PluginUpdate
" :PluginSearch foo - searches for foo; append `!` to refresh local cache
" :PluginClean      - confirms removal of unused plugins; append `!` to auto-approve removal
"
" see :h vundle for more details or wiki for FAQ
" Put your non-Plugin stuff after this line
```
#### 安装YCM
1. 然后在`call vundle#begin()`和`call vundle#end() `之间加上`Plugin 'Valloric/YouCompleteMe'`。
2. vim中运行`:PluginInstall`安装插件。

此时可以看到有个需要vim版本7.4.158+的提示，所以安装最新的vim

3. 下载libclang,因为我需要c-family的支持，所以下载libclang，作者建议使用官网编译好的二进制包。

 - 安装cmake:`yum install cmake`
 - 安装python-devel:`yum install python-devel`
 - 从官网下载LLVM-Clang二进制包，版本要求3.9以上.[LLVM官网](http://releases.llvm.org/download.html)		
		我的系统是Centos.
		`wget http://releases.llvm.org/5.0.1/clang+llvm-5.0.1-x86_64-linux-gnu-Fedora27.tar.xz`

4.  根目录下建立build目录和temp目录。
		
```	
cd ~
mkdir ycm_build
mkdir ycm_temp
```

5. 将下载的clang+llvm压缩包解压到`~/ycm_temp/llvm_root_dir`(目录中包含 `bin` `lib` `include`等文件)
6. 进入ycm_build目录`cd ~/ycm_build`，生成配置文件，执行

```
cmake -G "Unix Makefiles" -DPATH_TO_LLVM_ROOT=~/ycm_temp/llvm_root_dir . ~/.vim/bundle/YouCompleteMe/third_party/ycmd/cpp
```
7. 构建ycm_core
```
cmake --build . --target ycm_core --config Release
```


此时运行vim仍然会提示

<font color=blue>The ycmd server SHUT DOWN (restart with ':YcmRestartServer'). Unexpected exit code 1.</fond>

8. 重新编译YCM

进入到Youcomplete目录之后执行`./install.py --clang-completer` 然后在进就提示
<font color=blue>NoExtraConfDetected: No .ycm_extra_conf.py file detected, so no compile flags are available. Thus no semantic suppo...</fond>

到这里就很清楚了，在.vimrc 中添加 
`let g:ycm_global_ycm_extra_conf='~/.vim/.ycm_extra_conf.py'`

`~/.vim/.yvm_extra_conf.py`是我的路径，这里需要填自己的文件所在路径








   

		 
	


#### 安装最新版Vim

1. 下载源码文件`git clone https://github.com/vim/vim.git`
2.  进入到src目录，然后执行`./configure --with-features=huge --enable-multibyte --enable-pythoninterp=yes --with-python-config-dir=/usr/lib64/python2.7/config  --enable-cscope --prefix=/usr/local/vim8`,

其中with-python-config-dir为Python的配置文件路径，如果通过默认安装的一般在`/usr/lib64/pythonX/bin` X为版本号。 如果是自己安装在了其他路径也可以通过，`rpm -ql python | grep config`查找。

如果是python3 则将`pythoninterp`改为`python3interp`,`with-python-config-dir`改为`with-python3-config-dir` 
3. 执行`make`
4. 执行`make install`

