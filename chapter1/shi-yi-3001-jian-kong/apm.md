# Pinpoint 安装部署 #
### 一、基础架构 ###
**1、服务分布**
    
    hbase           数据存储
    pp-collector    数据收集
    pp-web          web展示
    zookeeper       服务协调

    agent           数据发送

**2、端口发布**
    
    hbase：          10610
    pp-collector:    18080 9996 9995 9994    
    pp-web:          28080 9997
    zookeeper:       2180   

    agent:


### 二、安装jdk依赖环境 ###

**1、安装jdk**

    yum -y install java-*
    wget --no-check-certificate  https://download.java.net/java/GA/jdk9/9.0.4/binaries/openjdk-9.0.4_linux-x64_bin.tar.gz
    tar xf openjdk-9.0.4_linux-x64_bin.tar.gz
    mv jdk-9.0.4/ /usr/local/

**2、配置环境变量**

    vim /etc/profile.d/java.sh
    
    export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk
    export PATH=$PATH:$JAVA_HOME/bin
    export CLASSPATH=.:$JAVA_HOME/jre/lib:$JAVA_HOME/lib:$JAVA_HOME/lib/tools.jar
    
    export JAVA_6_HOME=/usr/lib/jvm/java-1.6.0-openjdk.x86_64
    export JAVA_7_HOME=/usr/lib/jvm/java-1.7.0-openjdk
    export JAVA_8_HOME=/usr/lib/jvm/java-1.8.0-openjdk
    export JAVA_9_HOME=/usr/local/jdk-9.0.4

>**注意： 环境变量切换**

### 三、安装hbase ###

**1、下载相关软件**

    wget http://mirror.bit.edu.cn/apache/hbase/hbase-1.2.8/hbase-1.2.8-bin.tar.gz
    wget https://github.com/naver/pinpoint/blob/master/hbase/scripts/hbase-create.hbase

**2、部署hbase**

    tar xf hbase-1.2.8.bin.tar.gz && mv hbase-1.2.8 /usr/local

**3、配置hbase**
    
    vim /usr/local/hbase-1.2.8/conf/hbase-site.xml

    <configuration>
      <property>
        <name>hbase.rootdir</name>
        <value>file:///data/hbase</value>
      </property>
    </configuration>

    vim /usr/local/hbase-1.2.8/conf/hbase-env.sh
    export JAVA_HOME=/usr/lib/jvm/java-1.7.0-openjdk.x86_64/

**4、启动服务、测试连接**
    
    /usr/local/hbase-1.2.8/bin/start-hbase.sh 

    /usr/local/hbase-1.2.8/bin/hbase  shell

**5、导入数据表**
    
    /usr/local/hbase-1.2.8/bin/hbase  shell  hbase-create.hbase

>注意：会报主机名相关的报错。可以忽略或配置hosts解析
>> hbase 与pinpoint 安装在同一台机器可以不用安装zookeeper,hbase 会启用内置zookeeper 
>>不同机器需要安装独立的zookeeper

**6、测试访问 **   
    
    http://IP:16010

### 三、安装pinpoint ###
**1、下载相关软件**

    
    wget http://mirror.bit.edu.cn/apache/tomcat/tomcat-8/v8.0.53/bin/apache-tomcat-8.0.53.tar.gz

    wget --no-check-certificate https://github.com/naver/pinpoint/releases/download/1.8.0/pinpoint-agent-1.8.0.tar.gz
    wget --no-check-certificate https://github.com/naver/pinpoint/releases/download/1.8.0/pinpoint-collector-1.8.0.war
    wget --no-check-certificate https://github.com/naver/pinpoint/releases/download/1.8.0/pinpoint-web-1.8.0.war

**2、配置pinpoint-collector**
    
    tar xf apache-tomcat-8.0.53.tar.gz 
    cp apache-tomcat-8.0.53 /usr/local/pp-coll
    
    rm -rf /usr/local/pp-coll/weapps/*
    unzip pinpoint-collector-1.8.0.war -D /usr/local/pp-coll/weapps/ROOT
   
    cd /usr/local/pp-coll/conf
    sed -i 's/port="8005"/port="18005"/g' server.xml
    sed -i 's/port="8080"/port="18080"/g' server.xml
    sed -i 's/port="8443"/port="18443"/g' server.xml
    sed -i 's/port="8009"/port="18009"/g' server.xml
    sed -i 's/redirectPort="8443"/redirectPort="18443"/g' server.xml 
    sed -i "s/localhost/`ifconfig eth0 | grep 'inet addr' | awk '{print $2}' | awk -F: '{print $2}'`/g" server.xml

    /usr/local/pp-col/bin/catalina.sh  start

>collector 会监听 18080 9996 9995 9994
    
    
**3、配置pinpoint-web**

    tar xf apache-tomcat-8.0.53.tar.gz 
    cp apache-tomcat-8.0.53 /usr/local/pp-web
    
    rm -rf /usr/local/pp-web/weapps/*
    unzip pinpoint-collector-1.8.0.war -D /usr/local/pp-web/weapps/ROOT
   
    cd /usr/local/pp-web/conf
    sed -i 's/port="8005"/port="28005"/g' server.xml
    sed -i 's/port="8080"/port="28080"/g' server.xml
    sed -i 's/port="8443"/port="28443"/g' server.xml
    sed -i 's/port="8009"/port="28009"/g' server.xml
    sed -i 's/redirectPort="8443"/redirectPort="18443"/g' server.xml 
    sed -i "s/localhost/`ifconfig eth0 | grep 'inet addr' | awk '{print $2}' | awk -F: '{print $2}'`/g" server.xml

    /usr/local/pp-web/bin/catalina.sh  start

    http://IP:28080  访问pp-web

>web 会监听 28080 9997

**4.1、安装agent**

    tar xf pinpoint-agent-1.8.0.tar.gz
    vim /usr/local/src/agent/pinpoint.config
    #指定pp-collector IP
    profiler.collector.ip=10.0.0.110


**4.2.1、配置JAVA监控**
    #添加jvm opts
    vim JAVA-SERVER/catalina.sh
    -----------------------------------
    #指定agent-jar包
    CATALINA_OPTS="$CATALINA_OPTS -javaagent:/usr/local/src/agent/pinpoint-bootstrap-1.8.0.jar"
    #指定ID号
    CATALINA_OPTS="$CATALINA_OPTS -Dpinpoint.agentId=pp20181113"
    #制定服务名
    CATALINA_OPTS="$CATALINA_OPTS -Dpinpoint.applicationName=demo1"

**4.2.2、配置PHP监控**    

    


    
    
    


    