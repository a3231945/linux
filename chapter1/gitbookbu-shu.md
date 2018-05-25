# GitBook 使用 #
### 一、gitbook安装 ###
**1、安装nodejs 环境**
    
    tar xf node-v6.10.2-linux-x64.tar.xz  -C /usr/local/nodejs

配置环境变量 `vim /etc/profile.d/nodejs.sh`
    
    export PATH=/usr/local/nodejs/bin/:$PATH
    export PATH

重新加载环境变量  `source /etc/profile.d/nodejs.sh`

验证是否安装成功

    [root@rsync-node1 mnt]# node -v
    v6.10.2
    [root@rsync-node1 mnt]# npm -v
    3.10.10

**2、安装gitbook**

    npm install -g gitbook-cli
验证是否安装成功
    
    [root@rsync-node1 mnt]# gitbook  -V
    CLI version: 2.3.0
    GitBook version: 3.2.2

### 二、gitbook使用 ###
**1、初始化目录结构**

    [root@rsync-node1 1]# gitbook init first
    warn: no summary file in this book 
    info: create README.md 
    info: create SUMMARY.md 
    info: initialization is finished 

**2、查看生成文件**
    
    [root@rsync-node1 1]# ls first/ -al
    总用量 16
    drwxr-xr-x. 2 root root 4096 5月  14 12:34 .
    drwxr-xr-x. 3 root root 4096 5月  14 12:35 ..
    -rw-r--r--. 1 root root   16 5月  14 12:34 README.md
    -rw-r--r--. 1 root root   40 5月  14 12:34 SUMMARY.md

**3、文件介绍：**
    
    REAMDE.md 为介绍文件（可以没有）
    SUMARY.md 为目录结构文件
    ----------
    [root@rsync-node1 first]# cat SUMMARY.md 
    # Summary
    
    * [Introduction](README.md)


### 三、gitbook输出 ###
**1、web 服务输出**

    gitbook serve .

**2、web 服务输出 指定端口**

    gitbook  serve --port 3000

**3、**


### 四、附录 ###
**1、gitbook帮助**

    [root@rsync-node1 first]# gitbook  help
    build [book] [output]       build a book
        --log                   Minimum log level to display (Default is info; Values are debug, info, warn, error, disabled)
        --format                Format to build to (Default is website; Values are website, json, ebook)
        --[no-]timing           Print timing debug information (Default is false)

    serve [book] [output]       serve the book as a website for testing
        --port                  Port for server to listen on (Default is 4000)
        --lrport                Port for livereload server to listen on (Default is 35729)
        --[no-]watch            Enable file watcher and live reloading (Default is true)
        --[no-]live             Enable live reloading (Default is true)
        --[no-]open             Enable opening book in browser (Default is false)
        --browser               Specify browser for opening book (Default is )
        --log                   Minimum log level to display (Default is info; Values are debug, info, warn, error, disabled)
        --format                Format to build to (Default is website; Values are website, json, ebook)

    install [book]              install all plugins dependencies
        --log                   Minimum log level to display (Default is info; Values are debug, info, warn, error, disabled)

    parse [book]                parse and print debug information about a book
        --log                   Minimum log level to display (Default is info; Values are debug, info, warn, error, disabled)

    init [book]                 setup and create files for chapters
        --log                   Minimum log level to display (Default is info; Values are debug, info, warn, error, disabled)

    pdf [book] [output]         build a book into an ebook file
        --log                   Minimum log level to display (Default is info; Values are debug, info, warn, error, disabled)

    epub [book] [output]        build a book into an ebook file
        --log                   Minimum log level to display (Default is info; Values are debug, info, warn, error, disabled)

    mobi [book] [output]        build a book into an ebook file
        --log                   Minimum log level to display (Default is info; Values are debug, info, warn, error, disabled)


    
    


     