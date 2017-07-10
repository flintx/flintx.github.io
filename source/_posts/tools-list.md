---
title: PC程序清单 & 实用小工具
date: 2017-02-04 17:43:17
tags: 
	- 生活 
	- 工具 
	- 软件
---

## 前言

目前用的PC大限将至，即将随本科岁月一起成为过往。

在寿终正寝之前，整理一份陪伴五年时光的程序清单。

<!--more-->

## 清单

---



### 浏览器

> `Google Chrome`

> `Mozilla Firefox`



### 音乐

> `网易云音乐`

### 视频

> `PotPlayer`

### 聊天

> `QQ轻聊版`

### 下载

> `迅雷极速版`
>
> `百度云客户端`
>
> `µTorrent 2.0.4` 

### 游戏

> `Steam`
>
> `TGP`
>
> `UPlay`

### 编程

#### IDE

> **C/C++/C#:**
>
> ​	`Dev C++ 4.9.9.2`
>
> ​	`Code::Blocks IDE 16.1.1.0`
>
> ​	`Microsoft Visual Studio 14.0`
>
> 
>
> **Java:**
>
> ​	`IntelliJ IDEA 15.0.2`
>
> ​	`Android Studio 2.2`
>
> ​	`Eclipse Java Mars`
>
> 
>
> **Python:**
>
> ​	`PyCharm 2016.1`
>
> 
>
> **PHP:**
>
> ​	`PhpStorm 10.0.3`

#### 编辑器

> `Notepad++ 6.9.2` （普通文本处理）
>
> `gVim 7.4`（单文件程序、算法demo编写）
>
> `Sublime text 3`（前端处理）
>
> `Visual Studio Code 1.8.1`（闲置中Orz）
>
> `Typora 0.9.23`（markdown文件）

#### 编译器 & 解释器

> `gcc 4.7.1`

> `jdk 1.8.0_73`

> `Python 3.4 & 2.7`

> `PHP 7.0 & 5.5`

#### 其他工具

> `MySQL`
>
> `Git`
>
> `Node.js`
>
> `Apache 2.4`
>
> ...

### 学习/研究

> `Microsoft Office 2016` （文档处理）
>
> `Microsoft Visio 2016` （绘图）
>
> `MATLAB R2016a`（科学计算、图像处理）
>
> `Acrobat Reader DC`（pdf阅读）
>
> `VMware Workstation Pro 12.1`（虚拟机）
>
> `Adobe Photoshop CS6 ` （表情包制作【???】）

### 实用小工具

