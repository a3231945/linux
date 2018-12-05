**1、简介**

SELinux是「Security-Enhanced Linux」的简称，是美国国家安全局「NSA＝The National Security Agency」 和SCC（Secure Computing Corporation）开发的 Linux的一个扩张强制访问控制安全模块。

      因为企业的业务平台的服务器上存储着大量的商务机密，个人资料，个人资料它直接关系到个人的隐私问题。特别是政府的网站，作为信息公开的平台，它的安全就更显得重要了。这些连到互联网的服务器，不可避免的要受到来自世界各地的各种威胁。最坏的时候我们的服务器被入侵，主页文件被替换，机密文件被盗走。除了来自外部的威胁外，内部人员的不法访问，攻击也是不可忽视的。对于这些攻击或者说是威胁，当然有很多的办法，有防火墙，入侵检测系统，打补丁等等。因为Linux也和其他的商用UNIX一样，不断有各类的安全漏洞被发现。

传统的Linux OS的不足之处
虽然Linux 比起 Windows 来说，它的可靠性，稳定定要好得多，但是他也是和其他的UNIX 一样，有以下这些不足之处。


	1、存在特权用户root
		任何人只要得到root的权限，对于整个系统都可以为所欲为。这一点Windows也一样。
	2、对于文件的访问权的划分不够细
		在linux系统里，对于文件的操作，只有「所有者」,「所有组」,「其他」这３类的划分。对于「其他」这一类里的用户再细细的划分的话就没有办法了。
	3、SUID程序的权限升级
		如果设置了SUID权限的程序有了漏洞的话，很容易被攻击者所利用。
	4、DAC (Discretionary Access Control)问题
		文件目录的所有者可以对文件进行所有的操作，这给系统整体的管理带来不便。对于以上这些的不足，防火墙，入侵检测系统都是无能为力的。



DAC（Discretionary access control，自主访问控制）：

	DAC机制就是指对象（比如程序、文件或进程等）的的拥有者可以任意的修改或授予此对象相应的权限。例如传统Linux，Windows等。
MAC（Mandatory Access Control，强制访问控制）：

	MAC机制是指系统不再允许对象（比如程序、文件或文件夹等）的拥有者随意修改或授予此对象相应的权限，而是透过强制的方式为每个对象统一授予权限，例如SELinux。



SELinux的优点

SELinux系统比起通常的Linux系统来，安全性能要高的多，它通过对于用户，进程权限的最小化，即使受到攻击，进程或者用户权限被夺去，也不会对整个系统造成重大影响。在标准Linux中，主体的访问控制属性是与进程通过在内核中的进程结构关联的真实有效的用户和组ID，这些属性通过内核利用大量工具进行保护，包括登陆进程和setuid程序，对于文件，文件的inode包括一套访问模式位、文件用户和组ID。以前的访问控制基于读/写/执行这三个控制位，文件所有者、文件所有者所属组、其他人各一套。在SELinux中，访问控制属性总是安全上下文三人组形式，所有文件和主体都有一个关联的安全上下文，标准Linux使用进程用户/组ID，文件的访问模式，文件用户/组ID要么可以访问要么被拒绝，SELinux使用进程和客体的安全上下文，需要特别指出的是，因为SELinux的主要访问控制特性是类型强制，安全上下文中的类型标识符决定了访问权。若要访问文件，必须同时具有普通访问权限和SELinux访问权限。因此即使以超级用户身份root运行进程，根据进程以及文件或资源的SELinux安全性上下文可能拒绝访问文件或资源。

	例如：
	在Linux中，passwd程序是可信任的，修改存储经过加密的密码的影子密码文件（/etc/shadow），passwd程序执行它自己内部的安全策略，允许普通用户修改属于他们自己的密码，同时允许root修改所有密码。为了执行这个受信任的作业，passwd程序需要有移动和重新创建shadow文件的能力，在标准Linux中，它有这个特权，因为passwd程序可执行文件在执行时被加上了setuid位，它作为root用户（它能访问所有文件）允许，然而，许多程序都可以作为root允许（实际上，所有程序都有可能作为root允许）。这就意味着任何程序(当以root身份运行时)都有可能能够修改shadow文件。类型强制使我们能做的事情是确保只有passwd程序（或类似的受信任的程序）可以访问shadow文件，不管运行程序的用户是谁。




设置开机启动时，SELinux的运行模式

	[root@localhost ~]# cat /etc/sysconfig/selinux
	# This file controls the state of SELinux on the system.
	# SELINUX= can take one of these three values:
	# enforcing - SELinux security policy is enforced.
	# permissive - SELinux prints warnings instead of enforcing.
	# disabled - No SELinux policy is loaded.
	SELINUX=enforcing
	# SELINUXTYPE= type of policy in use. Possible values are:
	# targeted - Only targeted network daemons are protected.
	# strict - Full SELinux protection.
	SELINUXTYPE=targeted
	
	SELINUX 参数值：
	enforcing 强行（报警并限制）
	permissive 许可（报警不限制）
	disabled 禁用
	
	SELINUXTYPE 参数值：
	targeted 	保护网络相关服务
	strict 	完整的保护功能，包含网络服务、一般指令及应用程序
	mls 		多级别模块化


查看SELinux的当前状态，并对当前状态做调整

	[root@node1 ~]# sestatus
		SELinux status: enabled
		SELinuxfs mount: /selinux
		Current mode: enforcing
		Mode from config file: enforcing
		Policy version: 21
		Policy from config file: targeted
	[root@node1 ~]# getenforce
		Enforcing
	[root@node1 ~]# setenforce 0
		Permissive
	[root@node1 ~]# setenforce 1
	[root@node1 ~]# getenforce
		Enforcing
	


查看安全上下文 ls –Z (查看文件的)；ps –Z (查看进程的)

	[root@node1 ~]# ls -Z /etc/passwd
	-rw-r--r-- root root system_u:object_r:etc_t /etc/passwd
	[root@node1 ~]# ls -Zld /mnt/
	drwxr-xr-x 2 system_u:object_r:mnt_t root root 4096 Oct 1 2009 /mnt/

查看系统默认安全上下文

	[root@node1 ~]# semanage fcontext -l
