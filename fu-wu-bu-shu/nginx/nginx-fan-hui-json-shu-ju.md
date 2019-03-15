# nginx 返回指定数据

###  一、 返回json
**1、配置指定路劲返回相应json信息**

    location ~ ^/get_info {
        default_type application/json;
        return 200 '{"status":"success","result":"hello world!"}';
    }
    
    注意：当开发某个接口固定是一个返回值时，可以用此方法返回。节省后端处理过程
    
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
    
    
    #curl http://www.peter-zhou.com/get_info3 -I
    HTTP/1.1 200 OK
    Server: Nginx
    Date: Fri, 15 Mar 2019 06:21:58 GMT
    Content-Type: text/html; charset=utf-8
    Content-Length: 16
    Connection: keep-alive

### 三、根据url 返回数据

**1、配置匹配规则**
	
    location ~ ^/return/(.*)_(\d+).html$ {
        default_type text/html;
        set $string $1;
        set $data   $2;
        return 200 $string:$data;
    }
    
    location ~ ^/return/(.*)/(\d+)$ {
        default_type text/html;
        set $string $1;
        set $data $2;
        return 200 $string:$data;
    }
    
    注意：根据url参数http://xxx/test.html?name=xxx&id=xxx 同理也可以用这种方式匹配返回

**2、测试**

    #curl http://www.peter-zhou.com/return/test_01.html
    test:01
    
    #curl http://www.peter-zhou.com/return/aaa/123
    aaa:123
    
    