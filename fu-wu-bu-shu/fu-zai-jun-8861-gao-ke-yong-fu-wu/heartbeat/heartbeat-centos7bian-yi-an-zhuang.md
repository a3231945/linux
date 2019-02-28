### 一、安装依赖环境    

    yum install -y bzip2 autoconf automake libtool glib2-devel libxml2-devel bzip2-devel libtool-ltdl-devel asciidoc libuuid-devel psmisc

### 二、组件包 ###
**1、安装glue**

    wget http://hg.linux-ha.org/glue/archive/0a7add1d9996.tar.bz2
    tar jxvf 0a7add1d9996.tar.bz2
    cd Reusable-Cluster-Components-glue--0a7add1d9996/
    groupadd haclient
    useradd -g haclient hacluster
    ./autogen.sh
    ./configure --prefix=/usr/local/heartbeat/
    make
    make install

**2、安装Resource Agents**

    wget https://github.com/ClusterLabs/resource-agents/archive/v3.9.6.tar.gz
    tar zxvf v3.9.6.tar.gz
    cd resource-agents-3.9.6/
    ./autogen.sh
    export CFLAGS="$CFLAGS -I/usr/local/heartbeat/include -L/usr/local/heartbeat/lib"
    ./configure --prefix=/usr/local/heartbeat/

    vi /etc/ld.so.conf.d/heartbeat.conf
    /usr/local/heartbeat/lib
    ldconfig


    make
    make install

**3、安装HeartBeat**

    wget http://hg.linux-ha.org/heartbeat-STABLE_3_0/archive/958e11be8686.tar.bz2
    tar jxvf 958e11be8686.tar.bz2
    cd Heartbeat-3-0-958e11be8686
    ./bootstrap
    export CFLAGS="$CFLAGS -I/usr/local/heartbeat/include -L/usr/local/heartbeat/lib"
    ./configure --prefix=/usr/local/heartbeat/
    vi /usr/local/heartbeat/include/heartbeat/glue_config.h
    /*define HA_HBCONF_DIR “/usr/local/heartbeat/etc/ha.d/”*/   （注意这行用/**/注释掉）
    make
    make install


**4、拷贝文件**

    cp /usr/local/heartbeat/share/doc/heartbeat/ha.cf  /usr/local/heartbeat/etc/ha.d
    cp /usr/local/heartbeat/share/doc/heartbeat/authkeys /usr/local/heartbeat/etc/ha.d
    cp /usr/local/heartbeat/share/doc/heartbeat/haresources /usr/local/heartbeat/etc/ha.d

**5、映射插件**

    ln -svf /usr/local/heartbeat/lib64/heartbeat/plugins/RAExec/* /usr/local/heartbeat/lib/heartbeat/plugins/RAExec/
    ln -svf /usr/local/heartbeat/lib64/heartbeat/plugins/* /usr/local/heartbeat/lib/heartbeat/plugins/


### 三、注意 ###

**1、如果单机测试必须要包含 备份节点的node**

    实例：
        #vim ha.cf
        debugfile /var/log/ha-debug
        logfile	/var/log/ha-log
        
        logfacility	local0
        keepalive 2
        deadtime 30
        warntime 10
        initdead 60
        udpport	694
        ucast eth0  10.0.0.101
        auto_failback on
        
        node heartbeat
        node heartbeat2
        
        #vim authkeys
        auth 1
        1 crc

        #vim haresources
        heartbeat  IPaddr::10.0.0.200/24/eth0  irqbalance



**2、必须使用主机名长格式**
**3、必须有解析**
**4、authkeys 必须为600权限**
**5、不支持调用systemd 服务**



    