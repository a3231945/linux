###一、    Puppet exec简介

Puppet通过exec来执行外部的命令或者脚本，一般来讲是shell脚本   。这里面就涉及到一个重复执行的问题，因为默认的agent一连接上来就会自动执行对应的命令或者脚本。如果脚本重复执行对系统没影响的还无所谓，如果会对系统造成影响呢？一个有用的方法是使用像creates参数检查来避免运行命令，除非达到了某个条件才会指定。比如执行之前判断文件是否存在等等。你可以使用refreshonly参数限制一个exec只有收到某个事件才执行。

###二、    Puppet exec参数介绍
**1、command**

        指定要执行的命令。如果忽略，这个参数的值默认为资源的标题。必须填写命令的完整路径或者提供这个命令的查找路径。假如命令执行成功，执行过程的输出将会记录到普通(normal)日志中,但是如果命令执行失败，任何的输出都会记录到错误日志中。
**2、creates**

        命令创建的一个文件。加入这个参数设置的话，只有这个文件不存在的时候命令才会执行，比如下面的例子：
        exec { "tar -xf /Volumes/nfs02/important.tar":
               cwd     => "/var/tmp",
               creates => "/var/tmp/myfile",
                path    => ["/usr/bin", "/usr/sbin"]
        }
这例子中，只有/var/tmp/myfile文件不存在的时候，tar -xf /Volumes/nfs02/important.tar命令才会执行。
**3、cwd**

        命令执行的路径。假如目录不存在，命令执行将会失败。
**4、environment**

        为命令设置附加的环境变量。加入你用这个参数设置PATH，那么将会把path参数的值覆盖。多个环境变量需要使用数组指定。
**5、group**

        执行命令运行的用户组。这个看起来在各个平台运行结果不确定，这是平台的问题不是ruby或者puppet的问题。
**6、logoutput**

        是否记录输出信息。默认会根据exec资源的日志等级来记录输出信息，使用on_failure只有当命令执行出错的时候才会记录输出信息。值可以为true、fales、on_failure和任何合法的日志等级。
**7、onlyif**

        只有onlyif指定命令执行返回为0的时候，命令才会执行。比如下面的例子：
        exec { "logrotate":
          path   => "/usr/bin:/usr/sbin:/bin",    
          onlyif => "test `du /var/log/messages | cut -f1` -gt 100000"
        }
        只有当test `du /var/log/messages | cut -f1` -gt 100000执行的结果返回为0，也就是执行成功才与执行logrotate命令。Onlyif中指定的命令加入没有设置path的话要指定完整路径。Onlyif也可以将数组作为他的值，比如：
        onlyif => ["test -f /tmp/file1", "test -f /tmp/file2"]
        只有当数组中的所有命令都返回true的时候才会执行命令。
**8、path**

        命令执行搜索的路径。如果没有指定path，命令需要填写完整的路径。路径可以指定为一个数组并通过冒号分隔。
**9、provider**

        exec资源的特定后台。一般很少需要指定这个，puppet一般会自动为你的平台发现合适的provider，可用的provider如下：
        Ø  posix ---- 不通过shell解释或者执行如何插值，直接执行外部的二进制文件，这是一个执行大多数命令都更安全更确定的方式。但是阻止通配符和shell内置插件的使用(包括像for这样的控制逻辑和if语句)
        Ø  shell ---- 通过/bin/sh来解释提供的命令，只在POSIX系统中可用。致个模式允许使用通配符和shell内置插件，并且不需要为命令的路径指定完整的路径。虽然shell比posix更方便，但是同时也意味着在转译方面更小心。这个shell的exec类型在puppet 0.25.x.版本提供。
        Ø  windows ---- 在windows系统上执行外部的二进制文件。和posix模式一样，这个模式通过给定的参数直接调用命令，不通过shell解释或者执行如何插值。
**10、refresh**

        如何更新命令。默认的当exec收到其他的资源的一个事件时会重新执行。但是这个参数允许你定义更新不同的命令。
**11、refreshonly**

        作为一个更新机制当一个依赖的对象改变的时候命令才会执行。当这个命令依赖其他对象的时候这个选项才会有意义。当需要出发某个行为的时候会非常有用。比如下面的例子：
        # Pull down the main aliases file
        file { "/etc/aliases":
          source => "puppet://server/module/aliases"
        }
        # Rebuild the database, but only when the file changes
        exec { newaliases:
          path        => ["/usr/bin", "/usr/sbin"],
          subscribe   => File["/etc/aliases"],
          refreshonly => true
        }
        只有当/etc/aliases文件改变的时候才会触发从master中获取aliases文件到/etc/ aliases。
        备注：只有subscribe和notify才可以触发行为，而不是require，所以只有使用refreshonly和subscribe或notify一起使用的时候有意义。可用的值为true或false。
**12、returns**

        指定预期的返回代码。假如执行的命令返回其他的代码那么将会出现错误。默认是0，可以指定一个单一的值也可以指定一个包含多个值的数组。
