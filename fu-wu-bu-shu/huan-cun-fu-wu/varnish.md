#varnish 使用手册

###一、安装varnish

**1、编译安装**

- 安装依赖环境包


    yum -y install autoconf automake jemalloc-devel libedit-devel libtool ncurses-devel pcre-devel pkgconfig python-docutils python-sphinx

- 安装varnish

    
    wget http://varnish-cache.org/_downloads/varnish-6.0.0.tgz

    tar xf varnish-6.0.0.tgz
    cd varnish-6.0.0
    sh autogen.sh
    sh configure
    make
    make install

 **2、rpm安装**

- 配置yum 源


    vim /etc/yum.repo.d/varnish.repo

    [varnishcache_varnish41]
    name=varnishcache_varnish41
    baseurl=https://packagecloud.io/varnishcache/varnish41/el/6/$basearch
    repo_gpgcheck=1
    gpgcheck=0
    enabled=1
    gpgkey=https://packagecloud.io/varnishcache/varnish41/gpgkey
    sslverify=1
    sslcacert=/etc/pki/tls/certs/ca-bundle.crt
    metadata_expire=300
    
    [varnishcache_varnish41-source]
    name=varnishcache_varnish41-source
    baseurl=https://packagecloud.io/varnishcache/varnish41/el/6/SRPMS
    repo_gpgcheck=1
    gpgcheck=0
    enabled=1
    gpgkey=https://packagecloud.io/varnishcache/varnish41/gpgkey
    sslverify=1
    sslcacert=/etc/pki/tls/certs/ca-bundle.crt
    metadata_expire=300


- 安装varnish


    yum -y install varnish  jemalloc-devel

###二、varnish 工作流程
![](/assets/varnish.jpg)

    1)varnish从客户端接收请求后，由vcl_recv状态引擎处理，不能识别的请求将会通过参数pipe交给vcl_pipe状态引擎，需要查找缓存的
    请求通过lookup参数将会交给vcl_hash状态引擎，无需缓存的数据通过参数pass将会交给 vcl_pass状态引擎；
    2)vcl_hash状态引擎在接收到请求后会从缓存中查找数据，查询结果有两种，一种是hit缓存命中，另一种是miss缓存未命中；
    3)vcl_hit状态引擎将命中的缓存数据通过参数deliver交给vcl_deliver状态引擎，vcl_deliver状态引擎将数据处理后，最终返回给客户端；
    4)vcl_miss状态引擎将未命中的结果参数fetch交给vcl_fetch状态引擎，vcl_fetch状态引擎将会从数据库中查找数据；
    5)vcl_fetch状态引擎将从数据库中查询到的结果，返回给vcl_deliver状态引擎；
    6)vcl_deliver状态引擎将结果返回给master进程，最终返回给客户端；

###三、varnish 配置
**1、/etc/sysconfig/varnish**

    
    #最大文件数
    NFILES=131072

    #共享锁内存
    MEMLOCK=82000

    #最大线程数
    NPROCS="unlimited"

    #
    RELOAD_VCL=1

    #varnish 默认规则配置文件
    VARNISH_VCL_CONF=/etc/varnish/default.vcl

    #varnish 监听的端口
    VARNISH_LISTEN_PORT=80

    #varnish 监听的IP 
    VARNISH_ADMIN_LISTEN_ADDRESS=127.0.0.1

    #varnish 管理的端口
    VARNISH_ADMIN_LISTEN_PORT=6082

    #varnish 管理秘钥
    VARNISH_SECRET_FILE=/etc/varnish/secret

    #varnish 最小线程数
    VARNISH_MIN_THREADS=50

    #varnish 最大线程数
    VARNISH_MAX_THREADS=1000

    #varnish 内存块大小
    VARNISH_STORAGE_SIZE=256M

    #存储模型
    VARNISH_STORAGE="malloc,${VARNISH_STORAGE_SIZE}"

    #后端TTL
    VARNISH_TTL=120

    #启动参数配置
    DAEMON_OPTS="-a ${VARNISH_LISTEN_ADDRESS}:${VARNISH_LISTEN_PORT} \
             -f ${VARNISH_VCL_CONF} \
             -T ${VARNISH_ADMIN_LISTEN_ADDRESS}:${VARNISH_ADMIN_LISTEN_PORT} \
             -p thread_pool_min=${VARNISH_MIN_THREADS} \
             -p thread_pool_max=${VARNISH_MAX_THREADS} \
             -S ${VARNISH_SECRET_FILE} \
             -s ${VARNISH_STORAGE}"

