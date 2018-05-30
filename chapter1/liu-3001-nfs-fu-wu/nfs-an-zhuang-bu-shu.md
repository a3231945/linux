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


    

     

