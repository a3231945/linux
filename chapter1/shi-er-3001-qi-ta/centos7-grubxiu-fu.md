# CentOS7 手动修复grub

### 1、找到boot分区

在`grub>` 下输入：

    ls
    
    (hd0),(hd0,msdos1),(hd0,msdos2),(hd0,msdos3)

查找`boot`分区:

    ls (hd0,msdos1)/boot/grub2
    ls (hd0,msdos2)/boot/grub2
    ls (hd0,msdos3)/boot/grub2
    
    注意：一般情况下会在下 (hd0,msdos1)
### 2、手动引导进入系统
    
    #插入xfs模块
    grub> insmod xfs
    
    #设置boot分区
    grub> set root=(hd0,msdos1)
    
    #指定kernel内核和 根分区（注意：这里的根据实际情况的磁盘分区填写root字段）
    grub> linux16 /vmlinuz-xxxx root=/dev/sda3
    
    #指定init 程序
    grub> initrd16 /initramfs-xxx.img
    
    #启动
    grub> boot

### 3、修复grub
    
    sudo grub2-mkconfig -o /boot/grub2/grub.cfg
    
    
