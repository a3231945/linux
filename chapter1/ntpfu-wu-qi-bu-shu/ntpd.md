###一、安装Ntpd服务

    [root@nfs-node2 ~]# yum -y install ntpd
    
###二、配置Ntpd服务
    
    [root@nfs-node2 ~]# vim/etc/ntp.conf 
    driftfile /var/lib/ntp/drift
    restrict default kod nomodify notrap nopeer noquery
    restrict -6 default kod nomodify notrap nopeer noquery
    restrict 127.0.0.1 
    restrict -6 ::1
    
    #允许某个网段使用ntp服务
    restrict 10.0.0.0 mask 255.255.255.0 nomodify notrap
    
    server 0.centos.pool.ntp.org iburst
    server 1.centos.pool.ntp.org iburst
    server 2.centos.pool.ntp.org iburst
    server 3.centos.pool.ntp.org iburst
    includefile /etc/ntp/crypto/pw
    keys /etc/ntp/keys
    
###三、启动服务、配置开机自启动

    [root@nfs-node2 ~]#/etc/init.d/ntpd start
    正在启动 ntpd：                                            [确定]
    [root@nfs-node2 ~]# chkconfig ntpd on


###四、配置防火墙允许访问ntp服务
    [root@nfs-node2 ~]# iptables -I INPUT -p udp -m state --state NEW -m udp --dport 123 -j ACCEPT 
    
###五、测试工作是否正常
    
    [root@nfs-node2 ~]# ntpq -p
     remote           refid      st t when poll reach   delay   offset  jitter
    ==============================================================================
    *85.199.214.101  .GPS.            1 u   61   64    1  310.385   -3.302   2.834
    +static-5-103-13 .GPS.            1 u  122   64    2  410.189  -23.476   3.488
    +cn.ntp.faelix.n 185.134.196.169  2 u   57   64    3  159.115  -10.413   0.547
    