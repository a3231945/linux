# etc 目录下文件详解 #

### 一、基础配置 ###

**1、主机名**

    /etc/centos-release          Centos系统版本信息
    /etc/machine-id              本地计算机ID配置文件

**2、时间**
      
    /etc/localtime               本地时间配置
    /etc/adjtime                 更正同步系统时钟的时间(hwclock -w 会读取该配置文件)

**3、键盘**

    /etc/inputrc                 键盘映射表    
**4、语言**
**5、DNS**
      
    /etc/host.conf               dns解析配置文件
    /etc/hosts                   主机静态DNS配置
    /etc/resolv.conf             dns配置文件
    /etc/nsswitch.conf           nameserver 切换配置（例如：定义先查找hosts还是dns服务器）   
**6、编辑器**
    
    /etc/vimrc                   vim 全局配置文件
    /etc/virc                    vi  全局配置文件
    /etc/nanorc                  nano 全局配置文件

**7、目录颜色**
    
    /etc/DIR_COLORS              目录颜色配置
    /etc/DIR_COLORS.256color     
    /etc/DIR_COLORS.lightbgcolor
**8、man 手册**

    /etc/man.config              man 手册配置文件

**9、wget**
      
    /etc/wgetrc                  wget命令默认参数配置

**10、提示信息**
    
    /etc/issue                   终端登录前打印的提升信息（系统版本）
    /etc/issue.net
    
    /etc/motd                    登录后提示信息

