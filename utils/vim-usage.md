# 保存时获得sudo权限

```shell
:w !sudo tee %
```

1. 命令:`w !{cmd}`，让 `vim` 执行一个外部命令{cmd}，然后把当前缓冲区的内容从 stdin 传入。
2. `tee` 是一个把 `stdin` 保存到文件的小工具。
3. 而 `%`，是`vim`当中一个只读寄存器的名字，总保存着当前编辑文件的文件路径。

# 简单高效的vim配置

创建`~/.vimrc`文件并输入：
```vim
" =============================================================
"                           编辑设置
" =============================================================

" 显示行号
set number

" 允许鼠标的使用
set mouse=a

" 记录光标的位置, 打开同一文件时, 光标停留在上一次的位置
augroup resCur
                autocmd!
                autocmd BufReadPost * call setpos(".", getpos("'\""))
augroup END

" 语法高亮
syntax on

" 颜色方案
" colorscheme desert
colorscheme peachpuff
" colorscheme molokai
" colorscheme ron
" colorscheme default
" colorscheme delek

" 设置匹配模式, 类似当输入一个左括号时会匹配相应的右括号
set showmatch

" 文件类型检测
filetype indent on

" 空格缩进
" 按一次前进4个字符
set tabstop=4
" 设置多行缩进时用
set shiftwidth=4
" 用空格代替tab
set softtabstop=4

" 自动缩进,即为新行自动添加与当前行同等缩进
" set autoindent
" 基于autoindent的一些改进
" set smartindent
" 开启之后将输入的tab自动展开成空格, 开启后要输入tab, 需要ctrl-V<tab>"
set expandtab

" 显示标尺
set ruler

" 自动保存
set autowrite

" 光标移动到buffer的顶部和底部时保持2行距离
set scrolloff=2

" =================================================
"                     键盘映射
" =================================================

" 映射全选+ 复制 ctrl + a
map <C-a> ggVGY
map! <C-a> ggVGY
map <F12> gg=G


" ==================================================
" 高亮当前行
" ==================================================

" 浅色高亮当前行
" 结束插入模式高亮当前行取消
" autocmd InsertLeave * se nocul
" 只有进入文本之后开启插入模式才能高亮当前行
" autocmd InsertEnter * se cul
" 直接设置高亮当前行
set cursorline
" 以长条形状高亮当前行
hi cursorline cterm=NONE ctermbg=darkred ctermfg=white
" hi cursorline cterm=NONE ctermbg=blue ctermfg=NONE guibg=NONE guifg=NONE


" ===================================================
"                 快捷键设置
" ===================================================

" 自动补全
inoremap ' ''<Esc>i
inoremap " ""<Esc>i
inoremap ( ()<Esc>i
inoremap [ []<Esc>i
inoremap { {}<Esc>i
inoremap < <><Esc>i

" 编辑模式下快速移动
inoremap <C-l> <Right>
inoremap <C-h> <Left>
inoremap <C-k> <Up>
inoremap <C-j> <Down>
inoremap <C-b> <PageDown>
inoremap <C-f> <PageUp>

" 插入模式下和普通模式下保存快捷键
nnoremap <C-d> :wq<CR>
inoremap <C-d> <Esc>:wq<CR>i

" 插入模式下移动快捷键
" 插入模式下移动到行首
inoremap <C-u> <Home>
" 插入模式下移动到行尾
inoremap <C-o> <End>

" 插入模式下代码提示
inoremap <leader>i <C-x><C-l>
inoremap <leader>o <C-x><C-o>
inoremap <leader>o <C-x><C-o>
inoremap <leader>o <C-x><C-o>

" 否则无法使用退格键
set backspace=indent,eol,start


" ===============================================
"                    多窗口管理
" ===============================================
" 显示光标所在行相对与其他行的行号
" set relativenumber
" 是否显示状态栏, 0 不显示, 1 只在多窗口显示, 2显示
set laststatus=2
set statusline=%F%m%r%h%w\ [POS=%l,%v][%p%%]\ %{strftime(\"%d/%m/%y\ -\ %H:%M\")}


" ================================================
"                     搜索设置
" ================================================

" 搜索时, 高亮显示匹配结果
set hlsearch
" 大小写铭感设置
" 设置默认进行大小写不敏感查找, 如果不设置则/foo\c来大小写不敏感查找, \C敏感查找
set ignorecase
" 如果有一个大写字母, 则切换到大小写敏感查找
set smartcase


" ================================================
"                     编辑设置
" ================================================

" 设置自动换行
"
" 保留操作历史
set undofile
set backup
set swapfile
" 备份文件,交换文件,操作历史保存位置
" 结尾的//表示生成的文件名带有绝对路径,
" 路径中用%代替目录分割符,防止文件重命名
" 在vim 修改一个文件时, vim是可以自动创建一个备份文件. 如果写入失败会提示E510:Can't make back up file(add ! to override)'
" 提示为 vim 不能够创建备份文件, 可能原因1. vim备份的目录是否存在, vim不会自动创建备份目录 2. 备份目录是否有读写的权限
" :verbose set backupdir 查看备份信息
" 显示自动备份的目录 backupdir=~/.vim/.backup/ Last set from ~/.vimrc
set backupdir=~/.vim/.backup//
set directory=~/.vim/.swp//
set undodir=~/.vim/.undo//

" gvim e303 无法打开[文件名]的交换文件, 恢复将不可能
" 解决办法: 修改.vimrc, 增加下面一行. 因为默认为directory=.,c:\tmp,c:\temp
set directory=.,$TEMP


" 自动切换目录, 这主要用在一个vim会话之中打开多个文件的情况,
" 默认的工作目录是打开第一个文件的目录
" 该配置可以将工作目录自动切换到, 正在编辑的文件的目录
set autochdir

" 出错的时候不要发出声响
set noerrorbells

" vim需要记住多少次操作
set history=100

" 打开文件监视器,
" 如果在编辑的过程中发生外部改变(比如被其他编辑器编辑)会发出提示
set autoread

" 命令模式下, 底部指令按下tab键自动补全
" 第一次按下tab 会显示所有匹配的操作指令清单,第二次按下会依次选择各个指令
set wildmenu
set wildmode=longest:list,full
set nocompatible


" ==================================================
"                   个性脚本设置
" ==================================================
" py文件设置
func SetTitle()
call setline(1, "\#!/usr/bin/python3")
call setline(2, "\# -*- coding=utf8 -*-")
call setline(3, "\"\"\"")
call setline(4, "\# @Author: groveer")
call setline(5, "\# @CreatedTime:".strftime("%Y-%m-%d %H:%M:%S"))
call setline(6, "\# @Description: ")
call setline(7, "\"\"\"")
normal G
normal o
normal o
endfunc
autocmd bufnewfile *.py call SetTitle()

" sh文件设置
func Setsh()
call setline(1, "\#!/bin/bash")
" call setline(2, "\# @Author: groveer")
" call setline(3, "\# @CreatedTime:".strftime("%Y-%m-%d %H:%M:%S"))
" call setline(4, "\# @Description: ")
normal G
normal o
normal o
endfunc
autocmd bufnewfile *.sh call Setsh()


" ==================================================
"                快捷键运行脚本
" ==================================================
" <F5>运行python3脚本
map <F5> :call RunPython()<CR>
func! RunPython()
    exec "W"
    if &filetype == 'python'
        exec "!time python3 %"
    endif
endfunc
```