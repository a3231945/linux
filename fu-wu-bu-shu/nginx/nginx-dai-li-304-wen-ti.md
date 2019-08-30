# nginx 代理304问题

**1、配置代理不检查304**

    location ^~ /test/ {
        proxy_pass http://10.1.1.1/;
        proxy_set_header Host  test.peter-zhou.com;
        proxy_set_header If-Modified-Since "";
        proxy_set_header If-None-Match "";
    }
    
**2、测试**
    
    网页打开http://xxx.xxx.xxx/test/test.html 两次
    检查http code 是否一直为200



