# KVM手册 #
### 一、安装kvm ###
**1、初始化环境**
-   配置ip
-   关闭防火墙
-   初始化源
-   等等

**2、检查是否支持虚拟化**

    [root@bogon ~]# egrep -e '(vmx|svm)' --color=auto /proc/cpuinfo 
    flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant_tsc arch_perfmon pebs bts rep_good xtopology nonstop_tsc aperfmperf pni pclmulqdq dtes64 monitor ds_cpl vmx smx est tm2 ssse3 fma cx16 xtpr pdcm pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand lahf_lm abm ida arat epb xsaveopt pln pts dtherm tpr_shadow vnmi flexpriority ept vpid fsgsbase bmi1 hle avx2 smep bmi2 erms invpcid rtm
    flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant_tsc arch_perfmon pebs bts rep_good xtopology nonstop_tsc aperfmperf pni pclmulqdq dtes64 monitor ds_cpl vmx smx est tm2 ssse3 fma cx16 xtpr pdcm pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand lahf_lm abm ida arat epb xsaveopt pln pts dtherm tpr_shadow vnmi flexpriority ept vpid fsgsbase bmi1 hle avx2 smep bmi2 erms invpcid rtm
    flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant_tsc arch_perfmon pebs bts rep_good xtopology nonstop_tsc aperfmperf pni pclmulqdq dtes64 monitor ds_cpl vmx smx est tm2 ssse3 fma cx16 xtpr pdcm pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand lahf_lm abm ida arat epb xsaveopt pln pts dtherm tpr_shadow vnmi flexpriority ept vpid fsgsbase bmi1 hle avx2 smep bmi2 erms invpcid rtm
    flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant_tsc arch_perfmon pebs bts rep_good xtopology nonstop_tsc aperfmperf pni pclmulqdq dtes64 monitor ds_cpl vmx smx est tm2 ssse3 fma cx16 xtpr pdcm pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand lahf_lm abm ida arat epb xsaveopt pln pts dtherm tpr_shadow vnmi flexpriority ept vpid fsgsbase bmi1 hle avx2 smep bmi2 erms invpcid rtm

**3、安装kvm软件包**
    
    #命令行管理
    yum -y install qemu-kvm libvirt python-virtinst bridge-utils virt-viewer
    #图形化管理包
    yum -y install virt-viewer virt-manager
    #安装管理工具
    yum -y install libguestfs-tools


**4、检查是否安装成功**

    #查看模块
    [root@bogon ~]# lsmod  |grep kvm
    kvm_intel              55464  0 
    kvm                   345038  1 kvm_intel

    #查看服务& 依赖主机名解析
    /etc/init.d/libvirtd start  
    [root@bogon ~]# virsh -c qemu:///system list
     Id    Name                           State
    ----------------------------------------------------

**5、桥接网卡配置**

    [root@bogon network-scripts]# vim ifcfg-br0 
    DEVICE=br0
    TYPE=Bridge
    ONBOOT=yes
    NM_CONTROLLED=no
    BOOTPROTO=none
    IPADDR=10.0.0.210
    NETMASK=255.255.255.0
    GATEWAY=10.0.0.1
    DNS1=202.106.0.20
    USERCTL=no
    PEERDNS=yes
    IPV6INIT=no

    [root@bogon network-scripts]# vim ifcfg-eth0 
    DEVICE=eth0
    TYPE=Ethernet
    ONBOOT=yes
    NM_CONTROLLED=no
    BOOTPROTO=none
    BRIDGE=br0

**6、创建img磁盘**

    [root@kvm vhost]# qemu-img  create -f qcow2 moban_centos_6-8.img 20G
    Formatting 'moban_centos_6-8.img', fmt=qcow2 size=21474836480 encryption=off cluster_size=65536 
    
    [root@kvm vhost]# qemu-img  info moban_centos_6-8.img 
    image: moban_centos_6-8.img
    file format: qcow2
    virtual size: 20G (21474836480 bytes)
    disk size: 196K
    cluster_size: 65536
 
