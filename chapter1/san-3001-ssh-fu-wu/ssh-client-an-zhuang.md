###一、SSH 客户端安装

    yum -y install openssh-clients
    
###二、SSH 客户端配置

- **全局配置文件:** `vim /etc/ssh/ssh_config`

- **用户配置文件：** `vim ~/.ssh/config`

      Host 别名
        Hostname 主机名
        Port 端口
        User 用户名  
        
      #会话复制
      ControlMaster auto  
      ControlPath  /tmp/ssh-%r@%h    
    
###三、SSH 使用

- **登录：**
    
    ssh USERNAME@IP 
- **远程执行命令：** 
    
    ssh USERNAME@IP COMMAND
              

- **拷贝文件：**

    scp FILE   USERNAME@IP:/ 
    scp USERNAME@IP:/FILE   FILE 
    
    scp -r DIR  USERNAME@IP:/
    scp USERNAME@IP:/DIR  DIR

- **免秘钥：**  

    server:
        ssh-keygen   -t  rsa
        ssh-copy-id  -I  USERNAME@IP
        
- **SSH 远程不询问YES:**

    ssh -o StrictHostKeyChecking=no    
- **远程比较文件：**
    
    ssh USERNAME@IP "cat FILE" | diff - FILE
    
###四、其他技巧
- **远程挂载：**

    yum -y intall fuse-sshfs
    sshfs  USERNAME@IP:DIR  MOUNT-DIR
    
- **远程明文密码链接：**

    yum -y install sshpass
    sshpass  -p "password"    ssh  USERNAME@IP

- **嵌套ssh：** 

    ssh   -t  user@IP1   ssh   user@IP2     

###五、windows

    xshell  putter 等等  
    使用方法略          