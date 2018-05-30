###一、外部引用
	vim  node.pp
	
	import nginx.pp
	import tomcat.pp
	import mysql.pp
###二、模块

nginx服务：

	nginx.pp             	依赖于外部资源：文件、模板文件（生成适用于目标节点的文件），为了实现某种完备功能而组织成一个独立的，自我包含的目录结构：模块

             模块：目录结构，目录名称即为模块名
                    /tmp/modules/
                        nginx/
                             /
                                files/     : 文件存储目录
                                manifests/ : 清单存储目录
                                templates/ : 模板存储目录
                                lib/       : ruby插件存储目录，用于实现一些自定义的功能
             示例：
                    /tmp/modules/
                        nginx/
                             /
                                files/     : 文件存储目录
                                    nginx.conf
                                manifests/ : 清单存储目录
                                    init.pp
                                        必须包含且只能包含一个与模块同名的类
                                    nginx.pp
                                        每个清单文件通常只包含一个类
                                    ...
                                templates/ : 模板存储目录 
                                    *.erb    
###三、类
Class是用于通用目标或目的的一组资源，因此，它是命名的代码块，在某位置创建之后可在puppet全局使用，类似于其他编程语言中的类的功能，puppet的类可以继承，也可以包含子类，定义类的语法如下所示：
	
	class my_class{
		…puppet code …
	}
	例如下面定义了一个名为apache的类，其包含了两个资源，一个是package类型的httpd,另一个是service类型的httpd
	
	class apache{
		package{'httpd':
			ensure => installed,
		}
		file {'htpd.conf':
			path => '/etc/httpd/conf/httpd.conf',
			ensure => file,
			require => Package['httpd'],
		}
		service {'httpd':
			ensure => running,
			require => Package['httpd'],
			subscribe => File['httpd.conf']
		
		}
			
	}
	include apache #调用类
	class {'apache':} #调用类
	
	带参数调用
	
	$webserver = $operatingsystem ? {
		/^(?i-mx:redhat|centos|fedora)/ => 'httpd',
		/^(?i-mx:ubuntu|debian)/ => 'apache2',
	
	}
	$webprovider = $operatingsystem ?{
		/^(?i-mx:centos|fedora|redhat)/ => 'yum',
		/^(?i-mx:ubuntu|debian)/ => 'apt',
	}
	class apache($pkgname = 'apache2',$provider='apt'){
		package{"$pkgname":
			ensure => present,
			provider => $provider,
		}
		service {"$pkgname":
			ensure => running,
			require => Package["$pkgname"],
		
		}
			
	}
	#include apache
	class {'apache':
		pkgname => $webserver,
		provider => $webprovider,		
	}
	
	类继承：
		class C_NAME inherits PARENT_CLASS_NAME{
		
		}
		子类命名方式： nginx::rpoxy
		
	基类：安装 nginx
	子类1：提供web配置的配置文件
	子类2：提供反向代理专用的配置文件
	vim nginx.pp
	
	class nginx {
		package {"nginx":
			ensure => present,
		}
	}
	
	class nginx::rproxy inherits nginx {
		file {'/etc/nginx/nginx.conf':
			ensure => file,
			source => "/tmp/nginx/nginx.reverse_proxy.conf",
			require => Package['nginx'],
			force  => true,
			notify => Service['nginx'],
		}
		service { 'nginx':
			ensure => true,
		}
	
	}
	class nginx::web inherits nginx {
		file {'/etc/nginx/nginx.conf':
			ensure => file,
			source => "/tmp/nginx/nginx.reverse_web.conf",
			require => Package['nginx'],
			force  => true,
			notify => Service['nginx'],
		}
		service { 'nginx':
			ensure => true,
		}
	
	}
	
	vim node.pp
	import "/tmp/class3.pp"
	include nginx::web
###四、模块

	cd /etc/puppet/module/ && mkdir nginx
	mkdir -pv nginx/{mainfests,files,templates,lib}
	cd mainfests
	vim init.pp
	class nginx {
		package {'nginx':
			ensure => installed,
		
		}
	}
	---------------
	vim nginx_web.pp
	------------------------
	vim nginx.proxy.pp