**2、VCL内置函数**

    vcl_backend_error       当从后端主机获取源文件失败后，调用此函数
	vcl_backend_fetch       向后端主机发送请求钱，调用此函数，可修改发往后端的请求
	vcl_backend_response    获得后端主机的响应后，调用此函数
	vcl_deliver             将在缓存中找到请求的内容发送给客户端前调用此方法
	vcl_fini                当所有请求都离开当前VCL，且当前VCL被弃用时，调用此函数，经常用于清理varnish模块
	vcl_hash                在vcl_recv调用后为请求创建一个hash值，调用此函数，此hash值将作为varnish中搜索缓存对象的key
	vcl_hit                 在执行lookup指令后，在缓存中找到请求的内容将自动调用该函数
	vcl_init                VCL加载时调用此函数，经常用于初始化varnish模块（VMODs）
	vcl_miss                在执行lookup指令后，在缓存中没有找到请求的内容时自动调用该函数，此函数可用于判断是否需要从后端服务器获取内容
	vcl_pass                此函数在进入pass模式时被调用，用于将请求直接传递至后端主机，并将后端响应原样返回客户端
	vcl_pipe                此函数在进入pipe模式时被调用，用于将请求直接传递至后端主机，但后端主机的响应并不缓存直接返回客户端
	vcl_purge               pruge操作执行后调用此函数，可用于构建一个响应
	vcl_recv                用于接收和处理请求，当请求达到并成功接收后被调用，通过判断请求的数据来决定如何处理请求

**3、backend 配置**

> 默认配置


    backend default {
        .host = "127.0.0.1";
        .port = "8080";
    }
    
>健康检查
    
    #直接配置：
    backend default {
        .host = "127.0.0.1";
        .port = "8080";
        .probe = {
            .url = "/";
            .timeout = 1s;
            .interval = 5s;
            .window = 5;
            .threshold = 3;
        }
    }
    健康检查页面是 "/" ,超时时间为1秒，每5秒检查一次，5次有3次成功认为是健康的

    #以模块配置：
    probe backend_healthcheck {
        .url = health.html"
        .windows = 5;
        .threshold = 2;
        .interval = 3s;
    }

    backend web1 {
        .host = "127.0.0.1";
        .port = "8080";
        .probe = backend_healthcheck;
    }

    backend web2 {
        .host = "127.0.0.1";
        .port = "8080";
        .probe = backend_healthcheck;
    }    
    
    
    
>重新定义

    backend web {
        .host = "127.0.0.1";
        .port = "8080";
    }
    
    vcl_recv {
        set req.backend_hint = web ;
    }

>多主机

    #基于权重：
    import directors

    probe backend_healthcheck {
        .url = health.html"
        .windows = 5;
        .threshold = 2;
        .interval = 3;
    }

    backend web1 {
        .host = "192.168.1.1";
        .port = "80";
        .probe = backend_healthcheck;       
    }
    
    backend web2 {
        .host = "192.168.1.2";
        .port = "80";
        .probe = backend_healthcheck;
    }
    
    sub vcl_init {
        new web_cluster = directors.random();
        web_cluster.add_backend(web1,1.0);
        web_cluster.add_backend(web2,1.0);
    } 

    sub vcl_recv {
        set req.backend_hint = web_cluster;
    }
    
    #基于请求类型：
    backend html {
        .host = "192.168.1.1";
    }
    backend php {
        .host = "192.168.1.2";
    }

    sub vcl_recv {
        if ( req.url ~ "\.php" ){
            set req.backend_hint = php;
        }
        set  req.backend_hint = html;
    }

    #基于域名：
    backend moblie {
        .host = "192.168.1.1";
    }
    backend host {
        .host = "192.168.1.2";
    }
    backend default {
        .host = "192.168.1.3";
    }

    sub vcl_recv {
        if ( req.http.host == "moblie.demo.com" || req.http.host == "m.demo.com" ){
            set req.backend_hint = moblie;
        } elsif  ( req.http.host == "www.demo.com" || req.http.host == "demo.com" ) {
            set req.backend_hint = host;
        }
        set req.backend_hint = default;

    }

