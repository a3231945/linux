# Iptables详解 #
-----
## 语法 ##
    iptables （选项） （参数）
## 选项 ##
    iptables [-t table] {-A|-C|-D} chain rule-specification
    iptables [-t table] -I chain [rulenum] rule-specification
    iptables [-t table] -R chain rulenum rule-specification
    iptables [-t table] -D chain rulenum
    iptables [-t table] -S [chain [rulenum]]
    iptables [-t table] {-F|-L|-Z} [chain [rulenum]] [options...]
    iptables [-t table] -N chain
    iptables [-t table] -X [chain]
    iptables [-t table] -P chain target
    iptables [-t table] -E old-chain-name new-chain-name
    rule-specification = [matches...] [target]
    match = -m matchname [per-match-options]
    target = -j targetname [per-target-options]

    -t（表）：指定要操作的表
    -A:向规则链中添加条目
    -D:向规则链中删除条目
    -I:向规则链中插入条目
    -N:创建新的用户自定义规则链
    -X:删除用户自定义规则链
    
    -R:替换规则链中的条目
    -L:显示规则链中已有的条目
    -F:清除规则链中已有的条目
    -Z:清空规则链中的数据包计算器和字节计算器
    -P:定义规则链中的默认目标
    -p:指定要匹配的数据包协议类型
    -s:指定要匹配的数据包源IP地址
    -j:指定要跳转的目标
    -i:指定数据包进入本机的网络接口
    -o:指定数据包要离开本机锁使用的网络接口


### 表名 ###
- raw 高级功能，如：网址过滤
- mangle 数据包修改QOS ，用于实现服务质量
- nat 地址转换，用于网关路由器
- filter 包过滤，用于防火墙规则

### 规则链表名 ###
- INPUT链 处理输入数据包
- OUTPUT链 处理输出数据包
- PORWARD链 处理转发数据包
- PREROUTING链 用于目标地址转换（DNAT）
- POSTROUTING链 用于源地址转换（SNAT）

### 动作 ###
- accept 接受数据包
- DROP   丢弃数据包
- REDIRECT 重定向、映射、透明代理
- SNAT 源地址转换
- DNAT 目标地址转换
- MASQUERADE IP伪装，用于ADSL
- LOG 日志记录

### stat状态 ###
- ESTABLISHED  握手成功后 
- NEW  第一次建立连接发送的请求
- RELATED traceroute结束后的状态
- INVALID 状态不名的包


----
## 实例 ##
    基于源ip 限制
    iptables -t filter -A INPUT -s 192.168.1.1 -j DROP
    
    基于目标ip 限制
    iptables -t filter -A INPUT -d 192.168.1.10 -j DROP

    基于网段 限制
    iptables -t filter -A INPUT -s 192.168.1.0/24 -j DROP

    基于协议、端口 限制
    iptables -t filter -A INPUT -p tcp --dport 22 -j DROP

    基于状态 
    iptables -t filter -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
    
    基于多端口
    iptables -t filter -A INPUT -p tcp -m multiport 22,80 -j ACCEPT
    
    基于ip范围
    iptables -t filter -A INPUT -m iprange --src-range 192.168.1.10-192.168.1.20 -j DROP

    自定义链使用
    iptables -t filter  -N demo                         创建一个自定义链 
    iptables -t filter  -A demo -s 192.168.1.30 -j DROP 创建一个自定义链的规则 
    iptables -t filter  -A INPUT  -j demo               将自定义链作为INPUT 的一个操作

    添加描述
    iptables -t filter -A INPUT  -s 192.168.1.0/24 -m comment --comment "开放192.168.1.0段所有访问"  -j ACCEPT

    限制下载并发连接
    iptables -t filter -A FORWARD -p tcp 80 -m limit --limit 30/sec -j ACCEPT
    iptables -t filter -A FORWARD -j DROP

    限制上传并发连接数
    iptables -t filter -I FORWARD -p tcp -m connlimit --connlimit-above 20 -j REJECT
    iptables -t filter -I INPUT -p tcp -m connlimit --connlimit-above 20 -j REJECT


    #SNAT 192.168.160.101  使用 192.168.160.102 上网
    语法：iptables -t nat -A postrouting -s 内部网络地址或主机地址 -j SNAT --to-source NAT服务器上的某外部地址
    iptables -t nat -A POSTROUTING -s 192.168.160.101 -j SNAT --to-source 192.168.160.102

    #DNAT
    语法：iptables -t nat -A PREROUTING -d NAT服务器的某外部地址 -p 某协议 --dport 某端口 -j DNAT --to-destination 内网服务器地址[:port]
        

    #单网卡转发
    1、访问 192.168.160.101 8080  --> 转发到 192.168.160.102 80
    iptables -t nat -A PREROUTING -p tcp --dport 8080 -i eth0 -j DNAT --to 192.168.160.101:80
    2、源地址 192.168.160.101 源端口80 --> 192.168.160.102
    iptables -t nat -A POSTROUTING -p tcp --sport 80 -s 192.168.160.101 -j SNAT --to 192.168.160.102
    3、源地址 192.168.160.101 目标端口80 --> 192.168.160.102
    iptables -t nat -A POSTROUTING -p tcp --dport 80 -d 192.168.160.101 -j SNAT --to 192.168.160.102


    
    
    