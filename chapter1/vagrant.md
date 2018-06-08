#Vagrant 使用手册

###一、安装vagrant

    wget https://releases.hashicorp.com/vagrant/2.1.1/vagrant_2.1.1_x86_64.rpm --no-check-certificate

    rpm -ivh vagrant_2.1.1_x86_64.rpm

###二、安装virtuabox 

    yum -y install kernel-devel

    wget https://download.virtualbox.org/virtualbox/5.2.12/VirtualBox-5.2-5.2.12_122591_el6-1.x86_64.rpm --no-check-certificate

    yum localinstall  VirtualBox-5.2-5.2.12_122591_el6-1.x86_64.rpm 

###三、配置虚拟机

    #添加镜像    
    vagrant box add NAME FILE

    #初始化Vagrantfile 文件
    vagrant init NAME

    #启动虚拟机
    vagrant up
    
    #登录虚拟机
    vagrant ssh
    or
    
    ssh -p 2222 127.0.0.1
    
    
###四、打包镜像
    
    #查看状态
    vagrant status

    #关闭虚拟机
    vagrant halt

    #打包镜像
    vagrant package NAME --output NAME.box --vagrantfile Vagrantfile

###五、Vagrantfile 配置文件详解

**1、box 设置**

    config.vm.box = "XXX"
    

**2、虚拟机名称设置**
    
    config.vm.hostname = "XXX"

    
**3、网络设置**
    
    #host-only网络
    config.vm.network "private_network", ip: "192.168.33.10"
    
    
    #桥接网络
    config.vm.network "public_network", ip: "192.168.160.200"
    

**4、虚拟机主机名设置**

    config.vm.hostname = "XXX"
    

**5、共享目录设置**
    
    config.vm.synced_folder "/tmp", "/vagrant_data"

**6、端口转发**

    config.vm.network "forwarded_port", guest: 80, host: 81

    
###六、虚拟机初始化脚本

- **方法一：**   

  
    config.vm.provision "shell", inline: <<-SHELL
       yum install -y httpd
    SHELL

- **方法二：**


    vim install_httpd.sh
    #!/usr/bin/env bash
    yum install -y httpd


    config.vm.provision "shell", path: "install_httpd.sh"
 

###七、初始化
**1、shell**

    略
**2、ansible**
    
    略

**3、salt**

    略




###附录：

**常用命令：**

    vagrant up          启动  
    vagrant halt        关闭
    vagrant reload      重启
    vagrant provision   重新加载
    vagrant suspend     挂起
    vagrant resume      恢复挂起
    vagrant destroy     删除
    vagrant status      查看状态


    vagrant global-status           查看环境
    vagrant global-status --prune   删除环境
    
    vagrant init            初始化
    vagrant ssh             ssh登录虚拟机
    vagrant box             镜像管理
    
       
               



**例子：创建3台web 服务器**

    # -*- mode: ruby -*-
    # vi: set ft=ruby :
    
    Vagrant.configure("2") do |config|
    
    	#demo1
    	config.vm.define :web1 do |web1|
    		config.vm.box = "centos65"
    		config.vm.hostname = "web1"
    		config.vm.network "forwarded_port", guest: 80, host: 81
    		config.vm.network "public_network", ip: "192.168.160.201"
    		config.vm.synced_folder "/tmp", "/vagrant_data"
    		config.vm.provider "virtualbox" do |v|
    			v.customize ["modifyvm", :id, "--name", "web1", "--memory", "512"]
    		end
    		config.vm.provision "shell", inline: <<-SHELL
    			yum install -y httpd
    		SHELL
    	end
    
    	config.vm.define :web2 do |web2|
    		config.vm.box = "centos65"
    		config.vm.hostname = "web2"
    		config.vm.network "forwarded_port", guest: 80, host: 82
    		config.vm.network "public_network", ip: "192.168.160.202"
    		config.vm.synced_folder "/tmp", "/vagrant_data"
    		config.vm.provider "virtualbox" do |v|
    			v.customize ["modifyvm", :id, "--name", "web2", "--memory", "512"]
    		end
    		config.vm.provision "shell", inline: <<-SHELL
    			yum install -y httpd
    		SHELL
    	end
    
    	config.vm.define :web3 do |web3|
    		config.vm.box = "centos65"
    		config.vm.hostname = "web3"
    		config.vm.network "forwarded_port", guest: 80, host: 83
    		config.vm.network "public_network", ip: "192.168.160.203"
    		config.vm.synced_folder "/tmp", "/vagrant_data"
    		config.vm.provider "virtualbox" do |v|
    			v.customize ["modifyvm", :id, "--name", "web3", "--memory", "512"]
    		end
    		config.vm.provision "shell", inline: <<-SHELL
    			yum install -y httpd
    		SHELL
    	
    	end
    end


**官方文档： ** [https://www.vagrantup.com/docs/index.html](https://www.vagrantup.com/docs/index.html "vagrant 官方文档")

