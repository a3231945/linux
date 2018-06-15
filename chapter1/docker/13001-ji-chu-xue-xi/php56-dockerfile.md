**1、dockerfile**
    #基础镜像
    FROM centos
    
    #mail
    MAINTAINER zhouxulong <zhouxulong@peter-zhou.com>
    
    #install soft
    RUN yum -y install epel-release
    
    RUN yum -y install net-tools iproute vim vi wget curl openssh-server supervisor
    RUN echo "123456" |passwd --stdin root
    
    # sshd 配置
    RUN mkdir -p /var/run/sshd
    RUN sed -i 's/UsePAM yes/UsePAM no/g' /etc/ssh/sshd_config
    RUN sed -i "s/#UsePrivilegeSeparation.*/UsePrivilegeSeparation no/g" /etc/ssh/sshd_config
    RUN ssh-keygen -q -t dsa -b 1024 -f /etc/ssh/ssh_host_dsa_key -N '' 
    RUN ssh-keygen -q -t rsa -b 1024 -f /etc/ssh/ssh_host_rsa_key -N '' 
    RUN ssh-keygen -q -t ecdsa -f /etc/ssh/ssh_host_ecdsa_key -N ''
    RUN ssh-keygen -q -t dsa -f /etc/ssh/ssh_host_ed25519_key  -N '' 
    COPY supervisord.conf /etc/supervisord.d/supervisord.conf
    
    
    
    #php 配置
    RUN   yum -y install gcc gcc-c++ libxml2 libxml2-devel bzip2 bzip2-devel libmcrypt libmcrypt-devel openssl openssl-devel libcurl-devel libjpeg-devel libpng-devel freetype-devel readline readline-devel libxslt-devel perl perl-devel psmisc.x86_64 recode recode-devel libtidy libtidy-devel
    COPY php-5.6.23.tar.gz  /usr/local/src/php-5.6.23.tar.gz
    RUN  cd /usr/local/src  && tar xf php-5.6.23.tar.gz
    WORKDIR  /usr/local/src/php-5.6.23  
    RUN ./configure --prefix=/usr/local/php5.6 --with-curl  --with-freetype-dir  --with-gd  --with-gettext  --with-iconv-dir  --with-kerberos  --with-libdir=lib64  --with-libxml-dir  --with-mysqli  --with-openssl  --with-pcre-regex  --with-pdo-mysql  --with-pdo-sqlite  --with-pear  --with-png-dir  --with-xmlrpc  --with-xsl  --with-zlib  --enable-fpm  --enable-bcmath  --enable-libxml  --enable-inline-optimization  --enable-gd-native-ttf  --enable-mbregex  --enable-mbstring  --enable-opcache  --enable-pcntl  --enable-shmop  --enable-soap  --enable-sockets  --enable-sysvsem  --enable-xml  --enable-zip
    
    RUN make && make install 
    RUN cp /usr/local/src/php-5.6.23/php.ini-production  /usr/local/php5.6/lib/php.ini
    RUN cp /usr/local/src/php-5.6.23/sapi/fpm/init.d.php-fpm /etc/init.d/php-fpm
    RUN cp /usr/local/php5.6/etc/php-fpm.conf.default /usr/local/php5.6/etc/php-fpm.conf
    
    
    RUN groupadd php -g 3000 && useradd -s /sbin/nologin -g php php -u 3000 -M 
    RUN sed -i -e 's@;pid = run/php-fpm.pid@pid = run/php-fpm.pid@g' -e 's@nobody@php@g' -e 's@listen = 127.0.0.1:9000@listen = 0.0.0.0:9000@g' /usr/local/php5.6/etc/php-fpm.conf
    RUN sed -i 's@;daemonize = yes@daemonize = no@g' /usr/local/php5.6/etc/php-fpm.conf
    
    #docker 开启22,9000端口
    EXPOSE 9000
    EXPOSE 22
    
    #前台运行程序
    CMD ["/bin/bash","-c","/usr/bin/supervisord -c /etc/supervisord.d/supervisord.conf"]

2、supervisord

    [supervisord]
    nodaemon=true
    
    # 注意这里service.sh脚本不能使用后台启动（nohup或者&）
    [program:sshd]
    command=/usr/sbin/sshd -D
    
    [program:php]
    command=/usr/local/php5.6/sbin/php-fpm
