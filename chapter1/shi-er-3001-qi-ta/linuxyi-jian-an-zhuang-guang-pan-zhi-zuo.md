# linux系统光盘一键安装 #

### 一、安装系统依赖包 ###

    yum -y install anaconda repodata createrepo mkisofs rsync

### 二、制作centos6镜像 ###
**1、下载原始官方镜像**
    
    略

**2、挂载光盘**

    mount -o loop CentOS-6.8-x86_64-bin-DVD1.iso  /mnt

    注意：可以更换挂载目录

**3、拷贝文件**

    cp -r  /mnt /centos6.8-iso

    注意：
        1、可以更换目录
        2、可以裁剪rpm文件，这里不做处理。
        制作方法：删除rpm，重新生成repo源相关配置文件


**4、编辑安装目录**

    vim /centos6.8-iso/isolinux/isolinux.cfg

    default vesamenu.c32
    #prompt 1
    timeout 50
    
    display boot.msg
    
    menu background splash.jpg
    menu title Welcome to CentOS 6.8!
    menu color border 0 #ffffffff #00000000
    menu color sel 7 #ffffffff #ff000000
    menu color title 0 #ffffffff #00000000
    menu color tabmsg 0 #ffffffff #00000000
    menu color unsel 0 #ffffffff #00000000
    menu color hotsel 0 #ff000000 #ffffffff
    menu color hotkey 7 #ffffffff #ff000000
    menu color scrollbar 0 #ffffffff #00000000
    
    label linux
      menu label ^Install or upgrade an existing system
      kernel vmlinuz
      append initrd=initrd.img
    
    label linuxnew
      menu label install centos6.8
      menu default
      kernel vmlinuz
      append ks=cdrom:/ks.cfg initrd=initrd.img 
    label vesa
      menu label Install system with ^basic video driver
      kernel vmlinuz
      append initrd=initrd.img nomodeset
    label rescue
      menu label ^Rescue installed system
      kernel vmlinuz
      append initrd=initrd.img rescue
    label local
      menu label Boot from ^local drive
      localboot 0xffff
    label memtest86
      menu label ^Memory test
      kernel memtest
      append -

**5、编辑安装ks文件**

    vim /centos6.8-iso/ks.cfg

    #platform=x86, AMD64, 或 Intel EM64T
    #version=DEVEL
    key --skip
    # Firewall configuration
    firewall --disabled
    # Install OS instead of upgrade
    install
    # Use network installation
    #url --url="http://192.168.1.100/repo"
    cdrom
    # Root password
    rootpw !!)a1104
    # System authorization information
    auth  --useshadow  --passalgo=sha512
    # Use graphical install
    graphical
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
    timezone  Asia/Shanghai
    # System bootloader configuration
    bootloader --location=mbr --driveorder=sda --append="auto rhgb quiet biosdevname=0"
    #bootloader --location=mbr --driveorder=sda --append="auto rhgb quiet"
    # Clear the Master Boot Record
    zerombr
    # Partition clearing information
    clearpart --all --initlabel 
    # Disk partitioning information
    part /boot --fstype="ext4" --size=512 --ondrive=sda
    part swap --fstype="swap" --size=4096 --ondrive=sda
    part / --fstype="ext4" --grow --size=1 --ondrive=sda
    
    
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
    #curl "http://192.168.1.100/shell/system_init.sh" | bash
    %end

**6、生成镜像文件**

    mkisofs -o CentOS-6.8.iso -b isolinux/isolinux.bin  -c isolinux/boot.cat -no-emul-boot -boot-load-size 4 -boot-info-table -R -J -v -T /centos6.8-iso/

### 二、制作centos7镜像 ###
**1、下载原始官方镜像**
    
    略

**2、挂载光盘**

    mount -o loop CentOS-7-x86_64-DVD-1804.iso  /mnt

    注意：可以更换挂载目录

**3、拷贝文件**

    cp -r  /mnt /centos7.5-iso

    注意：
        1、可以更换目录
        2、可以裁剪rpm文件，这里不做处理。
        制作方法：删除rpm，重新生成repo源相关配置文件


