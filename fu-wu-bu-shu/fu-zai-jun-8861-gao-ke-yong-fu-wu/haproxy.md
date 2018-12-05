
                               	 ++++++++++++   
             				+ Client +   192.168.122.1/24 (真实机做客户端)
                               	++++++++++++   
                                       	|
                                       	|
                               	++++++++++++     192.168.122.254/24
                              		+ HAproxy +
                              		++++++++++++   
                                            |
                    ________________________|_______________________
         __________|____________                    __________|________        	
        |                      |                    |		  		|
	++++++++++++        ++++++++++++	 ++++++++++++	       ++++++++++++		
	+ HTML   A  +      + HTML  B   +       + PHP   A  +          +  PHP B  +
	++++++++++++        ++++++++++++        ++++++++++++         ++++++++++++
	eth0 192.168.122.10/24     eth0 192.168.122.20/24    eth0 192.168.122.30/24        eth0 192.168.122.40/24



HTML  A & HTML  B

	[root@localhost ~]# yum install httpd
	分别创建测试页面 index.html ,开启服务

PHP  A & php  B

	[root@localhost ~]# yum install httpd php
	分别创建测试页面 index.php ,开启服务



安装HAproxy

	[root@localhost ~]# tar xf haproxy-1.4.20.tar.gz
	[root@localhost ~]# cd haproxy-1.4.20
	[root@localhost haproxy-1.4.20]# make TARGET=linux26 PREFIX=/usr/local/haproxy install


生成HAproxy配置文件

	[root@localhost haproxy-1.4.20]# cd /usr/local/haproxy/
	[root@localhost haproxy]# mkdir conf logs
	[root@localhost haproxy]# cd conf/
	[root@localhost conf]# vim haproxy.cfg
	===================================================================
	global
		log 127.0.0.1 local3 info		#日志服务器
		maxconn 4096				#单个进程的最大并发连接数
		uid nobody				#用户身份
		gid nobody				#组身份
		daemon					#守护进程方式后台运行
		nbproc 1				#工作进程数量
		
		
	defaults
		log		global
		mode	http		#工作模式 http ,tcp 是 4 层,http是 7 层	
		maxconn 2048		#最大连接数
		retries 	3	#3 次连接失败就认为服务器不可用
		option	redispatch	#如果 cookie 写入了 serverId 而客户端不会刷新 cookie,当serverId 对应的服务器挂掉后,强制定向到其他健康的服务器
		stats	uri  /haproxy	#使用浏览器访问 http://192.168.122.254/haproxy,可以看到服务器状态
		contimeout	5000
		clitimeout	50000
		srvtimeout	50000
		
		
	frontend http-in
		bind 0.0.0.0:80
		mode http
		log global
		option httplog
		option httpclose		#打开支持主动关闭功能		
	     acl php url_reg  -i  \.php$	#acl <ACL名字>  <类型>  <大小写>  <规则>
	     acl html url_reg  -i  \.html$	#use_backend  <服务器组>  if  <ACL名字>
	     use_backend php-server if  php
	     use_backend html-server if  html
	     default_backend html-server	#默认使用的服务器组
	

	backend php-server
		mode http
		balance roundrobin			        #负载均衡的方式
		option httpchk GET /index.php		        #健康检查
		cookie SERVERID insert indirect nocache	 #客户端的 cookie 信息
		server php-A 192.168.122.30:80 weight 1 cookie 1 check inter 2000 rise 2 fall 5
		server php-B 192.168.122.40:80 weight 1 cookie 2 check inter 2000 rise 2 fall 5
		#cookie 1 标识 serverid 为 1
		#check inter 2000 检测心跳频率
		#rise 2 2 次正确认为服务器可用
		#fall 5 5 次失败认为服务器不可用
	
	backend html-server
		mode http
		balance roundrobin
		option httpchk GET /index.html
		cookie SERVERID insert indirect nocache
		server html-A 192.168.122.10:80 weight 1 cookie 3 check inter 2000 rise 2 fall 5
		server html-B 192.168.122.20:80 weight 1 cookie 4 check inter 2000 rise 2 fall 5		

		
启动HAproxy

	[root@localhost conf]# /usr/local/haproxy/sbin/haproxy -f /usr/local/haproxy/conf/haproxy.cfg

查看HAproxy状态

	[root@localhost conf]# firefox http://localhost/haproxy

在客户端访问 HAproxy 测试

	[root@localhost ~]# elinks –dump http:// 192.168.122.254
	[root@localhost ~]# elinks –dump http:// 192.168.122.254/index.html
	[root@localhost ~]# elinks –dump http:// 192.168.122.254/index.php
