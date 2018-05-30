###命令帮助
    [root@linuxmaster1poc ~]# puppet help
    Usage: puppet <subcommand> [options] <action> [options]
                                                  
    Available subcommands, from Puppet Faces:
      ca                Local Puppet Certificate Authority management. #管理本地证书
      catalog           Compile, save, view, and convert catalogs. #编译、保存、查看puppet代码，或转换成Catalogs
      certificate       Provide access to the CA for certificate management. #提供访问CA证书的管理
      certificate_request  Manage certificate requests. #管理证书的请求
      certificate_revocation_list  Manage the list of revoked certificates. #管理撤销证书的列表
      config            Interact with Puppet's configuration options. #配置现象
      facts             Retrieve and store facts. #系统信息检查
      file              Retrieve and store files in a filebucket #在filebucket中检索和存储文件
      help              Display Puppet help. #查看帮助
      instrumentation_data  Manage instrumentation listener accumulated data. #管理监听的数据
      instrumentation_listener  Manage instrumentation listeners. #管理监听的状态
      instrumentation_probe  Manage instrumentation probes. #管理监听探测
      key               Create, save, and remove certificate keys. #创建、保存、删除证书密钥
      man               Display Puppet manual pages. #查看手册
      module            Creates, installs and searches for modules on the Puppet Forge. #从Puppet Forge创建、安装、查询模块
      node              View and manage node definitions. #管理节点
      parser            Interact directly with the parser. #解析器管理，检查pp文件语法
      plugin            Interact with the Puppet plugin system. #插件管理
      report            Create, display, and submit reports. #创建、查看报告
      resource          API only: interact directly with resources via the RAL. #查看资源帮助
      resource_type     View classes, defined resource types, and nodes from all manifests. #查看类、默认资源类型与节点信息
      secret_agent      Mimics puppet agent. #模拟Agent
      status            View puppet server status. #查看puppet状态
                                                  
    Available applications, soon to be ported to Faces:
      agent             The puppet agent daemon #客户端进程，负责从Master端获取信息
      apply             Apply Puppet manifests locally #运行本地manifests
      cert              Manage certificates and requests #证书颁发，用于签署证书
      describe          Display help about resource types #资源帮助
      device            Manage remote network devices #管理远程网络设备
      doc               Generate Puppet documentation and references #生成puppet文档
      filebucket        Store and retrieve files in a filebucket #在filebucket中检索和存储文件
      inspect           Send an inspection report #发送report报告
      kick              Remotely control puppet agent #远程控制agent，远程触发puppet agent命令
      master            The puppet master daemon #编译配置文件、模板、节点的自定义插件
      queue             Queuing daemon for asynchronous storeconfigs #队列进程
                                                  
    See 'puppet help <subcommand> <action>' for help on a specific subcommand action.
    See 'puppet help <subcommand>' for help on a specific subcommand.
    Puppet v2.7.23


###一、常用命令

- `puppet master` #编译配置文件、模板、节点的自定义插件
- `puppet agent` #客户端进程，负责从Master获取数据
- `puppet cert` #证书颁发，用于签署证书
- `puppet kick` #远程控制agent，远程触发puppet agent命令
- `puppet apply` #运行本地manifests

###二、帮助：

- `puppet doc` #生成puppet文档
- `puppet help` #显示puppet帮助信息
- `puppet resource` #查看资源帮助
- `puppet describe` #资源帮助
- `puppet status` #查看puppet状态

###三、模块和不常用命令：

- `puppet module` #从puppet forge创建、安装、查询模块
- ` puppet device` #远程管理网络设备
-  `puppet inspect` #发送report报告
-  `puppet filebucket` #在filebucket中检索和存储文件
-  `puppet queue` #队列进程