**7、安装模板机**

    #安装linux-6
    virt-install --name moban-centos6.8 --boot network,cdrom,menu=on --ram 1024 --vcpus=1 --os-type=linux --accelerate --cdrom /mnt/iso/CentOS-6.8-x86_64-bin-DVD1.iso --disk path=/mnt/vhost/moban_centos_6-8.img,size=4,format=qcow2,bus=ide --bridge=br0 --vnc --vncport=5991 --vnclisten=0.0.0.0

    --extra-args 'console=ttyS0,115200n8 serial'   配置clonsole  未测试

    #安装centos-7
    virt-install  --name CentOS75 --ram 4096  \
    --disk     path=/data/os/kvm/system/CentOS75.img,size=4,format=qcow2 \
    --vcpus 2 \
    --os-type=linux \
    --os-variant rhel7 \
    --network bridge=br0 \
    --graphics vnc,port=5910 \
    --cdrom /data/os/kvm/images/CentOS-7-x86_64-DVD-1804.iso  \
    --boot network,cdrom,menu=on 


    #安装windows
    virt-install --name moban-win7 --ram 1024 --vcpus=1 --os-type=windows --accelerate --cdrom=/mnt/iso/Win7_sp1_cn_64.iso   --disk path=/mnt/vhost/moban_win7.img,size=4,format=qcow2,bus=ide --bridge=br0 --vnc --vncport=5991  --vnclisten=0.0.0.0 
    
**8、vnc 客户端连接  IP:5991 进行安装**

**9、linux 虚拟机配置console连接虚拟机**
    
    1）添加允许终端登录 echo "ttyS0" >> /etc/securetty
    2）编辑/etc/grub.conf中加入console=ttyS0
    3）编辑/etc/inittab，在最后一行加入内容 S0:12345:respawn:/sbin/agetty ttyS0 115200
    4）使用 virsh consloe DOMAIN
       退出当前连接 ctrl + ]

    centos7:
        grubby --update-kernel=ALL --args="console=ttyS0"
### 二、virsh 使用 ###
**1、virsh基本使用**

    查看当前所有开机虚拟机 
    virsh list 

    查看所有虚拟机
    virsh list --all 
    
    开启虚拟机
    virsh  start   DOMAIN-NAME 

    关闭虚拟机  &依赖虚拟机开启 ACPID 服务
    virsh  shutdown  DOMAIN-NAME 
    
    关闭虚拟机（关闭电源）
    virsh destroy  DOMAIN-NAME 

    重启虚拟机
    virsh reboot DOMAIN-NAME
    
    开机随虚拟机启动
    virsh autostart DOMAIN-NAME

    关闭开机自启动
    virsh autostart --disabled DOMAIN-NAME
    
    暂停/挂起虚拟机
    virsh suspend DOMAIN-NAME
    
    恢复虚拟机
    virsh resume  DOMAIN-NAME

    保存虚拟机状态
    virsh save  DOMAIN-NAME
    
    回复虚拟机状态
    virsh restore  DOMAIN-NAME

    查看虚拟机的基本信息
    virsh dominfo  DOMAIN-NAME

    查看虚拟机的ID
    virsh domid  DOMAIN-NAME

    查看虚拟机当前的状态
    virsh domstate DOMAIN-NAME

    
    

**2、virsh 高级使用**

**配置编辑：**    

    编辑虚拟机配置文件& 必须重启才能生效
    virsh edit DOMAIN-NAME 
    
    修改

  
**磁盘：**
    
    添加磁盘
    qemu-img  create -f raw nfs-node1-sdb.img 5G
    virsh attach-disk nfs-node1  /mnt/disk/nfs-node1-sdb.img sdb  --current
    
    删除磁盘
    virsh detach-disk nfs-node1  sdb  --current

    修改磁盘大小
    

    查看虚拟机所有磁盘
    virsh domblklist nfs-node1
    
    查看虚拟机指定磁盘信息
    virsh domblkinfo nfs-node1 sdb
    virsh domblkstat nfs-node1 sdb
    
    查看虚拟机里面的错误磁盘
    virsh domblkerror nfs-node1
        

