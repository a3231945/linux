# requests模块使用

### 一、requests安装

```
pip install requests
```



### 二、requests 模块常见使用

**1、发送get请求**

```python
import requests

r = requests.get('http://httpbin.org/get')
print(r.json())
```



**2、发送post请求**

```python
import requests

r = requests.post('http://httpbin.org/post')
print(r.json())

```



**3、发送url参数**

```
import requests

parse = {'username':'test','passwd':'123456'}

r = requests.get('http://httpbin.org/get',params=parse)
print(r.url)
print(r.json())
```



**4、指定请求头**

```python
import requests

headers = {'test-header':'test'}

r = requests.get('http://httpbin.org/headers',headers=headers)
print(r.json())
```



**5、post发送数据**

```python
import requests

data = {
    'username':'test',
    'passwd':'123456'
}

r = requests.post('http://httpbin.org/post',data=data)
print(r.json())
```



**6、获取状态码、状态信息**

```python
import requests

r = requests.get('http://httpbin.org/get')
print(r.status_code,r.reason)
```



**7、设置cookie**

```python
import requests

cookies=dict(_id='1111',token='xxxxxxxx')

r = requests.get('http://httpbin.org/cookies',cookies=cookies)
print(r.json())
```



**8、会话管理**

```python
import requests

s = requests.Session()
s.get('http://httpbin.org/cookies/set/id/1111111')
s.get('http://httpbin.org/cookies/set/token/xxxxxxxx')

r = s.get('http://httpbin.org/cookies')
print(r.json())
```



**9、代理**

```python
import requests

proxies= {
    'http':'http://172.104.80.229:8080'
}

#sock 代理
#proxies = {
#    'http': 'socks5://user:pass@host:port'
#}

r = requests.get('http://httpbin.org/ip',proxies=proxies)
print(r.json())
```



**10、basic auth**

```python
import requests

auth = requsts.auth.HTTPBasicAuth('test','123456')

r = requests.get('http://httpbin.org/basic-auth/test/123456',auth=auth)
print(r.json())

```

**11、错误**

```python
import requests


r = requests.get('http://httpbin.org/status_code/404',timeout=5)
r.raise_for_status()
```

