# Centos7 修改网卡名为eth0
### 1、编辑网卡名
    
    vim /etc/sysconfig/network-scripts/ifcfg-enoxxxx
    NAME=eth0
    
    vim /etc/sysconfig/network-scripts/ifcfg-enoxxxx
    NAME=eth1
    
    cd /etc/sysconfig/network-scripts/
    mv ifcfg-enoxxxx eth0
    mv ifcfg-enoxxxx eth1

### 2、修改grub配置文件

    vim /etc/default/grub
    添加 net.ifnames=0 biosdevname=0 至 GRUBCMDLINELINUX变量中
    
    例如：
        #修改前
        GRUB_CMDLINE_LINUX="auto crashkernel=auto  rhgb quiet"
        #修改后
        GRUB_CMDLINE_LINUX="auto crashkernel=auto biosdevname=0 net.ifnames=0 rhgb quiet"

### 3、重新生成grub配置

    grub2-mkconfig -o /boot/grub2/grub.cfg
    
### 4、重启机器

    reboot
    
>注意:如果没生效 删除 /etc/udev/rules.d/XXX-net.rules 重启机器再试试 
