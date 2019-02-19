
## 进程管理
### 一、进程的概念

**程序**： 文件，一般是二进制，静态 /usr/sbin/httpd，/usr/sbin/sshd
**进程**： 是程序运行的过程， 动态，有生命周期的，动态产生和消亡的

一个程序可能对应多个进程

    # ps aux |grep 'sshd'
    root      2705  0.0  0.0   7224  1020 ?        Ss   08:48   0:00 /usr/sbin/sshd
    root      8158  0.0  0.0   4264   676 pts/1    R+   14:05   0:00 grep sshd

    # ps aux | grep "nginx"
    root     208011  0.0  0.0  95800  8080 ?        Ss    2018   0:00 nginx: master process /usr/sbin/nginx -c /etc/nginx/nginx.conf
    web   402699  0.0  0.1 122940 34492 ?        S    15:52   0:00 nginx: worker process                   
    web   402700  0.0  0.1 122940 34560 ?        S    15:52   0:00 nginx: worker process                   
    web   402701  0.0  0.1 122940 33440 ?        S    15:52   0:00 nginx: worker process                   
    web   402702  0.0  0.1 122940 34536 ?        S    15:52   0:00 nginx: worker process                   
    web   402703  0.1  0.1 122940 34560 ?        S    15:52   0:00 nginx: worker process                   
    web   402704  0.0  0.1 122940 33440 ?        S    15:52   0:00 nginx: worker process                   
    web   402705  0.0  0.1 122940 34584 ?        S    15:52   0:00 nginx: worker process                   
    web   402706  0.0  0.1 122940 34520 ?        S    15:52   0:00 nginx: worker process                   

**父进程**：程序运行时产生的第一个进程
**子进程**：由父进程衍生出来的进程

**注意**：`如果父进程终止，子进程也会随之被终止`


### 二、查看进程
**1、静态ps**

    # ps aux
    USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
    root         1     0.0     0.0       2164   648 ?          Ss     08:47     0:00 init [5]  
    
    USER: 运行进程的用户
    PID： 进程ID
    %CPU: CPU占用率
    %MEM: 内存占用率
    RSS:  占用内存
    TTY： 程序运行的终端
    STAT：状态
          S 处于休眠 Sleep
          R 运行
    
          Z 僵尸进程
          T 停止的进程 
    	 Ss  s进程的领导者，父进程
    	 S<  <优先级较高的进程
    	 SN  N优先级较低的进程
    	 R+  +表示是后台的进程组
    START: 进程的启动时间
    TIME： 进程占用CPU的总时间
    COMMAND： 进程文件，进程名

    # ps axo user,pid,ppid,%mem,command |grep nginx
    root     208011      1  0.0 nginx: master process /usr/sbin/nginx -c /etc/nginx/nginx.conf
    web   402699 208011  0.1 nginx: worker process                   
    web   402700 208011  0.1 nginx: worker process                   
    web   402701 208011  0.1 nginx: worker process                   
    web   402702 208011  0.1 nginx: worker process                   
    web   402703 208011  0.1 nginx: worker process                   
    web   402704 208011  0.1 nginx: worker process                   
    web   402705 208011  0.1 nginx: worker process                   
    web   402706 208011  0.1 nginx: worker process  

    # ps auxf | grep "nginx"
    root     208011  0.0  0.0  95800  8080 ?        Ss    2018   0:00 nginx: master process /usr/sbin/nginx -c /etc/nginx/nginx.conf
    web   402699  0.0  0.1 122940 34492 ?        S    15:52   0:00  \_ nginx: worker process                   
    web   402700  0.0  0.1 122940 34564 ?        S    15:52   0:00  \_ nginx: worker process                   
    web   402701  0.0  0.1 122940 33496 ?        S    15:52   0:00  \_ nginx: worker process                   
    web   402702  0.0  0.1 122940 34536 ?        S    15:52   0:00  \_ nginx: worker process                   
    web   402703  0.1  0.1 122940 34564 ?        S    15:52   0:01  \_ nginx: worker process                   
    web   402704  0.0  0.1 122940 33488 ?        S    15:52   0:00  \_ nginx: worker process                   
    web   402705  0.0  0.1 122940 34584 ?        S    15:52   0:00  \_ nginx: worker process                   
    web   402706  0.0  0.1 122940 34520 ?        S    15:52   0:00  \_ nginx: worker process                   
    web   402707  0.0  0.1 122940 33508 ?        S    15:52   0:00  \_ nginx: worker process   

    # ps -ef
    UID        PID  PPID  C STIME TTY          TIME CMD
    root         1     0  0 08:47 ?        00:00:00 init [5]  

