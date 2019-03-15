# 文件下载防盗链

### 一、使用场景

    1、客户端发起下载申请--> app服务器(文件上传会生成自己格式的文件名)
    2、app服务器返回下载地址 --> 客户端
    3、客户端下载 --> web服务器(nginx)

### 二、nginx配置

**1、 开启secure_link 模块**

    #编译开启 --with-http_secure_link_module
    
**2、配置nginx**

    server {
        listen    80;
        server_name download.peter-zhou.com;
        
        location / {
            #设置md5 expires  
            secure_link $arg_md5,$arg_expires;
            
            #设置secret+url+expires
            secure_link_md5 "123456$uri$arg_expires";
  
        
            #比较哈希值
            if ($secure_link = "") {
                return 403;
            }
            
            #验证是否超时
            if ($secure_link = "0") {
                return 410;
            }
            
            #直接下载防止打开文件
            if ($request_filename ~* ^.*?\.(mp4|txt|jpg)$){
                add_header Content-Disposition 'attachment;';
            }

            #重命名
            add_header Content-Disposition "attachment;filename*=utf-8'zh_cn'$arg_filename";

        }
    }
    
**3、app服务器功能实现 ** 

实现返回客户端url为：`url+md5+expires+filename` 

    <?php

	$secret = '123456';
	$path   = 'xxxx';
	$expire = time()+300;
	
	$md5 = base64_encode(md5($secret.$path.$expire,true));
	$md5 = strtr($md5,'+/','-_');
	$md5 = str_replace('=','',$md5);
	echo 'http://download.peter-zhou.com/'.$path.'?md5='.$md5.'&expires='.$expire.'&filename='.$filename    
    

**例如：**

    http://down.peter-zhou.com/123.pdf?md5=FnDYyFzCooI9q8sh1Ffkxg&expires=1539847995&filename=test.pdf
    
    
    注意：md5、expires、filename 等参数均可以替换成别的名字，相应的nginx上的参数也替换
    
    
**官方文档**：[http://nginx.org/en/docs/http/ngx_http_secure_link_module.html](http://nginx.org/en/docs/http/ngx_http_secure_link_module.html)
        
        