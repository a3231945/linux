###一、linux 配置ntp

**1、安装ntp客户端**
    
     [root@nfs-node2 ~]# yum -y install ntpdate
     
**2、同步时间**

     [root@nfs-node2 ~]# /usr/sbin/ntpdate  nfs-node2
     30 May 11:07:22 ntpdate[11925]: step time server nfs-node2 offset 1.036259 sec
     
###二、Windows配置ntp  
**1、配置ntp：**开始->控制面板 -> 日期和时间   
 
  ![](/assets/1.png)