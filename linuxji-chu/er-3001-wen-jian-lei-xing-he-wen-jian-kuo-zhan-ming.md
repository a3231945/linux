###一、Linux中文件类型
	在Linux系统中，可以说一切设备（包括目录，普通文件）皆为文件。文件类型包含有普通文件，目录，字符设备文件，块设备文件，符号链接文件等等
	查看 ls -al

	2-10字符描述 ugo权限
	第一个字符表示文件属性：
		d：表示目录
		-：表示普通文件
		l:表示是一个符号链接文件
		b,c：分别表示区块设备和其他的外围设备。
		s,p：这些文件关系到系统的数据结构和管道，通常很少见
		
###二、文件类型分别介绍：
**1、普通文件（regular file） :** 一般是相关的应用程序或系统命令创建，比如：touch cp tar 等工具

		删除方式： rm
		
**2、目录（directory）：** 带d 开头的文件表示目录。目录在Linux中是一个比较特殊的文件

		查看	ls -ld
		删除方式： rm  rmdir(删除空目录)	
		查看： ls -F 目录后面会多一个斜线
		ls -F /etc/ | grep '/'
		ls -l /etc/ | grep '^d'
**3、字符设备或块设备：** 带b或c开头的 c 表示字符设备 b表示块设备

		mknod 创建	
		rm  	删除
	
**4、套接口文件：**当我们启动mysql服务时，会产生一个mysql.sock文件。这个文件的属性的第一个字符是s，这类文件通常用在网络之间进行数据连接。	
		例如：
			mysql -uroot -ppass -S /data/3306/mysql.sock 这就是一个MySQL客户端程序连接服务器的命令

**5、符号链接文件：** l开头。l表示链接文件（和windows下的快捷方式相似）
		
		ln -s 源文件名 新文件名


	
###三、Linux中的文件扩展名
一般来说，Linux下文件是不许要扩展名。
Linux下扩展名的作用：为了兼容windows，同时，便于我们大多数习惯了windows用户区分文件的不同。我们还习惯通过扩展名来表示不同文件的类型。

	1)tar,tar.gz ,tgz,zip,tar.bz 表示压缩文件，此类文件创建命令一般为 tar gzip unzip 等
	2）sh  表示shell脚本文件，通过shell语言开发的程序
	3）pl  perl语言文件
	4) py  Python语言文件
	5) html htm php jsp do  表示网页语言的文件 
	6）conf	表示系统配置文件
	7) rpm 	表示rpm安装包
	



find  -type  文件类型   <查找>
