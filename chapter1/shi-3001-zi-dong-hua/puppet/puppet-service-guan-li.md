###一、启动一个服务
	service{'nginx':
		ensure => true,
		enable => true,
	}