
## 软件包管理
### 一、安装/查询/卸载


- 源码包tarball    		没有编译
- 二进制包			已编译


      系统平台				包类型   		工具				在线安装（自动解决依赖关系）
      RedHat/Centos/Fedora  	RPM	 		rpm,rpmbuild		yum
      Ubuntu/Debian			DPKG	 	dpkg			apt

**注意**： 不管是源码包，还是二进制包，安装时都可能会有依赖关系！！！


### 二、RPM包管理

**1、获得RPM包途径**：

   - RedHat光盘或官方网站 ftp://ftp.redhat.com
   - rpmfind.net
   - ...


      notecase-1.9.8-1.fc6.i386.rpm			套件名
      ntfs-3g-2010.3.6-1.el5.i386.rpm			套件名
      stardict-langdao-ec-gb-2.4.2-1.noarch.rpm	套件名
      
      软件包名   版本号   	发布次数   	平台
      notecase  1.9.8      	1.fc6      	i386（系统平台i386  x86_64 noarch ）
      ntfs-3g     2010.3.6	1.el5	     	i386
      
      //version:  版本
      //release:  发布版本
      
      # uname -m		//查看当前系统是多少位
      x86_64
      
      # cat /etc/issue
      CentOS release 6.5 (Final)
      Kernel \r on an \m
   
      # cat /etc/redhat-release 
      CentOS release 6.5 (Final)



**2、使用rpm工具管理rpm包 (警告：需要手动解决包的依赖关系)**
- 安装

      # rpm -ivh stardict-langdao-ec-gb-2.4.2-1.noarch.rpm 	套件名
      # rpm -ivh ntfs-3g-2010.3.6-1.el5.i386.rpm			套件名
      额外选项：
      --force		//强制安装，一般适用于某个包文件损坏或丢失
      --nodeps		//忽略依赖关系，不建议使用

- 查询   (从本地的rpm数据库)

      # rpm -q ntfs-3g			//查询指定包是否安装
      # rpm -qa |grep ntfs
      # rpm -ql ntfs-3g			//查询ntfs-3g安装的文件   	
      # rpm -qf /usr/bin/ntfs-3g	//查询该文件属于哪个rpm包
      # rpm -qi ntfs-3g			//查询包的information
      
      # rpm -e ntfs-3g
      # rpm -qi ntfs-3g
      package ntfs-3g is not installed
      # rpm -qc ntfs-3g
      # rpm -q vsftpd
      vsftpd-2.0.5-24.el5
      # rpm -qc vsftpd			//查询某个包安装的配置文件
      /etc/logrotate.d/vsftpd.log
      /etc/pam.d/vsftpd
      /etc/vsftpd/ftpusers
      /etc/vsftpd/user_list
      /etc/vsftpd/vsftpd.conf
      /etc/vsftpd/vsftpd_conf_migrate.sh
      
      # rpm -qd vsftpd			//查安装的帮助文档

- 扩展知识： 针对没有安装的包，直接从套件中查询

      # rpm -qip vagrant_2.1.1_x86_64.rpm
      # rpm -qcp vagrant_2.1.1_x86_64.rpm
      # rpm -qdp vagrant_2.1.1_x86_64.rpm
      # rpm -qlp vagrant_2.1.1_x86_64.rpm


- 卸载

      # rpm -e vagrant
      # rpm -e vagrant --nodeps	
   

rpm工具管理软件包总结：
- 很难解决包依赖关系
- 如果某个文件没有，很难知道它由哪个rpm包提供，例如gunplot命令是由哪个包提供？




**3、使用YUM工具管理RPM包 (自动解决包的依赖关系)**
YUM仓库中必须包括：
- 软件包
- 数据库文件（数据库记录的是：每个包能提供哪些文件？每个包依赖哪些文件？）


      第一种情况：使用系统光盘安装rpm软件
      
      1.  本身存在yum数据库，不需要生成
      
      2.  YUM指定数据库位置
      [root@yangs ~]# df
      文件系统               1K-块        已用     可用 已用% 挂载点
      /dev/hdc               3435526   3435526         0 100% /media/cd
      /media/cd/Server
      /media/cd/VT
      /media/cd/Cluster 
      /media/cd/ClusterStorage
      
      [root@station230 ~]# cd /etc/yum.repos.d/
      [root@station230 yum.repos.d]# vim rhel5.repo		//文件名自定义，必须以.repo结尾
      [Server]					//仓库ID
      name=Server				//仓库名
      baseurl=file:///media/cd/Server	//仓库位置
      enabled=1				//启用仓库
      gpgcheck=0				//不检查软件包的签名


