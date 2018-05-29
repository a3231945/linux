**查看系统内存信息**

		[root@localhost ~]# free -m
				total 	used 	free 	shared 	buffers 	cached
		Mem: 	1010 	981 		29 		0 		145 		649
		-/+ buffers/cache: 	186 		824
		Swap: 	2047 	0 		2047

####vmstat 详解
[root@localhost ~]# vmstat
procs -----------memory----------    ---swap--   -----io---- 	--system-- -----cpu------
r b 	swpd free   buff       cache     	si 	so 	bi 	bo 	in 	cs 	  us sy id wa st
0 0 	0 	48584 118300 663564   	0  	0  	12 	23 	60 	240 	   3  2   95 0  0

- **procs**

		r 列表示运行和等待cpu时间片的进程数，如果长期大于cpu个数，说明cpu不足，需要增加cpu。
		b 列表示在等待资源的进程数，比如正在等待I/O、或者内存交换等。

- **memory**

		swpd 切换到内存交换区的内存数量(k表示)。如果swpd的值不为0，或者比较大，比如超过了100m，只要si、so的值长期为0，系统性能还是正常
		free 当前的空闲页面列表中内存数量(k表示)
		buff 作为buffer cache的内存数量，一般对块设备的读写才需要缓冲。
		cache: 作为page cache的内存数量，一般作为文件系统的cache，如果cache较大，说明用到cache的文件较多，如果此时IO中bi比较小，说明文件系统效率比较好。

- **swap**

		si 由内存进入内存交换区数量。
		so由内存交换区进入内存数量。

- **IO**

		bi 从块设备读入数据的总量（读磁盘）（每秒kb）。
		bo 块设备写入数据的总量（写磁盘）（每秒kb）

- **system 显示采集间隔内发生的中断数**

		in 列表示在某一时间间隔中观测到的每秒设备中断数。
		cs列表示每秒产生的上下文切换次数，如当 cs 比磁盘 I/O 和网络信息包速率高得多，都应进行进一步调查。

- **buffer/cache**

		[root@localhost ~]# cat /proc/sys/vm/dirty_
		dirty_background_ratio 	dirty_ratio
		dirty_expire_centisecs 	dirty_writeback_centisecs
		[root@localhost ~]# cat /proc/sys/vm/dirty_writeback_centisecs
		499
		5秒同步一次脏数据
		[root@localhost ~]# cat /proc/sys/vm/dirty_ratio
		40
		如果单个进程占用的buffer/cache达到内存总量的40%,立刻同步。
		[root@localhost ~]# cat /proc/sys/vm/dirty_background_ratio
		10
		所有进程占用的buffer/cache使得剩余内存低于内存总量的10%，立刻同步
		[root@localhost ~]# cat /proc/sys/vm/dirty_expire_centisecs
		2999
		30秒之内数据必须被同步

**2、释放buffer/cache**

		[root@localhost ~]# cat /proc/sys/vm/drop_caches
		0
		1 释放cache
		2 释放buffer
		3 buffer/cache都释放


**3、使用内存文件系统**

		[root@localhost ~]# df -h
		Filesystem 	Size 	Used 	Avail 	Use% 	Mounted on
		/dev/sda3 	37G 	3.1G 	32G 	9% 		/
		/dev/sda1 	99M 	12M 	83M 	13% 	/boot
		tmpfs 		506M 	0 		506M 	0% 		/dev/shm （共享内存）
		/dev/hdc 	2.9G 	2.9G 	0 		100% 	/media/RHEL_5.8 i386 DVD
		
		[root@localhost ~]# mount -t tmpfs -o size=100M tmpfs /mnt/
		[root@localhost ~]# mount | grep tmp
		tmpfs on /dev/shm type tmpfs (rw)
		tmpfs on /mnt type tmpfs (rw,size=100M)

**4、查看内存使用情况**

		[root@localhost ~]# sar -r 1 1


**5、内存测试**

		[root@localhost ~]# yum install memtest86+
		[root@localhost ~]# memtest-setup
		Setup complete.
		重新启动系统是使用memtest86+
