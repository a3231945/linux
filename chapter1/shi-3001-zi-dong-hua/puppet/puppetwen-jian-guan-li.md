###一、直接sit.pp管理

    vim /etc/puppet/mainfests/site.pp  
    file { '/home/testfile.txt':
       ensure  => file,
       owner   => 'root',
       group   => 'root',
       mode    => 644,
       content => 'This is the puppet test file.',
    }

变量形式管理

       $contents = 'This is the test Puppet manifest.
       Sample contents
       Test contents
       ‘
    
       file { '/home/testfile.txt':
           ensure  => file,
           owner   => 'root',
           group   => 'root',
           mode    => 644,
           content => "$contents",
       }
###二、模板文件管理
 **修改文件管理配置文件**
 
     vim  /etc/puppet/fileserver.conf
     添加
     [files]
           path  /etc/puppet/files  #定义模板文件路径
           allow   *                #定义权限
       
**编辑主机配置文件**

    vim  /etc/puppet/mainfests/site.pp
    file { '/home/testfile2.txt':
        ensure => file,
        owner  => 'root',
        group  => 'root',
        mode   => 644,
        source => 'puppet://server.puppet.com/files/test.txt',
    }
**创建模板文件**

     mkdir  -p /etc/puppet/files
     echo "this is puppet test file " > /etc/puppet/files/test.txt

###三、软连接管理
- **创建连接**

     vim  /etc/puppet/mainfests/site.pp
     file { '/home/testfile.link':
    	ensure => link,
    	target  => '/home/testfile.txt',
    }
- **删除连接**

     vim  /etc/puppet/mainfests/site.pp
     file {'/home/testfie.link': ensure => absent }
###四、目录管理
**修改文件管理配置文件**

         添加
         [dirs]
               path  /etc/puppet/dirs  #定义模板文件路劲
               allow   *             #定义权限
           
**编辑主机配置文件**

        vim  /etc/puppet/mainfests/site.pp
        file { '/home/testdir':
            ensure => file,
            owner  => 'root',
            group  => 'root',
            mode   => 755,
            source => 'puppet://server.puppet.com/dirs/testdir',
        }
        
这里我在testdir中创建了一个文件，但是没有被推送

###五、参数详解
**1、backup参数**

    指定在文件内容替换之前进行备份操作，可以备份在本地，也可以集中备份。集中远程备份的话可以使用filebucket（我们在后面的实战部分会进行详细介绍），这个备份的时候如果备份在本地可以指定备份的文件名。
    
**2、content参数**

    指定文件的内容(字符串)，这个参数和source、target参数冲突。

**3、ensure参数**

    这个参数指定是否创建、删除文件或者目录，有present、absent、file、directory等值。其中present会检查文件是否存在，不存在就会创建一个空文件。absent会删除文件或者目录，如果是目录需要指定recurse参数指定是否允许递归。如果指定的是其他的参数，则会创建连接文件，为了方便管理，建议在创建的时候使用ensure => link,并通过target参数指定文件。注意不能在windows系统上链接文件，

**4、force参数**

    该参数强制执行文件操作，进行如下操作的时候必须指定force参数
    
    Ø  purge 子目录
    
    Ø  用文件或者链接文件替换目录
    
    Ø  使用ensure => absent参数删除目录

**5、group参数**

    指定文件或者目录的属组，可以是组名或者组id，如果是windows的话属组和属主不能相同。

**6、ignore参数**

    这个参数指定在递归期间对符合指定的模式的文件操作将被忽略。

**7、links参数**

    这个参数指定处理文件期间如何处理链接文件，可以设置follow和manage。在拷贝文件的时候，follow将会拷贝目标文件代替链接文件，manage将只会拷贝链接文件，ignore将会跳过。

**8、mode参数**

    这个参数用来指定文件或者目录的权限，puppet使用传统的unix权限方案，如果系统采用的权限方案不同的，puppet为这些系统将权限翻译成等价的权限，比如windows。这些权限可以是数字(r=4,w=2,x=1)也可以是字符(rwxst)。

**9、owner参数**

    指定文件的属主，可以是用户名或是用户id，如果是windows的话属组和属主不能相同。

**10、path参数**

    指定文件管理的路径。Windows路径也使用/而不是\。

**11、purge参数**

    这个参数会删除在master上不存在的文件，这个参数只有在管理目录的时候指定了recurse => true参数的时候才有意义。

**12、recurse参数**

    这个参数指定是否进行递归调用以及递归调用的深度，选项如下
    
    Ø  inf,true  ---在远程和本地都进行递归调用
    
    Ø  remote ---只在远程进行递归调用
    
    Ø  false ---不进行递归调用
    
    Ø  [0-9]+ ---和true参数一样，但是限制递归调用目录的深度

**13、source参数**

    该参数指定将会被拷贝到指定位置的资源文件，值可以是指定远程文件的URIS或者本地的完整路径。可以指定多个sorce，这个参数和content、target冲突。

**14、target参数**

    这个参数指定创建链接文件的目标文件或者目录。
    上面只是列了一些常用的选项，更多选项请参考：
    http://docs.puppetlabs.com/references/latest/type.html#file
