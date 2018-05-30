###一、基本结构

	ansible          #
	ansible-doc      #查看帮助
	ansible-playbook #执行playbook
	ansible-pull     #
	ansible-galaxy   #
	ansible-vault    #
	
###二、配置结构

	ansible.cfg      #配置文件
	hosts            #inventory配置文件
	role             #角色目录

	1、查看所有可以使用的模块   ansible-doc -l
	2、产看某个模块的帮组	  ansible-doc -s MODULE_NAME
	
###三、ansible命令应用基础

	语法：ansible <host-pattern> [-f forks] [-m module_name] [-a args]
		-f forks  :启动的并发线程数
		-m module :要使用的模块
		-a args   :模块特有的参数
		-k key    :指定密码
