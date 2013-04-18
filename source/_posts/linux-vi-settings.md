title: Linux vi开发配置
date: 2013-04-06
categories: 学习
tags: [Linux, vi]
---

## 开发必备插件

1. ctags:为项目生成索引，方便阅读代码时的定位，`ctags -R`生成索引，`Ctrl + ]`定位到函数声明处，`Ctrl + o`返回。
2. taglist：借助ctags，现实代码中的宏定义，变量和函数，阅读一份代码就比较直观了。（在taglist窗口中通过Ctrl+w+w切换代码窗口）.
3. Autocomplpop:自动完成插件，编码时自动出现代码提示，下载插件后直接解压到~/.vim文件夹下就可以使用了。
4. CppCompleete：为C和C++优化的代码提示插件。
<!-- more -->
5. vimball: 是为了安装其他插件用的，有些插件使用了vimball的形式发布的。
6. netrw.vim：文件浏览用的，支持网络文件浏览如ftp
7. winmanager: 可以让你的vim变成windows风格的IDE
8. grep.vim: 在整个工程中查找，集成了grep比vi自带的查找好很多
9. a.vim: C和C++程序员的福音，在.h和.c文件之间来回切换(`:A`)
9. visualmark.vim: 高亮标记插件, `mm`进行标记，`F2`在标记之间进行切换

ctags,到[这里](http://sourceforge.net/projects/ctags/)下载.

其他插件到[这里](http://www.vim.org/scripts/script_search_results.php?order_by=creation_date&direction=descending)自行搜索下载。

** 注意 **

netrw.vim和winmanager插件安装是通过vimball的形式安装的，安装方式如下：

	cd 插件所在的目录
	vi 下载的插件文件名
	:so %
	:q

a.vim 和 grep.vim 直接将文件复制到~/.vim/plugin目录下即可完成安装

## vi配置

编辑～/.vimrc文件,直接在文件中添加需要的选项即可，vi启动是默认加载其中的选项，这样就不必每次都进行设置了。

常用的配置如下,对开发很有帮助哦：

	" 行号
	set nu
	" 语法高亮
	syntax on
	" 自动对齐
	set autoindent
	set smartindent
	" 第一行设置tab键为4个空格，第二行设置当行之间交错时使用4个空格
	set tabstop=4
	set shiftwidth=4
	" 设置匹配模式，类似当输入一个左括号时会匹配相应的那个右括号
	set showmatch
	
	" 设置ctags自动加载当前目录下的tags
	set tags=tags;
	set autochdir

	" 按F8按钮，在窗口的左侧出现taglist的窗口,像vc的左侧的workpace
	nnoremap <silent> <F8> :TlistToggle<CR><CR>
	" :Tlist              调用TagList            
	let Tlist_Show_One_File=0                    " 只显示当前文件的tags
	let Tlist_Exit_OnlyWindow=1                  "
	" 如果Taglist窗口是最后一个窗口则退出Vim     

	" 文件浏览窗口和TagList
	let g:winManagerWindowLayout='FileExplorer|TagList'
	nmap wm :WMToggle<cr>

** 附：** [史上最强的VI配置](http://amix.dk/vim/vimrc.html)

## 代码编辑进行时

编辑好代码以后，通过`:make`直接编译整个项目，`:cw`或`:copen`可以调出QuickFix窗口，查看调试过程中的警告和错误。通过`cclose`关闭QuickFix窗口。

## 最终打造的vi效果图

Happy的写代码吧！

![vi](/images/vi.png)
