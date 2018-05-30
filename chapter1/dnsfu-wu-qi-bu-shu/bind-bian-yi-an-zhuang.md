###一、安装依赖环境

        yum -y install gcc gcc-c++  openssl-devel openssl 
###二、编译安装bind

        tar xf bind-9.7.3.tar.gz 
        cd bind-9.7.3
        ./configure --prefix=/usr/local/dns  --enable-threads --enable-largefile --enable-epoll --disable-ipv6 
        make && make install

###三、配置dns

**1、生成rndc.conf**

        /usr/local/dns/sbin/rndc-confgen > rndc.conf
        cat rndc.conf  > rndc.key
        
**2、生成named.conf**

        tail -10 rndc.conf  | head -9 |sed s/#\ //g > named.conf   

        vim named.conf
        
        options {  
                directory "/usr/local/dns/var";  
             	pid-file "named.pid";   
        };
        
        zone "." {  
                type hint;  
                file "named.ca";  
        };
        
        options {  
        directory "/usr/local/bind/var";    //域名文件存放的绝对路径  
        pid-file "named.pid";              //如果bind启动，自动会在/usr/local/bind/var/目录生成一个named.pid文件，打开文件就是named进程的ID  
        };  
        zone "." IN {  
                type hint;                 //根域名服务器  
                file "named.ca";           //存放在/usr/local/bind/var/目录，文件名为named.ca  
        };  
        
**3、生成named.ca**

        dig -t NS .  #能连入互联网
        dig -t NS . >/usr/local/dns/var/named.ca  
**4、启动服务**

        /usr/local/dns/sbin/named  
        /usr/local/dns/sbin/rndc status 

###四、配置正解

        vim  named.conf  
        #添加
        zone "localhost"{  
                type master;  
                file "localhost.zone";  
        };
        
        zone "hosts.com"{  
                type master;  
                file "hosts.com.zone";  
        };

        vim  /usr/local/dns/var/localhost.zone
        
        $TTL  38400  
        @ IN    SOA     localhost.      root (  
                        2016122001      ;serial  
                        1H              ;refresh  
                        15M             ;retry  
                        1W              ;expire  
                        1D )            ;TTL  
          IN    NS      @  
          IN    A       127.0.0.1  

        vim  /usr/local/dns/var/hosts.com.zone
        
        $TTL  38400  
        @ IN    SOA     dns.host.com.  root (  
                        2016122001      ;serial  
                        1H              ;refresh  
                        15M             ;retry  
                        1W              ;expire  
                        1D )            ;TTL  
        @           IN    NS      bind  
        @           IN    MX 10   mail  
        dns         IN    A       10.0.0.30 
 


###五、配置反解
     
    vim named.conf
    #添加
    zone "0.0.10.in-addr.arpa" IN {	//设置反向DNS区域名称
    	type master;
    	file "10.0.0.zone";		//设置对应的反向区域地址数据库文件
    };

    vim  /usr/local/dns/var/10.0.0.zone
    
    $TTL  38400  
    @ IN    SOA     dns.hosts.com.  root (  
                        2018052301      ;serial  
                        1H              ;refresh  
                        15M             ;retry  
                        1W              ;expire  
                        1D )            ;TTL  
    @           IN    NS      bind.hosts.com.  
    30         	IN	PTR	dns.hosts.com

###六、配置主从
**master：**

        key "rndc-key" {
	algorithm hmac-md5;
	secret "xnPhInS54jJjZVu8usMyUQ==";
        };
        
        controls {
        	inet 127.0.0.1 port 953
        	allow { 127.0.0.1; } keys { "rndc-key"; };
        };
        
        options {  
        	listen-on port 53 { 127.0.0.1; 10.32.101.11; };
        	directory "/usr/local/dns/var";    //域名文件存放的绝对路径  
        	pid-file "named.pid";              //如果bind启动，自动会在/usr/local/dns/var/目录生成一个named.pid文件，打开文件就是named进程的ID  
        	allow-query { 10.0.0.0/8; };
        	forwarders { 223.5.5.5; 223.6.6.6; };
        	check-names     master warn;
        };
        
            //指定服务器日志记录的内容和日志信息来源 
        logging {  
            channel "default_syslog" {
                syslog daemon; // 发送给syslog 的daemon facility
                severity info; // 只发送此优先级和更高优先级的信息 
            };  
            channel default_debug {
                file "/usr/local/dns/var/run/named.run"; // 写入工作目录下的named.run 文件。注意：如果服务器用-f 参数启动，则"named.run"会被stderr 所替换。
                severity dynamic; //  按照服务器当前的debug 级别记录日志 
                };  
            channel xfer_in_log {
                file "/var/log/named/xfer_in_log" versions 100 size 10m;
                severity info;
                print-category yes;
                print-severity yes;
                print-time yes;
            };  
  
            channel xfer_out_log {
                file "/var/log/named/xfer_out_log" versions 100 size 10m;
                severity info;
                print-category yes;
                print-severity yes;
                print-time yes;
            };  
          
            channel notify_log {
                file "/var/log/named/notify_log" versions 100 size 10m;
                severity info;
                print-category yes;
                print-severity yes;
                print-time yes;
            };  
          
            channel general_log {
                file "/var/log/named/general_log" versions 400 size 100m;
                severity info;
                print-category yes;
                print-severity yes;
                print-time yes;
            };  
          
            channel default_log {
                file "/var/log/named/default_log" versions 400 size 100m;
                severity info;
                print-category yes;
                print-severity yes;
                print-time yes;
            };  
  
            channel update_log {
                file "/var/log/named/update_log" versions 100 size 10m;
                severity info;
                print-category yes;
                print-severity yes;
                print-time yes;
            };  
          
            channel query_log {
                file "/var/log/named/query_log" versions 1024 size 1m;
                severity info;
                print-category no;
                print-severity no;
                print-time yes;
            };  
            category queries { query_log; };  
            category default { default_log; };  
            category general { general_log; };  
            category xfer-in { xfer_in_log; };  
            category xfer-out { xfer_out_log; };  
            category notify { notify_log; };  
            category update { update_log; };  
        };
        zone "." IN {  
                type hint;                 //根域名服务器  
                file "named.ca";           //存放在/usr/local/dns/var/目录，文件名为named.ca  
        };
        
        zone "localhost"{  
                type master;  
                file "localhost.zone";  
        	allow-update { none; }; 
        };
        
        zone "hosts.com" {  
                type master;  
                file "hosts.com.zone";  
        	allow-transfer { 10.32.101.12; };
        };




        //反向解析
        zone "1.32.10.in-addr.arpa" IN {	//设置反向DNS区域名称
            	type master;
            	file "10.32.1.zone";		//设置对应的反向区域地址数据库文件
        	allow-transfer { 10.32.101.12; };
        };


