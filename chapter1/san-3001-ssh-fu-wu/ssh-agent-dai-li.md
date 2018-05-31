###一、ssh-agent 用途
    
        是一种控制用来保存公钥身份验证所使用的私钥的程序。ssh-agent在X会话或登录会话之初启动，所有其他窗口或程序则以客户端程序的身份启动并加入到ssh-agent程序中。通过使用环境变量，可定位代理并在登录到其他使用ssh机器上时使用代理自动进行身份验证。
        
###二、使用场景介绍

    当你对一台机器生成了秘钥访问。秘钥有密码，这时候你使用密钥登陆，无论如何都要输入密码
    
    ssh-keygen -t rsa -P "123456"
    ssh-copy-id  USER@IP
    
    ssh IP  #这时会提醒输入密码
    
**怎么解决：**
        
    启动 ssh-agent
    ssh-agent bash
    
    ssh-add  ~/.ssh/id_rsa 
    
    这时候在登录就不需要输入秘钥密码
###三、常用命令介绍

**ssh-agent：**
 
    启动： 
        ssh-agent  bash
        eval `ssh-agent`
    关闭：
        ssh-agnet -k
**ssh-add：**
    
    添加秘钥： 
        ssh-add  FILE
    查看秘钥：
        ssh-add -l
    查看秘钥：
        ssh-add -L
    删除秘钥：
        ssh-add -d 
    删除所有秘钥：
        ssh-add -D
        
                          