###一、安装软件包

    yum -y install python-pip
    
    pip install --upgrade pip
    pip install shadowsocks

###二、定义配置文件

    vim /etc/shadowsocks.json
    {
    "server":"172.104.xxx.xxx",
    "server_port":18388,
    "local_address": "127.0.0.1",
    "local_port":10801,
    "password": {
    	"8000":"XXXX",
    	"8001":"XXXX"
    }
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false,
    "workers": 1
    } 

* [ ] **注意: 指定服务器自己的IP和密码** 


###三、服务管理

     #启动
     ssserver -c /etc/shadowsocks.json -d start 
     
     #停止
     ssserver -c /etc/shadowsocks.json -d stop
     
     #重启
     ssserver -c /etc/shadowsocks.json -d restart
     
