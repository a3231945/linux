相关# ELK 日志系统 #

### 一、日志分类 ###
1、**设备日志**
  
    交换机
    服务器
    防火墙

2、**系统日志**

    rsyslog 日志
    audit   日志

3、**服务日志**
    
    nginx
    php
    mysql
    mysql-mmm
    base
    mongo
    zabbix
    heartbeat
    其他

### 二、环境架构 ###
1、**架构图**

    filebeat->kafka->logstash->elasticsearch->kibana

2、**自定义日志格式**
    
    xxx
3、**保存时长**
    
    nginx           1月
    squid           1月
    mysql           3月
    mysql_mmm       3月
    php             3月
    system_base     3月
    mongo           3月
    zabbix          3月
    heartbeat       3月

4、**端口列表**
    
    redis           6379
    filebeat        
    logstash        
    elasticsearch   9200,9300
    elasticsearch   9100    
    kibana          5601

5、**当前日志格式**
**nginx**：
    
*www.eeo.cn 访问日志： 日志格式可修改*

     '$upstream_addr - $remote_addr - $remote_user [$time_local] "$request_method $scheme://$http_host$request_uri $server_protocol" '
     '$status $body_bytes_sent "$http_referer" '
     '"$http_user_agent" "$http_x_forwarded_for" $request_time $upstream_response_time'

     例如：
     10.1.0.39:61185 - 139.199.33.45 - - [04/Apr/2018:09:53:29 +0800] "POST http://www.eeo.cn/partner/api/course.api.php?action=addClassLabels HTTP/1.1" 200 172 "-" "Java/1.7.0_80" "211.103.152.75" 0.072 0.072

     名词解释：
      upstream_addr ：后端节点ip及端口
      remote_addr   ：客户端地址
    　time_local    ：时间戳
    　request_method　：请求方法（GET/POST/HEAD 等）
      scheme        ：协议 HTTP/HTTPS
      http_host     : 主机名（IP：port/域名）
      request_uri   : 请求的地址
      server_protocol : http请求的版本（1.0/1.1/2.0）
      status        : http 状态码（200/302/304/403/500/502 等）
      body_bytes_sent : 传输的数据大小（以字节为单位）
      http_referer  : 从哪个站点跳转至该URI
      http_user_agent : 用户代理（火狐/苹果/google/ie/自定义 等）
      http_x_forwarder_for : 经过了几层代理
      request_time  :  响应改请求总共多少时间（单位毫秒）
      upstream_respone_time : 后端代理 响应时间（单位毫秒）

*www.eeo.cn 错误日志：日志格式不可修改*
   
      time_local   [log_level]  proccess_id  info 
    
      例如：
      2018/04/04 11:23:54 [error] 1793#0: *89229633 open() "/data/apps/business/web/eeo.cn/.well-known/apple-app-site-association" failed (2: No such file or directory), client: 49.83.177.117, server: www.eeo.cn, request: "GET /.well-known/apple-app-site-association HTTP/1.1", host: "www.eeo.cn"

    
      名词解释：
      time_local : 时间戳
      log_level :  日志级别
      process_id : 子进程ID 
      info       ： 详细的错误日志信息

**主机日志审计日志：**

    LOGINID=$LOGINID WHO=[$WHO] USER=$CUR_USER SSH=[$SSH_CONNECTION] PPID=$PPID PID=$CUR_PID CMD=login
    LOGINID=$LOGINID USER=$CUR_USER PID=$CUR_PID PWD=$CUR_PWD CMD=$CMD    

    例如：
     Apr  4 11:19:58 gz-r3tx_ch EEO_AUDIT[24333]: LOGINID=20180404111958498 WHO=[root     pts/0        2018-04-04 11:19 (106.3.144.10)] USER=root SSH=[106.3.144.10 7758 10.104.138.246 61104] PPID=24309 PID=24312 CMD=login
    
     Apr  4 11:20:01 gz-r3tx_ch EEO_AUDIT[24352]: LOGINID=20180404111958498 USER=root PID=24312 PWD=/root CMD=ip ad li


    名词解释：
        LOGINID: 登录用户ID
        WHO    : who 命令信息
        CUR_USER   : 登录用户
        SSH_CONNECTION :  ssh 连接信息
        PPID   : 父进程ID
        CUR_PID : 进程ID 
        CUR_PWD: 当前目录
        CMD    : 执行的命令
          
