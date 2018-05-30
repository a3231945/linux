###一、安装bind


    [root@nfs-node2 ~]# yum -y install bind bind-utils
    
###二、配置只使用IPV4

    [root@nfs-node2 ~]# echo 'OPTIONS="-4"' >> /etc/sysconfig/named


###三、配置bind
**1、配置主配置文件：**`vim /etc/named.conf`
        
        //
        // Provided by Red Hat bind package to configure the ISC BIND named(8) DNS
        // server as a caching only nameserver (as a localhost DNS resolver only).
        //
        // See /usr/share/doc/bind*/sample/ for example named configuration files.
        //
        
        options {
                listen-on port 53 { 127.0.0.1; };
                #不使用ip-v6
                listen-on-v6      { none; };
                directory       "/var/named";
                dump-file       "/var/named/data/cache_dump.db";
                statistics-file "/var/named/data/named_stats.txt";
                memstatistics-file "/var/named/data/named_mem_stats.txt";
                #允许查询的网段
                allow-query     { localhost;10.0.0.0/24; };
                
                #允许传送指定网段（slave dns）
                allow-transfer { localhost; 10.0.0.0/24; };
                recursion yes;
        
                dnssec-enable yes;
                dnssec-validation yes;
        
                /* Path to ISC DLV key */
                bindkeys-file "/etc/named.iscdlv.key";
        
                managed-keys-directory "/var/named/dynamic";
        };
        
        logging {
                channel default_debug {
                        file "data/named.run";
                        severity dynamic;
                };
        };
        
        
        
        #定义内部解析
        view "internal" {
                match-clients {
                        localhost;
                        10.0.0.0/24;
                };
                zone "." IN {
                        type hint;
                        file "named.ca";
                };
                zone "host.com" IN {
                        type master;
                        file "host.com.lan";
                        allow-update { none; };
                };
                zone "0.0.10.in-addr.arpa" IN {
                        type master;
                        file "0.0.10.db";
                        allow-update { none; };
                };
                include "/etc/named.rfc1912.zones";
                include "/etc/named.root.key";
        
        };
        
        #定义公网解析
        view "external" {
                match-clients { any; };
                allow-query { any; };
                recursion no;
                zone "host.com" IN {
                        type master;
                        file "host.com.wan";
                        allow-update { none; };
                };
                zone "0.0.16.172.in-addr.arpa" IN {
                        type master;
                        file "0.0.16.172.db";
                        allow-update { none; };
                };
        };

**2、配置zone文件:** 
- [root@nfs-node2 ~]# vim /var/named/host.com.lan
        
      $TTL 86400
      @   IN  SOA     nfs-node2.host.com. root.host.com. (
                2017080201  ;Serial
                3600        ;Refresh
                1800        ;Retry
                604800      ;Expire
                86400       ;Minimum TTL
      )
        
                IN  NS      nfs-node2.host.com.
                IN  A       10.0.0.30
                IN  MX 10   nfs-node2.host.com.
      nfs-node2     IN  A       10.0.0.30
        
- [root@nfs-node2 ~]# vim /var/named/host.com.wan

      $TTL 86400
      @   IN   SOA     nfs-node2.host.com. root.host.com. (
                2017080201   ;Serial
                3600         ;Refresh
                1800         ;Retry
                604800       ;Expire
                86400        ;Minimum TTL
      )
                IN NS nfs-node2.host.com.
                IN A 172.16.0.30
                IN MX 10 nfs-node2.host.com.
      nfs-node2 IN A 172.16.0.30
        
- [root@nfs-node2 ~]# vim /var/named/0.0.10.db       


    $TTL 86400
    @   IN  SOA     nfs-node2.host.com. root.host.com. (
                2017080201  ;Serial
                3600        ;Refresh
                1800        ;Retry
                604800      ;Expire
                86400       ;Minimum TTL
    )
        
                IN  NS      nfs-node2.host.com.
   
                IN  PTR     srv.world.
                IN  A       255.255.255.0
    30          IN  PTR     nfs-node2.host.com.

        
- [root@nfs-node2 ~]# vim /var/named/0.0.16.172.db 

      $TTL 86400
      @   IN  SOA    nfs-node2.host.com. root.host.com. (
                2017080201   ;Serial
                3600         ;Refresh
                1800         ;Retry
                604800       ;Expire
                86400        ;Minimum TTL
      )
                IN NS nfs-node2.host.com.
                IN PTR srv.world.
                IN A 255.255.255.0
      30        IN PTR nfs-node2.host.com.

###四、检查、启动服务、开机自启动
- 检查配置文件 `[root@nfs-node2 ~]# named-checkconf `
- 检查zone文件 

        [root@nfs-node2 ~]# named-checkzone host.com /var/named/host.com.lan 
        zone host.com/IN: loaded serial 2014080201
        OK
- 启动服务   

        [root@nfs-node2 ~]# /etc/init.d/named start
        启动 named：                                               [确定]

- 开机自启动 `[root@nfs-node2 ~]# chkconfig named on`

###五、客户端测试结果



        [root@nfs-node2 ~]# host nfs-node2.host.com localhost
        Using domain server:
        Name: localhost
        Address: 127.0.0.1#53
        Aliases: 
        
        nfs-node2.host.com has address 10.0.0.30
        [root@nfs-node2 ~]# host 10.0.0.30 localhost
        Using domain server:
        Name: localhost
        Address: 127.0.0.1#53
        Aliases: 
        
        30.0.0.10.in-addr.arpa domain name pointer nfs-node2.host.com.
        [root@nfs-node2 ~]# host 172.16.0.30 localhost
        Using domain server:
        Name: localhost
        Address: 127.0.0.1#53
        Aliases: 
        
        30.0.16.172.in-addr.arpa. domain name pointer nfs-node2.host.com.


        