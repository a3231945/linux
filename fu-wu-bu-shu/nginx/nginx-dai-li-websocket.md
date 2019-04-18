# 代理websocket

### 一、配置nginx
			
    upstream wsbackend {
      server 127.0.0.1:8080;
      server 127.0.0.1:8081;
    }
    server {
      listen  80;
      server_name ws.peter-zhou.com;
      location / {
         proxy_pass http://wsbackend;
         proxy_http_version 1.1;
         proxy_set_header Upgrade $http_upgrade;
         proxy_set_header Connection "upgrade";
      }
    }
  
### 二、websocket 服务调试
**1、websocket server 实现（Python）**
**2、websocket client实现(html)**
### 三、测试
**1、跳过nginx代理访问**
**2、访问nginx代理**