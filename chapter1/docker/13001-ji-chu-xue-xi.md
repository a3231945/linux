#Docker

###一、docker的部署安装
**1、安装软件包**



    #centos 6
    yum -y install docker-io

    or
    #centos 7
    yum -y install docker-engine

**2、启动docker**

    #centos6
    /etc/init.d/docker start
    chkconfig docker on

    #centos7
    systemctl start  docker
    sysctmctl enable docker

**3、验证版本**
    
    docker version

    docker info
    
###二、docker 配置文件与日志
docker 配置文件：`/etc/sysconfig/docker`

重要参数解释：
    
    options 用来控制Docker daemon 进程参数
    -H 表示Docker Daemon 绑定的地址 ,
        -H=unix：///var/run/docker.sock 
        -H=tcp://0.0.0.0:2375

    --registry-mirror 表示Docker Registry的镜像地址
        --registry-mirror=http://xxx.xxx.xxx

    --insecure-registry 表示（本地）私有DockerRegistry的地址
        --insecure-registry ${pivateRegistryHost}：5000

    --selinux-enabled 是否开启SElinux，默认开启 --selinux-enabled=true

    --bip 表示网桥docker0指定CIDR网络地址，--bip=172.17.42.1

    -b 表示采用已经创建好的网桥， -b=xxx

    OPTIONS= -H=unix:///var/run/docker.sock -H=tcp://0.0.0.0:2375 --registry=http://xxx.xxx.xxx --seliux-enabled=true

    下面是代理的设置
    http_proxy=xxx:8080
    https_proxy=xxx:8080   


###三、docker 基础命令 
**1、镜像管理**
    
    #查找镜像
    docker search XXX
    
    #下载镜像
    docker pull  xxx
    
    #查看镜像
    docker images

    #删除镜像
    docker rmi

**2、docker 容器管理**
    
    #启动容器
    docker run --name=XXX 

    #创建容器
    docker create -it --name=XXX XXX 
    
    #启动/停止容器
    docker start/stop XXX

    #挂起/恢复容器
    docker pause/unpause XXX
   
    #查看容器
    docker ps -a
    
    #进入容器
    docker exec | docker attach
    
    #删除容器 
    docker rm XXX
    
    ------------------
    创建mysql 容器并启动
    
    docker pull mysql

    docker create --name my-mysql -e MYSQL_ROOT_PASSWORD=123456 -p 3306:3306 mysql

**3、构建镜像**

    #将容器编成镜像
    docker commit XXX  XXX
    例如：
        docker commit 容器ID  my-centos    
    
    #Dockerfile
    vim Dockerfile
    -----------
    #基础镜像
    FROM centos
    
    #mail
    MAINTAINER zhouxulong <zhouxulong@peter-zhou.com>
    
    #install net-tools    
    RUN yum -y install net-tools
    
    #docker 开启22端口
    EXPOSE 22
    
    #前台运行程序
    CMD ["/bin/bash","-c","python -m SimpleHTTPServer 80"]

    -------------

    docker build -t python-http .
    
    #查看镜像
    docker images     
    
    #运行程序
    docker run -d --name http-demo python-http

    #进入容器
    docker exec -it http-demo /bin/bash

###四、docker + supervisor管理多进程
**ssh + python-httpd：**
    
    #依赖基础镜像
    FROM centos
    
    #mail
    MAINTAINER zhouxulong <zhouxulong@peter-zhou.com>
    
    #安装软件包
    RUN yum -y install epel-release   
    RUN yum -y install net-tools iproute vim vi wget curl openssh-server supervisor
    RUN echo "123456" |passwd --stdin root
    

    # 将sshd的UsePAM参数设置成no
    RUN mkdir -p /var/run/sshd
    RUN sed -i 's/UsePAM yes/UsePAM no/g' /etc/ssh/sshd_config
    RUN sed -i "s/#UsePrivilegeSeparation.*/UsePrivilegeSeparation no/g" /etc/ssh/sshd_config
    RUN ssh-keygen -q -t dsa -b 1024 -f /etc/ssh/ssh_host_dsa_key -N '' 
    RUN ssh-keygen -q -t rsa -b 1024 -f /etc/ssh/ssh_host_rsa_key -N '' 
    RUN ssh-keygen -q -t ecdsa -f /etc/ssh/ssh_host_ecdsa_key -N ''
    RUN ssh-keygen -q -t dsa -f /etc/ssh/ssh_host_ed25519_key  -N '' 
    
    copy supervisord.conf /etc/supervisord.d/supervisord.conf
    
    #docker 开启22端口
    EXPOSE 22
    EXPOSE 80
    
    #前台运行程序
    CMD ["/bin/bash","-c","/usr/bin/supervisord -c /etc/supervisord.d/supervisord.conf"]