**内存：**

    修改内存大小&开机状态
    virsh setmem  DOMAIN-NAME  --size  SIZE
    virsh setmem  demo --size 2G

    修改最大内存大小&关机状态
    virsh setmaxmem DOMAIN-NAME  --size  SIZE

**CPU：**
    
    设置CPU最大核心数
    virsh setvcpus DOMAIN-NAME --maximum 2 --config
        
    设置CPU 核心数
    virsh setvcpus DOMAIN-NAME  2
    
    宿主机CPU 特性查看
    virsh nodeinfo

    查看虚拟机和物理cpu的对应关系
    virsh vcpuinfo DOMAIN-NAME
    
    查看虚拟机可以使用那些物理逻辑CPU
    virsh emulatorpin DOMAIN-NAME
    
    绑定虚拟机只允许在那些物理逻辑CPU上调度
    virsh emulatorpin DOMAIN-NAME 0-1 --live

    绑定虚拟机vcpu在指定的物理逻辑CPU
    virsh vcpupin DOMAIN-NAME 0 1     （0 为vcpu，1 为物理cpu 通过vrish vcpuinfo 查询获得）


    
    

**网卡：**
  
    查看虚拟机的所有网卡接口
    virsh domiflist demo
    
    查看虚拟机的指定接口信息
    virsh domifstat  demo vnet0
    
    添加网卡
    virsh attach-interface demo --type bridge --source br0
    
    删除网卡
    virsh detach-interface demo --type bridge  --mac 52:54:00:75:14:0f
    
**交换机：**
    
    查看所有网络
    virsh net-list

    查看网络的详细信息
    virsh net-info NET-NAME
    virsh net-dumpxml NET-NAME
    

    添加交换机
    

    删除交换机
    
    修改交换机

    

**3、虚拟机克隆**

    #例如：
    virt-clone -o moban-centos6.8 -n  demo -f /mnt/vhost/demo.img 

    -o 指定模板虚拟机
    -n 克隆后的虚拟机名称
    -f 指定克隆后的磁盘所在位置
    
    清除硬件信息
    virt-sysprep -d demo  
    
    virt edit demo 
    修改vnc端口
    
     
**4、虚拟机快照**

    #磁盘格式为qcow2

    创建快照
    virsh snapshot-create DOMAIN-NAME
    virsh snapshot-create-as DOMAIN-NAME  DESC 

    查看快照
    virsh snapshot-list  DOMAIN-NAME
    qemu-img info  DOMAIN-DISK
    qemu-img snaphot -l DOMAIN-DISK
    
    查看最新快照
    virsh snapshot-current DOMAIN-NAME

    删除快照
    virsh snapshot-delete DOMAIN-NAME SNAPSHOT-ID
    
    恢复到指定快照
    virsh destory DOMAIN-NAME
    virsh snapshot-revent DOMAIN-NAME SNAPSHOT-ID

    
**5、虚拟机迁移**
    
    导出虚拟机xml 文件
    virsh dumpxml  DOMAIN > /tmp/dump.xml
    拷贝虚拟机xml 硬盘文件
    scp xxx.img   xxx:xxx:/tmp
    编辑 dump.xml文件指定对应的磁盘路径
    vim dump.xml

    加载虚拟机
    virsh define  /xxx/dump.xml
    virsh list --all 查看

### 三、虚拟机配额 ###
**1、CPU**
    
    #绑定CPU 到指定核心
    virsh emulatorpin win7-demo 1-3 --config
    
    <cputune>
    <emulatorpin cpuset='1-3'/>
    </cputune>

    #taskset 绑定CPU
    taskset -cp 0-1 1947
    
    查看
    taskset -cp 1947
    
