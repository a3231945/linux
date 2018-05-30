###一、变量

**1、变量命名：** 变量名仅能由字母、数字和下划线组成，且只能以字母开头
**2、facts:** facts是由正在通信的远程目标主机发回的消息，这些信息被保存在ansible变量中，要获取指定
远程主机所支持的所有Facts，可以使用命令：#ansible hostname -m setup
**3、register：** 把任务的输出定义为变量，然后用于其他任务

	示例如下：
	{
		tasks:
		   - shell: /usr/bin/foo
			 register: foo_reslt
			 ignore_errors: True
	}
**4、通过命令传递变量**

在运行playbook的时候也可以传递一些变量供playbook使用，**示例如下：**

	ansible-playbook test.yml --extra-vars "hosts=www user=www"
	
**5、通过roles传递变量**

当给一个主机应用角色的时候可以传递变量，然后再角色内使用这些变量。

	示例{
		- hosts：webserver
		  roles:
		    - common
			- { role: foo_app_instance, dir: '/web/htdocs/a.com', port:8080 }
			
	}	
			
###二、Inventory
ansible 的主要功用在于批量主机操作，为了便捷地使用其中的部分主机，
可以在inventory　file 中将其分组命名，默认的inventory file 为/etc/ansible/hosts

inventory file 可以用多个，且可以通过Dynamic Inventory来动态生成。

**1、inventory文件格式**

	inventory文件遵循INI 文件风格，中括号中的字符为组名，可以将同一个主机归并到多个不同的组中：
	此外，当如若目标主机使用了非默认的ssh端口，还可以在主机名称之后使用冒号加端口来标明。
	ntp.longge.com
	[webserver]
	www1.longge.com:2222
	www2.longge.com
	[dbserver]
	db1.longge.com
	db2.longge.com
	db3.longge.com
	
	如果主机名称遵循相似的命名模式，还可以使用列表的方式标识各主机，例如：
	[webserver]
	www[01:50].longge.com
	[dberver]
	db-[a:f].longge.com
	

**2、主机变量**

	[webserver]
	www1.longge.com http_port=80 maxRequestsPerchild=808
	www2.longge.com http_port=8080 maxRequestsPerchild=909

**3、组变量**

组变量是赋予给制定组内所有主机上的在Playbook 中可用的变量

	实例
		[webserver]
		www1.longge.com
		www2.longge.com
		
		[webservers:vars]
		ntp_server=ntp.longge.com
		nfs_server=nfs.longge.com
	

**4、组嵌套**

inventory中，组还可以包含其他的组，并且也可以向组中的主机指定变量，不过，这些变量只能在ansible-playbook中使用，而ansible不支持。
	
	例如：			
		[apache]
		http1.longge.com
		http2.longge.com
		
		[nginx]
		nginx1.longge.com
		nginx2.longge.com
		
		[webserver:children]
		apache
		nginx
		
		[webserver:vars]
		ntp_server=ntp.longge.com
	

**5、inventory参数**
ansible基于ssh连接inventory中制定的远程主时，还可以通过参数制定其交互方式：

	这些参数如下所示：

	ansible_ssh_host
		  将要连接的远程主机名.与你想要设定的主机的别名不同的话,可通过此变量设置.
		  
	ansible_ssh_port
		  ssh端口号.如果不是默认的端口号,通过此变量设置.
		  
	ansible_ssh_user
		  默认的 ssh 用户名

	ansible_ssh_pass
		  ssh 密码(这种方式并不安全,我们强烈建议使用 --ask-pass 或 SSH 密钥)

	ansible_sudo_pass
		  sudo 密码(这种方式并不安全,我们强烈建议使用 --ask-sudo-pass)

	ansible_sudo_exe (new in version 1.8)
		  sudo 命令路径(适用于1.8及以上版本)

	ansible_connection
		  与主机的连接类型.比如:local, ssh 或者 paramiko. Ansible 1.2 以前默认使用 paramiko.1.2 以后默认使用 'smart','smart' 方式会根据是否支持 ControlPersist, 来判断'ssh' 方式是否可行.

	ansible_ssh_private_key_file
		  ssh 使用的私钥文件.适用于有多个密钥,而你不想使用 SSH 代理的情况.

	ansible_shell_type
		  目标系统的shell类型.默认情况下,命令的执行使用 'sh' 语法,可设置为 'csh' 或 'fish'.

	ansible_python_interpreter
		  目标主机的 python 路径.适用于的情况: 系统中有多个 Python, 或者命令路径不是"/usr/bin/python",比如  \*BSD, 或者 /usr/bin/python
		  不是 2.X 版本的 Python.我们不使用 "/usr/bin/env" 机制,因为这要求远程用户的路径设置正确,且要求 "python" 可执行程序名不可为 python以外的名字(实际有可能名为python26).

		  与 ansible_python_interpreter 的工作方式相同,可设定如 ruby 或 perl 的路径....
			
		
	
###三、条件测试
如果需要根据变量，facts或此前任务的执行结果来为某task执行是否的前提时要用到条件测试
**1、when语句**

	when:
		实例：
		- hosts：all
		  remote_user: root
		  vars:
		  - username: user0
		  tasks:
		  - name: create {{username}} user
			user: name={{username}}
			when: ansible_fqdn == "node1.longge.com"
			
			
		
