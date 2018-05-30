###一、安装软件包

    vim /etc/puppet/manifests/site.pp 
    package { 'httpd':
        provider => yum,
        ensure   => installed,
    }
###二、指定版本

    vim /etc/puppet/manifests/site.pp 
    package { 'httpd':
        provider => yum,
        ensure   => latest,
    }
###三、rpm安装

    vim /etc/puppet/manifests/site.pp
    package { 'epel-release':
        provider => rpm,
        ensure   => installed,
        source   => 'http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm',
    }
###四、卸载包

    vim /etc/puppet/manifests/site.pp
    package { 'httpd':
        provider => yum,
        ensure   => purged,
    
    }