**4、 添加缓存标识**

    
    sub vcl_deliver {
        if ( obj.hits > 0 ) {
            set resp.http.X-cache = "HIT from " + server.ip;
        } else {
            set resp.http.X-cache = "MISS " + server.ip;
        }
    } 

**5、缓存清理**

    #定义配置
    acl purge_ip {
        "localhost";
        "127.0.0.1";
    }

    sub vcl_recv {
        if ( req.method ~ "PURGE" ){
                if ( client.ip ~ purge_ip ) {
                        return(purge);
                }
                return(synth(404,"Not Found"));
        }
    }

    sub vcl_purge {
        return(synth(200,"Success"));

    }
    
    #清除缓存
    curl -X PURGE  http://www.demo.com/test.html

    #规则清理
    varnishadm 
    >ban req.url /test.html


**6、Grace mode （优雅模式）**


    
    #过期仍然存放2分钟
    sub vcl_backend_response {
      set beresp.grace = 2m;
    }

    #如果健康检查成功，直接返回。健康检查失败，执行grace 模式

    import std;

    if (!std.healthy(req.backend_hint) && (obj.ttl + obj.grace > 0s)) {
          return (deliver);
    } else {
          return (fetch);
    }

**7、清除cookie **

    sub vcl_recv {
        if (req.url ~ "^/images") {
            unset req.http.cookie;
        }
    }

**8、改变后端响应**

    sub vcl_backend_response {
        if (bereq.url ~ "\.(png|gif|jpg)$") {
            unset beresp.http.set-cookie;
            set beresp.ttl = 1h;
        }
    }

**9、配置ACL**

    #只允许 localhost ，192.168.1.0/24 清除缓存
    acl local {
        "localhost";
        "192.168.1.0"/24; /* and everyone on the local network */
        ! "192.168.1.23"; /* except for the dialin router */
    }
    
    sub vcl_recv {
      if (req.method == "PURGE") {
        if (client.ip ~ local) {
           return(purge);
        } else {
           return(synth(403, "Access denied."));
        }
      }
    }
    

    #禁止 192.168.1.0/24  访问
    acl local {
        "192.168.1.0"/24; /* and everyone on the local network */
    }
    
    sub vcl_recv {
        if (client.ip ~ local) {
           return(synth(403, "Access denied."));
        }
    }

**10、添加头信息**

    if (req.restarts == 0) {
            if (req.http.x-forwarded-for) {
                    set req.http.X-Forwarded-For =
                            req.http.X-Forwarded-For + ", " + server.ip;
            } else {
                    set req.http.X-Forwarded-For = server.ip;
            }
    }

**11、定义缓存策略**

    sub vcl_recv {
        if (req.url ~ "\.php$") {
            return(pass);
        }
    }

    sub vcl_fetch {
        if (bereq.url ~ "\.(jpg|jpeg|gif|png)$") {
                set beresp.ttl = 7200s;
        }
        if (bereq.url ~ "\.(html|css|js)$") {
                set beresp.ttl = 1200s;
        }
    }


**12、清除头信息**    

    sub vcl_deliver {
        unset resp.http.Via;
        unset resp.http.X-Varnish;
    }
   

四、命令工具

- varnishadm          
- varnishhist         
- varnishncsa         
- varnishstat         
- varnishtop                    
- varnishlog          
- varnish_reload_vcl  
- varnishtest        