- 检查目前可用的仓库

      # yum clean all	//清空缓存
      # yum repolist    //查询可用的仓库
- 安装

      # yum install mysql-server
      # yum install nginx
      # yum -y install mysql-server
      # yum -y install php
      # rpm -e php
      # yum -y remove mysql-server
      # yum -y install "mysql*" httpd vsftpd samba dovecot
      # yum -y update samba
   
- 查询

      # yum list httpd mysql-server vsftpd tftp-server bind
      Installed Packages
      httpd.i386                  2.2.3-63.el5                 installed
      mysql-server.i386     5.0.77-4.el5_6.6             installed
      tftp-server.i386            0.49-2                        installed
      vsftpd.i386                 2.0.5-24.el5                 installed
      Available Packages
      bind.i386                   30:9.3.6-20.P1.el5         Server 

- 卸载

      # yum -y remove mysql-server


- 查询某个文件（通常是当前系统中没有）由哪个rpm包提供

      示例1：
      
            # gnuplot
            bash: gnuplot: command not found		//没有找到该命令
            # yum provides gnuplot
            Loaded plugins: katello, product-id, security, subscription-manager
            Updating certificate-based repositories.
            Unable to read consumer identity
            gnuplot-4.0.0-14.el5.i386 : 绘制数学表达式和数据的程序。
            Repo        : Server
            Matched from:
            # yum -y install gnuplot
            # gnuplot 

      示例2：
            # rpm -e httpd
            
            # htpasswd              	//没有找到该命令
            bash: htpasswd: command not found
            
            # yum provides */htpasswd
            Loaded plugins: katello, product-id, security, subscription-manager
            Updating certificate-based repositories.
            Unable to read consumer identity
            Server/filelists                                    | 2.4 MB     00:03     
            httpd-2.2.3-63.el5.i386 : Apache HTTP 服务器
            Repo        : Server
            Matched from:
            Filename    : /usr/bin/htpasswd
            
            # yum -y install httpd-2.2.3-63.el5.i386
            # htpasswd
      
      示例3：
            # createrepo
            bash: createrepo: command not found
            
            # yum provides createrepo
            Loaded plugins: katello, product-id, security, subscription-manager
            Updating certificate-based repositories.
            Unable to read consumer identity
            createrepo-0.4.11-3.el5.noarch : 生成一个通用元数据库
            Repo        : Server
            Matched from:
            
            # yum -y install createrepo


指定光盘上其他软件包仓库

      # yum -y install gfs-utils
      