**13、timeout**

        指定命令运行的最长时间。假如命令执行的时间查过timeout设定的时间，那么就会认为命令执行失败了并且会停止该命令。设置为0表示没有执行时间限制。时间以秒为单位。
**14、tries**

        命令执行重试次数，默认为1。设置这个参数命令将会重试你设置的次数直到合理的代码返回。
**15、try_sleep**

        设置命令重试的间隔时间，单位是秒。
**16、unless**

        加入这个参数设置的话，然后exec会执行，除非unless的命令返回为0，例：
        exec { "/bin/echo root >> /usr/lib/cron/cron.allow":
          path   => "/usr/bin:/usr/sbin:/bin",
          unless => "grep root /usr/lib/cron/cron.allow 2>/dev/null"
        }
这段代码将会将root添加到/usr/lib/cron/cron.allow文件中，除非grep发现root已经在那个文件里了。
**17、user**
        
        指定执行命令的用户。
        备注：加入你使用这个参数，任何错误输出不会在当下扑捉。这是源自ruby内部的一个bug。
        
###三、    Puppet exec实战
**1、如果不存在就创建一个空文件，代码如下：**

        exec { "/tmp/test1":
                 command => "touch /tmp/test1",
                 creates => "/tmp/test1"
        }
        
        这里没有指定path，命令又没有写完整路径，因此报如下错误，解决办法是加上path或者为touch写完整路径。
        
        err: Failed to apply catalog: 'touch /tmp/test1' is not qualified and no path was specified. Please qualify the command or specify a path.
        
        更改后代码为：
        exec { "/tmp/test1":
                command => "/bin/touch /tmp/test1",
                creates => "/tmp/test1"
        }
        或者：
        exec { "/tmp/test1":
                command => "touch /tmp/test1",
                path   => "/usr/bin:/usr/sbin:/bin:/sbin",
                creates => "/tmp/test1"
        }
        为了方便可以设置一个默认的path，这样就不会每次都要写path了。
        exec { path => [ "/bin/", "/sbin/" , "/usr/bin/", "/usr/sbin/" ] }
**2、自动安装nagios客户端的完整示例，代码如下：**

        node 'node1.zhang.com'{
        #将nagios-plugins和nrpe打包，复制到agent上
        file {"/data/software/install_nrpe.tar.gz":
                source => "puppet://puppet.zhang.com/files/install_nrpe.tar.gz";
        }
        #解压源码包
        exec { "unzip package":
                command => "tar xzvf /data/software/install_nrpe.tar.gz",
                path   => "/usr/bin:/usr/sbin:/bin:/sbin",
                creates => "/data/software/install_nrpe/nrpe.cfg";
        }
         
        #执行安装脚本
        exec { "install package":
                command => "/data/software/install_nrpe/install_nrpe.sh",
                path   => "/usr/bin:/usr/sbin:/bin:/sbin",
                logoutput => "true",
                creates => "/usr/local/nagios";
        }
        }
**3、满足某个条件的时候执行exec，代码如下：**

        #当result.log超过1K的就删除该文件
        exec { "clear comment":
                command => "rm -f  /tmp/result.log",
                path   => "/usr/bin:/usr/sbin:/bin:/sbin",
                onlyif => "test `du /tmp/result.log | cut -f1 ` -gt 1024"
        }
        奇怪的是我将cut换成awk的时候及时达到条件也不会删除该文件，而且没有任何报错:
        exec { "clear comment":
                command => "rm -f  /tmp/result.log",
                path   => "/usr/bin:/usr/sbin:/bin:/sbin",
                onlyif => "test `du /tmp/result.log |awk '{print $1}' ` -gt 1024"
        }
**4、当资源有依赖的时候，如果某个文件发生变更，exec就执行对应的命令，下面是一个ftp虚拟帐号管理的触发条件，代码如下：**

        #设置一个公共的path
        Exec { path => [ "/bin/", "/sbin/" , "/usr/bin/", "/usr/sbin/" ] }
        
        #设置vftpuser.txt资源
        file {"/etc/vsftpd/vftpuser.txt":
                source => "puppet://puppet.zhang.com/files/vftpuser.txt",
                backup => ".bak_$uptime_seconds",
        }
        
        #当vftpuser.txt发生变更的时候进行更ftp的账户信息
        exec { "new vftpuser.db":
                command => "db_load -T -t hash -f /etc/vsftpd/vftpuser.txt  /etc/vsftpd/vftpuser.db",
                subscribe => File["/etc/vsftpd/vftpuser.txt"],
                 refreshonly => true,
}
###四、    参考链接

http://docs.puppetlabs.com/references/latest/type.html#exec
http://www.mysqlops.com/2011/09/08/puppet-%E8%BF%90%E7%BB%B4%E8%87%AA%E5%8A%A8%E5%8C%96%E4%B9%8Bexec%E8%B5%84%E6%BA%90%E7%AE%A1%E7%90%86.html
