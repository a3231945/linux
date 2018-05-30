###一、 配置epel源
   
	yum installparamiko PyYAML jinja2 httplib2  #安装环境依赖包
	yum info ansible        #查看ansible安装包详细信息
	yum -y install ansible  #前提安装epel源
###二、客户端免秘钥：

	ssh-keygen -t rsa   #生成秘钥 
	ssh-copy-id -i /root/.ssh/id_rsa.pub USER@IP #拷贝秘钥到远程主机