- 软件包组管理

      # yum grouplist					      //查询当前软件包组的安装情况
      # yum -y groupinstall "DNS 名称服务器"		//安装软件包组
      
      作业：
      查询并安装 “开发工具” “开发库” 软件包组



      第二种情况：第三方rpm软件包（例如下载的rpm软件包）
      1. 生成yum数据库		//createrepo软件包
      2. 指定数据库位置
      3. yum安装软件包
      
      ＝＝准备仓库（软件包 和 数据库文件）
      [root@station230 soft]# yum -y install createrepo
      
      [root@yangs newsoft]# pwd
      /newsoft
      [root@yangs newsoft]# ls
      ntfs-3g-2011.4.12-5.el5.i386.rpm
      
      [root@yangs newsoft]# createrepo .			//在当前目录生成yum数据库文件
      1/1 - ntfs-3g-2011.4.12-5.el5.i386.rpm                                          
      Saving Primary metadata
      Saving file lists metadata
      Saving other metadata
      [root@yangs newsoft]# ls
      ntfs-3g-2011.4.12-5.el5.i386.rpm  repodata
      
      ＝＝给YUM指定仓库
      [root@yangs ~]# cd /etc/yum.repos.d/
      [root@yangs yum.repos.d]# cat newsoft.repo 
      [newsoft]
      name=newsoft
      baseurl=file:///newsoft
      enabled=1
      gpgcheck=0
      
      ＝＝安装
      [root@yangs ~]# yum clean all
      [root@yangs ~]# yum repolist
      Loaded plugins: katello, product-id, security, subscription-manager
      Updating certificate-based repositories.
      Unable to read consumer identity
      repo id                                                     repo name                                                   status
      ClusterStorage                                              ClusterStorage                                                 39
      Server                                                      Server                                                      2,472
      newsoft                                                     newsoft                                                         1
      repolist: 2,512
      [root@yangs ftp]# yum -y install ntfs-3g
      
      扩展知识：
      如果有新rpm包加到入/newsoft仓库
      [root@yangs ftp]# cp notecase-1.9.8-1.fc6.i386.rpm /newsoft/
      [root@yangs ftp]# cd /newsoft/
      [root@yangs newsoft]# createrepo .
      2/2 - ntfs-3g-2011.4.12-5.el5.i386.rpm                                          
      Saving Primary metadata
      Saving file lists metadata
      Saving other metadata
      
      
      ＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝支持RPM签名检查＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝
      ＝＝RPM工具使用签名机制：＝＝
      # rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release  导入红帽公钥
      # rpm -ivh tftp-server-0.49-2.i386.rpm 
          Preparing...                ########################################### [100%]
         1:tftp-server            ########################################### [100%]
      
      ＝＝YUM使用签名机制：＝＝
      方法一：
      # rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release  导入红帽公钥
      [root@station230 yum.repos.d] vim rhel-debuginfo.repo
      [Server]
      name=Server
      baseurl=file:///media/Server
      enabled=1
      gpgcheck=1	//是否检查软件包的签名
      
      方法二：
      [root@station230 yum.repos.d] vim rhel-debuginfo.repo
      [Server]
      name=Server
      baseurl=file:///media/Server
      enabled=1
      gpgcheck=1	//是否检查软件包的签名
      gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release  指定公钥文件
      =====================================================================

### 三、源码包tarball 
获得源码包途径
- 官方网站，可以获得最新的软件包  例如Apache www.apache.org   Nginx www.nginx.org
- www.google.com


准备工作
- 编译环境如gcc编译器，建议安装软件包组 开发工具 开发库
- 准备软件 httpd-2.2.11.tar.bz2

安装httpd

      # rpm -q httpd				//查询
      # service httpd stop			//停止httpd服务 
      # rpm -e httpd	--nodeps		//卸载 yum -y remove httpd
      # tar xvf httpd-2.2.11.tar.bz2	//解压
      # cd httpd-2.2.11

三步曲：

      # ./configure --prefix=/usr/local/apache2
         a. 指定安装路径，例如--prefix=DIR
         b. 启用或禁用某项功能, 例如 --enable-ssl, --disable-filter
         c. 和其它软件关联，例如--with-ssl=DIR
         d. 检查安装环境，例如是否有编译器gcc，是否满足软件的依赖需求
         最终生成：Makefile
      
      # make clean	//清理掉以前编译后产生的 *.o目标文件
      # make		//按Makefile文件编译
      # make install	//安装

验证安装结果：

      [root@yangs ~]# ls /usr/local/apache2/
      bin  build  cgi-bin  conf  error  htdocs  icons  include  lib  logs  man  manual  modules
      [root@yangs ~]# ls /usr/local/apache2/bin/
      ab               apu-1-config  dbmmanage    htcacheclean  htpasswd   logresolve
      apachectl     apxs          envvars      htdbm         httpd      rotatelogs
      apr-1-config  checkgid      envvars-std  htdigest      httxt2dbm
      [root@yangs ~]# /usr/local/apache2/bin/apachectl start	//启动


其它的二进制软件
bin格式软件包：jdk-6u27-linux-i586.bin

      [root@station230 software]# chmod a+x jdk-6u27-linux-i586.bin 
      [root@station230 software]# ./jdk-6u27-linux-i586.bin 













