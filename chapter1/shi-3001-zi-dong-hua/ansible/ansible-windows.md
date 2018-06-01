# Ansible 批量控制windows #

### 一、Ansible控制端安装 ###
**1、安装ansible** 
    
    yum -y install ansible 

**2、安装windows控制依赖包**
    
    yum -y install python-pip
    pip install "pywinrm>=0.2.2"
    yum -y install python-devel krb5-devel krb5-libs krb5-workstation

### 二、安装windows 依赖包 ###
**1、安装.NET 4.5(最低3.0)**
**2、修改注册表 **

    HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PowerShell\1\ShellIds\Microsoft\PowerShell\1\ShellIds\ScriptedDiagnotics 

    ExecutionPolicy  修改为 remotesigned


**3、安装powershell3.0**
**4、调整网络为家庭网络**
**5、管理员启动powershell**

-   winrm qc
-   winrm set winrm/config/service '@{AllowUnencrypted="true"}'
-   winrm set winrm/config/service/auth '@{Basic="true"}'
    

### 三、使用批量操作 ###
**1、ansile-server 机器配置ip**
    
    vim /etc/ansible/hosts
    [windows]
    10.0.0.216
    [windows:vars]
    ansible_ssh_user="work"
    ansible_ssh_pass="123456"
    ansible_ssh_port=5985
    ansible_connection="winrm"

    #测试网络连通性
    ansible windows -m win_ping
**2、其他操作**
- **上传文件:**

    ansile windows -m win_copy -a "src=/etc/passwd dest=c:\passwd"
-  **删除文件:**

    ansible windows -m win_file -a "dest=c:\passwd state=absent"

- **创建用户:**
    
    ansible windows -m win_user -a "name=demo passwd=123456"

- **执行命令:**

    ansible windows -m win_shell -a "echo hello,world"
    
- **下载文件:**
    
    ansible windows -m win_get_url -a  "dest=c:/123.html  url=http://www.baidu.com"

    



