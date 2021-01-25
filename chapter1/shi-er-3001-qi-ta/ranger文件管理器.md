# ranger 文件管理器

## 一、 安装ranger

    brew install ranger

## 二、 基础操作
1、查看帮助

    ？ 或 F1

2、移动

    上  j
    下  k
    左  h 
    右  l
    
    上页    ctrl + b/u
    下页    ctrl + f/d


    首行    gg
    尾行    G
    
    前进    L 
    后退    H
    上级目录    h
    下级目录    l
    
    查找    f
    搜索    /
    查找下一个  n
    查找上一个  N


3、 文件管理

    新建目录/文件       :touch xxx
    删除目录/文件       dD
    修改目录/文件       cw/I/A
    复制目录/文件       yy
    粘贴目录/文件       pp
    剪切目录/文件       dd
    刷新目录/文件       R

4、文件选择

    单选        空格
    反选        v
    多选模式    V

5、终端管理

    新建终端    gn    
    切换终端    gt/gT
    关闭终端    gc

6、调用shell

   ！/ @ / S

## 三、安装其他文件类型预览支持
1、html

    brew install elinks
    brew install w3m

2、多媒体

    brew install media-info

3、图片


4、doc

5、xlsx

6、语法高亮

    brew install highlight

## 四、其他配置
1、配置文件分类

    commands.py         能通过 : 执行的命令
    commands_full.py    能通过 : 执行的命令，但这个更全     
    rc.conf             选项设置和快捷键
    rifle.conf          指定不同类型的文件的默认打开程序  
    scope.sh            当 use_preview_script = true，这个脚本会被调用

2、自定义按键

    vim ~/.config/ranger/rc.conf
    #删除到垃圾文件
    map DD shell mv %s ~/.Trash
    #快速跳转到指定目录
    map gw cd ~/root/

3、开启图片预览

    vim ~/.config/ranger/rc.conf
    # 预览图片
    set preview_images true    
    # 使用什么方法来预览图片
    set preview_images_method iterm2  

4、配置颜色

    vim ~/.config/ranger/rc.conf
    
    set colorscheme jungle
    #有四个颜色可以选： default 、snow、jungle、solarized


5、参考站点
- https://www.52gvim.com/post/ranger-tool-usage
- https://wiki.archlinux.org/index.php/ranger_(简体中文)

