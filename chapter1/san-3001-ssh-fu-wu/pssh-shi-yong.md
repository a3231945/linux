###一、下载

    wget http://parallel-ssh.googlecode.com/files/pssh-2.3.1.tar.gz
###二、安装

    tar xf  pssh-2.3.1.tar.gz
    cd pssh-2.3.1/
    python setup.py install
###三、安装结果
![](/assets/pssh-1.png)
###四、参数介绍

    pssh 参数：
    -h 执行命令的远程主机列表         文件内容格式：[user@]host[:port]
    -H 执行命令的远程主机
    -p 一次最大允许多少连接
    -o 输出结果重定向到目录
    -e  执行的错误重定向到目录
    -t 设置超时时间
    -A 提示输入密码并把密码传递给ssh
    -l 远程机器的用户名
    -x 传递多个SSH命令，多个命令用空格隔开，用引号括起来
    -X 同 -x 但是一次只能传递一个命令
    -i 显示标准输出和标准错误输出在每台HOST执行完毕后
    -I 读取每个输入命令，并传递给ssh进程，允许命令脚本传送到标准输入
    
**例如：**


 查看负载：
 
 ![](/assets/pssh-2.png)



手动输入密码：

![](/assets/pssh-3.png)

    pscp 参数： 拷贝本地文件到远程主机
    -h 执行命令的远程主机列表         文件内容格式：[user@]host[:port]
    -H 执行命令的远程主机
    -p 一次最大允许多少连接
    -o 输出结果重定向到目录
    -e  执行的错误重定向到目录
    -t 设置超时时间
    -A 提示输入密码并把密码传递给ssh
     -l   远程机器的用户名
    -x 传递多个SSH命令，多个命令用空格隔开，用引号括起来
    -X   同 -x 但是一次只能传递一个命令

**例如：**

将本地 test.txt 拷贝到远程主机/home 目录下
![](/assets/pssh-4.png)

    Pslurp 参数：拷贝远程主机到本地
        同上，
    -L 指定本地目录
**例如：**

将远程主机下 /tmp 目录下的passwd 文件拷贝到本地， 改名为passwd （注：最后的改名必须要有，不想改名就使用原文件名即可）
![](/assets/pssh-5.png)

**验证结果:**

![](/assets/pssh-6.png)
	
    Pnuke 参数： 远程杀进程
    	同上
	
**注：（只能杀进程名，不能杀进程ID）**
**建议使用：**  `pssh  -h hosts -P 'kill -9  进程名or 进程ID'`

**Prsync 同 pscp 安装之后有问题。
Pssh-askpass  **
