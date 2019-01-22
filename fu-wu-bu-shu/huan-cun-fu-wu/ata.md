# ATS(apache traffic server) 手册

###一、组件、机制

**1、Traffic server（TS） 的组成**

> traffic Server 缓存

-   TS 缓存包括一个高速的对象数据库，数据库更具URL和相关头部来索引对象，对于同一个对象可以缓存不同版本（如不同的编码、语言）。
-   当缓存空间满后，TS 会移除过期的数据
-   当磁盘出错时，TS 将不再使用该磁盘，转而使用剩下的磁盘。所有磁盘都出错时，TS将切换至proxy-only（只代理） 模式，不缓存
-   可分区（可以给指定的协议和源服务器划分一定数量的磁盘空间）


>RAM缓存

-   内存缓存区存储比较热门的对象，在流量的高峰期时能加快处理速度和降低磁盘负载

>主机数据库

- 存储DNS信息，方便主机名到IP地址的快速转换
- 存储每个主机的HTTP版本，方便高级协议特性的使用
- 存储主机的可靠性和可用性信息

> DNS 解析器

- TS原生实现了DNS解析器，不依赖较慢的传统解析库，同时也降低了DNS的流量

> Traffic Server 进程

- traffic_server 进程负责接受连接，处理协议请求，然后从缓存或源服务器获取对象并返回
- traffic_manager 进程是TS 的命令和控制设施，负责启动、监控和配置traffic_server 进程，它也负责代理的端口配置、统计信息的接口、集群管理和虚拟IP 的故障转移
- traffic_cop 进程监视traffic_server和traffic_manager进程，此进程周期性的查询traffic_server 和traffic_manager进程的健康状况，如果查询在一定间隔时间内未返回或返回信息不正确，traffic_cop将重启 traffic_manager 和traffic_server进程

>管理工具

- Traffic Line是命令行程序，可以用来快速监视Traffic Server 的性能和网络流量，也能配置TS
- Traffic Shell 也是命令行工具，进入该shell 后有自己一套语法，可代替Traffic Line 完成监控、配置任务
- 通过 Traffic Line 和 Traffic Shell 对配置作出的修改将会自动写入配置文件中

**2、Traffic Server的底层机制**

Apache Traffic Server不同于大部分开源代理服务器，它结合了两种技术来处理高并发：
- 异步时间处理 （Asynchronous event processing）
- 多线程（Multi-threading）

Traffic Server 在多CPU、多核的硬件上扩展良好，能充分利用所有可用的CPU 和其他资源

**3、HTTP 代理缓存相关机制**

> Traffic Server 处理请求的过程

- 用户请求一个web 对象，TS 收到请求
- TS通过对象的地址，在对象数据库（缓存）中去定位该对象
    -  如果对象在缓存中，TS 会检查对象是否新鲜（fresh） ，如果新鲜，TS 从缓存里返回该对象给用户，此时成为缓存命中（cache hit）。如果不新鲜(stale),TS会连接源服务器去验证对象是否仍然新鲜，即重新验证（revalidation），如果仍然新鲜，TS立即将缓存中的副本返回给用户
    -  如果对象不在缓存中（缓存未命中，cache miss），或者缓存的副本不再有效，TS 会去源服务器获取对象，让后做下面两件事
        -  将对象返回给用户
        -  将对象放到本地缓存中

> Traffic Server 判断HTTP 对象是否新鲜（fresh）的过程

- 如果有Expires 或者 max-age 头部直接定义缓存的过期时间，TS将对比当前时间和过期时间去判断对象是否新鲜
- 如果没有上述头部，TS将检查Last-Modified 和Date头部（其中Date是源服务器返回对象的时间，如果没有Last-Modified头部，TS会用对象写入缓存的时间作代替），然后用一下公式算出新鲜的时间范围（freshness_limit,可理解为保质期）：
    - freshness_limit = (Date - Last-Modified ) x 0.1  
    - 0.1 这个参数可以做调整，并且能限制freshness_limit的上下限，默认最小是1小时，最大是1天

