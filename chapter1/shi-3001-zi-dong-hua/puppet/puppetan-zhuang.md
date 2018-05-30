###一、server端安装

**1、安装puppet yum源**

    yum -y install http://yum.puppetlabs.com/el/6/products/x86_64/puppetlabs-release-6-7.noarch.rpm 
    sed -i -e "s/enabled=1/enabled=0/g" /etc/yum.repos.d/puppetlabs.repo 
    yum --enablerepo=puppetlabs-products,puppetlabs-deps -y install puppet-server 
   
**2、修改配置**

    vim /etc/sysconfig/puppetmaster 
    PUPPETMASTER_MANIFEST=/etc/puppet/manifests/site.pp   #打开注释
    PUPPETMASTER_LOG=syslog                               #记录日志
    touch /etc/puppet/manifests/site.pp                   #创建文件

**3、启动服务、开机启动**

    /etc/rc.d/init.d/puppetmaster start 
    chkconfig puppetmaster on 
 
###二、client安装

**1、安装puppet yum 源**

    yum -y install http://yum.puppetlabs.com/el/6/products/x86_64/puppetlabs-release-6-7.noarch.rpm 
    sed -i -e "s/enabled=1/enabled=0/g" /etc/yum.repos.d/puppetlabs.repo 
    yum --enablerepo=puppetlabs-products,puppetlabs-deps -y install puppet

**2、修改配置**
  
    vi /etc/sysconfig/puppet 
    PUPPET_SERVER=server.puppet.com         #指定server端
    PUPPET_LOG=/var/log/puppet/puppet.log   #记录日志
**3、启动服务、开机启动**

    /etc/rc.d/init.d/puppet start 
    chkconfig puppet on 
  
###三、注册
**Server 端：** 

    puppet cert list                        # 查看注册信息
    puppet cert sign   client1.puppet.com   # 同意注册
###四、测试
**server端：**

    vim /etc/puppet/manifests/site.pp        #编辑主机配置文件
      
    group { 'testgroup':
                  ensure => present,
                  gid    => 2000,
    }
    puppet apply /etc/puppet/manifests/site.pp #运行主配置文件
**client端：**

    /etc/init.d/puppet reload        #重新载入配置文件
    grep  testgroup /etc/group       #查看是否有testgroup组
