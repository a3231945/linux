## nmap

### 一、nmap 安装

    yum -y install nmap

### 二、nmap 使用
**1、基本使用**

> 综合扫描 127.0.0.1

    # nmap  -A 127.0.0.1

    Starting Nmap 5.51 ( http://nmap.org ) at 2019-02-25 16:29 CST
    Nmap scan report for localhost (127.0.0.1)
    Host is up (0.000014s latency).
    Not shown: 998 closed ports
    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 5.3 (protocol 2.0)
    | ssh-hostkey: 1024 b0:74:52:e9:4f:78:be:67:0f:0f:c2:bd:71:5a:df:90 (DSA)
    |_2048 1e:07:c5:b6:c5:92:48:c1:2a:4d:22:6c:fa:95:a7:62 (RSA)
    80/tcp open  http    nginx 1.10.2
    |_http-methods: No Allow or Public header in OPTIONS response (status code 405)
    |_http-title: Test Page for the Nginx HTTP Server on EPEL
    No exact OS matches for host (If you know what OS is running on it, see http://nmap.org/submit/ ).
    TCP/IP fingerprint:
    OS:SCAN(V=5.51%D=2/25%OT=22%CT=1%CU=36830%PV=N%DS=0%DC=L%G=Y%TM=5C73A769%P=
    OS:x86_64-redhat-linux-gnu)SEQ(SP=106%GCD=1%ISR=107%TI=Z%CI=Z%II=I%TS=A)OPS
    OS:(O1=MFFD7ST11NW7%O2=MFFD7ST11NW7%O3=MFFD7NNT11NW7%O4=MFFD7ST11NW7%O5=MFF
    OS:D7ST11NW7%O6=MFFD7ST11)WIN(W1=FFCB%W2=FFCB%W3=FFCB%W4=FFCB%W5=FFCB%W6=FF
    OS:CB)ECN(R=Y%DF=Y%T=40%W=FFD7%O=MFFD7NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A
    OS:=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%
    OS:Q=)T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=
    OS:A%A=Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=
    OS:Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%
    OS:T=40%CD=S)
    
    Network Distance: 0 hops
    
    OS and Service detection performed. Please report any incorrect results at http://nmap.org/submit/ .
    Nmap done: 1 IP address (1 host up) scanned in 17.74 seconds


>综合扫描 www.baidu.com  
  
    #nmap -A www.baidu.com
    
    Starting Nmap 5.51 ( http://nmap.org ) at 2019-02-25 16:21 CST
    Nmap scan report for www.baidu.com (61.135.169.121)
    Host is up (0.0020s latency).
    Other addresses for www.baidu.com (not scanned): 61.135.169.125
    Not shown: 998 filtered ports
    PORT    STATE SERVICE  VERSION
    80/tcp  open  upnp     Microsoft Windows UPnP
    |_http-methods: No Allow or Public header in OPTIONS response (status code 302)
    | http-robots.txt: 9 disallowed entries 
    | /baidu /s? /ulink? /link? /home/news/data/ /shifen/ 
    |_/homepage/ /cpro /
    |_http-favicon: 
    443/tcp open  ssl/upnp Microsoft Windows UPnP
    |_http-methods: No Allow or Public header in OPTIONS response (status code 302)
    | http-robots.txt: 9 disallowed entries 
    | /baidu /s? /ulink? /link? /home/news/data/ /shifen/ 
    |_/homepage/ /cpro /
    |_http-favicon: 
    Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
    Device type: switch
    Running (JUST GUESSING): HP embedded (86%)
    Aggressive OS guesses: HP 4000M ProCurve switch (J4121A) (86%)
    No exact OS matches for host (test conditions non-ideal).
    Network Distance: 10 hops
    Service Info: OS: Windows
    
    TRACEROUTE (using port 80/tcp)
    HOP RTT      ADDRESS
    1   4.65 ms  localhost (10.0.2.1)
    2   1.86 ms  localhost (172.16.31.2)
    3   4.53 ms  124.65.136.145
    4   21.93 ms 124.65.217.249
    5   ...
    6   3.82 ms  bt-230-081.bta.net.cn (202.106.230.81)
    7   2.02 ms  124.65.59.222
    8   2.43 ms  202.106.43.30
    9   ...
    10  1.40 ms  61.135.169.121
    
    OS and Service detection performed. Please report any incorrect results at http://nmap.org/submit/ .
    Nmap done: 1 IP address (1 host up) scanned in 31.80 seconds

> 指定多IP扫描

    nmap 172.16.0.1-254
    nmap 172.16.0.1/24

**2、主机发现**

>-sP ping扫描

    #nmap -sP 127.0.0.1
    
    Starting Nmap 5.51 ( http://nmap.org ) at 2019-02-26 16:36 CST
    Nmap scan report for localhost (127.0.0.1)
    Host is up.
    Nmap done: 1 IP address (1 host up) scanned in 0.00 seconds
    
>-P0 无ping扫描

    #nmap -P0 127.0.0.1
    
    Starting Nmap 5.51 ( http://nmap.org ) at 2019-02-26 16:38 CST
    Nmap scan report for localhost (127.0.0.1)
    Host is up (0.0000040s latency).
    Not shown: 998 closed ports
    PORT   STATE SERVICE
    22/tcp open  ssh
    80/tcp open  http
    
>-PS TCP SYN Ping 扫描
    
    #nmap -PS80,22,100-200 -v 127.0.0.1
    
    Starting Nmap 5.51 ( http://nmap.org ) at 2019-02-26 16:42 CST
    Initiating SYN Stealth Scan at 16:42
    Scanning localhost (127.0.0.1) [1000 ports]
    Discovered open port 80/tcp on 127.0.0.1
    Discovered open port 22/tcp on 127.0.0.1
    Completed SYN Stealth Scan at 16:42, 0.01s elapsed (1000 total ports)
    Nmap scan report for localhost (127.0.0.1)
    Host is up (0.0000040s latency).
    Not shown: 998 closed ports
    PORT   STATE SERVICE
    22/tcp open  ssh
    80/tcp open  http
    
    Read data files from: /usr/share/nmap
    Nmap done: 1 IP address (1 host up) scanned in 0.03 seconds
               Raw packets sent: 1000 (44.000KB) | Rcvd: 2002 (84.088KB)

>-PA TCP ACK Ping 扫描
    
    # nmap  -PA -v 127.0.0.1

    Starting Nmap 5.51 ( http://nmap.org ) at 2019-02-26 16:57 CST
    Initiating SYN Stealth Scan at 16:57
    Scanning localhost (127.0.0.1) [1000 ports]
    Discovered open port 22/tcp on 127.0.0.1
    Discovered open port 80/tcp on 127.0.0.1
    Completed SYN Stealth Scan at 16:57, 0.01s elapsed (1000 total ports)
    Nmap scan report for localhost (127.0.0.1)
    Host is up (0.0000040s latency).
    Not shown: 998 closed ports
    PORT   STATE SERVICE
    22/tcp open  ssh
    80/tcp open  http
    
    Read data files from: /usr/share/nmap
    Nmap done: 1 IP address (1 host up) scanned in 0.03 seconds
               Raw packets sent: 1000 (44.000KB) | Rcvd: 2002 (84.088KB)

    
>-PU UDP Ping 扫描
    
    # nmap  -PU  127.0.0.1

    Starting Nmap 5.51 ( http://nmap.org ) at 2019-02-26 16:58 CST
    Nmap scan report for localhost (127.0.0.1)
    Host is up (0.0000040s latency).
    Not shown: 998 closed ports
    PORT   STATE SERVICE
    22/tcp open  ssh
    80/tcp open  http
    
    Nmap done: 1 IP address (1 host up) scanned in 0.03 seconds
    
    
>-PE;-PP;-PM
    
    PE: ICMP Echo 扫描
    PP: ICMP 时间戳ping 扫描
    PM: ICMP 地址子网掩码Ping 扫描
    
    #nmap -PE 127.0.0.1
    #nmap -PP 127.0.0.1
    #nmap -PM 127.0.0.1 
    
>-PR ARP Ping 扫描
    
    # nmap  -PR 127.0.0.1

    Starting Nmap 5.51 ( http://nmap.org ) at 2019-02-26 17:06 CST
    Nmap scan report for localhost (127.0.0.1)
    Host is up (0.0000040s latency).
    Not shown: 998 closed ports
    PORT   STATE SERVICE
    22/tcp open  ssh
    80/tcp open  http
    
    Nmap done: 1 IP address (1 host up) scanned in 0.03 seconds

>-n 禁止DNS反向解析
    
    #nmap  -n www.baidu.com

    Starting Nmap 5.51 ( http://nmap.org ) at 2019-02-26 16:49 CST
    Nmap scan report for www.baidu.com (61.135.169.121)
    Host is up (0.0015s latency).
    Other addresses for www.baidu.com (not scanned): 61.135.169.125
    Not shown: 998 filtered ports
    PORT    STATE SERVICE
    80/tcp  open  http
    443/tcp open  https
    
    
>-R 反向解析域名
    
    # nmap  -R -sL 127.0.0.1

    Starting Nmap 5.51 ( http://nmap.org ) at 2019-02-26 16:54 CST
    Nmap scan report for localhost (127.0.0.1)
    Nmap done: 1 IP address (0 hosts up) scanned in 0.00 seconds

    
> --system-dns 使用系统域名解析器
    
    # nmap  --system-dns 127.0.0.1 127.0.0.2

    Starting Nmap 5.51 ( http://nmap.org ) at 2019-02-26 16:55 CST
    Nmap scan report for localhost (127.0.0.1)
    Host is up (0.0000040s latency).
    Not shown: 998 closed ports
    PORT   STATE SERVICE
    22/tcp open  ssh
    80/tcp open  http
    
    Nmap done: 2 IP addresses (1 host up) scanned in 3.04 seconds
    
    
>--sL 列表扫描
    
    # nmap -sL 127.0.0.1 10.0.0.1

    Starting Nmap 5.51 ( http://nmap.org ) at 2019-02-26 16:56 CST
    Nmap scan report for localhost (127.0.0.1)
    Nmap scan report for localhost (10.0.0.1)
    Nmap done: 2 IP addresses (0 hosts up) scanned in 0.00 seconds

>-6 扫描IPV6地址
    
    # nmap  -6 ::1

    Starting Nmap 5.51 ( http://nmap.org ) at 2019-02-26 16:51 CST
    Nmap scan report for localhost (::1)
    Host is up (0.00011s latency).
    Not shown: 998 closed ports
    PORT   STATE SERVICE
    22/tcp open  ssh
    80/tcp open  http
    
    Nmap done: 1 IP address (1 host up) scanned in 0.03 seconds
    
    
>--traceroute 路由跟踪
    
    # nmap  --traceroute 10.0.0.1

    Starting Nmap 5.51 ( http://nmap.org ) at 2019-02-26 16:52 CST
    Nmap scan report for localhost (10.0.0.1)
    Host is up (0.042s latency).
    Not shown: 995 closed ports
    PORT    STATE    SERVICE
    21/tcp  filtered ftp
    22/tcp  open     ssh
    23/tcp  filtered telnet
    80/tcp  open     http
    443/tcp open     https
    
    TRACEROUTE (using port 1723/tcp)
    HOP RTT      ADDRESS
    1   79.48 ms localhost (10.0.0.1)
    
    Nmap done: 1 IP address (1 host up) scanned in 9.77 seconds

    
>-PY SCTP INIT Ping扫描
    
    nmap  -PY -v 127.0.0.1

    Starting Nmap 5.51 ( http://nmap.org ) at 2019-02-26 16:53 CST
    Initiating SYN Stealth Scan at 16:53
    Scanning localhost (127.0.0.1) [1000 ports]
    Discovered open port 80/tcp on 127.0.0.1
    Discovered open port 22/tcp on 127.0.0.1
    Completed SYN Stealth Scan at 16:53, 0.01s elapsed (1000 total ports)
    Nmap scan report for localhost (127.0.0.1)
    Host is up (0.0000040s latency).
    Not shown: 998 closed ports
    PORT   STATE SERVICE
    22/tcp open  ssh
    80/tcp open  http
    
    Read data files from: /usr/share/nmap
    Nmap done: 1 IP address (1 host up) scanned in 0.03 seconds
               Raw packets sent: 1000 (44.000KB) | Rcvd: 2002 (84.088KB)

    
    

**3、探索网络**
> -T 时序选项

    -T0(偏执的)非常慢的扫描，用于IDS逃避
    -T1(鬼祟的)缓慢的扫描，用于IDS逃避
    -T2(文雅的)降低速度以降低对带宽的消耗
    -T3(普通的)默认，根据目标反自动调整时间
    -T4(野蛮的)快速扫描，常用
    -T5(疯狂的)
    #nmap -T0 127.0.0.1
    #nmap -T1 127.0.0.1

> -p 
> -sS
> -sT
> -sU
> -sN;-sF;-sX
> -sA
> -sW
> -sM
> --scanflags
> -sI
> -sO
> -b
        
**4、指纹识别与探测**
**5、伺机而动**
**6、防火墙、IDS逃逸**
**7、信息收集**
**8、数据库渗透测试**
**9、渗透测试**
**10、nmap技巧**
**11、nmap保存和输出**

    