查看指定进程的PID

    # ps aux |grep sshd
    root     10180  0.0  0.0   7224  1024 ?        Ss   16:00   0:00 /usr/sbin/sshd
    # pgrep sshd
    10180
    # pgrep vino-server
    10126
    
    # pstree	//查看进程树


**2、动态top（查看、管理进程）**

    # top
    # top -d 1
    # top -d 1 -p 10126	查看指定进程的动态信息
    # top -d 1 -p `pgrep sshd`    命令替换
    # top -d 1 -p $(pgrep sshd)   命令替换
    # top -d 1 -p $(pgrep vino-server),1
    # top -d 1 -u apache	查看指定用户的进程
    # top -b -n 2 > top.txt 将2次top信息写入到文件

**第一部分**：系统整体统计信息

    load average: 0.86, 0.56, 0.78	  CPU 1分钟，5分钟，15分钟平均负载

**第二部分**：进程信息

    命令
    M	按内存的使用排序
    P	按CPU使用排序
    N	以PID的大小排序
    R	对排序进行反转
    f	自定义显示字段
    
    h|?	帮助
    <	向前
    >	向后
    z	彩色
    
            0－－－实时－－99  100－－－－－－－－－－139
    r	调整进程的优先级 （-20高）  －－－0－－－ （19低）
    k	给进程发送信号 1,9,15
    


### 三、shell查看、管理进程
设置或调整进程的优先级
    # nice -n -5 sleep 6000 &	//程序运行时设置优先级
    # sleep 7000 &
    [3] 10089
    # renice -20 10089		//对已运行的进程设置新的优先级
    10089: old priority 0, new priority -20

给进程发送信号

    # kill -l				//列出所有支持的信号
    编号 信号名
    1) SIGHUP 	重启
    9) SIGKILL		强制终止
    15) SIGTERM	终止（正常退出，干净），缺省信号

### 四、进程前后台

    略

### 五、其他

作业1： 给sshd进程发送信号

    # ps aux |grep sshd
    root      9486  0.0  0.0   7224  1056 ?        Ss   15:01   0:00 /usr/sbin/sshd
    # kill -1 9486			//发送重启信号
    # ps aux |grep sshd
    root      9947  0.0  0.0   7224  1028 ?        Ss   15:42   0:00 /usr/sbin/sshd
    # kill 9947				//发送停止信号
    # ps aux |grep sshd
    root      9953  0.0  0.0   4264   676 pts/1    R+   15:44   0:00 grep sshd

作业2：
    # touch file1 file2
    # tty 
    /dev/pts/1
    # vim file1
    
    # tty
    /dev/pts/2
    # vim file2
    
    # ps aux |grep vim
    root      4362  0.0  0.2  11104  2888 pts/1    S+   23:02   0:00 vim file1
    root      4363  0.1  0.2  11068  2948 pts/2    S+   23:02   0:00 vim file2

    # kill 4362
    # kill -9 4363
    
    
    # killall vim				//给所有vim进程发送信号
    # killall httpd


