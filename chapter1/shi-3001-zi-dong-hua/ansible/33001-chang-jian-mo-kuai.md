###一、常见模块

**1、command: **命令模块，默认模块，用于在远程主机执行命令（缺陷：执行命令不能使用变量和参数）

	例：	ansible all -a 'date'
**2、cron:**

			state:
			present:安装
			absent:移除
			例：	使用ansible 添加任务计划 */10 * * * * /bin/echo hello	
			添加：
			ansible all -m cron -a 'minute="*/10" job="/bin/echo hello" name="test job" state=present'
			检查：
			ansible all -a 'crontab -l'
			移除：
			ansible all -m cron -a 'minute="*/10" job="/bin/echo hello" name="test job" state=absent'

**3、user:**

			name=:指定用户名
			
			添加用户
			ansible all -m user -a 'name="user1" state=present'
			
			删除用户
			ansible all -m user -a 'name="user1" state=absent'
**4、group:**

			添加用户组
			ansible all -m group -a 'name="mysql" gid=306 system=yes'
**5、copy:**

			src= :定义本地源文件
			dest=:定义目标路劲（绝对路劲）
			content= :取代src=,表示用指定的内容生成为目标文件的内容，不能与src同时使用
			拷贝本地的/etc/fstab 到远程的 /tmp 下权限为640 属主为root
			ansible all -m copy -a 'src=/etc/fstab /dest=/tmp/fstab.ansible owner=root mode=640'
			
			拷贝内容为“hello longge”到远程主机
			ansible all -m copy -a 'content="hello longge" dest=/tmp/test.ansible'
			
**6、file:**

			设置文件属性
			ansible all -m file -a 'owner=mysql group=mysql mode=644 path=/tmp/fstab.ansible'
			设置链接 
				src:  源文件的路径
				path：表示符号链接的文件路径
			ansible all -m file -a 'src=/tmp/fstab.ansible path=/tmp/link state=link'
			
**7、ping:**

			测试远程主机的连通性
			ansible all -m ping
**8、service:**

			服务管理
			ansible all -m service -a 'enabled=true name=httpd state=started'
**9、shell:**

			与command 类似，可以执行带管道和变量的命令
**10、script:**

			将本地脚本在远程主机运行
			ansible all -m script -a '/tmp/test.sh'
**11、yum:**

			安装软件包
			ansible all -m yum -a 'name=zsh'
			卸载
			ansible all -m yum -a 'name=zsh state=absent'
**12、setup:**

			收集远程主机的Facts(每个被管理节点在接收运行管理命令之前，会将自己主机相关信息，如操作系统，IP等信息传递给ansible主机)
			ansible all -m setup
