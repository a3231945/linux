影响I/O性能的因素
1、缓存
2、文件系统：日志文件系统和非日志文件系统
3、I/O调度算法
4、RAID，LVM（条带化） 网络附加存储（cpu的iowait转移到存储服务器）

**1、缓存**

	分别使用async、sync挂载，dd文件测试速度差异
	[root@node1 ~]# mount /dev/sdb1 /mnt/
	[root@node1 ~]# mount -o remount,sync /dev/sdb1 /mnt/

**2、日志文件系统和非日志文件系统**

	1、创建2个分区，分别格式化为ext2、ext3，dd文件测试速度差异
	
	2、日志空间与数据空间分离
	[root@localhost ~]# mount -o data=journal /dev/sdb1 /mnt/
	[root@localhost ~]# mke2fs -O journal_dev /dev/sdb5
	[root@localhost ~]# mkfs.ext3 -J device=/dev/sdb5 /dev/sdb1
	[root@localhost ~]# tune2fs -l /dev/sdb1 | grep Journal
	Journal UUID: 2a8ae11d-e895-4fd9-9b44-aa2c5e1f4e13
	Journal device: 0x0815
	

**3、I/O 调度算法，在linux下面列出4种调度算法**

	[root@localhost ~]# cat /sys/block/sda/queue/scheduler
	noop anticipatory deadline [cfq]

- **CFQ (Completely Fair Queuing 完全公平的排队)(elevator=cfq)：**

	这是默认算法，对于通用服务器来说通常是最好的选择。它试图均匀地分布对I/O带宽的访问。在多媒体应用, 总能保证audio、video及时从磁盘读取数据。但对于其他各类应用表现也很好。每个进程一个queue，每个queue按照上述规则进行merge 和sort。进程之间round robin调度，每次执行一个进程的4个请求。

- **Deadline (elevator=deadline)：**

	这个算法试图把每次请求的延迟降至最低。该算法重排了请求的顺序来提高性能。
- **NOOP (elevator=noop):**

	这个算法实现了一个简单FIFO队列。他假定I/O请求由驱动程序或者设备做了优化或者重排了顺序(就像一个智能控制器完成的工作那样)。在有些SAN环境下，这个选择可能是最好选择。适用于随机存取设备, no seek cost，非机械可随机寻址的磁盘。
	
- **Anticipatory (elevator=as):**

	这个算法推迟I/O请求，希望能对它们进行排序，获得最高的效率。同deadline不同之处在于每次处理完读请求之后, 不是立即返回, 而是等待几个微妙在这段时间内, 任何来自临近区域的请求都被立即执行. 超时以后, 继续原来的处理.基于下面的假设: 几个微妙内, 程序有很大机会提交另一次请求.调度器跟踪每个进程的io读写统计信息, 以获得最佳预期.


**对IO调度使用的建议**

	Deadline I/O scheduler 使用轮询的调度器,简洁小巧,提供了最小的读取延迟和尚佳的吞吐量,特别适合于读取较多的环境(比如数据库,Oracle 10G 之类).
	
	Anticipatory I/O scheduler 假设一个块设备只有一个物理查找磁头(例如一个单独的SATA硬盘),将多个随机的小写入流合并成一个大写入流,用写入延时换取最大的写入吞吐量.适用于大多数环境,特别是写入较多的环境(比如文件服务器)Web,App等应用我们可以采纳as调度.
	
	CFQ I/O scheduler使用QoS策略为所有任务分配等量的带宽,避免进程被饿死并实现了较低的延迟,可以认为是上述两种调度器的折中.适用于有大量进程的多用户系统
	在生产环境中测试过一台机器本来流量只有350M的样子,有时压力就不行了,流量也上不去了,由于读比较多,所以使用deadline后,流量可以上升大约50M.

linux启动时设置默认IO调度,让系统启动时就使用默认的IO方法,只需在grub.conf文件中加入类似如下行
kernel /vmlinuz-2.6.24 ro root=/dev/sda1 elevator=deadline

**4、条带化（条带的大小，条带小意味这条带多，条带多大？（根据读写的数据量长度决定）avgrq-sz）**

	LVM条带化
	lvcreate -i 4 -I 64 -n lvtest -L 20G vgtest
	mke2fs -j -E stride=16 /dev/vgtest/lvtest
	
	RAID
	mdadm -C /dev/md0 -l 0 -n 4 -c 16 /dev/sda{7,8,9,10}
	mdadm -D /dev/md0
	Chunk Size : 16K


**查看I/O状态**

	[root@localhost ~]# sar -d 1 100
	
	[root@localhost ~]# iostat -k
	
	[root@localhost ~]# iostat -x 1
