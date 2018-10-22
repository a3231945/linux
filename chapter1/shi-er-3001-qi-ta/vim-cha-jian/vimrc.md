    call plug#begin('~/.vim/plugged')
    Plug 'scrooloose/nerdtree'
    Plug 'godlygeek/tabular'
    Plug 'plasticboy/vim-markdown'
    Plug 'vim-scripts/taglist.vim'
    call plug#end()
    
    
    
    """"""""ctags + taglist
    let Tlist_Show_One_File=1     "不同时显示多个文件的tag，只显示当前文件的    
    let Tlist_Exit_OnlyWindow=1   "如果taglist窗口是最后一个窗口，则退出vim   
    let Tlist_Ctags_Cmd="/usr/bin/ctags" "将taglist与ctags关联  
    
    map <F2> :Tlist<CR>
    
    """"""""NERDTree
    ""map <F3> :NERDTreeMirror<CR>
    map <F3> :NERDTreeToggle<CR>
    ""窗口位置
    let g:NERDTreeWinPos='right' 
    ""窗口尺寸 
    let g:NERDTreewinSize=31 
    ""窗口是否显示行号 
    let g:NERDTreeShowLineNumbers=1 
    ""不显示隐藏文件 
    let g:NERDTreeShowHidden=0
    
    
    set number
    set smartindent  
    set tabstop=4  
    set shiftwidth=4  
    set expandtab  
    set softtabstop=4  
    syntax on
    
    
    ""注释，去注释
    map zs :call Notes()<CR>
    map ,zs ^xx
    
    func! Notes()
        if &filetype == 'c'
              normal  I//
        elseif &filetype == 'cpp'
              normal  I//
        elseif &filetype == 'java' 
              normal  I//
        elseif &filetype == 'sh'
              normal  I##
        elseif &filetype == 'py'
              normal  I##
        endif
    endfunc
         
    
    """录制宏，批量注释，批量去注释
    let @a="I//"
    let @b="^xx"
    
    """补全main 函数
    map mf i#include <stdio.h><Esc>o<Esc>iint main(int argc, char *argv[])<Esc>o{<Esc>o<Esc>i<Tab>return 0;<Esc>o}<Esc>2ko<Esc>i<Tab>
    
    """C，C++ 按F5编译运行
    map <F4> :call CompileRunGcc()<CR>
    func! CompileRunGcc()
        exec "w"
        if &filetype == 'c'
            exec "!g++ % -o %<"
            exec "! ./%<"
        elseif &filetype == 'cpp'
            exec "!g++ % -o %<"
            exec "! ./%<"
        elseif &filetype == 'java' 
            exec "!javac %" 
            exec "!java %<"
        elseif &filetype == 'sh'
            :!./%
        endif
    endfunc
    
    """C,C++的调试
    map <F8> :call Rungdb()<CR>
    func! Rungdb()
        exec "w"
        exec "!g++ % -g -o %<"
        exec "!gdb ./%<"
    endfunc
    
    
    """标签相关的快捷键 Ctrl
    map tn :tabnext<cr>
    map tp :tabprevious<cr>
    map tc :tabclose<cr>
    map <C-t> :tabnew<cr>
    map <C-p> :tabprevious<cr>
    map <C-n> :tabnext<cr>
    map <C-k> :tabclose<cr>
    map <C-Tab> :tabnext<cr>
    
    
    """自动补全括号引号
    inoremap ( ()<Esc>i
    inoremap [ []<Esc>i
    inoremap { {<CR>}<Esc>O
    
    autocmd Syntax html,vim inoremap < <lt>><Esc>i| inoremap > <c-r>=ClosePair('>')<CR>
    inoremap ) <c-r>=ClosePair(')')<CR>
    inoremap ] <c-r>=ClosePair(']')<CR>
    inoremap } <c-r>=CloseBracket()<CR>
    inoremap " <c-r>=QuoteDelim('"')<CR>
    inoremap ' <c-r>=QuoteDelim("'")<CR>
    
    function ClosePair(char)
        if getline('.')[col('.') - 1] == a:char
            return "\<Right>"
        else
            return a:char
        endif
    endf
    
    function CloseBracket() 
        if match(getline(line('.') + 1), '\s*}') < 0
            return "\<CR>}"
        else
            return "\<Esc>j0f}a"
        endif
    endf
    
    function QuoteDelim(char)
        let line = getline('.')
        let col = col('.')
        if line[col - 2] == "\\"
            return a:char
        elseif line[col - 1] == a:char
            return "\<Right>"
        else
            return a:char.a:char."\<Esc>i"
        endif
    endf
    