> 本地文件检索：
>
> [Everything.exe](http://okqi2ipwh.bkt.clouddn.com/Everything-1.3.4.686.x64.Multilingual-Setup.exe)



> 电脑色温控制：
>
> [flux.exe](http://okqi2ipwh.bkt.clouddn.com/flux-setup.exe)



> host修改器：
>
> [HostTool.exe](http://okqi2ipwh.bkt.clouddn.com/tool.exe)
>
> 源码可参考(https://github.com/HostsTools/Windows)



> Windows字体渲染：
>
> [MacType](http://www.mactype.net/)



> 截图:
>
> [Snipaste.exe](http://okqi2ipwh.bkt.clouddn.com/Snipaste-1.11.3-x64.zip)



> 最喜欢的两款字体：
>
> [Source Code Pro](https://github.com/adobe-fonts/source-code-pro/archive/2.030R-ro/1.050R-it.zip)
>
> [Moncao](http://okqi2ipwh.bkt.clouddn.com/MONACO.TTF)



> MinGW编译器：
>
> [mingw-get-setup.exe](http://okqi2ipwh.bkt.clouddn.com/mingw-get-setup.exe)



> Windows系统下查看Linux（双系统）文件：
>
> [Linux_Reader.exe](http://okqi2ipwh.bkt.clouddn.com/Linux_Reader.exe)

## 补充

### vimrc

```shell
source $VIMRUNTIME/mswin.vim

behave mswin
set nocompatible
set nobackup
set ignorecase
syn on
"set t_Co=256 
colo molokai
set hlsearch
filetype indent on
se ru nu ar sw=4 ts=4 noswf et sta nowrap ww=<,>,[,] gfn=Source_Code_Pro_Medium:h14:cANSI
set shiftwidth=4
set tabstop=4

"高亮当前行
"set cursorline

"统计字数
map <Enter><Enter> g<c-G>

"隐藏工具栏与菜单栏
"set guioptions-=m
set guioptions-=T
set guioptions-=r
set guioptions-=L

autocmd BufEnter * lcd %:p:h


imap <c-d> <c-o>dd
imap <c-]> {<cr>}<c-o>O<left><right>
imap <c-k> <up>
imap <c-j> <down>
imap <c-h> <left>
imap <c-l> <right>
imap <c-tab> <esc>gt
imap <cr> <cr><left><right>

inoremap ( ()<left>
inoremap [ []<left>

noremap <F6> =a{

map <F7> :g/^\s*$/d<CR>
map <c-t> :tabnew<CR>
map <F3> :e test.cpp<CR>
map <F9> :call CR()<CR><CR>
func CR()
exec 'update'
if &filetype == "java"  
    exec "!javac % && java %<" 
elseif &filetype == "python"
    exec "!python %"
else
    exec '!g++ %<.cpp -o %< && %<'
endif
endfunc
map <f2> :call SetTitle()<CR>Gkkk
func SetTitle()
let l = 0
let l = l + 1 | call setline(l, '/*')
let l = l + 1 | call setline(l, 'Creat Time:'.strftime('%c'))
let l = l + 1 | call setline(l, '*/')
let l = l + 1 | call setline(l, '//#pragma comment(linker, "/STACK:102400000,102400000")')
let l = l + 1 | call setline(l, '#include<iostream>')
let l = l + 1 | call setline(l, '#include<cstdio>')
let l = l + 1 | call setline(l, '#include<cstring>')
let l = l + 1 | call setline(l, '#include<cstdlib>')
let l = l + 1 | call setline(l, '#include<cmath>')
let l = l + 1 | call setline(l, '#include<algorithm>')
let l = l + 1 | call setline(l, '#include<functional>')
let l = l + 1 | call setline(l, '#include<string>')
let l = l + 1 | call setline(l, '#include<map>')
let l = l + 1 | call setline(l, '#include<set>')
let l = l + 1 | call setline(l, '#include<vector>')
let l = l + 1 | call setline(l, '#include<queue>')
let l = l + 1 | call setline(l, '#include<stack>')
let l = l + 1 | call setline(l, '#include<bitset>')
let l = l + 1 | call setline(l, '#include<ctime>')
let l = l + 1 | call setline(l, 'using namespace std;')
let l = l + 1 | call setline(l, '#define clr( x , y ) memset(x,y,sizeof(x))')
let l = l + 1 | call setline(l, '#define cls( x ) memset(x,0,sizeof(x))')
let l = l + 1 | call setline(l, '#define pr( x ) cout << #x << " = " << x << endl ')
let l = l + 1 | call setline(l, '#define pri( x ) cout << #x << " = " << x << " " ')
let l = l + 1 | call setline(l, '#define test( t ) int t ; cin >> t ; int kase = 1 ; while( t-- )')
let l = l + 1 | call setline(l, '#define out( kase ) printf( "Case %d: " , kase++ )')
let l = l + 1 | call setline(l, '#define sqr( x ) ( x * x )')
let l = l + 1 | call setline(l, '#define mp make_pair')
let l = l + 1 | call setline(l, '#define pii pair<int,int>')
let l = l + 1 | call setline(l, '#define fi first')
let l = l + 1 | call setline(l, '#define se second')
let l = l + 1 | call setline(l, '#define pb push_back')
let l = l + 1 | call setline(l, 'typedef long long lint;')
let l = l + 1 | call setline(l, 'const double eps = 1e-8 ;')
let l = l + 1 | call setline(l, 'const int inf = 0x3f3f3f3f ;')
let l = l + 1 | call setline(l, 'const long long INF = 0x3f3f3f3f3f3f3f3fLL ;')
let l = l + 1 | call setline(l, '')
let l = l + 1 | call setline(l, 'int main()')
let l = l + 1 | call setline(l, '{')
let l = l + 1 | call setline(l, '    //freopen("my.in","r",stdin);')
let l = l + 1 | call setline(l, '    //freopen("my.out","w",stdout);')
let l = l + 1 | call setline(l, '    return 0;')
let l = l + 1 | call setline(l, '}')
endfunc

map<f4> :call AddComment()<cr>
func AddComment()
    if matchstr(getline('.'), '[^ ]') == '/'
        normal ^xx
    else
        normal ^i//
    endif
endfunc

map <f5> :call SetTitle2()<CR>Gkkk
func SetTitle2()
let l = 0
let l = l + 1 | call setline(l, '#include<bits/stdc++.h>')
let l = l + 1 | call setline(l, 'using namespace std;')
let l = l + 1 | call setline(l, '')
let l = l + 1 | call setline(l, 'int main()')
let l = l + 1 | call setline(l, '{')
let l = l + 1 | call setline(l, '    freopen("my.in","r",stdin);')
let l = l + 1 | call setline(l, '    //freopen("my.out","w",stdout);')
let l = l + 1 | call setline(l, '    return 0;')
let l = l + 1 | call setline(l, '}')
endfunc


set printoptions=syntax:n,number:y,portrait:y

cd E:\Coder\my vim\

set fileencodings=utf-8,gb2312,gb18030,gbk,ucs-bom,cp936,latin1
au GUIEnter * simalt ~x                           "窗口启动时自动最大化
```



