###一、openssl报错
![](/assets/2.png)

**处理方法：** `yum  -y openssl-devel openssl` #配置主机名

**客户端清理 key **
    
    cd /var/lib/pupet/ssl/  && rm -rf *
    /etc/init.d/puppet

