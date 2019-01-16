#redis 数据迁移

###一、redis-dump /redis-load

**1、安装**

    #安装rvm（ruby多版本管理工具）
    gpg2 --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
    curl -sSL https://get.rvm.io | bash -s stable
    source /etc/profile.d/rvm.sh 
    echo rvm_autoupdate_flag=2 >> ~/.rvmrc

    #安装ruby
    rvm install ruby 2.4.1 

    #替换gem源
    gem sources --remove https://rubygems.org/
    gem sources -a http://mirrors.aliyun.com/rubygems/

    #安装redis-dump
    gem install redis-dump -V

**2、使用**
    
    #备份
    redis-dump  -u 192.168.11.12:6379  >backup.json

    #恢复
    <backup.json redis-load  -u 192.168.11.12:6380




###二、redis-port

**1、安装**

    

    #下载别人编译好的
    wget 'http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/66006/cn_zh/1531121747155/redis-port%282%29?spm=a2c4e.11153940.blogcont394417.12.6e0e1ed9hTwQDu' -O redis-port

    添加执行权限
    chmod +x redis-port


    #下载官方
    wget https://github.com/CodisLabs/redis-port/releases/download/v1.2.1/redis-port-v1.2.1-go1.7.5-linux.tar.gz -O redis-port.tar.gz
    
    tar xf redis-port.tar.gz

    

**2、使用**

    #备份：
        ./redis-port dump --from=192.168.11.12:6379  --output=./backup.rdb 

    #恢复：
        ./redis-port restore --input=./backup.rdb  --target=192.168.11.12:6380 --redis