**2、内存（MEM)**

    #设置内存配额
    hard-limit 强制最大内存
    soft-limit 可用最大内存
    
    virsh memtune win7-demo --hard-limit 1048576 --soft-limit 1048576   --config

    <memtune>
    <hard_limit unit='KiB'>1048576</hard_limit>
    <soft_limit unit='KiB'>1048576</soft_limit>
    </memtune>


**3、磁盘（IO)**
     
     #设置磁盘IOPS
     virsh blkdeviotune win7-demo  hda --read-bytes-sec 4096 --write-bytes-sec 2048 --read-iops-sec 20  --write-iops-sec 20 --config
     
     #设置磁盘权重
     virsh blkiotune win7-demo --weight 500 --config
    
    <blkiotune>
    <weight>500</weight>
    </blkiotune>

     #设置磁盘策略
     法一：
     virsh attach-disk  ...  --cache  
     
     法二：   
     <disk type='file' device='disk'>
      <driver name='qemu' type='qcow2' cache='none'/>  # cache 可为 writeback, none, writethrough，directsync，unsafe 等
    </disk>

     
**4、网络（IO)**
    
    #配置带宽限速
    virsh domiftune win7-demo 52:54:00:10:8b:bb --inbound 3000,3000,3000 --outbound 3000,3000,3000 --config

### 四、虚拟机文件操作
**1、编辑虚拟机文件**
    
    virt-edit -d DOMAIN /etc/hosts
    
    virt-edit -a DOMAIN.IMG /etc/hosts
**2、查看虚拟机文件**

    virt-ls -d DOMAIN /etc    
    virt-ls -a DOMAIN.img /etc
    
    virt-cat -d DOMAIN /etc/hosts
    virt-cat -a DOMAIN.img /etc/hosts

    virt-tail -d DOMAIN /etc/hosts
    virt-tail -a DOMAIN.img /etc/hosts
    
**3、从虚拟机拷贝出文件**

    virt-copy-out -d DOMAIN /etc/hosts .
    virt-copy-out -a DOMAIN.img /etc/hosts .
    

    virt-tar-out -d DOMAIN /tmp files.tar
    virt-tar-out -a DOMAIN.img /tmp files.tar

    
    

**4、拷贝文件到虚拟机**

    virt-copy-in -d DOMAIN /etc/hosts /tmp
    virt-copy-in -a DOMAIN.img /etc/hosts /tmp
    //注意：需要调整挂载选项才能在线拷贝，否则虚拟机需要再停机状态
    
**5、给windows虚拟机增加注册表**

### 五、相关问题
**1、windows时钟间隔8小时**

    <clock offset='utc'>
      <timer name='rtc' tickpolicy='catchup'/> 
      <timer name='pit' tickpolicy='delay'/> 
      <timer name='hpet' present='no'/> 
    </clock>
    
    变更为：
    <clock offset='localtime'>
        <timer name='rtc' tickpolicy='catchup'/> 
        <timer name='pit' tickpolicy='delay'/>
        <timer name='hpet' present='no'/>
    </clock>
    
    


**2、网卡速率不对，适应为10Mb**
    
    <interface type='bridge'>
      <mac address='52:54:00:b6:01:4f'/>
      <source bridge='br0'/>
      <model type='rtl8139'/>  
      <address type='pci' domain='0x0000' bus='0x00' slot='0x03' function='0x0'/>
    </interface>
    
    #对应网卡速率如下
    <model type='virtio'/>   10M
    <model type='rtl8139'/>  100M
    <model type='e1000'/>    1000M
    
**3、迁移虚拟机发现cpu特性不支持**

      <cpu mode='custom' match='exact' check='partial'>
        <model fallback='allow'>IvyBridge-IBRS</model>
      </cpu>
      
      #删除相关配置即可
      
**4、鼠标不同步**

    <devices> 字段添加 <input type=’tablet’ bus=’usb’/> 即可

官方文档：[https://libvirt.org/docs.html](https://libvirt.org/docs.html)