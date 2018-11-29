    call plug#begin('~/.vim/plugged')
    
        "目录树
        Plug 'scrooloose/nerdtree'
        "对齐
        Plug 'godlygeek/tabular'
        "markdown
        Plug 'plasticboy/vim-markdown'
        "ctags 函数列表
        Plug 'vim-scripts/taglist.vim'
        "注释
        Plug 'scrooloose/nerdcommenter'
        "补全代码
        Plug 'vim-scripts/OmniCppComplete'
        "自动补全括号
        Plug 'jiangmiao/auto-pairs'
        "git 支持
        Plug 'tpope/vim-fugitive'
        """
        Plug 'mileszs/ack.vim'
        Plug 'w0rp/ale'
        "开始界面,最近编辑
        Plug 'mhinz/vim-startify'
        Plug 'majutsushi/tagbar'
        "文件查找
        Plug 'kien/ctrlp.vim'
        "Plug 'vim-airline-themes'
    
    call plug#end()
    
    
    """集成 taglist + nerdtree
    map <F9> <F2><F3>
    
    "设置 状态栏
    set statusline=%<%F%h%m%r\ [%{&ff}]\ (%{strftime(\"%H:%M\ %d/%m/%Y\",getftime(expand(\"%:p\")))})%=%l,%c%V\ %P
    "set statusline=%F%m%r%h%w\ [FORMAT=%{&ff}]\ [TYPE=%Y]\ [POS=%l,%v][%p%%]\ %{strftime(\"%d/%m/%y\ -\ %H:%M\")}
    "set statusline=[%F]%y%r%m%*%=[Line:%l/%L,Column:%c][%p%%]"
    
    """"""""ctags + taglist
    let Tlist_Show_One_File=1     "不同时显示多个文件的tag，只显示当前文件的    
    let Tlist_Exit_OnlyWindow=1   "如果taglist窗口是最后一个窗口，则退出vim   
    let Tlist_Ctags_Cmd="/usr/bin/ctags" "将taglist与ctags关联  
    
    set tags =/usr/include/tags
    
    map <F2> :Tlist<CR>
    
    map <F10> :call UpdateCtags()<CR>
    function! UpdateCtags()
        let curdir=getcwd()
        while !filereadable("./tags")
            cd ..
            if getcwd() == "/"
                break
            endif
        endwhile
        if filewritable("./tags")
            !ctags -R --file-scope=yes --langmap=c:+.h --languages=c,c++ --links=yes --c-kinds=+p --c++-kinds=+p --fields=+iaS --extra=+q
            TlistUpdate
        endif
        execute ":cd " . curdir
    endfunction
    
    """"""""NERDTree
    ""map <F3> :NERDTreeMirror<CR>
    map <F3> :NERDTreeToggle<CR>
    nmap ,t :NERDTreeFind<CR>
    
    ""窗口位置
    let g:NERDTreeWinPos='right' 
    ""窗口尺寸 
    let g:NERDTreewinSize=25
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
        elseif &filetype == 'python'
              normal  I#
        elseif &filetype == 'sh'
              normal  I#
        endif
    endfunc
         
    
    """录制宏，批量注释，批量去注释
    let @a="I//"
    let @b="^xx"
    
    """补全main 函数
    ""map mf i#include <stdio.h><Esc>o<Esc>iint main(int argc, char *argv[])<Esc>o{<Esc>o<Esc>i<Tab>return 0;<Esc>o}<Esc>2ko<Esc>i<Tab>
    map mf :call AddTitle()<cr>
    
    func! AddTitle()
        if &filetype == 'c'
            call append( 0, "#include <stdio.h>")
            call append( 1, "")
            call append( 2, "")
            call append( 3, "")
            call append( 4, "int main(int argc, const char *argv[])")
            call append( 5, "{")
            call append( 6, "")
            call append( 7, "")
            call append( 8, "    return 0;")
            call append( 9, "}")
        elseif &filetype == 'python'
            call append( 0, "#!/usr/bin/env python")
            call append( 1, "#-*- coding:utf-8 -*-")
            call append( 2, "")
            call append( 3, "")
        elseif &filetype == 'sh'
            call append( 0, "#!/usr/bin/env bash")
            call append( 1, "")
            call append( 2, "")
            call append( 3, "")
        endif
    endfunc
    
    """C，C++ 按F4编译运行
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
    map tt :tabnew<cr>
    map tn :tabnext<cr>
    map tp :tabprevious<cr>
    map tc :tabclose<cr>
    "map <C-t> :tabnew<cr>
    "map <C-p> :tabprevious<cr>
    "map <C-n> :tabnext<cr>
    "map <C-k> :tabclose<cr>
    "map <C-Tab> :tabnext<cr>
    
    
    """自动补全括号引号
    ""inoremap ( ()<Esc>i
    ""inoremap [ []<Esc>i
    ""inoremap { {<CR>}<Esc>O
    
    ""autocmd Syntax html,vim inoremap < <lt>><Esc>i| inoremap > <c-r>=ClosePair('>')<CR>
    ""inoremap ) <c-r>=ClosePair(')')<CR>
    ""inoremap ] <c-r>=ClosePair(']')<CR>
    ""inoremap } <c-r>=CloseBracket()<CR>
    ""inoremap " <c-r>=QuoteDelim('"')<CR>
    ""inoremap ' <c-r>=QuoteDelim("'")<CR>
    
    """function ClosePair(char)
    """    if getline('.')[col('.') - 1] == a:char
    """        return "\<Right>"
    """    else
    """        return a:char
    """    endif
    """endf
    """
    """function CloseBracket() 
    """    if match(getline(line('.') + 1), '\s*}') < 0
    """        return "\<CR>}"
    """    else
    """        return "\<Esc>j0f}a"
    """    endif
    """endf
    """
    """function QuoteDelim(char)
    """    let line = getline('.')
    """    let col = col('.')
    """    if line[col - 2] == "\\"
    """        return a:char
    """    elseif line[col - 1] == a:char
    """        return "\<Right>"
    """    else
    """        return a:char.a:char."\<Esc>i"
    """    endif
    """endf
    """
    set nocp
    filetype plugin on
    
    let OmniCpp_GlobalScopeSearch=1 
    let OmniCpp_NamespaceSearch=1
    let OmniCpp_DisplayMode=1
    let OmniCpp_ShowScopeInAbbr=0
    let OmniCpp_ShowPrototypeInAbbr=1 
    let OmniCpp_ShowAccess=1 
    let OmniCpp_MayCompleteDot=1 
    let OmniCpp_MayCompleteArrow=1
    let OmniCpp_MayCompleteScope=1
    "imap <C-Tab> <C-X><C-N>
    
    
    inoremap <c-l> <Right>
    inoremap <c-h> <Left>
    inoremap <c-k> <Up>
    inoremap <c-j> <Down>
    inoremap <c-u> <PageUp>
    inoremap <c-d> <PageDown>
    
    nmap <leader>w :w!<cr>
    nmap <leader>f :find<cr>
    
    
    nmap <leader>sa :cs add cscope.out<cr>
    nmap <leader>s :cs find s <C-R>=expand("<cword>")<CR><CR>
    nmap <leader>g :cs find g <C-R>=expand("<cword>")<CR><CR>
    nmap <leader>c :cs find c <C-R>=expand("<cword>")<CR><CR>
    nmap <leader>t :cs find t <C-R>=expand("<cword>")<CR><CR>
    nmap <leader>e :cs find e <C-R>=expand("<cword>")<CR><CR>
    nmap <leader>f :cs find f <C-R>=expand("<cfile>")<CR><CR>
    nmap <leader>i :cs find i <C-R>=expand("<cfile>")<CR><CR>
    
    if has("cscope")
        set csprg=/usr/bin/cscope
        set csto=1
        set cst
        set nocsverb
        set cspc=3
        "add any database in current dir
        if filereadable("cscope.out")
            cs add cscope.out
            "else search cscope.out elsewhere
        else
            let cscope_file=findfile("cscope.out", ".;")
            let cscope_pre=matchstr(cscope_file, ".*/")
            if !empty(cscope_file) && filereadable(cscope_file)
                exe "cs add" cscope_file cscope_pre
            endif
        endif
        set csverb
    endif
    
    
    map js <Esc>:%!python -m json.tool<CR>
    
    
    "
    "nmap <leader>tb :TagbarToggle<CR>
    
    
    "设置ctrlp的快捷方式 ctrp
    let g:ctrlp_map = '<c-p>'
    let g:ctrlp_cmd = 'CtrlP'
    
    "设置ctrlp的窗口位置
    let g:ctrlp_match_window = 'bottom,order:btt,min:1,max:10,results:20'
    
    
    "
    "map dq :call Updatealignment()<CR>
    "function! Updatealignment()
    "    execute ":Tabularize /, "
    "    execute ":Tabularize /: "
    "    execute ":Tabularize /\/\/ "
    "    "execute ":Tabularize /= "
    "endfunction
    "
    
    