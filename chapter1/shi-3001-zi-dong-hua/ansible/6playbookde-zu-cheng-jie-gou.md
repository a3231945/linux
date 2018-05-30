###一、playbook的组成结构

1、Invetory
2、Modules
3、Ad Hoc Commands
4、Playbooks

	Tasks:任务，即调用模块完成的某炒作
	Variables：变量
	Templates：模板
	Hadlers：处理器，由某时间触发执行的操作
	Roles：角色
	
###二、基本结构

	-hosts: weserver
	 remote_uesr: root
	 tasks:
	 - task1
	   modulename： module_args
	 - task2
	   modulename: module_args

	