###httping命令常用方法

**1、探测站点**

    域名：httping  www.baidu.com
          httping -h 61.135.169.121
    ip： httping  61.135.169.121 
         httping -h 61.135.169.121
    url：httping -g http://www.baidu.com
    
**2、指定次数**

    httping -c10  -g http://www.baidu.com
    
**3、输出http code **

    httping -g  http://www.baidu.com -s
    
**4、输出颜色**
    
    httping -g http://www.baidu.com -s -Y
    
**5、代理**

    httping -x IP:PORT  -g http://www.baidu.com
    
**6、不缓存**

    httping -Z -g http://www.baidu.com
    httping --no-cache -g http://www.baidu.com

**7、指定referer**

    httping -R http://m.baidu.com  -g http://www.baidu.com
    httping --referer http://m.baidu.com  -g http://www.baidu.com

**8、指定UA**

    httping -I "my UA"-g http://www.baidu.com
    httping --user-agent "my UA" -g http://www.baidu.com

**9、输出每个阶段时间**
    
    httping -g http://www.baidu.com -S
    
    resolve, connect, send, etc
    
    PING www.baidu.com:80 (/):
    connected to 61.135.169.121:80 (312 bytes), seq=0 time=  2.18+  1.38+  1.72+  1.63+  0.04=  6.93 ms 
    connected to 61.135.169.125:80 (312 bytes), seq=1 time=  1.56+  1.29+  2.89+  2.71+  0.04=  8.45 ms 
    connected to 61.135.169.125:80 (312 bytes), seq=2 time=  2.07+  3.32+  1.75+  2.04+  0.03=  9.18 ms 