**slave：**

    key "rndc-key" {
    	algorithm hmac-md5;
    	secret "xnPhInS54jJjZVu8usMyUQ==";
    };
    
    controls {
    	inet 127.0.0.1 port 953
    	allow { 127.0.0.1; } keys { "rndc-key"; };
    };
    
    options {  
    	listen-on port 53 { 127.0.0.1; 10.32.101.12; };
    	directory "/usr/local/dns/var";    //域名文件存放的绝对路径  
    	pid-file "named.pid";              //如果bind启动，自动会在/usr/local/dns/var/目录生成一个named.pid文件，打开文件就是named进程的ID  
    	allow-query { 10.0.0.0/8; };
    	forwarders { 223.5.5.5; 223.6.6.6; };
    	check-names     slave warn;
    };

    //指定服务器日志记录的内容和日志信息来源 
    logging {  
        channel "default_syslog" {
            syslog daemon; // 发送给syslog 的daemon facility
            severity info; // 只发送此优先级和更高优先级的信息 
        };  
        channel default_debug {
            file "/usr/local/dns/var/run/named.run"; // 写入工作目录下的named.run 文
            severity dynamic; //  按照服务器当前的debug 级别记录日志 
            };  
        channel xfer_in_log {
            file "/var/log/named/xfer_in_log" versions 100 size 10m;
            severity info;
            print-category yes;
            print-severity yes;
            print-time yes;
        };  
      
        channel xfer_out_log {
            file "/var/log/named/xfer_out_log" versions 100 size 10m;
            severity info;
            print-category yes;
            print-severity yes;
            print-time yes;
        };  
      
        channel notify_log {
            file "/var/log/named/notify_log" versions 100 size 10m;
            severity info;
            print-category yes;
            print-severity yes;
            print-time yes;
        };  
      
        channel general_log {
            file "/var/log/named/general_log" versions 400 size 100m;
            severity info;
            print-category yes;
            print-severity yes;
            print-time yes;
        };  
  
        channel default_log {
            file "/var/log/named/default_log" versions 400 size 100m;
            severity info;
            print-category yes;
            print-severity yes;
            print-time yes;
        };  
      
        channel update_log {
            file "/var/log/named/update_log" versions 100 size 10m;
            severity info;
            print-category yes;
            print-severity yes;
            print-time yes;
        };  
      
        channel query_log {
            file "/var/log/named/query_log" versions 1024 size 1m;
            severity info;
            print-category no;
            print-severity no;
            print-time yes;
        };  
        category queries { query_log; };  
        category default { default_log; };  
        category general { general_log; };  
        category xfer-in { xfer_in_log; };  
        category xfer-out { xfer_out_log; };  
        category notify { notify_log; };  
        category update { update_log; };  
    };
    
    zone "." IN {  
            type hint;                 //根域名服务器  
            file "named.ca";           //存放在/usr/local/dns/var/目录，文件名为named.ca  
    };
    
    zone "localhost"{  
            type slave;  
    	masters { 10.32.101.11; };
            file "localhost.zone";  
    };
    
    zone "hosts.com"{  
            type slave;  
    	masters { 10.32.101.11; };
            file "hosts.com.zone";  
    };




    //反向解析
    zone "1.32.10.in-addr.arpa" IN {	//设置反向DNS区域名称
        	type slave;
    	        masters { 10.32.101.11; };
        	file "10.32.1.zone";		//设置对应的反向区域地址数据库文件
    };
       
  
   


            

        