**4、编辑安装目录**

    vim /centos7.5-iso/isolinux/isolinux.cfg

    default vesamenu.c32
    timeout 50
    
    display boot.msg
    
    # Clear the screen when exiting the menu, instead of leaving the menu displayed.
    # For vesamenu, this means the graphical background is still displayed without
    # the menu itself for as long as the screen remains in graphics mode.
    menu clear
    menu background splash.png
    menu title CentOS Linux 7
    menu vshift 8
    menu rows 18
    menu margin 8
    #menu hidden
    menu helpmsgrow 15
    menu tabmsgrow 13
    
    # Border Area
    menu color border * #00000000 #00000000 none
    
    # Selected item
    menu color sel 0 #ffffffff #00000000 none
    
    # Title bar
    menu color title 0 #ff7ba3d0 #00000000 none
    
    # Press [Tab] message
    menu color tabmsg 0 #ff3a6496 #00000000 none
    
    # Unselected menu item
    menu color unsel 0 #84b8ffff #00000000 none
    
    # Selected hotkey
    menu color hotsel 0 #84b8ffff #00000000 none
    
    # Unselected hotkey
    menu color hotkey 0 #ffffffff #00000000 none
    
    # Help text
    menu color help 0 #ffffffff #00000000 none
    
    # A scrollbar of some type? Not sure.
    menu color scrollbar 0 #ffffffff #ff355594 none
    
    # Timeout msg
    menu color timeout 0 #ffffffff #00000000 none
    menu color timeout_msg 0 #ffffffff #00000000 none
    
    # Command prompt text
    menu color cmdmark 0 #84b8ffff #00000000 none
    menu color cmdline 0 #ffffffff #00000000 none
    
    # Do not display the actual menu unless the user presses a key. All that is displayed is a timeout message.
    
    menu tabmsg Press Tab for full configuration options on menu items.
    
    menu separator # insert an empty line
    menu separator # insert an empty line
    
    label linux-new
      menu label Centos7.5
      menu default
      kernel vmlinuz
      append inst.ks=cdrom:/ks.cfg initrd=initrd.img inst.stage2=hd:/dev/cdrom
    
    label linux
      menu label ^Install CentOS Linux 7
      kernel vmlinuz
      append initrd=initrd.img inst.stage2=hd:LABEL=CentOS\x207\x20x86_64 quiet
    
    label check
      menu label Test this ^media & install CentOS Linux 7
      kernel vmlinuz
      append initrd=initrd.img inst.stage2=hd:LABEL=CentOS\x207\x20x86_64 rd.live.check quiet
    
    menu separator # insert an empty line
    
    # utilities submenu
    menu begin ^Troubleshooting
      menu title Troubleshooting
    
    label vesa
      menu indent count 5
      menu label Install CentOS Linux 7 in ^basic graphics mode
      text help
    	Try this option out if you're having trouble installing
    	CentOS Linux 7.
      endtext
      kernel vmlinuz
      append initrd=initrd.img inst.stage2=hd:LABEL=CentOS\x207\x20x86_64 xdriver=vesa nomodeset quiet
    
    label rescue
      menu indent count 5
      menu label ^Rescue a CentOS Linux system
      text help
    	If the system will not boot, this lets you access files
    	and edit config files to try to get it booting again.
      endtext
      kernel vmlinuz
      append initrd=initrd.img inst.stage2=hd:LABEL=CentOS\x207\x20x86_64 rescue quiet
    
    label memtest
      menu label Run a ^memory test
      text help
    	If your system is having issues, a problem with your
    	system's memory may be the cause. Use this utility to
    	see if the memory is working correctly.
      endtext
      kernel memtest
    
    menu separator # insert an empty line
    
    label local
      menu label Boot from ^local drive
      localboot 0xffff
    
    menu separator # insert an empty line
    menu separator # insert an empty line
    
    label returntomain
      menu label Return to ^main menu
      menu exit
    
    menu end


**5、编辑安装ks文件**

    vim /centos7.5-iso/ks.cfg
    
    #platform=x86, AMD64, 或 Intel EM64T
    #version=DEVEL
    #key --skip
    # Firewall configuration
    firewall --disabled
    # Install OS instead of upgrade
    install
    # Use network installation
    cdrom
    # Root password
    rootpw !!)a1104
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
    #part biosboot  --size=1 --ondrive=sda
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


**6、生成镜像文件**

    mkisofs -o CentOS-7.5.iso -b isolinux/isolinux.bin  -c isolinux/boot.cat -no-emul-boot -boot-load-size 4 -boot-info-table -R -J -v -T /centos7.5-iso/