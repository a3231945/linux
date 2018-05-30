###一、添加任务计划

        vim /etc/puppet/mainfests/site.pp
        cron { 'ntp update':
                ensure => present,
                command => "ntpupdate ntp.time.com",
                user    => "root",
                minute  => "0",
                hour    => "0",
                monthday => "*",
                month    => '*',
                weekday  => '*',
        }
###二、删除任务计划

        vim /etc/puppet/mainfests/site.pp
        cron { 'ntp update':
                ensure => absent,
                command => "ntpupdate ntp.time.com",
                user    => "root",
                minute  => "0",
                hour    => "0",
                monthday => "*",
                month    => '*',
                weekday  => '*',
        }
        
