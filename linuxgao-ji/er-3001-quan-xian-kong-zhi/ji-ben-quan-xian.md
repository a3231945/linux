###1、 权限 ugo ( r - w - x )
当一个用户访问一个文件的时候

- 如果当前用户的UID与文件owner的UID匹配，那么就遵守第一个三位组的权限，如果不匹配那么看GID
- 如果当前用户的GID与文件group的GID匹配，那么就遵守第二个三位组的权限
- 如果当前用户的UID与GID和文件的owner与group都不匹配，那么就看第三个三位组other的权限
了解到匹配权限的过程之后，先面我们介绍一个三位组中rwx是什么意思


		文件		目录
	r	浏览（cat）	浏览（ls）
	w	编辑（vim）	创建/删除（touch，rm）
	x	执行（script）	改变（cd）
	-	无		无


###2、 默认权限与umask
默认情况下管理员的umask 022，普通用户的umask 002.
下面我们就以文件默认权限是666，umask是033，来讲解一下最后我们得到一个文件的权限。
- 666的权限是rw- rw- rw-，换算成2进制 110 110 110
- umask033 权限是 --- -wx -wx ，换算成2进制 000 011 011
- umask 2进制取反 000 011 011 -------->111 100 100
- umask 取反后的2进制和默认权限做与运算


	111 100 100	umask
	110 110 110	默认权限	
	-----------
	110 100 100
	rw- r-- r--
		
- 得到最终的权限 644


###3、SUID SGID Sticky

**可执行文件(SUID SGID):**

	执行可执行文件的时候，不以执行者的身份去运行，而是以文件所属人（组）的身份去执行
**目录 (SGID Sticky):**

	SGID：当一个A目录具有SGID的时候，在这个目录下新建的文件文件夹默认所属组会是A目录的所属组
	Sticky：当A目录具有Sticky位的时候，在A目录下的文件只有文件的所属人才能够删除文件，其他人哪怕对A目录有写的权限也不能删。




###4、ext3文件系统属性

	chattr lsattr
	a 	设定此属性的档案只可以以附加模式 (append mode) 开启。
	i 	设定此属性的档案不可以被任何使用者 (包括超级使用者 root) 改变内容、删除、改变名称。


###5、文件系统的访问控制列表

	setfacl -m (--modify) u:user:permission file | directory
	setfacl -m (--modify) u:group:permission file | directory
	setfacl -m (--modify) d:u:user:permission directory