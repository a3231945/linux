###一、安装vsftpd
     [root@www ~]# yum -y install vsftpd
     
###二、配置vsftpd
     [root@www ~]# vi /etc/vsftpd/vsftpd.conf
     
     #关闭匿名用户
     anonymous_enable=NO
     
     #允许 ascii 模式
     ascii_upload_enable=YES
     ascii_download_enable=YES
     
     #不允许切换目录
     chroot_local_user=YES
     chroot_list_enable=YES
     
     #chroot_list 配置用户可以切换目录
     chroot_list_file=/etc/vsftpd/chroot_list
     
     # 允许执行 ls -R
     ls_recurse_enable=YES
     
     # 用户家目录
     local_root=public_html
     
     #使用本地时间
     use_localtime=YES
     
     [root@www ~]# vi /etc/vsftpd/chroot_list
     # add users who are not applied with chroot
     cent
     
###三、启动服务、开机自启动
     [root@www ~]# /etc/rc.d/init.d/vsftpd start
     Starting vsftpd for vsftpd:
     [  OK  ]
     [root@www ~]# chkconfig vsftpd on 


###四、定义客户端端口、防火墙允许访问vsftpd

**配置被动模式**

     [root@www ~]# vi /etc/vsftpd/vsftpd.conf
     #开启被动模式，指定端口范围   
     pasv_enable=YES
     pasv_min_port=21000
     pasv_max_port=21010
         
     [root@www ~]# /etc/rc.d/init.d/vsftpd restart

**防火墙配置**
     
     [root@www ~]# iptables -I INPUT  -p tcp -m state --state NEW -m tcp --dport 21 -j ACCEPT    
     [root@www ~]# iptables -I INPUT  -p tcp -m state --state NEW -m tcp --dport 21000:21010 -j ACCEPT 