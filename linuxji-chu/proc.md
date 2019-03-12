## /proc 目录下文件详解 ##

### 一、xxx

**1、内存**
    
    /proc/buddyinfo           伙伴系统的信息  
    /proc/pagetypeinfo        伙伴系统进一步细分信息
    /proc/zoneinfo            内存区域使用情况
    /proc/slabinfo 

    /proc/meminfo             当前内存信息
    /proc/vmstat              虚拟内存统计信息
    /proc/vmallocinfo         虚拟内存分配信息
    /proc/swaps               swap分区使用情况
    /proc/mtd                 内存设备分区表信息
    /proc/dma                 DMA（直接内存访问）通道的列表

    /proc/mtrr                系统使用的Memory Type Range Registers (MTRRs)

    /proc/kpagecount  
    /proc/kpageflags  


**2、IO**
    
    /proc/filesystems         目前系统支持的文件系统
    /proc/diskstats           磁盘设备的统计信息  
    /proc/ioports             当前系统硬件设备使用的IO端口列表
    /proc/iomem               I/O 内存映射
    /proc/locks               当前被内核锁定的文件
    /proc/mounts              当前挂载信息


**3、cpu**

    /proc/cpuinfo             cpu相关信息
    /proc/loadavg             当前系统负载
    /proc/softirqs            系统软中断信息
    /proc/schedstat           调度器信息
    /proc/sched_debug         调度器debug信息


**4、网卡**
    

**5、网络**


**6、kernel**
    
    /proc/cmdline             在引导启动时传递给Linux内核的参数
    /proc/crypto              内核支持的加密方式
    /proc/modules             当前系统已经加载的模块（lsmod）
    /proc/version             内核版本信息
    /proc/stat                系统和内核的统计信息
    /proc/fb                  内核编译期间帧缓冲信息
    /proc/kmsg                内核日志信息
    /proc/kcore               表示系统物理内存，可以用gdb检查内核数据结构的当前状态    
    /proc/kallsyms            内核符号信息，主要用于调试
    /proc/timer_list          内核各种计时器信息
    /proc/timer_stats          

    /proc/sysrq-trigger       内核触发器（危险！！！）
    /proc/execdomains         Linux内核当前支持的execution domains


**7、other**
    
    /proc/interrupts          中断表
    /proc/uptime              系统运行时间

    /proc/devices             设备信息（主设备号等）
    /proc/mdstat              虚拟设备信息（软raid等）
    /proc/misc                其他的主要设备(设备号为10)上注册的驱动
    /proc/cgroup              cgroup相关信息
    /proc/consoles            
    
    /proc/keys                证书相关 
    /proc/key-users 
        
    
二、目录
        
    acpi
    bus
    driver
    fs
    ipmi
    irq
    scsi
    sys
    sysvipc
    tty
  
  
 
 
 
 


