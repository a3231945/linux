# Nali 工具使用 #


### 一、安装 ###
    #解压包
    tar xf  nali-0.2.tar.gz 
    cd nali-0.2
    ./configure 
    make  && make install

    
### 二、使用手册 ###
**1、添加环境变量**

    vim  /etc/proflie.d/nali.sh
    PATH=$PATH:/usr/local/nali/bin/
    
    #重新加载变量
    source /etc/profile.d/nali.sh

**2、更新IP数据库**

    手动更新
    nali-update

    设置定时更新：每天1点更新
    crontab -e
    ------
    #更新IP库
    0 1 * * *  nali-update

**3、nali命令**
    
    [root@rsync-node1 share]# nali 202.106.0.20
    202.106.0.20[北京市 联通DNS服务器]

**4、nali-dig 命令**

    [root@rsync-node1 share]# nali-dig www.baidu.com
    ; <<>> DiG 9.8.2rc1-RedHat-9.8.2-0.47.rc1.el6 <<>> www.baidu.com
    ;; global options: +cmd
    ;; Got answer:
    ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 48659
    ;; flags: qr rd ra; QUERY: 1, ANSWER: 3, AUTHORITY: 0, ADDITIONAL: 0
    
    ;; QUESTION SECTION:
    ;www.baidu.com.			IN	A
    
    ;; ANSWER SECTION:
    www.baidu.com.		980	IN	CNAME	www.a.shifen.com.
    www.a.shifen.com.	36	IN	A	61.135.169.125[北京市 百度蜘蛛]
    www.a.shifen.com.	36	IN	A	61.135.169.121[北京市 百度蜘蛛]
    
    ;; Query time: 92 msec
    ;; SERVER: 202.106.0.20[北京市 联通DNS服务器]#53(202.106.0.20[北京市 联通DNS服务器])
    ;; WHEN: Thu Apr 27 11:38:32 2017
    ;; MSG SIZE  rcvd: 90


**5、nali-nslookup 命令**
    
    [root@rsync-node1 share]# nali-nslookup   www.baidu.com
    Server:		202.106.0.20[北京市 联通DNS服务器]
    Address:	202.106.0.20[北京市 联通DNS服务器]#53
    
    Non-authoritative answer:
    www.baidu.com	canonical name = www.a.shifen.com.
    Name:	www.a.shifen.com
    Address: 61.135.169.121[北京市 百度蜘蛛]
    Name:	www.a.shifen.com
    Address: 61.135.169.125[北京市 百度蜘蛛]

**6、nali-traceroute 命令**

    [root@rsync-node1 share]# nali-traceroute   www.baidu.com
    traceroute to www.baidu.com (61.135.169.125[北京市 百度蜘蛛]), 30 hops max, 60 byte packets
    1  bogon (192.168.160.2[局域网 对方和您在同一内部网])  0.172 ms  0.146 ms  0.131 ms
    2  * * *
    3  * * *

**7、nali-tracepath 命令**

    [root@rsync-node1 share]# nali-tracepath  www.baidu.com
     1?: [LOCALHOST]     pmtu 1500 
     1:  bogon (192.168.160.2[局域网 对方和您在同一内部网])                                  0.163ms 
     2:  no reply
     3:  no reply

    

**8、nali-ping 命令**

    [root@rsync-node1 httpd]# nali-ping www.baidu.com
    PING www.a.shifen.com (61.135.169.121[北京市 百度蜘蛛]) 56(84) bytes of data.
    64 bytes from 61.135.169.121[北京市 百度蜘蛛]: icmp_seq=1 ttl=128 time=36.3 ms
    64 bytes from 61.135.169.121[北京市 百度蜘蛛]: icmp_seq=2 ttl=128 time=37.8 ms
    64 bytes from 61.135.169.121[北京市 百度蜘蛛]: icmp_seq=3 ttl=128 time=42.1 ms
    64 bytes from 61.135.169.121[北京市 百度蜘蛛]: icmp_seq=4 ttl=128 time=40.4 ms


**9、其他调用**
       
    [root@rsync-node1 httpd]# ping www.baidu.com | nali
    PING www.a.shifen.com (61.135.169.121[北京市 百度蜘蛛]) 56(84) bytes of data.
    64 bytes from 61.135.169.121[北京市 百度蜘蛛]: icmp_seq=1 ttl=128 time=46.0 ms
    64 bytes from 61.135.169.121[北京市 百度蜘蛛]: icmp_seq=2 ttl=128 time=44.2 ms
    64 bytes from 61.135.169.121[北京市 百度蜘蛛]: icmp_seq=3 ttl=128 time=47.8 ms
    64 bytes from 61.135.169.121[北京市 百度蜘蛛]: icmp_seq=4 ttl=128 time=48.6 ms

    
    


    

    