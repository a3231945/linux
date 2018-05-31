###一、安装软件包

    yum -y install createrepo
    
###二、创建yum源
  
    #准备好下载的包及目录
    
    createrepo  ./
    
    #查看版本信息
    createrepo -v ./
    
    #更新yum 源
    createrepo --update ./
    
    
    #配置yum 配置
    vim /etc/yum.repos.d/local_base.repo
    name=local yum 
    baseurl=file:////DIR
    gpgcheck=0
    enabled=1
    priority=1 
 
 ###三、网络yum源
   
    安装web服务器-指定目录
       