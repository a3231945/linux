分时操作系统按照相等的时间片调度进程轮流运行，分时操作系统由调度程序自动计算进程的优先级，而不是由用户控制进程的优先级。这样的系统无法实时响应外部异步事件。分时系统主要应用于科学计算和一般实时性要求不高的场合。实时性系统主要应用于过程控制、数据采集、通信、多媒体信息处理等对时间敏感的场合。一台中型机（或者小型机）带着一大堆终端，然后轮流响应各终端的请求，就是一个典型的分时系统。

我们将CPU就类比为电话亭，每一个进程都是一个需要打电话的人。现在一共有4个电话亭（就好比我们的机器有4核），有10个人需要打电话。现在使用电话的规则是管理员会按照顺序给每一个人轮流分配1分钟的使用电话时间，如果使用者在1分钟内使用完毕，那么可以立刻将电话使用权返还给管理员，如果到了1分钟电话使用者还没有使用完毕，那么需要重新排队，等待再次分配使用。
	
实时操作系统能够在限定的时间内执行完所规定的功能，并能在限定的时间内对外部的异步事件作出响应。实时系统主要用在工业控制或者订票等方面，对响应速度要求非常高的场合从操作系统能否满足实时性要求来区分，可把操作系统分成分时操作系统和实时操作系统。
	
	
	[root@localhost ~]# top
	top - 22:14:02 up 22:06, 12 users, load average: 1.00, 0.66, 0.30
				             系统平均负载: 1分钟 5分钟 15分钟
						
系统平均负载被定义为在特定时间间隔内运行队列中的平均进程数。如果一个进程满足以下条件则其就会位于运行队列中：
	- 它没有在等待I/O操作的结果
	- 它没有主动进入等待状态(也就是没有调用'wait')
	- 没有被停止(例如：等待终止)
	
一般来说只要每个CPU的当前活动进程数不大于3那么系统的性能就是良好的，如果每个CPU的任务数大于5，那么就表示这台机器的性能有严重问题。但具体要看应用，一般推荐与CPU核数相当，例：8核CPU，每个应用跑满了50％，可以跑16个进程，但是也要考虑到突发量


**进程优先级**

	[root@localhost ~]# ps -l
	F S 	UID 	PID 		PPID 	C 	PRI 	NI 	ADDR 	SZ 		WCHAN 	TTY 		TIME 	CMD
	0 S 	0 	18684 	32381 	0 	75 	0 	- 		1196 	wait 	pts/4 	00:00:00 bash
	4 R 	0 	19442 	18684 	0 	77 	0 	- 		1115 	- 		pts/4 	00:00:00 ps

	NI 0 ------- PRI 80

**查看实时进程的优先级**	

	[root@localhost ~]# chrt -p 2
	pid 2's current scheduling policy: SCHED_FIFO
	pid 2's current scheduling priority: 99

**查看进程调度策略**

	[root@localhost ~]# chrt -m
	SCHED_OTHER min/max priority : 0/0 非实时进程
	SCHED_FIFO min/max priority : 1/99 实时进程
	SCHED_RR min/max priority : 1/99
	SCHED_BATCH min/max priority : 0/0

**程序以实时进程方式运行**

	[root@localhost ~]# chrt --fifo 10 ./a.sh



程序的并发执行
进程和线程
进程：单线程进程（数据独享，无竞争问题，创建撤销成本高）
线程：多线程进程（数据共享，写竞争问题，创建撤销成本低）

单核与多核
单核区分I/O消耗还是CPU消耗
多核并发，可以绑定CPU执行


**taskset 把任务绑定在指定CPU执行（CPU亲和力）**

	[root@localhost ~]# taskset -p 1
	pid 1's current affinity mask: 1
	[root@localhost ~]# taskset -c 1 ./a.sh


**系统调优5贱客mpstat、vmstat、iostat netstat、sar （安装sysstat）**

	[root@localhost ~]# mpstat
	Linux 2.6.18-194.el5 (localhost.localdomain) 04/01/2012
	12:59:14 AM CPU %user %nice %sys %iowait %irq %soft %steal %idle intr/s
	12:59:14 AM all 2.47 0.11 1.89 0.45 0.08 0.07 0.00 94.93 1016.79
	[root@localhost ~]# mpstat -p ALL 1 10


**查看已经注册的中断**

	[root@localhost ~]# cat /proc/interrupts 
	每秒中的中断数kernel-xen 250 kernel 1000
	希望进程吞吐量高 少
	希望响应速度快 多
	可以编译内核做调整
	增大网络响应，修改网卡中断，交给多个核心处理
	cat /proc/irq/num/smp_affinity
	[root@localhost Documentation]# cat /proc/irq/67/smp_affinity
	00000001

**CPU下线**

	[root@localhost Documentation]# cat /sys/devices/system/cpu/cpu1/online




**安装sysstat后会使用自动化任务收集系统信息**

	[root@localhost ~]# cat /etc/cron.d/sysstat
	# run system activity accounting tool every 10 minutes
	*/10 * * * * root /usr/lib/sa/sa1 1 1
	# generate a daily summary of process accounting at 23:53
	53 23 * * * root /usr/lib/sa/sa2 -A


**Gnuplot**

	[root@localhost ~]# LANG=c sar -f /var/log/sa/sa01 > /tmp/sar.log
	[root@localhost ~]# yum install gnuplot
	[root@localhost ~]# vim ./gnuplot.sh
	#!/bin/bash
	gnuplot << EOF
	set xdata time
	set timefmt "%H:%M:%S"
	set terminal gif size 640,480
	set output "/tmp/cpu.gif"
	set title "CPU of STATUS"
	set xlabel "TIME"
	set ylabel "user、system、iowait"
	set grid
	plot \
	"/tmp/sar.log" using 1:3 title "user" with lines , "/tmp/sar.log" using 1:5 title "system" with lines , "/tmp/sar.log" using 1:4 title "iowait" with lines
	EOF
	
	[root@localhost ~]# eog /tmp/cpu.gif