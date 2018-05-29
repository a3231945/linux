###一、用户类型
**1、Linux 的单用户多任务**

	一个用户登入，执行多个任务。比如你使用电脑，聊着QQ，听着音乐。


**2、Linux 的多用户多任务**
	
	apache用户提供web服务
	root用户操作系统，互不影响
	
**3、Linux 系统用户角色划分**
	
	用户在系统中是分角色的，在Linux系统中，由于角色不同，权限和所完成的任务也不同。值得注意的是用户的角色是通过UID和GID 识别的。特别是UID，在运维工作中，一个UID是唯一标识一个系统用户的账号。

			
- 超级用户：

	默认是root用户，其UID和GID 均为O，在每台unix/linux操作系统中都是唯一且真实存在的，通过它可以登录系统。可以操作系统中任何文件。拥有最高的管理权限。在生产环境中，一般会禁止root账号远程SSH连接服务器，以加强系统安全。


- 普通用户：

	这类用户一般是由具备系统管理员root的权限的运维人员添加的

- 虚拟用户：

	与真实普通用户区分来，这类用户最大的特点是安装系统后默认就会存在，且默认情况不能登录系统，他们是系统正常运行不可缺少的，他们的存在主要是方便管理系统，满足相应的系统进程对文件属主的要求。如：bin adm nobody

_多用户系统从实际上来说使得系统更为方便。从安全角度来说，多用户系统也更为安全。_




###二、用户和用户组



**1、用户及用户组配置文件介绍**
		
	/etc/passwd,/etc/shadow,/etc/group,/etc/gshadow 四个文件。

**2、用户：/etc/passwd**

	文件中每一行定义一个用户账号。各内容之间又通过“：”划分为多个字段。
	（提示：passwd文件中有很多虚拟账号、如bin,daemon等），一般来说，这些账号是系统运行所需要的。在不确定的情况下，请不要随意删改此类账号。


	root:	x		:0		:0		:root		:/root		:/bin/bash
	账号名称	账号密码 	账号UID		账号GID 	用户说明	用户家目录	shell解释器


**3、UID字段限制说明：** 

	0为超级用户
	1-499 为系统保留使用的UID。防止人为建立账户的UID与系统的UID之间冲突
	500-65535 普通账户UID
	权限：644
	/etc/shadow
	权限：400，只有root可读
	同样用“：” 分为9个字段。
含义分别为：

	账号密码：最近更改密码的时间：禁止修改密码的天数：用户必须更改口令的天数：警告更改密码的天数：不活动时间：失效时间：标志

**4、用户组配置文件：**

	/etc/group
	/etc/gshadow
	/etc/group 字段含义
	用户组名：用户组密码：GID:用户组成员
	/etc/gshadow
	用户组名：用户组密码：用户组管理员账号：用户组成员

###三、用户管理

**1、用户与用户组管理基本命令总结：**

- **用户：**

	useradd 	同 adduser，执行命令可在系统中添加用户。
	passwd 		执行此命令可为用户设置密码。	
	usermod 	修改用户的命令，可以通过usermod来修改用户登录名，用户的家目录等等
	id		查看用户的uid,gid及所归属的用户组
	su		用户切换工具
	sudo 		sudo 是通过另一个用户来执行命令（execute a command as another user),su是用来切换用户，然后切换到的用户来完成相应的任务，但sudo能在命令后面直接接命令执行，比如 sudo ls /root/,不需要root 密码就可以执行只有root才能执行相应的命令或具备目录权限：这个权限需要通过visudo命令或直接编辑/etc/sudoers 来实现
	visudo		visodu配置sudo权限的编辑命令；也可以不用这个命令，直接用vi编辑/ect/sudoers实现。
	pwcov 		同步用户从 /etc/passwd 到 /etc/shadow.
	pwck		pwck是校验用户配置文件/etc/passwd 和/etc/shadow 文件内容是否合法或完整
	pwunconv 	是pwcov的逆向操作，是从/etc/shadow和/etc/passwd 创建 /etc/passwd.然后会删除 /etc/shadow文件
	chfn 		更改用户信息工具
	finger		查看用户信息工具
	sudoedit	和sudo功能差不多

- **用户组：**

	groupadd 	添加用户组
	groupdel	删除用户组
	groupmod	修改用户组信息
	groups		显示用户所属的用户组
	grpck	
	grpconv 	通过/etc/group 和/etc/gshadow/ 的文件内容来同步或创建 /etc/gshadow,如果/etc/gshadow 不存在则创建。
	grpunconv 	通过/etc/group 和/etc/gshadow 文件内容来同步或创建/etc/group ,然后删除gshadow文件。
	


