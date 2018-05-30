###一、安装Chrony服务

    [root@nfs-node2 ~]# yum -y install chrony
    
###二、配置Chrony服务
    
    [root@nfs-node2 ~]# vim /etc/chrony.conf 
    driftfile /var/lib/ntp/drift
    restrict default kod nomodify notrap nopeer noquery
    restrict -6 default kod nomodify notrap nopeer noquery
    restrict 127.0.0.1 
    restrict -6 ::1
    
    #允许某个网段使用ntp服务
    allow 10.0.0.0/24
    
    server 0.centos.pool.ntp.org iburst
    server 1.centos.pool.ntp.org iburst
    server 2.centos.pool.ntp.org iburst
    server 3.centos.pool.ntp.org iburst
    includefile /etc/ntp/crypto/pw
    keys /etc/ntp/keys
    
###三、启动服务、配置开机自启动

    [root@nfs-node2 ~]#/etc/rc.d/init.d/chronyd start 
    正在启动 ntpd：                                            [确定]
    [root@nfs-node2 ~]# chkconfig chronyd on


###四、配置防火墙允许访问Chrony服务
    [root@nfs-node2 ~]# iptables -p udp -m state --state NEW -m udp --dport 123 -j ACCEPT 
    
###五、测试工作是否正常
    
    [root@nfs-node2 ~]# chronyc sources 
     remote           refid      st t when poll reach   delay   offset  jitter
    ==============================================================================
    *85.199.214.101  .GPS.            1 u   61   64    1  310.385   -3.302   2.834
    +static-5-103-13 .GPS.            1 u  122   64    2  410.189  -23.476   3.488
    +cn.ntp.faelix.n 185.134.196.169  2 u   57   64    3  159.115  -10.413   0.547
    