title: Mac-配置vim
date: 2015-12-10 01:17:00
categories: 
- Mac
tags: 
- Mac
---
* 下载配色方案[molokai](https://github.com/tomasr/molokai)
* 在当前用户根目录下的`~/.vim`创建`colors`目录，将下载的配色方案中的`molokai.vim`移动到`~/.vim/colors`中
* 在当前用户根目录下新建`.vimrc`，添加`colorscheme molokai`
	* `vimrc`可以把`/usr/share/vim/vimrc`拷贝过来
* 更多的配置如下

		" Configuration file for vim
		set modelines=0 " CVE-2007-2438

		" Normally we use vim-extensions. If you want true vi-compatibility
		" remove change the following statements
		set nocompatible " Use Vim defaults instead of 100% vi compatibility
		set backspace=2 " more powerful backspacing

		" Don't write backup file if vim is being called by "crontab -e"
		au BufWrite /private/tmp/crontab.* set nowritebackup nobackup
		" Don't write backup file if vim is being called by "chpass"
		au BufWrite /private/etc/pw.* set nowritebackup nobackup

		" 语法高亮
		syntax on

		autocmd InsertLeave * se nocul
		autocmd InsertEnter * se cul
		" 用浅色高亮当前行

		set smartindent
		" 智能对齐

		set autoindent
		" 自动对齐

		set confirm
		" 在处理未保存或只读文件的时候，弹出确认

		set tabstop=4
		" Tab键的宽度

		set softtabstop=4
		set shiftwidth=4
		"  统一缩进为4

		set noexpandtab
		" 不要用空格代替制表符

		set number
		" 显示行号

		set number
		" 显示行号

		set hlsearch
		set incsearch
		" 搜索逐字符高亮

		set gdefault
		" 行内替换

		set encoding=utf-8
		set fileencodings=utf-8,ucs-bom,shift-jis,gb18030,gbk,gb2312,cp936,utf-16,big5,euc-jp,latin1
		" 编码设置

		colorscheme molokai
		" 设置颜色主题

		set guifont=Menlo:h16:cANSI
		" 设置字体

		set langmenu=zn_CN.UTF-8
		set helplang=cn
		" 语言设置

		set cmdheight=2
		" 命令行（在状态行）的高度，默认为1,这里是2

		set ruler
		" 在编辑过程中，在右下角显示光标位置的状态行

		set laststatus=2
		" 总是显示状态行

		set showcmd
		" 在状态行显示目前所执行的命令，未完成的指令片段亦会显示出来

		set scrolloff=3
		" 光标移动到buffer的顶部和底部时保持3行距离

		set showmatch
		" 高亮显示对应的括号

		set matchtime=5
		" 对应括号高亮的时间（单位是十分之一秒）

		set autowrite
		" 在切换buffer时自动保存当前文件

		set wildmenu
		" 增强模式中的命令行自动完成操作

		set linespace=2
		" 字符间插入的像素行数目

		set whichwrap=b,s,<,>,[,]
		" 开启Normal或Visual模式下Backspace键，空格键，左方向键，右方向键，Insert或replace模式下左方向键，右方向键跳行的功能。