netstat：针对网络进程

    # netstat -tnlp			//查看正在监听的，使用tcp协议的进程
    -t   tcp协议
    -u  udp协议
    -l   listen
    -p  PID/Program name
    -n  不反解，不将IP地址解析为主机名，不将端口号解析成协议名（80 ---> http）
    # netstat -tnlp |grep :5900
    tcp        0      0 0.0.0.0:5900                0.0.0.0:*                   LISTEN      10126/vino-server   
    
    # netstat -tnlp |grep :80
    # service httpd start
    # netstat -tnlp |grep vino-server
    tcp        0      0 0.0.0.0:5900                0.0.0.0:*                   LISTEN      3772/vino-server    
    # netstat -tnlp |grep sshd
    tcp        0      0 0.0.0.0:22                  0.0.0.0:*                    LISTEN      8737/sshd 
    # netstat -tnlp |grep :80
    tcp        0      0 :::80                       :::*                                LISTEN      10364/httpd 
    
    # netstat -an |grep ：5900	//查看5900端口连接的状态
    'tcp        0      0 0.0.0.0:5900                	0.0.0.0:*                          LISTEN      
    tcp       10      0 192.168.2.115:5900          192.168.2.129:46303         ESTABLISHED 
    tcp       10      0 192.168.2.115:5900          192.168.2.33:39213           ESTABLISHED 
    tcp       10      0 192.168.2.115:5900          192.168.2.116:37023         ESTABLISHED 
    tcp        0      0 192.168.2.115:5900           192.168.2.126:35725         ESTABLISHED 
    tcp       10      0 192.168.2.115:5900          192.168.2.124:33955         ESTABLISHED 
    
    # netstat -an |grep :80
    tcp        0      0 :::80                       :::*                        LISTEN      
    tcp        0      0 ::ffff:192.168.5.230:80     ::ffff:192.168.5.14:59872   TIME_WAIT   
    tcp        0      0 ::ffff:192.168.5.230:80     ::ffff:192.168.5.14:59873   TIME_WAIT   
    tcp        0      0 ::ffff:192.168.5.230:80     ::ffff:192.168.5.14:59874   TIME_WAIT   
    tcp        0      0 ::ffff:192.168.5.230:80     ::ffff:192.168.5.14:59875   TIME_WAIT   
    tcp        0      0 ::ffff:192.168.5.230:80     ::ffff:192.168.5.1:43007    TIME_WAIT   
    tcp        0      0 ::ffff:192.168.5.230:80     ::ffff:192.168.5.14:59848   TIME_WAIT   
    tcp        0      0 ::ffff:192.168.5.230:80     ::ffff:192.168.5.14:59849   TIME_WAIT   
    
    # netstat -an |grep :80 |awk -F":" '{print $8}' |sort |uniq -c |sort -k1 -rn |head -3
       2231 192.168.5.55
       1031 192.168.5.29
        330 192.168.5.11
    

/proc目录
**虚拟文件系统**： 内核、进程运行的状态信息
    # du -sh /proc 
    0       /proc
    
    /etc/cpuinfo
    # grep 'processor' /proc/cpuinfo 	//逻辑cpu的个数
    processor       : 0
    processor       : 1
    # grep 'processor' /proc/cpuinfo |wc -l
    2
    
    # grep 'physical id' /proc/cpuinfo 	//物理cpu
    physical id     : 0
    physical id     : 0
    # grep 'physical id' /proc/cpuinfo |sort |uniq -c
          2 physical id     : 0

    flags         
    lm（64位）
    vmx 支持虚拟化 intel
    svm 支持虚拟化 AMD

    # egrep --color 'lm|vmx|svm' /proc/cpuinfo 
    flags           : fpu vme de clflush dts acpi lm constant_tsc pni monitor ds_cpl vmx est tm2 ssse3 cx16 xtpr lahf_lm
    flags           : fpu vme de clflush dts acpi lm constant_tsc pni monitor ds_cpl vmx est tm2 ssse3 cx16 xtpr lahf_lm
    
    /proc/cmdline
    # cat /proc/cmdline	//内核启动时的参数
    ro root=LABEL=/1 rhgb quiet
    
    /proc/meminfo
    # cat /proc/meminfo
    
    # free -m
                 total       used       free     shared    buffers     cached
    Mem:          2017       1955         61          0        113       1426
    -/+ buffers/cache:        416       1600
    Swap:         8001          0       8001
    
    # uptime 
     17:20:58 up  8:33,  3 users,  load average: 0.43, 0.36, 0.36
    
    如果卸载proc文件系统后。。。
    # mount
    # umount /proc/ -l
    # free -m
    Error: /proc must be mounted
      To mount /proc at boot you need an /etc/fstab line like:
          /proc   /proc   proc    defaults
      In the meantime, run "mount /proc /proc -t proc"
    
    # uptime 
    Error: /proc must be mounted
      To mount /proc at boot you need an /etc/fstab line like:
          /proc   /proc   proc    defaults
      In the meantime, run "mount /proc /proc -t proc"
    
    
    # mount -t proc proc /proc
    -t proc		指定文件系统的类型
    proc			文件系统，虚拟文件系统
    /proc		挂载点
    







