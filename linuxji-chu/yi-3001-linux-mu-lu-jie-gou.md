###一、目录结构的特点
linux 文件系统如下有两个特点：
 - 逻辑上，所有的目录都在最高级别的根目录 “/”下。
 - 所有的目录内容按照类别组织。

###二、目录结构
**1、Linux目录结构：**

	 根目录结构
	 查看：
	 ls -la /
	 或者
	 tree  -L 1 /  # -L1  表示显示 "/ "下目录的层次，1表示一层 

	/bin 	二进制命令目录
	/boot 	内核及启动程序所需要的文件目录
	/dev  	设备文件目录
	/etc   	常见系统及二进制安装包配置文件默认路径和服务启动命令目录
	/home	普通用户的家目录
	/lib	库文件存放目录
	/mnt	临时挂载目录
	/opt	
	/proc   操作系统运行时，进程信息及内核信息存放的目录
	/usr 	系统存放程序的目录
	/tmp	临时目录
 


**2、重要子目录：**


	/etc/sysconfig/network-scripts/ifcfg-eth0  网卡
	/etc/resolv.conf   DNS
	/etc/hosts    host解析文件
	/etc/sysconfig/network  主机名，网卡启动配置
	/etc/fstab  	开机挂载文件
	/etc/inittab 	init 程序配置文件
	/etc/exports	nfs配置文件
	/etc/init.d  	系统服务脚本存放目录
	/etc/profile	系统全局环境配置路径
	
**3、/etc 下重要的目录：**

	/etc/issue 	记录用户登录前显示的信息
	/etc/group	设定用户的组名与相关信息
	/etc/passwd	账号信息
	/etc/shadow	密码信息
	/etc/sudoers	可以sudo命令的配置文件
	/etc/login.defs	所有用户登录时的缺省配置
	/etc/modprobe.conf 内核模块额外参数设定
	/etc/syslog.conf	日志设置文件
	
**4、其他目录：**

	/var	 日志文件
	/var/log 各种系统日志存放地
	/var/log/message 系统信息默认日志文件，非常重要，按周轮训
	/var/log/secure 记录登入系统存储信息的文件，按周轮训。
	/var/log/wtmp	记录登录者信息的文件
	/var/spool	定时任务crontab 默认目录，按用户名命名的文件
	/var/spool/cron	
	/var/spool/mail	系统用户邮件存放目录
	/var/spool/clientmqueue	临时邮件目录，有很多原因会导致这个目录碎文件很多

**5、/proc 下的重要路径知识：**

	/proc 虚拟目录，是内存的映射
	/proc/version  内核版本
	/proc/sys/kernel 系统内核功能
	/proc/cpuinfo   关于CPU的信息
	/proc/meminfo   关于内存的信息
	/proc/devices	当前运行内核所配置的所有设备清单
	/proc/dma	当前正在使用的DMA通道
	/proc/filesystems	当前运行内核所配置的文件系统
	/proc/interrupts	正在使用的中断，和曾经有多少个中断
	/proc/ioports		当前正在使用的I/0端口
	/proc/loadavg 		系统负载信息，uptime的结果
	
**6、其他路径知识（了解）：**

	/etc/DIR_COLORS 设定颜色
	/etc/host.conf 	文件说明用户的系统如何查询节点名
	/etc/hosts.allow  设置允许使用inetd的机器使用
	/etc/hosts.deny
	/etc/protocols	系统支持的协议文件	
	/etc/X11	X windows的配置文件
