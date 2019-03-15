
**1、问题现象**
问题现象：nfs服务器对同一台机器有2个共享目录，因为fsid 设置的都是1.这是后面挂载的文件和前一个挂载文件的内容想相同

    
    /web     10.0.1.1(insecure,rw,sync,no_all_squash,hide,fsid=1,anonuid=502,anongid=502)
    /data    10.0.1.1(insecure,rw,sync,no_all_squash,hide,fsid=1,anonuid=502,anongid=502)
    
    mount 10.0.1.100:/web  /web
    mount 10.0.1.100:/data /data
    
    #ls /web
    web.txt
    
    #ls /data
    web.txt
    

**2、处理方式**

    #修改nfs配置、重启服务
    /web     10.0.1.1(insecure,rw,sync,no_all_squash,hide,fsid=1,anonuid=502,anongid=502)
    /data    10.0.1.1(insecure,rw,sync,no_all_squash,hide,fsid=2,anonuid=502,anongid=502)
    
    

    #客户端重新挂载
    mount -o remount 10.0.1.100:/web /web
    mount -o remount 10.0.1.100:/data /data

    #测试结果：
        #ls /web
        web.txt
        
        #ls /data
        data.txt