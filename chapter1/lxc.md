
#LXC 学习
###一、安装LXC
**1、配置epel源**

    yum -y install  epel-release
**2、安装依赖包**
    
    yum install debootstrap perl libvirt


**3、安装lxc** 

    yum -y install lxc lxc-extra lxc-templates 
    
    lxc                  容器程序
    lxc-extra            容器工具
    lxc-templates        容器模板
    
**4、启动服务**
    
    systemctl start     lxc
    systemctl start     libvirtd
    systemctl enable    lxc
    systemctl enable    libvirtd    

** 5、检查配置**
 
    lxc-checkconfig
     
###二、容器管理
**1、创建/删除容器**
    
    #创建
    lxc-create -t centos  -n demo1
    
    -t    指定模板
    -n    指定名称
    
    #删除
    lxc-destroy -n demo1

    
**2、查看容器**

    #查看有哪些容器
    lxc-ls  
    
    #查看启动的容器
    lxc-ls --active
    
    #查看容器（指定格式）
    lxc-ls  -f
    
    #查看容器的信息
    lxc-info -n demo1
    lxc-info --name demo1
    
    
**3、启动/关闭/开机自启动容器**
    
    #前台启动
    lxc-start  -n demo1
    
    #后台启动        
    lxc-start  -d -n demo1
    
    #关闭
    lxc-stop   -n demo1
    
    #开机自启动
    lxc-autostart -n demo1
    
    #挂起
    lxc-freeze -n demo1
    
    #恢复挂起
    lxc-unfreeze -n demo1

**4、登录容器**

    #console 登录
    lxc-console -n demo1
    
    #容器登录
    lxc-attach -n demo1
    
    #ssh登录
        1>查看IP
        lxc-ls -f
        
        2>查看密码
        cat  /var/lib/lxc/demo1/tmp_root_pass  
        
        3>登录
        ssh root@xxx
    
**5、克隆/快照管理**

    #克隆
    lxc-clone demo1 demo2
    
    #创建快照
    lxc-snapshot -n demo1 -c test
    
    -c 快照说明信息 
    
    #删除快照
    lxc-snapshot -n demo1 -d SNAPSHOT
     
    #查看快照
    lxc-snapshot -n demo1 -L
    lxc-snapshot -n demo1 -L -C
    
    
    #恢复指定快照
    lxc-snapshot -n demo1 -r SNAPSHOT
    
**6、网络管理**

    #桥接
    
    #nat

**7、资源管理**

    #网卡
    
    #磁盘            
    

**8、监控**

    #查看指定容器
    lxc-monitor -n demo2
    
    #查看所有
    lxc-top 
    
    #指定排序
    lxc-top -s n
        n    名称
        c    cpu使用率   
        d    磁盘使用率
        m    内存使用率
        k    kernel使用率
        
    #反向排序
    lxc-top  -r    

**9、其他**
    
    #执行容器命令（关机状态）
    lxc-execute  -n demo1  -f /usr/share/lxc/config/centos.common.conf cat /etc/passwd    

    #限制内存
    1>命令行配置
        设置
        lxc-cgroup -n demo1 memory.limit_in_bytes 512M
        查看
        cat /sys/fs/cgroup/memory/lxc/demo1/memory.limit_in_bytes 
    2>配置文件
        vim /var/lib/lxc/demo1/config
        lxc.cgroup.memory.limit_in_bytes = 1G
        
    #限制cpu
    lxc-cgroup -n demo1 cpuset.cpus 0-1
    lxc-cgroup -n demo1 cpu.shares 512

###三、模板制作

    