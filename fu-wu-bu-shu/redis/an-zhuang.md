###安装

**1、下载软件包**

    wget http://download.redis.io/releases/redis-3.0.5.tar.gz  -P  /usr/local/src
**2、下载tcl支持**

    yum -y install tcl tcl-devel
**3、解压编译**

    cd /usr/local/src && tar xf redis-3.0.5.tar.gz && cd redis-3.0.5 && make && make
    PREFIX=/usr/local/redis install
**4、拷贝conf 文件**

    cp /usr/local/src/redis-3.0.5/redis.conf /usr/local/redis
**5、启动服务**
- **前端启动：**

    /usr/local/redis/bin/redis-server /usr/local/redis/redis.conf
- **后端启动：**

    sed -i 's/^daemonize no/daemonize yes/g'  /usr/local/redis/redis.conf
    /usr/local/redis/bin/redis-server /usr/local/redis/redis.conf
    
**6、测试服务**

    /usr/local/redis/bin/redis-cli 
    set   site   www.longge.com   #添加一个KEY
    get   site		           #删除一个KEY
    
**7、添加环境变量**

    vim  /etc/profile.d/redis.sh
    export REDIS_HOME=/usr/local/redis
    export PATH=$REDIS_HOME/bin:$PATH

    #	cat > /etc/profile.d/redis.sh <<eof
    #	export REDIS_HOME=/usr/local/redis
    #	export PATH=$REDIS_HOME/bin:$PATH
    #	eof