**2、/etc/skel 目录**

	/etc/skel 目录是用来存放新用户配置文件的目录，当我们添加新用户时，这个目录下的所有文件会被自动复制到新添加的用户的家目录下；默认情况下，/etc/skel 目录下的所有文件都是隐藏文件（以点开头的文件）；通过修改，添加，删除/etc/skel目录下的文件，我们可为新创建的用户提供统一，标准的，初始化用户环境。


	[root@localhost skel]# ls -al
	总用量 24
	drwxr-xr-x.  3 root root 4096 6月  24 03:12 .
	drwxr-xr-x. 85 root root 4096 7月   2 03:43 ..
	-rw-r--r--.  1 root root   18 7月  18 2013 .bash_logout
	-rw-r--r--.  1 root root  176 7月  18 2013 .bash_profile
	-rw-r--r--.  1 root root  124 7月  18 2013 .bashrc
	drwxr-xr-x.  2 root root 4096 11月 12 2010 .gnome2

_当我们用useradd命令添加新用户的时候，Linux系统会自动复制/etc/skel/下的所有文件（包括隐藏文件）到新添加用户的家目录下。_


**实例一：**

	[root@localhost skel]# touch longge
	[root@localhost skel]# useradd longdidi
	
	[root@localhost longdidi]# ls /home/longdidi/ -al
	总用量 24
	drwx------. 3 longdidi longdidi 4096 7月   2 06:31 .
	drwxr-xr-x. 3 root     root     4096 7月   2 06:31 ..
	-rw-r--r--. 1 longdidi longdidi   18 7月  18 2013 .bash_logout
	-rw-r--r--. 1 longdidi longdidi  176 7月  18 2013 .bash_profile
	-rw-r--r--. 1 longdidi longdidi  124 7月  18 2013 .bashrc
	drwxr-xr-x. 2 longdidi longdidi 4096 11月 12 2010 .gnome2
	-rw-r--r--. 1 longdidi longdidi    0 7月   2 06:30 longge


**提示：**_上面所述功能，实际工作中可以考虑统一设置环境变量等，但一般中小公司，在生产环境一般不会随意改这个目录及内容。_
	
**3、/etc/login.defs 配置文件**

	/etc/login.defs 文件是用来定义创建用户时需要的一些用户的配置信息。如创建用户时，是否需要家目录，UID和GID的范围，用户及密码的有效期限等。
	

	[root@localhost longdidi]# egrep -v '^#|^$' /etc/login.defs 
	MAIL_DIR        /var/spool/mail				#创建用户时， 要在目录/var/spool/mail中创建一个用户mail文件
	PASS_MAX_DAYS   99999					#一个密码最长可以使用的天数；
	PASS_MIN_DAYS   0					#更换密码的最小天数；
	PASS_MIN_LEN    5					#密码的最小长度；
	PASS_WARN_AGE   7					#密码失效前提前多少天 警告
	UID_MIN                   500				#最小UID为500，也就是说添加用户的UID默认从500开始
	UID_MAX                 60000				#最大UID为60000，
	GID_MIN                   500				
	GID_MAX                 60000
	CREATE_HOME     yes					#是否创建家目录
	UMASK           077					#默认家目录的umask
	USERGROUPS_ENAB yes					#删除用户同时产出用户组
	ENCRYPT_METHOD SHA512 					#MD5 密码加密
	
	/etc/default/useradd 文件
	/etc/default/useradd 文件是在使用useradd添加用户时的一个需要调用的一个默认的配置文件，可以使用useradd -D参数，这样的命令格式来修改文件里面的内容。


	[root@localhost default]# egrep -v '^# | ^$' /etc/default/useradd 
	GROUP=100
	HOME=/home		#制定用户家目录的父目录
	INACTIVE=-1		#是否启用账号过期停权， -1 表示不启用
	EXPIRE=			#账号终止日期，不设置表示不启用
	SHELL=/bin/bash		#新用户默认所用的shell 类型
	SKEL=/etc/skel		#配置新用户家目录的默认文件存放路劲。前文提到的/etc/skel，就是配置在这里生效的，即当我们用useradd添加用户时，用户家目录下的文件，都是从这里配置的目录中复制过去的。
	CREATE_MAIL_SPOOL=yes	#创建mail文件
	提示：useradd -D 


**4、Linux用户管理命令**

	linux 是一个多用户多任务的操作系统，有着很丰富的用户管理工具，这些工具包括用户的查询，添加，修改，以及不同用户之间的互相切换等；通过这些工具，我们可以简单、方便、安全的进行用户管理工作。


**与用户管理相关的一些文件：**

	/etc/passwd 和 /etc/group	我们对Linux的系统用户和用户组进行添加，修改，删除的最终结果就是修改系统用户文件/etc/passwd 和/etc/shadows 以及用户组的 /etc/group 和文件/etc/gshadow.
	/etc/login.defs 和/etc/default/useradd 	/etc/login.defs文件是用来定义在创建用户时的一些默认配置。如创建用户时，是否需要家目录，UID和GID的范围，用户及密码的有效期限等等，。
	/etc/default/useradd 文件是在使用useradd 添加用户时需要调用的一个默认的配置文件，可以使用 useradd -D参数，这样的命令格式来修改文件里面的内容，当然也可以直接编辑修改。
		