**11、环境变量**
    
    /etc/profile                 全局环境变量
    /etc/bashrc                  全局环境变量、shell风格、缩进等配置
    /etc/shells                  本机支持的shell
    /etc/environment             机器的环境变量
    
    /etc/bash_completion.d/*     bash命令补全        
    /etc/csh.cshrc                 
    /etc/csh.login

**12、字体**
    
    /etc/fonts/*                 字体配置

### 二、用户、用户组管理 ###

**1、用户**
    
    /etc/passwd                  用户配置文件  
    /etc/passwd-                 用户备份配置文件
    /etc/shadow                  用户密码配置文件
    /etc/shadow-                 用户备份密码配置文件
    
    /etc/login.defs              用户密码策略配置文件
    /etc/default/useradd         创建用户使用的默认文件
    
    /etc/skel/*                  用户模板文件

**2、用户组**
    
    /etc/group                   用户组配置文件
    /etc/group-                  用户组备份配置文件
    /etc/gshadow                 用户组密码配置文件
    /etc/gshadow-                用户组备份密码配置文件

### 三、软件管理 ###
    
    /etc/yum.conf                    yum配置
    /etc/yum/*
    /etc/yum.repos.d/*

### 四、任务计划 ###
**1、at**
    
    /etc/at.deny                 at定时执行任务黑名单

**2、crond**
    
    /etc/crontab                 任务计划主配置文件
    /etc/cron.deny               任务计划黑名单
        
    /etc/cron.d/*                
    /etc/cron.daily/*   
    /etc/cron.weekly/*               
    /etc/cron.hourly/*
    /etc/cron.monthly/*
        
### 五、设备管理 ###
**1、文件系统**
    
    /etc/filesystems             系统支持的文件系统类型
    /etc/fstab                   文件系统挂载信息
    /etc/mtab                    当前系统已经挂载的信息
    
    /etc/mke2fs.conf             mke2fs 格式化参数配置文件
    /etc/crypttab                加密块设备表


    /etc/warnquota.conf          磁盘配额警告信息
    /etc/quotagrpadmins          磁盘配额用户组相关悉尼型 
    /etc/quotatab                磁盘配额表

    /etc/udev/*                  设备相关配置（例如：网卡）                   

### 六、网络管理 ###
        
    /etc/networks                记录本机已经存在的网段 
    
    /etc/ethers                  mac地址对应IP表（配置静态mac表，防止arp攻击）    
    /etc/netconfig               

    /etc/ppp/*                     ppp 点对点拨号协议
      
    /etc/gai.conf                getaddrinfo 配置文件
    /etc/GeoIP.conf              geoipupdate 配置文件
	

### 七、内核与模块管理 ###
**1、模块管理**
    
    /etc/ld.so.cache             动态库加载缓存信息
    /etc/ld.so.conf              动态库加载配置文件  
    /etc/ld.so.conf.d/*     
    /etc/libaudit.conf           审计动态库配置   
    /etc/libuser.conf

    /etc/prelink.cache           动态库优化-预连接 缓存
    /etc/prelink.conf            动态库优化-预连接 配置

    /etc/modprobe.d/*
        
**2、init进程**
    
    /etc/init.conf               init 配置文件
    /etc/inittab                 初始化init进程配置
    /etc/init/*                  
    
    /etc/dracut.conf             配置 /boot/initramfs.img 加载模块
    /etc/dracut.conf.d/*


**3、内核参数**
    
    /etc/sysctl.conf             系统内核参数配置   
    /etc/sysctl.d/*

**4、启动项**
    
    /etc/kdump.conf              kdump 内核配置文件
    /etc/grub.conf               grub  配置
    
    #启动服务相关
    /etc/rc -> rc.d/rc
    /etc/rc.d
    /etc/rc0.d -> rc.d/rc0.d
    /etc/rc1.d -> rc.d/rc1.d
    /etc/rc2.d -> rc.d/rc2.d
    /etc/rc3.d -> rc.d/rc3.d
    /etc/rc4.d -> rc.d/rc4.d
    /etc/rc5.d -> rc.d/rc5.d
    /etc/rc6.d -> rc.d/rc6.d
    /etc/rc.local -> rc.d/rc.local
    /etc/rc.sysinit -> rc.d/rc.sysinit


### 八、服务相关 ###
**1、nfs服务**
    
    /etc/exports                 nfs server配置文件
    /etc/idmapd.conf             nfs配置文件
    /etc/nfsmount.conf           nfs 挂载选项配置

**2、ntp服务**
    
    /etc/ntp.conf                NTP  服务配置
    /etc/ntp/*

**3、mail服务**
    
    /etc/mail.rc                 邮件相关配置
    
    /etc/aliases                 别名（sendmail 发送邮件）
    /etc/aliases.db              别名二进制文件
    
    /etc/mailcap      

**4、rsyslog服务**
    
    /etc/rsyslog.conf            系统日志配置文件
    /etc/rsyslog.d/*
    
    /etc/logrotate.conf          日志轮转配置

**5、ssh服务**
    
    /etc/ssh/*                   ssh 服务相关
### 九、安全配置 ###
**1、TCP wrappers**
    
    /etc/hosts.allow             白名单
    /etc/hosts.deny              黑名单

**2、SElinux**

    /etc/sestatus.conf           SELinux配置文件

**3、cgroup**
    
    /etc/cgconfig.conf           cgroup 配置文件
    /etc/cgrules.conf            cgroup 规则文件
    /etc/cgconfig.d/*
    /etc/cgsnapshot_blacklist.conf 

**4、audit审计**

    /etc/audit/*                 审计相关配置

**5、sudo **
    
    /etc/sudo.conf               sudo 管理配置文件
    /etc/sudoers                  
    /etc/sudo-ldap.conf
    /etc/sudoers.d/*            

**6、pam**
    
    /etc/pam.d/*                 pam 模块相关配置

**7、other**
    
    /etc/krb5.conf               Kerberos 验证配置文件
    /etc/gssapi_mech.conf        GSSAPI 配置
    /etc/securetty               允许登录的终端编号

    /etc/security/*              安全配置相关（例如 文件描述符）

### 十、其他杂项 ###
    
    /etc/updatedb.conf           updatedb查找库索引更新配置
    /etc/services                服务编号-端口表 
    /etc/protocols               协议表
    /etc/rpc                     RPC程序号数据库
    /etc/wvdial.conf             拨号上网
    /etc/sos.conf                sosreport 信息收集的配置
    /etc/printcap                打印机
    /etc/asound.conf             音频配置文件
    /etc/magic                   数据类型 - 字节数
    /etc/rwtab
    
    /etc/acpi/*                  电源服务
    /etc/default/*               一些默认配置

    /etc/pki/*                   证书相关
    /etc/ssl/*


### 十一、sysconfig 目录
    
    略
    






