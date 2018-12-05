**1、简介**

Pluggable Authentication Modules(可插入验证模块，简称PAM)
Linux-PAM(Pluggable Authentication Modules for Linux.基于Linux的插入式验证模块)是一组共享库，使用这些模块，系统管理者可以自由选择应用程序使用的验证机制。也就是说，勿需重新编译应用程序就可以切换应用程序使用的验证机制。甚至，不必触动应用程序就可以完全升级系统使用的验证机制。 将系统提供的服务 和该服务的认证方式分开，使得系统管理员可以灵活地根据需要给不同的服务配置不同的认证方式而无需更改服务程序，同时也便于向系统中添加新的认证手段。应用程序通过libpam函数库来提供服务,应用程序与PAM的结合通过配置文件来完成




使用ldd命令查看有那些程序使用pam验证，并非所有程序都使用PAM

	[root@localhost ~]# ldd `which login` | grep pam.so
		libpam.so.0 => /lib/libpam.so.0 (0x0714b000)
	[root@localhost ~]# ldd `which sshd` | grep pam.so
		libpam.so.0 => /lib/libpam.so.0 (0x0093b000)

这些功能模块存放在 /lib/security/ 目录里,应用程序通过libpam函数库来动态加载所需的模块,实现认证方式,每一个认证模块都会返回pass和fail结果,从而决定验证的成功与否.
通过配置文件来定制服务使用哪些模块，一般来说它们都存放在/etc/pam.d/目录下,

	/etc/pam.d/login
	/etc/pam.d/sshd
	/etc/pam.d/login

pam的学习可以参考pam管理员手册 ：`firefox /usr/share/doc/pam-0.99.6.2/html/Linux-PAM_SAG.html`
**注意：**pam产生的日志会记录在 /var/log/secure


	以字符终端验证程序login为例，来初步了解一下pam的验证过程

	[root@localhost ~]# cat /etc/pam.d/login
	#%PAM-1.0
	auth 		[user_unknown=ignore success=ok ignore=ignore default=bad] 	pam_securetty.so
	auth 		include 	system-auth
	account 	required 	pam_nologin.so
	account 	include 	system-auth
	password 	include 	system-auth
	# pam_selinux.so close should be the first session rule
	session 	required 	pam_selinux.so close
	session 	include 	system-auth
	session 	required 	pam_loginuid.so
	session 	optional 	pam_console.so
	# pam_selinux.so open should only be followed by sessions to be executed in the user context
	session 	required 	pam_selinux.so open
	session 	optional 	pam_keyinit.so force revoke


**2、PAM验证类型 **

	* auth 		验证使用者身份，提示输入账号和密码 
	* account 	基于时间或者密码有效期来决定是否允许访问 
	* password 	禁止用户反复尝试登录，在变更密码时进行密码复杂性控制 
	* session 	进行日志记录，或者限制用户登录的次数,资源使用

**3、PAM控制类型**

	required 必要条件	表示本模块必须返回成功才能通过认证；如果返回成功，继续后续验证，最后否成功由后续验证决定；
				但是如果该模块返回失败的话，失败结果也不会立即通知用户，而是要等所有模块全部执行完毕再将失败结果返回给应用程序。
			
	requisite 必要条件	与required类似，该模块必须返回成功才能通过认证；如果返回成功，继续后续验证，最后否成功由后续验证决定；
				但是一旦该模块返回失败，将不再执行任何模块，而是直接将控制权返回给应用程序。

	sufficient 充分条件	表明本模块返回成功已经足以通过身份认证的要求，不必再执行其它模块；如果验证成功，就立刻返回成功；
				但是如果本模块返回失败的话可以忽略。

	optional 可选条件	表明本模块是可选的，它的成功与否一般不会对身份认证起关键作用，其返回值一般被忽略。

	include 包含		后面是个文件


	[root@localhost ~]# cat /etc/pam.d/system-auth
	#%PAM-1.0
	# This file is auto-generated.
	# User changes will be destroyed the next time authconfig is run.
	auth 		required 		pam_env.so
	auth 		sufficient 	pam_unix.so nullok try_first_pass
	auth 		requisite 	pam_succeed_if.so uid >= 500 quiet
	auth 		required 		pam_deny.so
	account 	required 		pam_unix.so
	account 	sufficient 	pam_succeed_if.so uid < 500 quiet
	account 	required 		pam_permit.so
	password 	requisite 	pam_cracklib.so try_first_pass retry=3
	password 	sufficient 	pam_unix.so md5 shadow nullok try_first_pass use_authtok
	password 	required 		pam_deny.so
	session 	optional 		pam_keyinit.so revoke
	session 	required 		pam_limits.so
	session 	[success=1 default=ignore] 	pam_succeed_if.so service in crond quiet use_uid
	session 	required 		pam_unix.so
	


	*pam_securetty.so
	pam_securetty is a PAM module that allows root logins only if the user is logging in on a "secure" tty, as defined by the listing in /etc/securetty.
	pam_securetty root可以登录的tty
	[root@localhost ~]# cat /etc/securetty
	删除tty3看是否root还能在tty3登录


	*pam_env.so 登录时选择是否设置环境变量
	The pam_env PAM module allows the (un)setting of environment variables.
	/etc/security/pam_env.conf



	*pam_unix.so 系统中核心的一个pam模块 专门验证下面2个文件 验证用户名密码/etc/passwd,/etc/shadow
	[root@localhost ~]# vim /etc/pam.d/system-auth
	account 	required 	pam_unix.so
	account 	sufficient 	pam_succeed_if.so uid < 500 quiet
	account 	required 	pam_permit.so
	auth 	pam_unix.so 验证用户名密码
	account pam_unix.so 验证用户过期



	*pam_seccesed_if.so uid < 500
	uid < 500 立刻成功


	*pam_permit.so 永远返回成功

	*pam_nologin.so
	pam_nologin is a PAM module that prevents users from logging into the system when /etc/nologin exists. The contents of the /etc/nologin file are displayed to the user.
	/etc/nologin 这个文件只要存在，非root用户不能登录系统，但是已经登录的没有影响。在这个文件中还可以随意写一些信息，root登录是可以看到
	