**4.1添加用户命令useradd:**

	添加用户的命令有useradd和adduser ，这两个命令所能达到的效果是一样的。当然出了useradd和 adduser命令之外，我们还能通过修改用户配置文件/etc/passwd 和/etc/group及手动创建文件的办法来直接添加用户。
	useradd命令：当使用useradd命令不加参数选项，后面直接跟所添加的用户名时，系统首先会读取配置文件/etc/login.defs 和/etc/default/useradd 中所定义的参数或规则，根据设置的规则添加用户，同时会向/etc/passwd 和 /etc/group 文件内添加新建用户和用户组几记录。
	当然/etc/passwd和/etc/group的加密资讯文件/etc/shadows 和/etc/gshadow 也会同步生产记录，同时系统还会根据/etc/default/useradd文件中所配置的信息建立用户的家目录，并复制/etc/skel 中的所有文件（包括隐藏的环境配置文件）到新用户的家目录中。
	
	
- **useradd 参数详解：**

	-c comment 	新账号password档的说明栏
	-d home_dir 	新账号每次登入时所使用的home_dir。预设置位 login名称、
	-e expire_date	账号终止日期。
	-f inactive_days	账号过期后几日永久停权。当设置为0时账号则立刻被停权。当设置为-1时则关闭此功能，
	-g initial_group	group名称或以数字来做为用户登入起始用户组。用户组名须为系统现有存在的名称，
	-G group		定义此用户为多个不同groups的成员，每个用户组使用“，”都好分隔。


**4.2添加用户组命令groupadd**

	groupadd 命令有关的文件有：
		/etc/group 用户组相关文件    /etc/gshadow  用户组加密相关文件
	groupadd命令语法:
		groupadd [-g gid [-o] ] [ -r ] [ -f ] groupname	
	groupadd 参数选项：
		-g gid 	制定用户组GID值。除非接-o 参数（如：groupadd -g 1234 -o longge),否则ID值必须是唯一的数字（不能为负数）。如果不指定-g参数，则预设值会从500开始
		-r 	建立系统用户组，GID值会比/etc/login.defs 中定义的UID_MIN值小。
		-f	新增一个账户，强制覆盖一个已经存在的用户组账号，
	
	文件：	
		/etc/group 用户组相关文件
		/etc/gshadow 用户组加密相关文件
		
		提示：以上为用户组相关的文件，使用groupadd 命令的结果实际上就是更改维护这两个文件。
		相关命令: chfn  chsh   useradd userdel  usermod passwd  groupdel groupmod  

		groupadd命令实例:
			在生产环境中，一般增加用户组的用法都是非常简单的。
- **4.3用户密码相关命令passwd:**

普通用户和超级用户都可以运行passwd命令，但普通用户只能更改自身的用户密码。超级用户root则可以设置或修改所有用户的密码。
当直接 passwd 命令后面不接任何参数或用户名时，则表示修改当前登录用户的密码。
	
	passwd 参数选项：
	
	-k --keep-tokens   保留即将过期的用户在期满后仍能使用
	-d --delete		删除用户密码，仅能以root权限操作
	-l --lock		锁住用户无权更改密码，仅能通过root权限操作
	-u  --unlock		解除锁定
	-f  --force		强制操作；仅root权限才能操作
	-x  --maximum=DAYS	两次密码修改的最大天数，后面接数字；仅能root权限操作
	-n   --minimum=DAYS	两次密码修改的最小天数，后面接数字；仅能root权限操作
	-w   --warning=DAYS	在距多少天提醒用户修改密码:仅能root操作
	-i --inacive=DAYS	在密码过期后多少天，用户被禁掉，仅能root操作
	-S --status 		查询用户的密码状态，仅能root操作

**实例：**我们用--stdin  参数实现非交互式的批量设置或修改密码
	
	[root@localhost ~]# echo "123.com" | passwd  --stdin longdidi
	更改用户 longdidi 的密码 。
	passwd： 所有的身份验证令牌已经成功更新。

**4.4修改用户密码有效期限相关命令chage：**
	
	chage命令的用法很多，和passwd等命令功能也有不少是重复的。
	语法：
		chage [选项] 用户名
	-d --lastday	将最近一次密码设置时间设为“最近日期”
	-E --expiredate  将账户过期时间设为“过期日期”	
	-h	    	  显示帮助
	-i		  将因过期而失效的密码设为“失效密码”
	-l		  显示账户年龄信息
	-m		  将两次改变密码之间相距的最小天数设置为“最小天数”

**4.5删除用户相关命令userdel**
	
	相关文件：
	/etc/passwd 用户账号资料文件
	/etc/shadow 用户账户资讯加密文件
	/etc/group  用户组资讯文件
	userdel 语法：
		userdel [-r] 用户名
	groupdel
	chfn 
	
**4.6更改用户的shell类型 chsh**
	

**4.7用户信息修改相关命令usermod：**
	
	语法：
	usermod [-c comment] [-d home_dir [-m]] ..... 
	usermod 参数选项：
	-c 
	-d 更新用户新的家目录。如果给定—m选项，用户旧的家目录会搬到新的家目录去，
	-e 加上用户账号停止日期2
	-f 账号过期几日后永久停权
	....


finger  id

w,who,users,last,lastlog,groups


