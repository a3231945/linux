#HIDS

**1、进程安全**
使用top ps命令监控是否有可疑进程;查看进程状态是否正常
ulimit 功能说明：控制shell程序的资源。
- 限制用户登陆数
- 限制用户创建文件最大尺寸。
- 限制用户创建进程数。



**2、文件安全**

查看文件是否有可疑的改变（权限，内容，大小，时间）
rpm -V
RPM软件包校验RPM使用“-V”选项进行校验，可以根据RPM数据库对比系统所有文件属性（大小、日期、所有者、md5sum值等），列出不同之处。如果执行没有输出，表示自从软件包安装之日起就没有发生过任何变化。
find /etc -type f -exec md5sum {} \;


**3、日志安全**

重要的日志文件
- /var/log/boot.log `该文件记录了系统在引导过程中发生的事件，就是Linux系统开机自检过程显示的信息。`
- /var/log/cron `每当cron进程开始一个工作时，就会将相关信息记录在这个文件中。`
- /var/log/secure `该日志文件记录与安全相关的信息。`
- /var/log/lastlog `该日志文件记录最近成功登录的事件和最后一次不成功的登录事件，由login生成。在每次用户登录时被查询，该文件是二进制文件，需要使用lastlog命令查看，根据UID排序显示登录名、端口号和上次登录时间。`
- /var/log/wtmp `该日志文件永久记录每个用户登录、注销及系统的启动、停机的事件。因此随着系统正常运行时间的增加，该文件的大小也会越来越大，增加的速度取决于系统用户登录的次数。last命令`
- /var/run/utmp `该日志文件记录有关当前登录的每个用户的信息。因此这个文件会随着用户登录和注销系统而不断变化，它只保留当时联机的用户记录，不会为用户保留永久的记录。`
- /var/log/btmp `关于失败登陆的日志。lastb命令`
- /var/log/xferlog `FTP日志 该文件的格式为：第一个域是日期和时间，第二个域是下载文件所花费的秒数、远程系统名称、文件大小、本地路径名、传输类型（a：ASCII，b：二进制）、与压缩相关的标志或tar，或“_”（如果没有压缩的话）、传输方向（相对于服务器而言：i代表进，o代出）、访问模式（a：匿名，g：输入口令，r：真实用户）、用户名、服务名（通常是ftp）、认证方法（l：RFC931，或0），认证用户的ID或“*”。`

日志使用的重要原则

- 观察用户在非常规事件登录后的操作,不正常的日志记录，比如日志的残缺不全或者是诸如wtmp这样的日志文件无故地缺少了中间的记录文件;
- 用户登录来源IP异常,用户登录系统的IP地址和以往的不一样;
- 用户登录尝试次数,用户登录失败的日志记录，尤其是那些一再连续尝试进入失败的日志记录;
- 非法的su和sudo尝试,非法使用或不正当使用超级用户权限su的指令;/var/log/sudo.log
- 无故或非法的重启,kernel(种木马)
- 本地日志不完全可靠，最好异地日志（syslog远程日志）汇总 尤其提醒管理人员注意的是：日志并不是完全可靠的。高明的黑客在入侵系统后，经常会打扫现场。所以需要综合运用以上的系统命令，全面、综合的进行审查和检测，切忌断章取义，否则很难发现入侵或者做出错误的判断。
- 需要及时的异地日志异常报警机制,脚本监视某用户，口令输错次数。


**4、audit 审计**

audit子系统提供了一种纪录系统安全方面信息的方法，同时能为系统管理员在用户违反系统安全法则或存在违反的潜在可能时，提供及时的警告信息，这些audit子系统所搜集的信息包括：可被审计的事件名称，事件状态（成功或失败），别的安全相关的信息。可被审计的事件，通常，这些事件都是定义在系统调用级别的。

审计的软件包一般默认已经安装

    [root@node1 ~]# rpm -qf /etc/init.d/auditd
    audit-1.7.17-3.el5

服务一般默认就已经启动

    [root@node1 ~]# service auditd status
    auditd (pid 2815) is running...

查看audit状态，enabled=1开启审计

    [root@node1 ~]# auditctl -s
    AUDIT_STATUS: enabled=1 flag=1 pid=2815 rate_limit=0 backlog_limit=320 lost=0 backlog=0

如何设置审计策略可以查看auditctl man手册中的EXAMPLE

    [root@node1 ~]# man auditctl
    To recursively watch a directory for changes (2 ways to express):
    auditctl -w /etc/ -p wa
    auditctl -a exit,always -F dir=/etc/ -F perm=wa
    -w 监控
    -p 权限 r=read, w=write, x=execute, a=attribute(所属者所属组)
    [root@node1 ~]# auditctl -w /tmp/ -p rwxa -k "TEST"
    [root@node1 ~]# auditctl -l
    LIST_RULES: exit,always dir=/tmp (0x4) perm=rwxa key=TEST
    auditctl -l 查看所有
    auditctl -D 删除清空

开启一个新的终端，使用user1进行测试

    [root@node1 ~]# su - user1
    [user1@node1 ~]$
    [user1@node1 ~]$
    [user1@node1 ~]$ ls /tmp


切换回管理员终端，查看审计信息

    [root@node1 ~]# ausearch -k "TEST"
    ----
    time->Tue Aug 6 08:29:17 2013
    type=CONFIG_CHANGE msg=audit(1375748957.416:110): auid=0 op=add rule key="TEST" list=4 res=1
    ----
    time->Tue Aug 6 08:30:02 2013
    type=PATH msg=audit(1375749002.563:111): item=0 name="/tmp" inode=393217 dev=fc:00 mode=041777 ouid=0 ogid=0 rdev=00:00
    type=CWD msg=audit(1375749002.563:111): cwd="/home/user1"
    type=SYSCALL msg=audit(1375749002.563:111): arch=40000003 syscall=5 success=yes exit=3 a0=94578f8 a1=18800 a2=805e560 a3=94578e0 items=1 ppid=26349 pid=26396 auid=0 uid=500 gid=500 euid=500 suid=500 fsuid=500 egid=500 sgid=500 fsgid=500 tty=pts1 ses=11 comm="ls" exe="/bin/ls" key="TEST"


以下2个命令的效果是一致的

    [root@node1 ~]# auditctl -w /tmp -p rwxa
    [root@node1 ~]# auditctl -a exit,always -F dir=/tmp -F perm=rwxa
    -a exit,always exit：行为完成后记录审计（一般常用），always： 总是记录审计
    -F 规则字段



    auid为初始登录ID，auid不为0，uid为0，表示登录系统的时候为非root用户，执行操作时却变为root，危险行为。
    auditctl -a exit,always -F auid!=0 -F uid=0
    uid不为0，euid为0，表示执行者是一个非root用户，但是执行过程中却是以root的身份执行的，是一个提权操作，危险行为。
    auditctl -a exit,always -F uid!=0 -F euid=0
    工作中常对 /tmp /etc 审计，攻击者常用/tmp提权


    aureport 可以用来查看系统审计日志的汇总信息，例如aureport -l 可以用来查看login信息
