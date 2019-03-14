# 浏览器不自动解析播放文件

### 1、配置 文件类型为 流
    location /download/ {
      types        { }
      default_type application/octet-stream;
    }
    
### 2、测试
主要观察:`Content-Type: application/octet-stream` 字段

    1 - 火狐浏览器
    Server	Nginx
    Date	Thu, 14 Mar 2019 10:33:33 GMT
    Content-Type    application/octet-stream
    Content-Length	33855
    Last-Modified	Wed, 06 Mar 2019 21:16:18 GMT
    Connection	keep-alive
    ETag	"5c8038a2-843f"
    Expires    Sat, 13 Apr 2019 10:33:33 GMT
    Cache-Control	max-age=2592000
    Accept-Ranges	bytes
    
    #注意：火狐浏览器会自动播放，这个是浏览器的特性。
    
    2 - chrome
    Accept-Ranges: bytes
    Cache-Control: max-age=2592000
    Connection: keep-alive
    Content-Length: 33855
    Content-Type: application/octet-stream
    Date: Thu, 14 Mar 2019 10:35:25 GMT
    ETag: "5c8038a2-843f"
    Expires: Sat, 13 Apr 2019 10:35:25 GMT
    Last-Modified: Wed, 06 Mar 2019 21:16:18 GMT
    Server: Nginx

    #注意：chrome 添加前，和添加后都不会播放
    
    3 - curl
    
    # curl http://www.peter-zhou.com/download/xxx.mp4 -I
    HTTP/1.1 200 OK
    Server: Nginx
    Date: Thu, 14 Mar 2019 10:32:07 GMT
    Content-Type: application/octet-stream
    Content-Length: 33855
    Last-Modified: Wed, 06 Mar 2019 21:16:18 GMT
    Connection: keep-alive
    ETag: "5c8038a2-843f"
    Expires: Sat, 13 Apr 2019 10:32:07 GMT
    Cache-Control: max-age=2592000
    Accept-Ranges: bytes

    
    