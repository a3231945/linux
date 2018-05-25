###1、安装racadm

    wget -q -O - http://linux.dell.com/repo/hardware/latest/bootstrap.cgi | bash
    yum -y  install srvadmin-all
###2、命令详解

**获取网卡信息：**

    racadm get niccfg  
**获取 内存 风扇 cpu信息：**

    racadm getsensorinfo
**所有联机的磁盘：**

    racadm raid get pdisks
**raid卡信息：**

    racadm raid get controllers -o
**列出所有网卡：**

    racadm nicstatistics
**网卡状态：**
    
    racadm nicstatistics