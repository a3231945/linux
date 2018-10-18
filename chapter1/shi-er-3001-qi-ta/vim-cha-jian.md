### Vim 插件管理


### 一、安装vim-plugin


    curl -fLo ~/.vim/autoload/plug.vim --create-dirs https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim

### 二、安装插件

- **安装 nerdtree**

      #vim ~/.vimrc
      
      call plug#begin('~/.vim/plugged')
          Plug 'scrooloose/nerdtree'
      call plug#end()
      
      map <F3> :NERDTreeMirror<CR>
      map <F3> :NERDTreeToggle<CR>
  
      #vim  
      :PlugInstall
      
 
- **安装 vim-markdown **


    #vim ~/.vimrc
    call plug#begin('~/.vim/plugged')
        Plug 'godlygeek/tabular'
        Plug 'plasticboy/vim-markdown'
    call plug#end()
    
    #vim
    :PlugInstall