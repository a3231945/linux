# wget

### 一、wget 安装

```
yum -y install wget
```



### 二、wget 使用

**1、下载文件**

```
#普通下载
wget  http://www.example.com/files.txt

#下载重命名
wget -O newname.txt http://www.example.com/files.txt

#限速下载
wget --limit-rate=10k http://www.example.com/files.txt

#断点续传下载
wget -c http://www.example.com/files.txt

#后台下载
wget -c -b http://www.example.com/files.txt

#测试下载连接
wget --spider http://www.example.com/files.txt

#下载多个文件
wget http://www.example.com/files[1-3].txt
or
cat > filename.txt
url1
url2
url3

wget -i filename.txt 
```



**2、镜像拷贝站点**

```
#拷贝站点
wget --mirror -p --convert-links http://www.example.com

#排除指定文件类型
wget --mirror --reject=png http://www.example.com

#下载指定文件类型
wget --mirror -A.css http://www.example.com

#下载总限额 5k
wget --mirror -Q5k http://www.example.com

```

**3、查看响应头**

```
#debug 获取响应头
wget --debug http://httpbin.org/get

#保存响应头
wget -S http://httpbin.org/get
```



**4、指定特定参数**

```
#指定ua
wget --header="user-agent: test-ua" http://httpbin.org/user-agent
wget --user-agent=test-ua http://httpbin.org/user-agent

#指定refer
wget --referer=http://www.example.com http://httpbin.org/get

```

