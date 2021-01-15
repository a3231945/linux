# urllib模块使用

### 一、urllib模块介绍

- `urllib.request` 打开和读取 URL
- `urllib.error` 包含 `urllib.request` 抛出的异常
- `urllib.parse` 用于解析 URL
- `urllib.robotparser` 用于解析 `robots.txt` 文件



### 二、urllib 常见使用

**1、get 请求**

```python
from urllib import request

r = request.urlopen("http://httpbin.org/get")
print(r.read())
```



**2、post请求**

```python
from urllib import request

#post请求
headers={'Content-Type':'application/json'}
req = request.Request("http://httpbin.org/post",headers=headers,method="POST")
r = request.urlopen(req)
print(r.read())
```



**3、带参请求**

```python
from urllib import request
import json

#带参请求
r = request.urlopen("http://httpbin.org/get?a=1&b=2")
print(json.loads(r.read()))
```



**4、post数据**

```python
from urllib import request
from urllib import parse
import json

dict = {'username': 'test', 'passwd': '123456'}
data = parse.urlencode(dict).encode('utf-8')
headers = {'Content-Type': 'application/json'}

req = request.Request('http://httpbin.org/post', data=data, method='POST')

r = request.urlopen(req)
print(json.loads(r.read()))
```



**5、指定header**

```python
from urllib import request
import json

headers =  {'test-header': 'this test header'}
req = request.Request('http://httpbin.org/headers',headers=headers)

r = request.urlopen(req)
print(json.loads(r.read()))

```



**6、解析链接**

```python
from urllib import request
from urllib parse

url = 'http://httpbin.org/get?a=1&b=2'


```



**7、base auth**

```python
#未完成，有错误
from urllib import request

auth_handler = request.HTTPBasicAuthHandler()
auth_handler.add_password(realm='test',
                         uri='http://httpbin.org/basic-auth/test/123456',
                         user='test',
                         passwd='123456')

opener = request.build_opener(auth_handler)

request.install_opener(opener)
request.urlopen('http://httpbin.org/basic-auth/test/123456')
```



**8、代理**

```python
from urllib import request

proxy_handler = request.ProxyHandler({'http': 'http://172.104.80.229:8080'})
opener = request.build_opener(proxy_handler)
r = opener.open('http://httpbin.org/ip')
print(json.loads(r.read()))
```



**9、cookie操作**

```python
from urllib import request

headers = {'Cookie': 'user=test;token=xxxxxxxxxxxx'}

req = request.Request('http://httpbin.org/cookies', headers=headers)

r = request.urlopen(req)
print(json.loads(r.read()))
```



**10、错误异常处理**

```python

```

