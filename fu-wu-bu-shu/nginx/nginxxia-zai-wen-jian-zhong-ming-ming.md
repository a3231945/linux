# nginx 下载文件重命名

### 1、配置 文件类型
    location /download/ {
        if ($arg_n) {
	    #add_header Content-Disposition "attachment;filename=$arg_n";
	    add_header Content-Disposition "attachment;filename*=utf-8'zh_cn'$arg_n";
	}
    }
    
>注意:
>>配置： add_header Content-Disposition "attachment;filename=$arg_n";
chrome 正常，火狐浏览器会出现下载文件名乱码的问题，后端处理完后unicode 后还是一样的

>调整完：add_header Content-Disposition "attachment;filename*=utf-8'zh_cn'$arg_n";
chrome 正常，火狐也正常(意思是将文件名转换成utf-8中文)

### 2、测试

    #firefox
    http://www.peter-zhou.com/download/123.png?n=你好.png
    
    
    #chrome
    http://www.peter-zhou.com/download/123.png?n=你好.png

    



    
    