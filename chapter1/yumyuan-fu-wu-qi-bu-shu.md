# 内部YUM 配置

### 一、cobbler同步yum源

**1、安装cobbler**

```
yum -y install httpd tftp dhcp xinetd cobbler
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

### 二、自定义yum源



