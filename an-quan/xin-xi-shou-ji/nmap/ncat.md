### ncat 使用场景
**1、简单传输数据**

    server:
        #ncat -v -lp 8000
        Ncat: Version 5.51 ( http://nmap.org/ncat )
        Ncat: Listening on 0.0.0.0:8000
        
        
        
        Ncat: Connection from 127.0.0.1:48960.
        hello

    client:
        # ncat localhost 8000
        
        hello
        
    这时双方都可以发送消息
    
**2、探测端口**

    
    # ncat localhost 80
    GET / HTTP/1.1
    
    HTTP/1.1 400 Bad Request
    Server: nginx/1.10.2
    Date: Wed, 27 Feb 2019 11:51:49 GMT
    Content-Type: text/html
    Content-Length: 173
    Connection: close
    
    <html>
    <head><title>400 Bad Request</title></head>
    <body bgcolor="white">
    <center><h1>400 Bad Request</h1></center>
    <hr><center>nginx/1.10.2</center>
    </body>
    </html>
    
    # ncat localhost 22
    SSH-2.0-OpenSSH_5.3

**3、文件传输**

    接收方：
        ncat -v -lp 8000 > test 
        
    发送方：      
        ncat localhost 8000 --send-only < test
        
**4、目录传输**

    发送方：
    
    接收方：
    
    
**5、压缩传输**

**6、反弹shell**

    server：
        ncat -v -c /bin/bash  -lp 8000
    
    client:
        ncat localhost 8000
        whoami
        root
     
**7、代理**

    #单向没有输出值
    ncat -l 8000 | ncat www.baidu.com 80
    
    ncat localhost 8000
    
    #利用管道做代理
    server:
        mkfifo  myfifo
        ncat -l 8000 <myfifo | ncat www.baidu.com >myfifo
    
    client:
        ncat localhost 8000
        HTTP/1.1 400 Bad Request





        
