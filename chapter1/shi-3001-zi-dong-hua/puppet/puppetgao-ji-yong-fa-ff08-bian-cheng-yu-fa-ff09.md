###一、资源引用

	Type['title']
	例如：Package['nginx']
###二、元参数

	用于定义资源间的依赖关系，及应用次序，通知机制
	特殊属性：
	require:                            #依赖
		package{'nginx'：
			ensure => present,
		}
		
		service{'nginx':
			ensure => true,
			enable => true,
			require => Package['nginx'],
		}
	
	before:                  
	      package {'nginx':
	           ensure  =>  present,
	           before  =>  Service['nginx'],
	      }
	
	      service {'nginx':
	           ensure  => true,
	           enable  => true, 
	       }
	notify和subscribe： #通知和订阅
	                package {'nginx':
	                    ensure  =>  present,
	                } ->
	
	                service {'nginx':
	                    ensure  => true,
	                    enable  => true, 
	                    restart => '/etc/rc.d/init.d/nginx reload'
	                }
	
	
	
	            package{'nginx': } ->      #依赖链
	           file {'nginx.conf':} ~>      #通知链
	            service {'nginx':} 
	
	            Package['nginx'] -> File['nginx.conf'] ~> Service['nginx']
###三、变量

	变量支持的类型：字符型、数值型、数组、布尔型、undef、hash、正则表达式
	自定义变量:
	
	$pkgname='haproxy'
	package { $pkgname:
		ensure => present,
	}
	
	facter变量:查看 facter
	
	file {'/etc/issue.test':
	        ensure => file,
	        content => $operatingsystem,
	}
	
	内置变量:
	
	                agent: $environment, $clientcert, $clientversion
	                masger: $serverip, $servername, $serverversion
	
	变量作用域：
		puppet模块：
			模块A：
				$test=hello
			模块B:
###四、条件表达式

	单分支、双分支、多分支
	If {
	}elsif{
	
	}else
	}
###五、函数

	if $operatingsystem =~ /^(?i-mx:(centos|redhat))/ {
		notice("Welcome to $1 linux,")
	}
###六、case语句

	    case 和 selector实现同一种功能的示例:
	
	        $webserver = $operatingsystem ? {
	                /^(?i-mx:centos|fedora|redhat)/ => 'httpd',
	                /^(?i-mx:ubuntu|debian)/        => 'apache2',
	        }
	        $webprovider = $operatingsystem ? {
	                /^(?i-mx:centos|fedora|redhat)/ => 'yum',
	                /^(?i-mx:ubuntu|debian)/        => 'apt',
	        }
	
	        package {"$webserver":
	                ensure  => present,
	                provider => $webprovider,
	        }
	
	
	        case $operatingsystem {
	            /^(?i-mx:redhat|centos|fedora)/: { package {'httpd': ensure => present, provider => yum, } }
	            /^(?i-mx:ubuntu|debian)/: { package {'apache2': ensure => present, provider => apt, } }
	            default: { notify {'notice': message => "unknown system.", }}
	        }
