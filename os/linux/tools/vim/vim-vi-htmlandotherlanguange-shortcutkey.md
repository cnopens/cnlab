# vim-vi-htmlandotherlanguange-shortcutkey.md

***vim plugins manage***

首先，新装的系统只有vi没有vim，虽说vi有编辑功能但不如vim强大，对于我们程序员开发来说一个好的代码编辑器肯定能省下不少时间去学习新事物。那么没有vim的话就安装vim呗。Ubuntu系统终端输入：

sudo apt-get install vim   

centos或fedora系统用yum安装包管理器。

yum install vim

一开始，不会很顺利的，所以要更新下系统的源。怎么更新系统源，下篇再介绍吧。

二、安装好了后，可以在用户目录下创建.vim目录和.vimrc配置文件。终端输入：

cd ~
mkdir .vim
vim .vimrc

cd ~，进入用户目录。vim .vimrc，进入命令行模式，再输入i就可以编辑文本了。编辑内容如下，记得编辑完保存：

set encoding=utf-8	"使用utf-8编码
set number
set showcmd
"set clipboard=unnamed,unnamedplus	"可以从vim复制到剪贴板中
set mouse=a		"可以在buffer的任何地方使用鼠标
set cursorline		"显示当前行
set hlsearch		"显示高亮搜索
"set incsearch
set history=40		"默认指令记录是20
set ruler		"显示行号和列号
set pastetoggle=F3	"F3快捷键于paste模式与否之间转化，防止自动缩进
"set helplang=cn	"设置为中文帮助文档，需下载并配置之后生效

"===============文本格式排版====================
set tabstop=4
set shiftwidth=4	"设置自动对齐的缩进级别
set autoindent		"配合下面一条命令根据不同语言类型进行不同的缩进操作
filetype plugin indent on
"set cindent		"以c语言风格自动缩进
"set smartindent	"自动识别以#开头的注释，不进行换行


"===========================选择solarized的模式========================== 
syntax enable  
syntax on 
"solarzed的深色模式  
"set background=dark 
"solarized的浅色模式 
"set background=light 
"colorscheme solarized        "开启背景颜色模式 
 
"===========================选择molokai的模式============================ 
"let g:rehash256 = 1 
let g:molokai_original = 1    "相较于上一个模式，个人比较喜欢此种模式 
highlight NonText guibg=#060606 
highlight Folded  guibg=#0A0A0A guifg=#9090D0 
"set t_Co=256 
"set background=dark 
colorscheme  molokai

这里编辑器颜色背景我是参照网友的博客上写的，自己也实验过下载molokai的颜色，如下。

下载比较简单：①首先在github上获取这个颜色的主题，终端输入命令获取：

git clone https://github.com/tomasr/molokai.git

②当前目录下会有一个文件夹：molokai，进入到文件夹内部的color目录内，有个molokai.vim文件

③进入之前创建好的.vim目录内，在创建一个colors目录，把刚才那个颜色主题剪切或复制进来就可以了。

cd .vim
mkdir colors
mv ~/molokai/color/molorkai.vim ./colors

到这里，vim的简单配置就可以了。但是有人看到别人的vim都有自动补全功能，这个怎么实现呢。

这需要强大的插件来管理配置了。

①首先安装vundle管理插件。终端输入：

git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim

这样获取到了bundle后，就可以在配置文件.vimrc中编辑bundle的特性。在刚刚的.vimrc文件中添加如下语句：

"===========================Vundle环境设置=================================
set nocompatible	"vim比vi支持更多功能，如showcmd，避免冲突和副作用
filetype off
set rtp+=~/.vim/bundle/Vundle.vim
set rtp+=~/.vim/plugin/clang_complete.vim
"vundle管理的插件列表必须位于vundle#begin()和vundle#end()之间
call vundle#begin()

"避免PluginClean把自己卸载了
Plugin 'VundleVim/Vundle.vim'
"添加clang插件
Plugin 'rip-rip/clang_complete'

let g:clang_complete_copen=1
let g:clang_periodic_quickfix=1
let g:clang_snippets=1
let g:clang_user_options='-std=c99 -stdlib=libc++ -std=c++11 -IIncludePath'
let g:clang_auto_select=1
let g:clang_close_preview=1
let g:clang_complete_macros=1
let g:clang_use_library=1
let g:clang_library_path="/usr/lib/llvm/"
let g:neocomplcache_enable_at_startup=1

"插件列表结束
call vundle#end()
filetype plugin indent on
"安装插件，先找到其在github的地址，再将配置信息加入.vimrc中的call
"vundle#begin()和call vundle#end()之间，最后进入vim执行
":PluginInstall 便可自动安装
"要卸载插件，先在.vimrc中注释或删除对应插件配置信息，然后在vim中执行
":PluginClean便可卸载对应插件
"批量更新，只需执行:PluginUpdate

这就好了么？细心的同学会发现，我这多安装了一个clang插件。这个插件就是C/C++自动补全的，还有语法检测哟。

要安装clang，还需要这几个步骤：

①终端输入安装clang库：

yum install clang

②终端输入获取clang_complete插件：

git clone https://github.com/Rip-Rip/clang_complete.git ~/.vim/

在.vim目录内会多一个complete目录，里面都有clang_complete.vim的配置文件。

③打开vim，输入：

:PluginInstall 

? 这就完事了么？

到这里执行不了。


