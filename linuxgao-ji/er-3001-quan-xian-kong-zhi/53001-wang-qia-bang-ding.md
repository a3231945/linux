Ethernet Channel Bonding
在这介绍的Linux双网卡绑定实现就是使用两块网卡虚拟成为一块网卡，这个聚合起来的设备看起来是一个单独的以太网接口设备，通俗点讲就是两块网卡具有相同的IP地址而并行链接聚合成一个逻辑链路工作。其实这项技术在Sun和Cisco中早已存在，被称为Trunking和Etherchannel技术，在Linux的2.4.x的内核中也采用这这种技术，被称为bonding。bonding技术的最早应用是在集群，为了提高集群节点间的数据传输而设计的。


可以在文档中找到bonding的配置方式
[root@localhost ~]# rpm -q kernel-doc
/usr/share/doc/kernel-doc-2.6.18/Documentation/networking/bonding.txt


分别修改2个网卡配置文件，声明自己为slave，master是bond0
[root@localhost ~]# vim /etc/sysconfig/network-scripts/ifcfg-eth0
－－－
DEVICE=eth0
USERCTL=no
ONBOOT=yes
BOOTPROTO=none
MASTER=bond0
SLAVE=yes

－－－

生成master设备的配置文件
[root@localhost ~]# vim /etc/sysconfig/network-scripts/ifcfg-bond0
-----------
DEVICE=bond0
IPADDR=192.168.122.254
NETMASK=255.255.255.0
ONBOOT=yes
BOOTPROTO=none
USERCTL=no
-----------

bond0是什么设备？实际我们做的网卡绑定，是通过bonding模块来实现的，所以要bonding模块设置一个别名，指向我们创建的bond0
[root@localhost ~]# vim /etc/modprobe.conf
－－－
alias bond0 bonding
options bonding miimon=100 mode=balance-rr
－－－

miimon是用来进行链路监测的。 比如:miimon=100，那么系统每100ms监测一次链路连接状态，如果有一条线路不通就转入另一条线路；
mode的值表示工作模式，他共有0，1,2,3四种模式，常用的为0,1两种。
	mode=0表示load balancing (round-robin)为负载均衡方式，两块网卡都工作。
	mode=1表示fault-tolerance (active-backup)提供冗余功能，工作方式是主备的工作方式,也就是说默认情况下只有一块网卡工作,另一块做备份.

[root@localhost ~]# service network restart

[root@localhost ~]# cat /proc/net/bonding/bond0
