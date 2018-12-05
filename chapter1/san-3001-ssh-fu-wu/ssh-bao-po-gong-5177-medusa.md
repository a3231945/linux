###一、安装
**1、编译安装**
	
	wget  href="http://www.foofus.net/jmk/tools/medusa-2.1.1.tar.gz">http://www.foofus.net/jmk/tools/medusa-2.1.1.tar.gz
	tar -zxvf medusa-2.1.1.tar.gz
	cd medusa-2.1.1
	./configure
	make
	make install
	
**2、yum安装**

	yum   -y  install medusa
###二、使用
       
	medusa -d #查看medua支持的模块
	medusa -H ip.txt -u root -P p.txt -M ssh
