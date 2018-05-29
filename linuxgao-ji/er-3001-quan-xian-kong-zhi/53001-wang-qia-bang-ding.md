Ethernet Channel Bonding

	在这介绍的Linux双网卡绑定实现就是使用两块网卡虚拟成为一块网卡，这个聚合起来的设备看起来是一个单独的以太网接口设备，通俗点讲就是两块网卡具有相同的IP地址而并行链接聚合成一个逻辑链路工作。其实这项技术在Sun和Cisco中早已存在，被称为Trunking和Etherchannel技术，在Linux的2.4.x的内核中也采用这这种技术，被称为bonding。bonding技术的最早应用是在集群，为了提高集群节点间的数据传输而设计的。


可以在文档中找到bonding的配置方式

	[root@localhost ~]# rpm -q kernel-doc
	/usr/share/doc/kernel-doc-2.6.18/Documentation/networking/bonding.txt


**分别修改2个网卡配置文件，声明自己为slave，master是bond0**

	[root@localhost ~]# vim /etc/sysconfig/network-scripts/ifcfg-eth0
	
	DEVICE=eth0
	USERCTL=no
	ONBOOT=yes
	BOOTPROTO=none
	MASTER=bond0
	SLAVE=yes

**生成master设备的配置文件**

	[root@localhost ~]# vim /etc/sysconfig/network-scripts/ifcfg-bond0
	DEVICE=bond0
	IPADDR=192.168.122.254
	NETMASK=255.255.255.0
	ONBOOT=yes
	BOOTPROTO=none
	USERCTL=no


bond0是什么设备？实际我们做的网卡绑定，是通过bonding模块来实现的，所以要bonding模块设置一个别名，指向我们创建的bond0

	[root@localhost ~]# vim /etc/modprobe.conf
	alias bond0 bonding
	options bonding miimon=100 mode=balance-rr

	miimon是用来进行链路监测的。 比如:miimon=100，那么系统每100ms监测一次链路连接状态，如果有一条线路不通就转入另一条线路；
	mode指定了bond0的工作模式，在redhat中有0－6共7种工作模式，常用的是0和1。
	mode=0 表示 load balancing (round-robin)为负载均衡方式，两块网卡都工作。 
	mode=1 表示 fault-tolerance (active-backup)提供冗余功能，工作方式是主 从的工作方式,也就是说默认情况下只有一块网卡工作,另一块做备份。  
	mode=2 表示 XOR policy 为平衡策略。此模式提供负载平衡和容错能力  
	mode=3 表示 broadcast 为广播策略。此模式提供了容错能力  
	mode=4 表示 IEEE 802.3ad Dynamic link aggregation 为 IEEE 802.3ad 为 动态链接聚合。该策略可以通过 xmit_hash_policy 选项从缺省的 XOR 策略改变到其他策略。  
	mode=5 表示 Adaptive transmit load balancing 为适配器传输负载均衡。该 模式的必要条件：ethtool 支持获取每个 slave 的速率  
	mode=6 表示 Adaptive load balancing 为适配器适应性负载均衡。该模式包含 了 balance-tlb 模式，同时加上针对 IPV4 流量的接收负载均衡(receive load   balance, rlb)，而且不需要任何 switch(交换机)的支持。  
	bonding 只能提供链路监测，即从主机到交换机的链路是否接通。如果只是交换机对 外的链路 down 掉了，而交换机本身并没有故障，那么 bonding 会认为链路没有问题而继 续使用。



[root@localhost ~]# service network restart

[root@localhost ~]# cat /proc/net/bonding/bond0
