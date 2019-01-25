### Wget使用
####一、常用使用方法
**1、下载单个文件**

    wget http://URL
    
**2、将下载的文件重命名**

    wget http://URL -O name

**3、下载限速**

    wget --limit-rage=128k http://URL
    
**4、断点续传**

    wget -c http://URL
    
**5、后台下载**

    wget -b http://URL
    
**6、指定UA**

    wget --user-agent="my ua" http://URL
    
**7、测试目标文件**

    wget --spider http://URL 
    
**8、设置重试次数**

    wget --tries=5 http://URL
    
**9、批量下载**
    
    vim url.txt
    URL1
    URL2
    
    wget -i url.txt

**10、下载整个网站**

    wget --mirror -p --convert-links -P ./DIR  http://URL

**11、排除某种类型文件**

    --reject=png
    
    配合 下载整个网站使用
    
**12、下载保存日志**

    wget -o LOG http://URL
    
**13、设置下载限制**

    wget -Q1G http://URL

**14、递归下载，指定文件类型**

    wget -r -A.png http://URL

**15、代理**

    wget -e "http_proxy=http://IP:PORT" http://URL
    wget -e "https_proxy=http://IP:PORT" http://URL

**16、debug **
    
    wget -d http://URL
  
**17、指定refer **

    wget --referer=http://URL http://URL  

**18、添加header **

    wget  --header="test:test1" http://URL

**19、指定cookies**

    wget --load-cookies cookies.txt http://URL

**20、post 请求**

    wget --post-data "username=xxx&&password=xxx" http://URL
#### 二、配置文件

- .wgetrc


    略
    
- /etc/wgetrc


    略
