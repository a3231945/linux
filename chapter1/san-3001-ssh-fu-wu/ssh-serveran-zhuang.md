###一、安装SSH
    
    通常情况下，安装系统，默认就安装上ssh-server 服务
    
    yum -y install openssh-server
    
###二、SSH 配置
    
    [root@nfs-node2 ~]# vim /etc/ssh/sshd_config 
    ###注意： /etc/ssh/ssh_config  为客户端配置文件
    
    #定义端口
    13  Port 22
    
    #关闭dns解析
    122 UseDNS no
    
    #关闭GSSAPI 验证（通常使用 密码 或者 KEY 的验证方式）
    80  GSSAPIAuthentication no


    
    

    

###三、配置防火墙允许访问dns服务

    [root@nfs-node2 ~]# iptables -I INPUT -p udp -m state --state NEW -m udp --dport 22 -j ACCEPT 
    [root@nfs-node2 ~]# iptables -I INPUT -p udp -m state --state NEW -m tcp --dport 22 -j ACCEPT 

###四、启动服务、开机自启动
    
    [root@nfs-node2 ~]# /etc/init.d/sshd start
    [root@nfs-node2 ~]# chkconfig sshd on


###五、SSH 配置详解


    Port    22                  #默认端口
    ListenAddress IP            #监听服务器端的IP，ss -ntl 查看22端口绑定的iP地址
    LoginGraceTime 2m           #登录时不输入密码时超时时间
    HostKey                   # HostKey本地服务端的公钥路径
    UseDNS no                   #禁止将IP逆向解析为主机名，然后比对正向解析的结果，防止客户端欺骗
    PermitRootLogin yes         #是否允许root使用SSH远程登录
    MaxAuthTries 6              #密码错误的次数6/2=3(MAN帮助中写明要除2)次后断开连接
    MaxSessions 10              #最大的会话连接数(连接未登录的会话最大值，默认拒绝旧的连接未登录的会话)
    StrictModes yes             #检查用户家目录中ssh相关的配置文件是否正确
    PubkeyAuthentication yes    #是否使用基于key验证登录
    AuthorizedKeysFile      .ssh/authorized_keys    #key验证登录的客户端公钥路径
    PasswordAuthentication yes  #是否允许使用密码登录
    PermitEmptyPasswords no     #用户使用空口令登录
    GatewayPorts no             #启用网关功能，开启后可以将建立的SSH隧道(端口转发)共享出去
    ClientAliveCountMax 3       #探测3次客户端是否为空闲会话，↓3*10分钟后断开连接
    ClientAliveInterval 10      #空闲会话时长，每10分钟探测一次
    MaxStartups 10:30:100       #start:rate:full；当连接但为进行认证的用户超过10个，drop30%(rate/full)的连接当连接但未登录的连接达到100个后，新建立的连接将被拒绝
    Banner /path/file           #认证前输出的登录提示信息，指定文件路径
    GSSAPIAuthentication no 
    AllowUsers username         #白名单，如果白名单有用户只有白名单的用户可以登陆
    DenyUsers                  #黑名单，被拒绝的用户，如果即允许又拒绝则拒绝生效
    AllowGroups                 #组白名单
    DenyGroups                  #组黑名单

###六、SSH 安全相关

SSH也可能成为DOS攻击的对象，例如恶意用户连接SSH但不输入密码进行验证，由于设置了MaxStartups会导致正常用户无法进行登录。针对此情况建议：

    修改默认端口
    MaxStartups 调大一些例如 MaxStartups 100:30:1000
    LoginGraceTime 10 调整连接超时未10秒
    MaxSessions 10 设置连接但未登录的用户最大值为10

- 其他优化


    限制可登录用户
    设定空闲会话超时时长
    充分利用防火墙设置ssh访问策略
    仅监听指定IP的ssh
    禁止使用空口令登录
    禁止使用root直接进行登录
    做好日志分析
    加强用户登录的密码口令


    