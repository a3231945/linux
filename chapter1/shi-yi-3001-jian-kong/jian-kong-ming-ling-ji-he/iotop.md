### 一、安装iotop
    
    yum -y install iotop
### 二、iotop 使用

**1、查看帮助**

    # iotop  -h
    Usage: /usr/sbin/iotop [OPTIONS]
    
    DISK READ and DISK WRITE are the block I/O bandwidth used during the sampling
    period. SWAPIN and IO are the percentages of time the thread spent respectively
    while swapping in and waiting on I/O more generally. PRIO is the I/O priority at
    which the thread is running (set using the ionice command).
    
    Controls: left and right arrows to change the sorting column, r to invert the
    sorting order, o to toggle the --only option, p to toggle the --processes
    option, a to toggle the --accumulated option, q to quit, any other key to force
    a refresh.
    
    Options:
      --version             show program's version number and exit
      -h, --help            show this help message and exit
      -o, --only            only show processes or threads actually doing I/O
      -b, --batch           non-interactive mode
      -n NUM, --iter=NUM    number of iterations before ending [infinite]
      -d SEC, --delay=SEC   delay between iterations [1 second]
      -p PID, --pid=PID     processes/threads to monitor [all]
      -u USER, --user=USER  users to monitor [all]
      -P, --processes       only show processes, not all threads
      -a, --accumulated     show accumulated I/O instead of bandwidth
      -k, --kilobytes       use kilobytes instead of a human friendly unit
      -t, --time            add a timestamp on each line (implies --batch)
      -q, --quiet           suppress some lines of header (implies --batch)

**2、字段解释**

    --version #显示版本号
    -h, --help #显示帮助信息
    -o, --only #显示进程或者线程实际上正在做的I/O，而不是全部的，可以随时切换按o
    -b, --batch #运行在非交互式的模式
    -n NUM, --iter=NUM #在非交互式模式下，设置显示的次数，
    -d SEC, --delay=SEC #设置显示的间隔秒数，支持非整数值
    -p PID, --pid=PID #只显示指定PID的信息
    -u USER, --user=USER #显示指定的用户的进程的信息
    -P, --processes #只显示进程，一般为显示所有的线程
    -a, --accumulated #显示从iotop启动后每个线程完成了的IO总数
    -k, --kilobytes #以千字节显示
    -t, --time #在每一行前添加一个当前的时间
    -q, --quiet #suppress some lines of header (implies --batch). This option can be specified up to three times to remove header lines.

**3、使用实例**

    1) 直接查看
    iotop
    
    可操作：
        <-  ->  左右移动
        r        反向排序
        o        切换到  --only 模式
        a        切换到  --accumulated 模式 
        p        切换到  --processes 模式
        q        退出
        