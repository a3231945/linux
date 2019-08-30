# 跨域错误处理

**错误信息：**

     Request header field authorization is not allowed by Access-Control-Allow-Headers in preflight response
     
**解决方法：**
     
     #添加响应header
     header("Access-Control-Allow-Headers: Origin, X-Requested-With, Content-Type, Accept, Authorization");