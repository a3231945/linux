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
	
	
	
	#nginx 配置
	RUN  yum -y install gcc gcc-c++  pecl pecl-devel openssl openssl-devel gzlib gzlib-devel
	RUN groupadd nginx -g 4000 && useradd -s /sbin/nologin -g nginx nginx -u 4000 -M 
	COPY nginx-1.14.0.tar.gz  /usr/local/src/nginx-1.14.0.tar.gz 
	RUN  cd /usr/local/src/ && tar xf nginx-1.14.0.tar.gz
	RUN  cd /usr/local/src/nginx-1.14.0 && ./configure 	--prefix=/usr/local/nginx \
			    	--user=nginx --group=nginx \
			    	--with-http_ssl_module 	\
				--with-http_flv_module  \
				--with-http_sub_module  \
				--with-http_stub_status_module  \
				--with-http_gzip_static_module  \
				--http-fastcgi-temp-path=/usr/local/nginx/tmp/fcgi  \
				--http-client-body-temp-path=/usr/local/nginx/tmp/client \
				--http-proxy-temp-path=/usr/local/nginx/tmp/proxy  \
				--http-scgi-temp-path=/usr/local/nginx/tmp/scgi  \
				--http-uwsgi-temp-path=/usr/local/nginx/tmp/uwsgi  && make && make install 
	
	RUN echo "daemon off;">>/usr/local/nginx/conf/nginx.conf
	RUN mkdir -pv /usr/local/nginx/tmp
	
	
	#docker 开启22端口
	EXPOSE 22
	EXPOSE 80
	EXPOSE 443
	
	#前台运行程序
	CMD ["/bin/bash","-c","/usr/bin/supervisord -c /etc/supervisord.d/supervisord.conf"]
	
**2、supvervisord**

	[supervisord]
	nodaemon=true
	
	# 注意这里service.sh脚本不能使用后台启动（nohup或者&）
	[program:sshd]
	command=/usr/sbin/sshd -D
	
	[program:nginx]
	command=/usr/local/nginx/sbin/nginx


