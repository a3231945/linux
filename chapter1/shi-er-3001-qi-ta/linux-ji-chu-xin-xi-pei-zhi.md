## linux 基础配置

### 一、IP配置 ###
    略

### 二、DNS配置 ###
**1、配置在网卡文件**

    vim /etc/sysconfig/network-scripts/ifcfg-ethX
    
    DNS1=XXX
    DNS2=XXX
**2、配置在公共配置**

    vim /etc/resolv.conf

    nameserver 223.5.5.5
    nameserver 223.6.6.6

### 三、配置主机名 ###
**1、临时设置：** `hostname  xxx`


**2、永久生效**

    centos6：
        vim /etc/sysconfig/network       
        NETWORKING=yes
        HOSTNAME=xxx
    
    centos7:
        vim /etc/hostname
        xxx

### 三、motd欢迎页面 ###
**1、编辑配置  **  

    vim /etc/motd
    hello world！

**2、输出带颜色**

    echo -e "\033[34m hello world！ \033[0m" >/etc/motd

    

### 四、时间同步 ###
**1、临时同步：**`/usr/sbin/ntpdate -s ntp5.aliyun.com`

**2、设置任务计划同步**
- 配置任务计划


    crontab -e 
    */10 * * * * (/usr/sbin/ntpdate -s ntp5.aliyun.com && /sbin/hwclock -w) > /dev/null &

- 重启任务计划


    centos65:    
        /etc/init.d/crond restart
    centos75:
        systemctl restart crond

### 五、时区 ###
**1、编辑配置**

    # vim /etc/sysconfig/clock 
    ZONE="Asia/Shanghai"

**2、生效配置**
    
    source /etc/sysconfig/clock 
    
**3、其他方法**
    # cp -p /usr/share/zoneinfo/Asia/Shanghai /etc/localtime 
    cp: overwrite `/etc/localtime'? y
    

### 六、键盘 ###
**1、编辑配置**

    # vim /etc/sysconfig/keyboard 
    KEYTABLE="us"
    MODEL="pc105+inet"
    LAYOUT="us"
    KEYBOARDTYPE="pc"

**2、生效配置**

    source /etc/sysconfig/keyboard 

### 七、语言 ###

    # vim /etc/sysconfig/i18n 
    LANG="en_US.UTF-8"
    SYSFONT="latarcyrheb-sun16"
    
    source /etc/sysconfig/i18n

    echo $LANG

### 八、ssh免密钥 ###
**1、生成密钥 **   

    #被信任机器上执行
    ssh-keygen -t rsa
    一路回车

**2、互信**

    ssh-copy-id  root@IP


### 九、PS1环境格式 ###

    vim /etc/bashrc

    #最后一行添加
    export PS1='\[\e[37;0m\][\[\e[32;36m\]\u\[\e[37;32m\]@\h \[\e[36;33m\]\w\[\e[0m\]]\n\$ '
