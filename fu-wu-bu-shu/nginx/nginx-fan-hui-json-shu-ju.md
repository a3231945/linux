# nginx 返回指定数据

###  一、 返回json
**1、配置指定路劲返回相应json信息**

    location ~ ^/get_info {
        default_type application/json;
        return 200 '{"status":"success","result":"hello world!"}';
    }
    
**2、测试**

    # curl http://www.peter-zhou.com/get_info
    {"status":"success","result":"hello world!"}
    
### 二、返回text
**1、配置指定路劲返回相应text信息**

    location ~ ^/get_info1 {
        default_type text/html;
        return 200 'hello world!';
    }

    location ~ ^/get_info2 {
        default_type text/html;
        return 200 '你好，世界!';
    }
    
    location ~ ^/get_info3 {
        default_type text/html;
        add_header Content-Type 'text/html; charset=utf-8'; 
        return 200 '你好，世界!';
    }
    
    注意：当有些浏览器默认用gbk 来解析就会出现中文乱码，这时候需要添加header转换为utf-8
**2、测试**

    # curl http://www.peter-zhou.com/get_info1
    hello world!
    

    #curl http://www.peter-zhou.com/get_info2
    你好，世界!
    
    
    #curl http://www.peter-zhou.com/get_info3
    你好，世界!