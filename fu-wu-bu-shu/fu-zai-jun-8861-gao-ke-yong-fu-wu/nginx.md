Nginx proxy 是 Nginx 的王牌功能,利用 proxy 基本可以实现一个完整的 7 层负载均。

- 功能强大,性能卓越,运行稳定。
- 配置简单灵活。
- 能够自动剔除工作不正常的后端服务器。
- 上传文件使用异步模式。
- 支持多种分配策略,可以分配权重,分配方式灵活。



                                        ++++++++++++
                                        +  Client + 192.168.122.1/24 (真实机做客户端)
                                        ++++++++++++
                                                |
                                                |
                                        ++++++++++++ 192.168.122.254/24
                                        +  Nginx   +
                                        ++++++++++++
                                                |
                        ________________________|_______________________
        _______________|__________                         ___________|________
        |                         |                       |	                  |
        ++++++++++++         ++++++++++++	        ++++++++++++	        ++++++++++++
        + HTML A  +           + HTML B +	         + PHP A +                + PHP B +
        ++++++++++++         ++++++++++++             ++++++++++++           ++++++++++++
        eth0 192.168.122.10/24 eth0 192.168.122.20/24 eth0 192.168.122.30/24 eth0 192.168.122.40/24


HTML A & HTML B

        [root@localhost ~]# yum install httpd
        分别创建测试页面 index.html ,开启服务

PHP A & php B

        [root@localhost ~]# yum install httpd
        分别创建测试页面 index.php ,开启服务


安装配置Nginx

        [root@localhost ~]# rpm -ivh nginx-0.6.36-1.el5.i386.rpm
        [root@localhost ~]# vim /etc/nginx/nginx.conf
        location / {
                root /usr/share/nginx/html;
                index index.html index.htm;
                if ($request_uri ~* \.html$) {
                        proxy_pass http://htmlserver;
                }
                if ($request_uri ~* \.php$) {
                        proxy_pass http://phpserver;
                }
        }
        
        [root@localhost ~]# vim /etc/nginx/conf.d/test.conf
        upstream htmlserver {
                server 192.168.122.10;
                server 192.168.122.20;
        }
        upstream phpserver {
                server 192.168.122.30;
                server 192.168.122.40;
        }
        
        [root@localhost ~]# service nginx start


在客户端访问 Nginx 测试

        [root@localhost ~]# elinks –dump http:// 192.168.122.254
        [root@localhost ~]# elinks –dump http:// 192.168.122.254/index.html
        [root@localhost ~]# elinks –dump http:// 192.168.122.254/index.php