**squid：**   

    [%{%Y-%m-%d:%H:%M:%S}tl.%03tu] %6tr %dt %>a:%>p %Ss/%03>Hs %<st %rm %ru %[un %Sh/%<A %mt

    例如：
    [2018-04-01:08:43:30.343]     39 - 113.78.66.111:63453 TCP_MISS/304 250 GET https://h1237-wb.eeo.cn/saas/common/theme-classin/index.css?version=1522222822811 - ROUNDROBIN_PARENT/peer_eeo_www2 -

    名词解释：
       [%{%Y-%m-%d:%H:%M:%S}tl.%03tu]  ：时间戳
        %6tr                        ：   
        %dt                         ： 客户端地址（端口）
        %>a:%>p                     ： 
        %Ss/%03>Hs                  ：
        %<st                        ：
        %rm                         ：http 请求方法（GET/POST 等）
        %ru                         ：
        %[un                        ：
        %Sh/%<A                     ：
        %mt                          :
 
    
     
    
      
         
　　　

    
6、**其他**
    
    xxx

### 三、服务部署 ###
1、**安装filebeat**

    rpm -ivh filebeat-6.1.3-x86_64.rpm
2、**安装redis**

    rpm -ivh redis-3.2.11-1.el6.x86_64.rpm
2-1、**安装kafka**

    #安装java环境
    rpm -ivh jdk-8u161-linux-x64.rpm

    #安装zookeeper
    tar xf zookeeper-3.4.10.tar.gz   
    mv  zookeeper-3.4.10 /usr/local && ln -s /usr/local/zookeeper-3.4.10 /usr/local/zookeeper

    #安装kafka
    tar xf kafka_2.10-0.10.2.1.tgz
    mv kafka_2.10-0.10.2.1 /usr/local && ln -s /usr/local/kafka_2.10-0.10.2.1 /usr/local/kafka

3、**安装logstash**

    rpm -ivh logstash-6.1.3.rpm

    
4、**安装elasticsearch**

    #安装java 环境
    rpm -ivh jdk-8u161-linux-x64.rpm

    #安装elasticsearch
    rpm -ivh elasticsearch-6.1.3.rpm
    
    #安装nodejs
    wget https://npm.taobao.org/mirrors/node/latest-v4.x/node-v4.4.7-linux-x64.tar.gz
    tar xf node-v4.4.7-linux-x64.tar.gz
    mv node-v4.4.7-linux-x64 /usr/local/nodejs

    #配置nodejs环境变量
    vim /etc/profile.d/node.sh
    export NODE_HOME=/usr/local/nodejs
    export PATH=$NODE_HOME/bin:$PATH
    export NODE_PATH=$NODE_HOME/lib/node_modules
    export PATH

    npm install -g grunt-cli
    
    #安装elasticsearch-head 插件
    git clone git://github.com/mobz/elasticsearch-head.git
    mv elasticsearch-head /usr/local

4、**安装kibana**

    rpm -ivh kibana-6.1.3-x86_64.rpm 

#### 四、配置服务 ####
1、**filebeat配置文件**：`/etc/filebeat/filebeat.yml`
    
    filebeat.prospectors:
    - type: log
      enabled: true
      paths:
        - /data/logs/services/mysql/mysqld.log
        - /data/logs/services/mysql/slow.log
    - type: log    
      enabled: true
      paths:
        - /var/log/cron
        - /var/log/dmesg
        - /var/log/messages
        - /var/log/secure
        - /data/logs/oss/security/bash_history_audit.log 
      fields:
        name: mysql
      
    filebeat.config.modules:
      path: ${path.config}/modules.d/*.yml
      reload.enabled: false
    setup.template.settings:
      index.number_of_shards: 3
    output.logstash:
      hosts: ["10.0.0.200:5044"]

2-1、**kafka配置文件**：`/usr/local/zookeeper-3.4.10/conf/zoo.cfg `

    cp /usr/local/zookeeper/conf/zoo_sample.cfg  /usr/local/zookeeper/conf/zoo.cfg
    mkdir -pv /tmp/zookeeper
    mkdir -pv /tmp/zookeeperlog

    2:tickTime=2000
    5:initLimit=10
    8:syncLimit=5
    12:dataDir=/tmp/zookeeper
    13:dataLogDir=/tmp/zookeeperlog
    15:clientPort=2181


2-2、**kafka配置文件**：`/usr/local/kafka_2.10-0.10.2.1/config/server.properties`
    
    mkdir -pv /tmp/kafka-logs
 
    21:broker.id=0
    45:num.network.threads=3
    48:num.io.threads=8
    51:socket.send.buffer.bytes=102400
    54:socket.receive.buffer.bytes=102400
    57:socket.request.max.bytes=104857600
    63:log.dirs=/tmp/kafka-logs
    68:num.partitions=1
    72:num.recovery.threads.per.data.dir=1
    99:log.retention.hours=168
    106:log.segment.bytes=1073741824
    110:log.retention.check.interval.ms=300000
    119:zookeeper.connect=localhost:2181
    122:zookeeper.connection.timeout.ms=6000




3、**logstash配置文件**：`/etc/logstash/conf.d/*.conf`

    见附件

4、**elasticsearch配置文件**：`/etc/elasticsearch/elasticsearch.yml`

    cluster.name: my-elk
    node.name: node-1
    path.data: /var/lib/elasticsearch
    path.logs: /var/log/elasticsearch
    bootstrap.memory_lock: false
    bootstrap.system_call_filter: false
    network.host: 10.0.0.200
    http.port: 9200
    http.cors.enabled: true  
    http.cors.allow-origin: "*"

4.1 修改文件描述符
    
    vim /etc/security/limits.d/90-nproc.conf 
    *          soft    nproc     4096
    root       soft    nproc     unlimited

    vim /etc/security/limits.conf
    * soft nofile 65536
    * hard nofile 131072


5、**kibana配置文件**：`/etc/kibana/kibana.yml`

    server.port: 5601
    server.host: "10.0.0.200"
    elasticsearch.url: "http://10.0.0.200:9200"
    elasticsearch.pingTimeout: 1500
    elasticsearch.requestTimeout: 30000

#### 五、启动服务 ####
1、**filebeat启动服务**
    
    /etc/init.d/filebeat start
2、**kafka启动服务**
    
    #启动zookeeper
    cd /usr/local/zookeeper && bin/zkServer.sh start

    #启动kafka
    cd /usr/local/kafka && nohup bin/kafka-server-start.sh  config/server.properties &
    cd /usr/local/kafka && bin/kafka-server-start.sh  -daemon config/server.properties 
3、**logstatsh启动服务**
    #生成启动脚本
    /usr/share/logstash/bin/system-install  /etc/logstash/startup.options  sysv
    #修改用户名
    vim /etc/init.d/logstash
    25 user="root"
    26 group="root"

    /etc/init.d/logstash start
4、**elasticsearch启动服务**
    
    /etc/init.d/elasticsearch start
5、**elasticsearch-head启动服务**
   
    cd /usr/local/elasticsearch-head && nohup grunt server&
6、**kibana启动服务**

    /etc/init.d/kibana start