###五、docker volume（数据的共享）的互联

**方法一：**

    #映射容器目录到docker 容器 /data
    docker run -it --name centos  -v /data centos /bin/bash

    #查看虚拟机目录
    docker exec -it centos 
    ls data

    #查看docker容器信息（映射目录）
    docker inspect  centos

    //注意：容器删除，容器目录也会删除
       
**方法二：**
    
    #映射本机指定目录 到docker目录
    docker run -it --name centos-2 -v /data:/data centos /bin/bash

    #centos-3 使用centos-2 的映射目录
    docker run -it --name centos-3 --volumes-from centos-2 centos /bin/bash

    //注意：源目录请使用完全路劲，使用相对路径会变成 容器下的目录映射（当容器删除目录会删除）

    //文件权限的问题： --privileged=true

###六、docker link（host别名） 的互联

**1、同主机下 容器之间的互联**

    #监听web服务
    docker run -d  --name web1  http-web  

    #将web1 映射到host 解析
    docker run -it --name test --link web1:host  centos /bin/bash
    
        #查看host
        cat  /etc/hosts

        #访问web1 / host 都能访问
        curl http://host  
        curl http://web1 

**2、不同主机 容器之间的互联**
     
###七、docker 网络之间的互联

**1、使用宿主机的网络**
    
    docker  run --rm=true --net=host --name=demo http-python

**2、使用指定容器的网络**

    docker  run --name=demo1 http-python
    docker  run --rm=true --name=demo2 --net=container:demo1 mysql /bin/bash

    #进入容器
    docker exec -it demo1
    #查看网络监听
    netstat -tlunp

    docker exec demo1 ip ad li
    docker exec demo2 ip ad li 
    //发现demo2 并没有创建自己的网络，使用的是demo1的网络

###八、docker容器网络机制

**1、linux路由机制打通网络**
- docker1：


    #配置网段
    vim /etc/docker/daemon.json 
    {
      "bip": "172.16.100.1/24"
    }

    #添加路由
    route add -net 172.16.200.0/24 gw 10.0.2.220
    or 
    ip route add 172.16.200.0/24 via 10.0.2.220

- docker2：


    #配置网段
    vim /etc/docker/daemon.json 
    {
      "bip": "172.16.200.1/24"
    }

    #添加路由
    route add -net 172.16.100.0/24 gw 10.0.2.221  
    or
    ip route add 172.16.100.0/24 via 10.0.2.221

**2、双网卡大二层**

**3、Overlay网络**
- Openvswitch（GRE/Vxlan）

    
    #安装openvswitch
    yum -y install openvswitch 
    systemctl start openvswitch

    #docker1
    
    ovs-vsctl add-br br0
    ovs-vsctl add-port br0 gre1 -- set interface gre1 type=gre option:remote_ip=10.0.2.220
    brctl addif docker0 br0
    brctl  show
    ip link set dev br0 up
    ip link set dev docker0 up

    #docker2

    ovs-vsctl add-br br0
    ovs-vsctl add-port br0 gre1 -- set interface gre1 type=gre option:remote_ip=10.0.2.220
    brctl addif docker0 br0
    brctl  show
    ip link set dev br0 up
    ip link set dev docker0 up
    
    #启动2个docker 容器测试网络连通性

    #脚本
    #######################################
    # The 'other' host
    REMOTE_IP=XXX.XXX.XXX.XXX
    # Name of the bridge
    BRIDGE_NAME=docker0
    # Bridge address
    BRIDGE_ADDRESS=XXX.XXX.XXX.XXX
    
    # Deactivate the docker0 bridge
    ip link set $BRIDGE_NAME down
    # Remove the docker0 bridge
    brctl delbr $BRIDGE_NAME
    # Delete the Open vSwitch bridge
    ovs-vsctl del-br br0
    # Add the docker0 bridge
    brctl addbr $BRIDGE_NAME
    # Set up the IP for the docker0 bridge
    ip a add $BRIDGE_ADDRESS dev $BRIDGE_NAME
    # Activate the bridge
    ip link set $BRIDGE_NAME up
    # Add the br0 Open vSwitch bridge
    ovs-vsctl add-br br0
    # Create the tunnel to the other host and attach it to the
    # br0 bridge
    ovs-vsctl add-port br0 gre0 -- set interface gre0 type=gre options:remote_ip=$REMOTE_IP
    # Add the br0 bridge to docker0 bridge
    brctl addif $BRIDGE_NAME br0
    
    # Some useful commands to confirm the settings:
    # ip a s
    # ip r s
    # ovs-vsctl show
    # brctl show

**4、neutron网络**

**5、官方的Libnetwork**


    


    
    
     
    
    