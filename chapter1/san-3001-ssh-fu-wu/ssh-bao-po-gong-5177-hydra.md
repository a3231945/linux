###一、安装
**1、编译安装：**


    wget http://freeworld.thc.org/releases/hydra-6.3-src.tar.gz
    tar zxf hydra-6.3-src.tar.gz
    cd hydra-6.3-src
    ./configure
    make
    make install
**2、yum安装**

    yum   -y  install hydra
###二、使用

    #pass.txt为爆破字典
    
    hydra 127.0.0.2 ssh -l root -P pass.txt 
