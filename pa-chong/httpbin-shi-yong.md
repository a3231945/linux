# httpbin 使用

### 一、httpbin 介绍

httpbin这个网站能测试 HTTP 请求和响应的各种信息，比如 cookie、ip、headers 和登录验证等，且支持 GET、POST 等多种方法，对 web 开发和测试很有帮助。它用 Python + Flask 编写，是一个开源项目。

官方网站：[http://httpbin.org/](http://httpbin.org/)

开源地址：[https://github.com/Runscope/httpbin](https://github.com/Runscope/httpbin)



### 二、httpbin 安装

```
docker pull kennethreitz/httpbin
docker run -p 80:80 kennethreitz/httpbin
```



### 三、httpbin 使用

**1、http 请求方法**

| 请求方式 | url     | 备注 |
| -------- | ------- | ---- |
| DELETE   | /delete |      |
| GET      | /get    |      |
| PATCH    | /patch  |      |
| POST     | /post   |      |
| PUT      | /put    |      |



```
#模拟get 请求
# curl -XGET  http://httpbin.org/get
{
  "args": {}, 
  "headers": {
    "Accept": "*/*", 
    "Host": "httpbin.org", 
    "User-Agent": "curl/7.29.0", 
    "X-Amzn-Trace-Id": "Root=1-5ffff632-0fc3f828798d4ea17a6aeca9"
  }, 
  "origin": "49.233.250.178", 
  "url": "http://httpbin.org/get"
}

#模拟post 请求
# curl -XPOST  http://httpbin.org/post

#模拟delete 请求
# curl -XDELETE  http://httpbin.org/delete

#模拟put 请求
# curl -XPUT  http://httpbin.org/put

#模拟patch 请求
# curl -XPATCH  http://httpbin.org/patch
```

**2、http 认证**

| 请求方式 | url                                                          | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| GET      | /basic-auth/{user}/{passwd}                                  |      |
| GET      | /bearer                                                      |      |
| GET      | /digest-auth/{qop}/{user}/{passwd}                           |      |
| GET      | /digest-auth/{qop}/{user}/{passwd}/{algorithm}               |      |
| GET      | /digest-auth/{qop}/{user}/{passwd}/{algorithm}/{stale_after} |      |
| GET      | /hidden-basic-auth/{user}/{passwd}                           |      |

```
#base-auth

```

**3、http 状态码**

| 请求方式 | url           | 备注 |
| -------- | ------------- | ---- |
| DELETE   | /status/codes |      |
| GET      | /status/codes |      |
| PATCH    | /status/codes |      |
| POST     | /status/codes |      |
| PUT      | /status/codes |      |

```
#get 请求408
# curl   http://httpbin.org/status/408 -I
HTTP/1.1 408 REQUEST TIMEOUT
Date: Thu, 14 Jan 2021 09:03:11 GMT
Content-Type: text/html; charset=utf-8
Content-Length: 0
Connection: keep-alive
Server: gunicorn/19.9.0
Access-Control-Allow-Origin: *
Access-Control-Allow-Credentials: true

```

**4、http 请求头检查**

| 请求方式 | url         | 备注 |
| -------- | ----------- | ---- |
| GET      | /ip         |      |
| GET      | /headers    |      |
| GET      | /user-agent |      |

```
#获取ip
# curl   http://httpbin.org/ip
{
  "origin": "49.233.250.178"
}

#获取headers
# curl   http://httpbin.org/headers
{
  "headers": {
    "Accept": "*/*", 
    "Host": "httpbin.org", 
    "User-Agent": "curl/7.29.0", 
    "X-Amzn-Trace-Id": "Root=1-60000903-391514281b4af9d352dc3f8d"
  }
}

#获取ua
# curl   http://httpbin.org/user-agent
{
  "user-agent": "curl/7.29.0"
}
```

**5、http 响应头检查**

| 请求方式 | url               | 备注 |
| -------- | ----------------- | ---- |
| GET      | /cache            |      |
| GET      | /cache/{value}    |      |
| GET      | /etag/{etag}      |      |
| GET      | /response-headers |      |
| POST     | /response-headers |      |

```
#获取cache
curl   http://httpbin.org/cache -I

#获取指定cache
curl   http://httpbin.org/cache/600 -I

#获取etag
curl   http://httpbin.org/etag/50b1c1d4f775c61:df3 -I

#get获取response
curl  http://httpbin.org/response-headers

#post获取response
curl -XPOST   http://httpbin.org/response-headers
```

**6、http 响应格式**

| 请求方式 | url            | 备注 |
| -------- | -------------- | ---- |
| GET      | /brotli        |      |
| GET      | /deflate       |      |
| GET      | /deny          |      |
| GET      | /encoding/utf8 |      |
| GET      | /gzip          |      |
| GET      | /html          |      |
| GET      | /json          |      |
| GET      | /rebots.txt    |      |
| GET      | /xml           |      |

```
#指定 brotli压缩
# curl    http://httpbin.org/brotli -I
HTTP/1.1 200 OK
Date: Thu, 14 Jan 2021 09:10:21 GMT
Content-Type: application/json
Content-Length: 185
Connection: keep-alive
Server: gunicorn/19.9.0
Content-Encoding: br
Access-Control-Allow-Origin: *
Access-Control-Allow-Credentials: true

#指定 deflate
curl    http://httpbin.org/deflate -I

#指定deny
curl    http://httpbin.org/deny

#指定 encoding utf8
curl    http://httpbin.org/encoding/utf8 -I

#获取gzip
curl    http://httpbin.org/gzip -I

#获取html
curl    http://httpbin.org/html

#获取json
curl    http://httpbin.org/json 

#获取rebots.txt
curl    http://httpbin.org/rebots.txt

#获取xml
curl    http://httpbin.org/xml

```

**7、http 动态数据**

| 请求方式 | url                 | 备注     |
| -------- | ------------------- | -------- |
| GET      | /base64/{value}     |          |
| GET      | /bytes/n            |          |
| DELETE   | /delay/{delay}      |          |
| GET      | /delay/{delay}      |          |
| PATCH    | /delay/{delay}      |          |
| POST     | /delay/{delay}      |          |
| PUT      | /delay/{delay}      | 最大10秒 |
| GET      | /drip               |          |
| GET      | /links/{n}/{offset} |          |
| GET      | /range/{numbytes}   |          |
| GET      | /stream-bytes/{n}   |          |
| GET      | /stream/{n}         |          |
| GET      | /uuid               |          |

```
#获取base64
# curl    http://httpbin.org/base64/aaa
Incorrect Base64 data try: SFRUUEJJTiBpcyBhd2Vzb21l

#获取指定bytes
curl    http://httpbin.org/bytes/9999 -I

#指定延时
curl    http://httpbin.org/delay/10 -I

#drip

#返回一个links
curl    http://httpbin.org/links/1/1

#获取range 
curl    http://httpbin.org/range/100 -I

#指定二进制大小
curl    http://httpbin.org/stream-bytes/100

#指定流大小
curl    http://httpbin.org/stream/100

#获取uuid
curl    http://httpbin.org/uuid


```

**8、http cookies**

| 请求方式 | url                         | 备注 |
| -------- | --------------------------- | ---- |
| GET      | /cookies                    |      |
| GET      | /cookies/delete             |      |
| GET      | /cookies/set                |      |
| GET      | /cookies/set/{name}/{value} |      |

```
#获取cookies
curl -b 'test=aaa'  http://httpbin.org/cookies

#删除cookies
curl    http://httpbin.org/cookies/delete

# 设置cookies
curl    http://httpbin.org/cookies/set
curl    http://httpbin.org/cookies/set/a/111

```

**9、http 图片**

| 请求方式 | url         | 备注 |
| -------- | ----------- | ---- |
| GET      | /image      |      |
| GET      | /image/jpeg |      |
| GET      | /image/png  |      |
| GET      | /image/svg  |      |
| GET      | /image/webp |      |

```
#获取图片
# curl    http://httpbin.org/image
{"message": "Client did not request a supported media type.", "accept": ["image/webp", "image/svg+xml", "image/jpeg", "image/png", "image/*"]}

#获取 jpeg
curl    http://httpbin.org/image/jpeg  -I

#获取 png
curl    http://httpbin.org/image/png  -I

#获取 svg
curl    http://httpbin.org/image/svg  -I

#获取webp
curl    http://httpbin.org/image/webp  -I

```

**10、 http 重定向**

| 请求方式 | url                    | 备注       |
| -------- | ---------------------- | ---------- |
| GET      | /absolute-redirect/{n} |            |
| DELETE   | /redirect-to           |            |
| GET      | /redirect-to           |            |
| PATCH    | /redirect-to           |            |
| POST     | /redirect-to           |            |
| PUT      | /redirect-to           |            |
| GET      | /redirect/{n}          | 跳转多少次 |
| GET      | /relative-redirect/{n} |            |

```
#重定向
curl    http://httpbin.org/redirect/2 -v
curl    http://httpbin.org/relative-redirect/2 -v

```

**11、 http anything**

| 请求方式 | url                  | 备注 |
| -------- | -------------------- | ---- |
| DELETE   | /anything            |      |
| GET      | /anything            |      |
| PATCH    | /anything            |      |
| POST     | /anything            |      |
| PUT      | /anything            |      |
| DELETE   | /anything/{anything} |      |
| GET      | /anything/{anything} |      |
| PATCH    | /anything/{anything} |      |
| POST     | /anything/{anything} |      |
| PUT      | /anything/{anything} |      |

```
#anything
curl    http://httpbin.org/anything/aaaa 

```

