# Heartbeat 安装  #


### 一、环境初始化 ###

**1、主机名**

    server-node1  192.168.160.101
    server-node2  192.168.160.102 
    virt-ip       192.168.160.200

**2、配置ntp 时间同步**

    crontab -e
    */5 * * * * ntpupdate ntpserver

**3、清除iptables or 开启UDP 694端口**
    
    iptables -F
    iptables -X
    iptables -Z
    /etc/init.d/iptables save
    /etc/init.d/iptables stop
    
**4、配置epel源**

    rpm -ivh https://mirrors.aliyun.com/epel/epel-release-latest-6.noarch.rpm

**5、安装软件**

    yum -y install heartbeat
    
### 二、配置heartbeat ###
**1、配置文件管理**

- 秘钥文件    权限 600    authkeys
- heartbeat 服务的配置    ha.cf
- 资源管理配置文件         haresources

**2、拷贝模板文件**

    cp -p /usr/share/doc/heartbeat-3.0.4/authkeys  /etc/ha.d/ 
    cp /usr/share/doc/heartbeat-3.0.4/ha.cf  /etc/ha.d/
    cp /usr/share/doc/heartbeat-3.0.4/haresources  /etc/ha.d/

**3、修改authkeys配置文件**

    vim /etc/ha.d/authkeys
    ####添加，使用MD5验证，验证密码是www.peter-zhou.com
    auth 1
    1 md5 www.peter-zhou.com
    
**4、修改ha.cf 配置文件**

    vim /etc/ha.d/ha.cf

    #日志记录方式，rsyslog 或者指定路劲
    logfacility	local0

    #探测间隔时间，可以为毫秒 1500ms  代表 1.5秒
    keepalive 1
    deadtime 30
    warntime 10

    #启动准备时间
    initdead 120
    udpport	694

    #心跳方式：单播、组播、广播
    ucast eth0 192.168.160.102
    auto_failback on

    #指定节点主机名： 与uname -n 对应
    node rsync-node1
    node rsync-node2

    #三方仲裁ip
    ping_group group1 192.168.160.2

** 5、修改 haresources 配置**
    
    vim /etc/ha.d/haresources 
    #指定主节点   #共享资源IP                      #共享服务
    rsync-node1  IPaddr::192.168.160.200/24/eth0 httpd

   
**6、同步配置到另一台机器（单播IP不一样）**

    
**7、启动服务**
 
    /etc/init.d/heartbeat start
            

  

**官方地址：**[http://clusterlabs.org/](http://clusterlabs.org/)

**官方地址2：**[http://www.linux-ha.org/wiki/Main_Page](http://www.linux-ha.org/wiki/Main_Page)