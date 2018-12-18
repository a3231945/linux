###一、部署dhcp服务器
**1、安装软件包**

    yum -y install dhcp
**2、配置文件**

    
    
    echo >/etc/dhcp/dhcpd.conf
    vim /etc/dhcp/dhcpd.conf
    -----------------------------
    option domain-name "example.org";
    #option domain-name-servers ns1.example.org, ns2.example.org;
    default-lease-time 600;
    max-lease-time 7200;
    log-facility local7;
    next-server 192.168.1.118;
    filename "pxelinux.0";
    subnet 192.168.1.0 netmask 255.255.255.0 {
    range 192.168.1.50 192.168.1.100;
    option routers 192.168.1.254;
    default-lease-time 600;
    max-lease-time 7200;
    
    }

**3、启动服务、配置开机自启动**

    sed -i 's#user=dhcpd#user=root#g' /etc/init.d/dhcpd
    sed -i 's#group=dhcpd#group=root#g' /etc/init.d/dhcpd    
    /etc/init.d/dhcpd start && chkconfig dhcpd on
###二、部署tftp服务器
**1、安装软件包**

    yum -y install tftp-server syslinux
**2、修改配置文件**

    sed -i 's#yes#no#g' /etc/xinetd.d/tftp
    /etc/init.d/xinetd restart

**3、拷贝xftp 引导系统需要的配置文件**

    cp /usr/share/syslinux/pxelinux.0 /var/lib/tftpboot/
    mkdir /var/lib/tftpboot/pxelinux.cfg/
    #挂载光盘到mnt目录# 
    mount -o loop -t iso9660 /dev/cdrom /mnt
    cp /mnt/isolinux/isolinux.cfg /var/lib/tftpboot/pxelinux.cfg/default
    sed -i 's#img$#img ks=http://192.168.1.118/ks.cfg ksdevice=eth0#'  /var/lib/tftpboot/pxelinux.cfg/default
    sed -i 's#600#60#g' /var/lib/tftpboot/pxelinux.cfg/default
    sed -i '/menu default/d' /var/lib/tftpboot/pxelinux.cfg/default
    sed -i '/label local/a \  menu default' /var/lib/tftpboot/pxelinux.cfg/default
    cp /mnt/isolinux/vesamenu.c32  /var/lib/tftpboot/
    cp /mnt/isolinux/boot.msg /var/lib/tftpboot/
    cp /mnt/isolinux/initrd.img /var/lib/tftpboot/
    cp /mnt/isolinux/vmlinuz /var/lib/tftpboot/
    cp /mnt/isolinux/splash.jpg   /var/lib/tftpboot/
    chmod -R +x /var/lib/tftpboot
    
###三、部署httpd服务
**1、安装软件包**

    yum -y install httpd && chkconfig httpd on && /etc/init.d/httpd start
 
**2、配置文件 **  

    mkdir -p /var/www/html/repo
    #重新挂载光驱到/var/www/html/repo
    cd / && umount /mnt && mount -o loop  -t  iso9660 /dev/cdrom   /var/www/html/repo 
    
    vim /var/www/html/ks.cfg
    ---------------------------------------
    #platform=x86, AMD64, 或 Intel EM64T
    #version=DEVEL
    key --skip
    # Firewall configuration
    firewall --disabled
    # Install OS instead of upgrade
    install
    # Use network installation
    url --url="http://192.168.1.118/repo"
    # Root password
    rootpw --iscrypted $1$g0tQ5JMU$emqWnmOy0b1c5DeF.LUAw0
    # System authorization information
    auth  --useshadow  --passalgo=sha512
    # Use graphical install
    graphical
    # System keyboard
    keyboard us
    # System language
    lang zh_CN
    # SELinux configuration
    selinux --disabled
    # Do not configure the X Window System
    skipx
    # Installation logging level
    logging --level=info
    # Reboot after installation
    reboot
    # System timezone
    timezone  Asia/Shanghai
    # System bootloader configuration
    bootloader --location=mbr
    # Clear the Master Boot Record
    zerombr
    # Partition clearing information
    clearpart --all --initlabel 
    # Disk partitioning information
    part /boot --fstype="ext4" --size=200
    part swap --fstype="swap" --size=4096
    part / --fstype="ext4" --grow --size=1
    
    
    %packages
    @base
    @development
    vim
    lrzsz
    unzip
    sysstat
    %end
   
- centos7

 
    
    #platform=x86, AMD64, 或 Intel EM64T
    #version=DEVEL
    #key --skip
    # Firewall configuration
    firewall --disabled
    # Install OS instead of upgrade
    install
    # Use network installation
    url --url="http://192.168.1.118/repo-7"
    # Root password
    rootpw --iscrypted $1$g0tQ5JMU$emqWnmOy0b1c5DeF.LUAw0
    # System authorization information
    auth  --useshadow  --passalgo=sha512
    # Use graphical install
    text
    # System keyboard
    keyboard us
    # System language
    lang en_US
    # SELinux configuration
    selinux --disabled
    # Do not configure the X Window System
    skipx
    # Installation logging level
    logging --level=info
    # Reboot after installation
    reboot
    # System timezone
    timezone Asia/Shanghai
    # System bootloader configuration
    bootloader --location=mbr --driveorder=sda --append="auto rhgb quiet biosdevname=0 net.ifnames=0"
    
    # Clear the Master Boot Record
    zerombr
    # Partition clearing information
    clearpart --all --initlabel
    # Disk partitioning information
    part /boot --fstype="xfs" --size=512 --ondrive=sda
    part swap --fstype="swap" --size=4096 --ondrive=sda
    part biosboot  --size=1 --ondrive=sda
    part / --fstype="xfs" --grow --size=1 --ondrive=sda
    
    
    %packages
    @base
    @development
    vim
    lrzsz
    unzip
    sysstat
    tree
    telnet
    nc
    iotop
    %end
    
    %post
    curl "http://192.168.1.118/shell/system_init-7.sh" | bash
    %end
     




 
