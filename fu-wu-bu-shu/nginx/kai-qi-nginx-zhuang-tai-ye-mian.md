## 开启nginx状态页面

### 一、安装指定模块


        nginx -V 查看是否安装 
                --with-http_stub_status_module 模块
                
        #编译安装时添加如下参数
        ./configure --with-http_stub_status_module

### 二、配置
**1、配置实例 **    
 
        server {
                listen 80;
                server_name 127.0.0.1;
        
                location /status {
                        stub_status on;
                        access_log off;
                        allow 127.0.0.1;
                        deny all;
                }
        } 
        
**2、检查结果**
        
        curl http://127.0.0.1/status
        
        Active connections: 1 
        server accepts handled requests
         687 687 1726 
        Reading: 0 Writing: 1 Waiting: 0 
        
**3、参数详解**

        Active connections: 当前nginx正在处理的活动连接数.
        Server accepts handled requests request_time: nginx总共处理了687 个连接,成功创建687 握手(证明中间没有失败的),总共处理了1726 个请求
        Reading: nginx读取到客户端的Header信息数.
        Writing: nginx返回给客户端的Header信息数.
        Waiting: 开启keep-alive的情况下,这个值等于 active – (reading + writing),意思就是nginx已经处理完成,正在等候下一次请求指令的驻留连接。
        
        所以,在访问效率高,请求很快被处理完毕的情况下,Waiting数比较多是正常的.如果reading +writing数较多,则说明并发访问量非常大,正在处理过程中。