- 如果没有expiress 头部或者没有Last-Modified、Date 头部，TS 将使用默认的freeness_limit
- 另外，TS还回检查cache.config 配置文件中的 revalidate 规则，将规则可以对特定HTTP对象设置特定的验证时间（特定的域名、IP、一定规则的URL、特定的客户端等等）

> 缓存过期（stale），Traffic Server去源服务器重新验证对象可能的情况

- 仍然fresh，TS重置freshness_limit，并返回对象
- 对象新副本可用，TS缓存新对象，并同时返回给用户
- 源服务器上的对象不再存在，TS也不再返回该副本给用户
- 源服务器没有响应，TS返回过期的对象并发出警告


###二、Traffic Server 安装
**1、编译环境**

    yum -y install  gcc gcc-c++ 

**2、安装依赖环境**
    
    yum -y install openssl-devel  tcl-devel libxml2-devel  pcre-devel expat-devel sqlite-devel  libdbi-devel libtool db4-devel

**3、软件包下载**

    wget http://mirrors.hust.edu.cn/apache/trafficserver/trafficserver-6.2.2.tar.bz2

**4、 安装**

    tar xf trafficserver-6.2.2.tar.bz2
    cd trafficserver-6.2.2
    ./configure --prefix=/usr/local/ats
    make && make install 

**5、简单回归测试**
    
    /usr/local/ats/bin/traffic_server -R 1
    
###三、Traffic Server 配置
**1、配置文件**

>缓存：

    Cache.config                缓存
    congestion.config           上游拉取数据拥塞控制
    hosting.config              缓存分割与上游分配
    icp.config                  定义上游的peer
    ip_allow.config             定义可以使用cache的白名单
    parent.config               缓存可以定义多级，定义级别的配置
    storage.config              缓存持久化


>LOG配置：

    log_hosts.config            将不同上游的log放到不同的log文件中
    logs_xml.config             定义不同的log格式
    plugin.config               插件管理
    records.config              主程序可调用参数

>代理：

    remap.config                请求和响应的URL修改配置
    splitdns.config             域名解析
    ssl_multicert.config        配置多个SSL证书

>其他：

    cluster.config              集群配置
    metrics.config              
    socks.config
    vaddrs.config
    volume.config

**2、命令行工具**

    header_rewrite_test
    traffic_cop             #独立的监控程序，监控traffic_server 和traffic_manager的职责和内存交换空间使用情况，发现异常重启进程。
    traffic_crashlog        #由traffic_server进程启动，在traffic_server崩溃的时候打印一个崩溃报告到log目录
    traffic_ctl             #在线配置一些traffic_server可以配置的参数
    traffic_layout          #
    traffic_line            #命令行工具
    traffic_logcat          #将traffic_server的二进制log转变成可读的ASCII log
    traffic_logstats        #traffic_server 的log 分析工具
    traffic_manager         #为traffic_ctl提供服务
    traffic_sac             #日志收集器，用在traffic_server集群中，可以用来收集各个节点的日志集中到本机进行处理，一个节点可以不安装traffic server，只安装sac可以发挥更大的日志能力
    trafficserver           #trafficserver 服务的管理工具
    traffic_server          #核心进程       
    traffic_via             #可以配置proxy_config.http.insert_request_via_str、proxy.config.http.insert_response_via_str 两个参数使得所有的数据的http头部都携带via信息（表示cache状态，可以看出是从哪里拿来的），wget这个文件就会在http头部看到这个信息，而这个信息是被traffic server编码过的，使用traffic_via命令可以将这个信息解码就可以看到缓存的获取路劲
    tspush                  #不需要用户请求可以直接使用这个命令将内容投递到traffic server 的cache中，使用这个命令需要打开proxy.config.http.push_method_enabled选项
    tsxs                    #插件编译程序，用来编译和安装插件


