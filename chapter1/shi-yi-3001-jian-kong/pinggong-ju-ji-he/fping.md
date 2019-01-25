###fping命令常用方法

**1、ping 多个地址**
    
    方法一:
    fping 192.168.1.1 192.168.1.2
    
    方法二:
    vim ip.txt
    www.baidu.com
    www.qq.com
    
    fping <ip.txt
    
    fping -f ip.txt
    

**2、ping IP区间**

    
    fping -g 192.168.1.1 192.168.1.100
    
**3、ping 网段**

    fping -g 192.168.1.0/24

**4、指定循环次数**

    fping -r 1 -g 192.168.1.0/24

**5、打印ping 成功|失败 地址**

    成功：
    fping -a -g 192.168.1.0/24
    
    失败：
    fping -u -g 192.168.1.0/24
    
**6、ping 指定大小**

    fping -b 60000 www.baidu.com 