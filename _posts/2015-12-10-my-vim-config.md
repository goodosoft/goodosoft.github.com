---
layout: post
title: "我的vim配置文件"
description: "my vim config file"
category: [learn]
tags: [vim]
---
{% include JB/setup %}


    set nocompatible
    source $VIMRUNTIME/vimrc_example.vim
    source $VIMRUNTIME/mswin.vim
    behave mswin
    
    set diffexpr=MyDiff()
    function MyDiff()
      let opt = '-a --binary '
      if &diffopt =~ 'icase' | let opt = opt . '-i ' | endif
      if &diffopt =~ 'iwhite' | let opt = opt . '-b ' | endif
      let arg1 = v:fname_in
      if arg1 =~ ' ' | let arg1 = '"' . arg1 . '"' | endif
      let arg2 = v:fname_new
      if arg2 =~ ' ' | let arg2 = '"' . arg2 . '"' | endif
      let arg3 = v:fname_out
      if arg3 =~ ' ' | let arg3 = '"' . arg3 . '"' | endif
      let eq = ''
      if $VIMRUNTIME =~ ' '
        if &sh =~ '\<cmd'
          let cmd = '""' . $VIMRUNTIME . '\diff"'
          let eq = '"'
        else
          let cmd = substitute($VIMRUNTIME, ' ', '" ', '') . '\diff"'
        endif
      else
        let cmd = $VIMRUNTIME . '\diff'
      endif
      silent execute '!' . cmd . ' ' . opt . arg1 . ' ' . arg2 . ' > ' . arg3 . eq
    endfunction
    
    " my vim config
    
    set nocompatible                " be iMproved
    filetype on
    
    " windows下自动最大化
    if has('win32')    
      au GUIEnter * simalt ~x
    else    
      au GUIEnter * call MaximizeWindow()
    endif 
    function! MaximizeWindow()    
      silent !wmctrl -r :ACTIVE: -b add,maximized_vert,maximized_horz
    endfunction
    " windows下esc闪烁
    set novb
    
    " 配色方案&字体
    set background=dark
    colorscheme solarized
    "colorscheme molokai
    "colorscheme phd
    set guifont=Consolas:h20 "字体与字号
    
    set tabstop=4 " 设置tab键的宽度
    set expandtab " 用space替代tab的输入
    set autoindent " 自动对齐
    set smartindent " 智能自动缩进
    set shiftwidth=4 "设置缩进宽度为 4
    set backspace=2 " 设置退格键可用
    set ai! " 设置自动缩进
    set nu! " 显示行号
    set mouse=a " 启用鼠标
    set ruler " 右下角显示光标位置的状态行
    set hlsearch " 开启高亮显示结果
    set incsearch " 开启实时搜索功能
    set nowrapscan " 搜索到文件两端时不重新搜索
    " show a visual line under the cursor's current line
    set cursorline
    " show the matching part of the pair for [] {} and ()
    set showmatch
    " enable all Python syntax highlighting features
    let python_highlight_all = 1
    " 关闭提示音
    set noerrorbells 
    set novisualbell 
    set t_vb= 
    set hidden " 允许在有未保存的修改时切换缓冲区
    "set list  " 显示不可见字符
    
    syntax enable " 打开语法高亮
    syntax on " 开启文件类型侦测
    
    filetype indent on " 针对不同的文件类型采用不同的缩进格式
    filetype plugin on " 针对不同的文件类型加载对应的插件
    filetype plugin indent on " 启用自动补全
    
    set encoding=utf-8
    set fileencodings=utf-8,gbk,gb18030,gk2312
    
    "解决菜单乱码
    source $VIMRUNTIME/delmenu.vim
    source $VIMRUNTIME/menu.vim
    
    "解决consle输出乱码
    language messages zh_CN.utf-8
    
    " Vundle
    set rtp+=$VIM/vimfiles/bundle/vundle/
    call vundle#rc('$VIM/vimfiles/bundle/')
    " let Vundle manage Vundle
    Bundle 'gmarik/vundle'
    
    " 使用 NERDTree 插件查看工程文件。设置快捷键，速记：file list
    Bundle 'scrooloose/nerdtree'
    map <F2> :NERDTreeMirror<CR>  
    map <F2> :NERDTreeToggle<CR>  
    " 设置NERDTree子窗口宽度
    let NERDTreeWinSize=32
    " 设置NERDTree子窗口位置
    let NERDTreeWinPos="left"
    " 显示隐藏文件
    let NERDTreeShowHidden=1
    " NERDTree 子窗口中不显示冗余帮助信息
    let NERDTreeMinimalUI=1
    " 删除文件时自动删除文件对应 buffer
    let NERDTreeAutoDeleteBuffer=1




