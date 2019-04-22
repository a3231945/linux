# 代理websocket

### 一、配置nginx
			
    upstream wsbackend {
      server 127.0.0.1:10000;
      server 127.0.0.1:10000;
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

    #! /usr/bin/env python  
    # -*- coding: utf-8 -*-  
    # vim:fenc=utf-8  
    #  
    # pip install bottle_websocket  
    # pip install bottle  
    from bottle import get, run, template  
    from bottle.ext.websocket import GeventWebSocketServer  
    from bottle.ext.websocket import websocket  
    import gevent  
    users = set()  
    @get('/')  
    def index():  
        return template('index')  
    @get('/websocket', apply=[websocket])  
    def chat(ws):  
        users.add(ws)  
        while True:  
            msg = ws.receive()  
            if msg is not None:  
                for u in users:  
                    print type(u)  
                    u.send(msg)  
                    print u,msg  
            else: break  
        users.remove(ws)  
    run(host='xxx.xxx.xxx.xxx', port=10000, server=GeventWebSocketServer) 
    
**2、websocket client实现(html)**

    <html>  
    <head>  
    <script type="text/javascript" src="http://cdn.staticfile.org/jquery/1.11.0/jquery.min.js"></script>  
    <script>  
            $(document).ready(function() {  
                if (!window.WebSocket) {  
                    if (window.MozWebSocket) {  
                        windowwindow.WebSocket = window.MozWebSocket;  
                    } else {  
                        $('#messages').append("<li>Your browser doesn't support WebSockets.</li>");  
                    }  
                }  
                ws = new WebSocket('ws://xxx.xxx.xxx.xxx:10000/websocket');  
                ws.onopen = function(evt) {  
                    $('#messages').append('<li>Connected to chat.</li>');  
                }  
                ws.onmessage = function(evt) {  
                    $('#messages').append('<li>' + evt.data + '</li>');  
                }  
                $('#send-message').submit(function() {  
                    ws.send($('#name').val() + ": " + $('#message').val());  
                    $('#message').val('').focus();  
                    return false;  
                });  
            });  
        </script>  
    </head>  
    <body>  
    <form id="send-message" class="form-inline">  
            <input id="name" type="text" value="name">  
            <input id="message" type="text" value="contain" />  
           &nbsp; <button class="btn btn-success" type="submit">Send</button>  
        </form>  
        <div id="messages"></div>  
    </body>  
    </html> 
### 三、测试
**1、跳过nginx代理访问**
    
    直接修改为websocket地址，打开两个页面看是否能互相通信
    
**2、访问nginx代理**

    修改为nginx上下游的域名，打开两个页面看是否能互相通信