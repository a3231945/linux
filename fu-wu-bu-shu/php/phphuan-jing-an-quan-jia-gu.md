# PHP环境加固

**1、启用php的安全模式**
PHP环境提供的安全模式是一个非常重要的内嵌安全机制。
PHP安全模式能有效控制一些PHP环境中的函数（例如：`system()`函数），对大部分的文件操作函数进行权限控制

    #vim php.ini
    safe_mode = on

**2、用户组安全**
当启用安全模式后，如果`safe_mode_gid`选项被关闭，PHP脚本能够对被文件进行访问，且相同用户组的用户也能够对该文件进行访问，建议关闭

    #vim php.ini
    safe_mode_gid = off

**3、安全模式下执行程序主目录**
当启用安全模式后，想要执行某些程序的时候，可以指定需要执行程序的主目录

    #vim php.ini
    safe_mode_exec_dir = /usr/bin

一般情况下，如果不需要执行什么程序，建议不要执行系统程序的目录，可以指定一个目录，然后把需要执行的程序拷贝到这个目录即可

    #vim php.ini
    safe_mode_exec_dir = /temp/cmd
    
推荐不要执行任何程序,将目录指向网页目录

    #vim php.ini
    safe_mode_exec_dir = /www        
    
**4、安全模式下包含文件**
如果在安全模式下包括某些公共文件

    #vim php.ini
    safe_mode_include_dir = /www/include/

**5、控制PHP脚本能访问的目录**
使用`open_basedir` 选项能够控制PHP脚本只能访问指定的目录，这样避免PHP脚本访问不应该访问的文件

    #vim php.ini
    open_basedir = /www
    
**6、关闭危险函数**
如果启用了安全模式，那么可以不需要设置函数禁止

    #vim php.ini
    disable_functions = system, passthru, exec, shell_exec, popen, phpinfo, escapeshellarg, escapeshellcmd, proc_close, proc_open, dl

禁止对文件和目录的操作

    #vim php.ini
    disable_functions = chdir, chroot, dir, getcwd, opendir, readdir, scandir, fopen, unlink, delete, copy, mkdir, rmdir, rename, file, file_get_contents, fputs, fwrite, chgrp,chmod, chown

**7、关闭PHP版本信息在HTTP头中泄露**
防止获取服务器关于PHP版本的信息

    #vim php.ini
    expose_php = off


**8、关闭注册全局变量**
在PHP环境中提交的变量，包括使用POST或者GET命令提交的变量，都将自动注册为全局变量，能够被直接访问

    #vim php.ini
    register_globals = off

**9、SQL注入防护**
开启`magic_quotes_gpc`，PHP将自动把用户提交对SQL查询的请求进行转换：例如 把 ' 转换为 \'

    #vim php.ini
    magic_quotes_gpc = on
    
