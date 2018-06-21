#fpm

###一、fpm 安装
**1、安装依赖环境**

    yum -y install gcc gcc-c++ make rpm-build
    
**2、安装ruby**

    yum -y install ruby rubygems ruby-devel
    
**3、修改gem源**

    gem sources --add https://gems.ruby-china.org/ 
    gem sources --remove https://rubygems.org/

**4、安装fpm**
    
    gem install fpm
###二、docker 安装

    #下载
    git clone https://github.com/jordansissel/fpm.git
    
    #制作镜像
    cd fpm  && docker build -t fpm .
    
    #启动fpm
    docker run -it --name demo fpm /bin/sh
    
###三、fpm 命令管理

    常用参数：
    
        -s                  指定源类型
        -t                  指定目标类型，即想要制作为什么包
        -n                  指定包的名字
        -v                  指定包的版本号
        -C                  指定打包的相对路径
        -d                  指定依赖于哪些包
        -f                  第二次打包时目录下如果有同名安装包存在，则覆盖它
        -p                  输出的安装包的目录，不想放在当前目录下就需要指定
        --post-install      软件包安装完成之后所要运行的脚本；同--after-install
        --pre-install       软件包安装完成之前所要运行的脚本；同--before-install
        --post-uninstall    软件包卸载完成之后所要运行的脚本；同--after-remove
        --pre-uninstall     软件包卸载完成之前所要运行的脚本；同--before-remove
        
###四、实例
**1、nginx**
    
**2、php**

**3、mysql**
