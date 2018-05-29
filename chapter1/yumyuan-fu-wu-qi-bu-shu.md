# 内部YUM 配置

### 一、cobbler同步yum源

**1、安装cobbler**

```
yum -y install  cobbler
```

**2、配置同步yum源配置**

```
#配置centos源（中科大）
cobbler repo add --name=centos6.5-x86_64-base --mirror=rsync://mirrors.ustc.edu.cn/centos/6/os/x86_64/ --arch=x86_64 --breed=rsync
cobbler repo add --name=centos6.5-x86_64-updates --mirror=rsync://mirrors.ustc.edu.cn/centos/6/updates/x86_64/ --arch=x86_64 --breed=rsync
cobbler repo add --name=centos6.5-x86_64-extras --mirror=rsync://mirrors.ustc.edu.cn/centos/6/extras/x86_64/ --arch=x86_64 --breed=rsync
cobbler repo add --name=centos6.5-x86_64-centosplus --mirror=rsync://mirrors.ustc.edu.cn/centos/6/centosplus/x86_64/ --arch=x86_64 --breed=rsync
cobbler repo add --name=centos6.5-x86_64-contrib --mirror=rsync://mirrors.ustc.edu.cn/centos/6/contrib/x86_64/ --arch=x86_64 --breed=rsync
cobbler repo add --name=centos6.5-x86_64-fasttrack --mirror=rsync://mirrors.ustc.edu.cn/centos/6/fasttrack/x86_64/ --arch=x86_64 --breed=rsync

#配置epel源
cobbler repo add --name=epel-6 --mirror=rsync://mirrors.ustc.edu.cn/epel/6/x86_64/ --arch=x86_64 --breed=rsync

#配置其他源（pipy npm gem等）
```

**3、定时同步**

```
crontab -e 
"1 3 * * * /usr/bin/cobbler reposync --tries=3 --no-fail"
```

**4、配置web服务**

    #安装nginx 服务
    yum -y install nginx
    
    #配置nginx服务
    server {
        listen       80;
        server_name  yum.hosts.com; 
        root /var/www/cobbler/repo_mirror/;
    
        client_body_buffer_size 5m;
    
        fastcgi_buffers 16 256k;
        fastcgi_busy_buffers_size 256k;
        fastcgi_temp_file_write_size 256k;
        fastcgi_intercept_errors on;

        #配置安全策略
        allow 10.0.0.0/8;
        deny  all;

    location / {
        #开启列表
	autoindex 		on;
	#显示本地时间
	autoindex_localtime 	on; 
	root /var/www/cobbler/repo_mirror/;
        }
    }

**5、客户端配置**

    ####vim /etc/yum.repos.d/yum.repo
    
    #centos6.5-x86_64-base 
    [base]
    name=centos6.5-x86_64-base
    baseurl=http://yum.hosts.com/centos6.5-x86_64-base/
    gpgcheck=0
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
    
    #centos6.5-x86_64-updates
    [updates]
    name=centos6.5-x86_64-updates
    baseurl=http://yum.hosts.com/centos6.5-x86_64-updates/
    gpgcheck=0
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
    
    #centos6.5-x86_64-extras
    [extras]
    name=centos6.5-x86_64-extras
    baseurl=http://yum.hosts.com/centos6.5-x86_64-extras/
    gpgcheck=0
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
    
    #centos6.5-x86_64-centosplus
    [centosplus]
    name=centos6.5-x86_64-centosplus
    baseurl=http://yum.hosts.com/centos6.5-x86_64-centosplus/
    gpgcheck=0
    enabled=0
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
    
    #centos6.5-x86_64-contrib
    [contrib]
    name=centos6.5-x86_64-contrib
    baseurl=http://yum.hosts.com/centos6.5-x86_64-contrib/
    gpgcheck=0
    enabled=0
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
    
    #epel-6
    [epel]
    name=epel-6
    baseurl=http://yum.hosts.com/epel-6/
    failovermethod=priority
    enabled=1
    gpgcheck=0
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-6
    
    #epel-6-debug
    [epel-debuginfo]
    name=epel-6-debug
    baseurl=http://yum.hosts.com/epel-6/debug
    failovermethod=priority
    enabled=0
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-6
    gpgcheck=0
    
    #epel-6-source
    #[epel-source]
    #name=Extra Packages for Enterprise Linux 6 - $basearch - Source
    #baseurl=http://download.fedoraproject.org/pub/epel/6/SRPMS
    #failovermethod=priority
    #enabled=0
    #gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-6
    #gpgcheck=0
    



### 二、自定义yum源



