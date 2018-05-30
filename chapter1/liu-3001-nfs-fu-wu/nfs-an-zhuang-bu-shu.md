###一、NFS安装

    [root@nfs-node2 ~]# yum -y install nfs-utils
###二、配置NFS

**1、配置挂载用户及域名**
        
    [root@nfs-node2 ~]# vim /etc/idmapd.conf 

     #如果主机名有域名解析，可以配置域名所使用根域名（二级）   
     5 Domain = local.domain.edu
         
     #配置挂载后的用户（配置文件也可以配置）
     41 [Mapping]
     42 
     43 Nobody-User = nobody
     44 Nobody-Group = nobody
     
     
**2、配置NFS主配置文件**

    [root@nfs-node2 ~]# vim /etc/exports 
    /mnt/disk       *(rw,no_root_squash,sync,hide,fsid=0)
 
**3、配置NFS、RPC端口**

    [root@nfs-node2 ~]# vim /etc/sysconfig/nfs 

    
    20 LOCKD_TCPPORT=32803
    
    22 LOCKD_UDPPORT=32769
    
    57 MOUNTD_PORT=892
    
    63 STATD_PORT=662

       
**4、启动服务，配置开机自启动**

    [root@nfs-node2 ~]# /etc/init.d/rpcbind start
    正在启动 rpcbind：                                         [确定]

    [root@nfs-node2 ~]# /etc/init.d/nfs start
    启动 NFS 服务：                                            [确定]
    关掉 NFS 配额：                                            [确定]
    启动 NFS mountd：                                          [确定]
    启动 NFS 守护进程：                                        [确定]
    正在启动 RPC idmapd：                                      [确定]
        
        
    [root@nfs-node2 ~]# chkconfig rpcbind on
    [root@nfs-node2 ~]# chkconfig nfs on

**5、配置防火墙允许访问NFS,RPC端口**

    [root@nfs-node2 ~]# for port in 111 662 892 2049 32803; do iptables -I INPUT -p tcp -m state --state NEW -m tcp --dport $port -j ACCEPT; done 
                    
**6、客户端测试挂载**

    [root@nfs-node2 ~]# showmount -e localhost  
    Export list for localhost:
    /mnt/disk *
    
    [root@nfs-node2 ~]# mount -t nfs localhost:/mnt/disk/ /tmp/test 

    [root@nfs-node2 ~]# df -h
    Filesystem            Size  Used Avail Use% Mounted on
    /dev/sda3              20G   11G  7.7G  59% /
    tmpfs                 1.9G     0  1.9G   0% /dev/shm
    /dev/sda1             190M   96M   84M  54% /boot
    localhost:/mnt/disk/   20G   11G  7.7G  59% /tmp/test

三、NFS挂载参数详解
    
    ro                      只读访问 
    rw                      读写访问 
    sync                    所有数据在请求时写入共享 
    async                   NFS在写入数据前可以相应请求 
    secure                  NFS通过1024以下的安全TCP/IP端口发送 
    insecure                NFS通过1024以上的端口发送 
    wdelay                  如果多个用户要写入NFS目录，则归组写入（默认） 
    no_wdelay               如果多个用户要写入NFS目录，则立即写入，当使用async时，无需此设置。 
    hide                    在NFS共享目录中不共享其子目录 
    no_hide                 共享NFS目录的子目录 
    subtree_check           如果共享/usr/bin之类的子目录时，强制NFS检查父目录的权限（默认） 
    no_subtree_check        和上面相对，不检查父目录权限 
    all_squash              共享文件的UID和GID映射匿名用户anonymous，适合公用目录。 
    no_all_squash           保留共享文件的UID和GID（默认） 
    root_squash             root用户的所有请求映射成如anonymous用户一样的权限（默认） 
    no_root_squas           root用户具有根目录的完全管理访问权限 
    anonuid=xxx             指定NFS服务器/etc/passwd文件中匿名用户的UID 
    anongid=xxx             指定NFS服务器/etc/passwd文件中匿名用户的GID  

     
四、NFS V4

    略
    
