# NTP服务器搭建 #
### 一、NTP服务器规划 ###
**1、同步方式：**
    
    if1： 向公网同步
    if2： 向if1 和公网同步 优先向if1同步

**2、服务端使用方式：**
    
    1>使用限制：只允许指定网段同步
    2>安全限制：只监听内网地址
    3>使用内网dns服务器 轮训策略 达到负载均衡

**3、客户端使用方式：** `10分钟同步一次`
    
    
### 一、NTP服务安装 ###

    yum -y install ntp ntpdate


### 二、配置NTP服务 ###
**1、备份配置文件：** `cp /etc/ntp.conf /etc/ntp.conf.bak`

**2、修改配置文件:** `vim /etc/ntp.conf`
    
    driftfile /var/lib/ntp/drift
    restrict default kod nomodify notrap nopeer noquery
    restrict -6 default kod nomodify notrap nopeer noquery
    
    #允许10.0.0.0 网段使用ntp服务器
    restrict 10.0.0.0 mask 255.0.0.0 nomodify notrap 

    restrict 127.0.0.1 
    restrict -6 ::1

    #优先向该服务器同步
    server 0.centos.pool.ntp.org iburst prefer 

    server 1.centos.pool.ntp.org iburst
    server 2.centos.pool.ntp.org iburst
    server 3.centos.pool.ntp.org iburst
    
    #监听IP
    server xxx.xxx.xxx.xxx
    server xxx.xxx.xxx.xxx
    #当服务器与公用服务器时间失去联系，以局域网 10.32.101.11 服务器为客户端提供时间同步服务
    fudge  xxx.xxx.xxx.xxx  startum 10

    includefile /etc/ntp/crypto/pw
    keys /etc/ntp/keys

**3、启动服务:**`/etc/init.d/ntpd start`

**4、服务开机自启动:** `chkconfig ntpd on`
###三、配置客户端  ###
**1、dns 解析  **

    ntp.op.com  -> xxx.xxx.xxx.xxx

    ntp.op.com  -> xxx.xxx.xxx.xxx

    


**2、客户端配置时间同步**

    */10 * * * * (/usr/sbin/ntpdate -s ntp.op.com && /sbin/hwclock -w) > /dev/null &



