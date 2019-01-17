### 1、安装软件包

    1> dell 官方下载文件  dell-dset-lx64-3.7.0.219.bin 
    
    2> 拨打dell 客服电话 让技术人员提供下载地址
    
### 2、 保修准备信息

    1> 获取序列号：dmidecode -t system | grep "Serial Number:"
    
    2> 获取服务器型号：dmidecode -t system | grep "Product Name:
    
    注意: dell 客服人员会确认此信息
    
    3>确认是否在保：     http://supportapj.dell.com/support/topics/topic.aspx/ap/shared/support/my_systems_info/zh/cn/details?c=cn&cs=cnbsd1&l=zh&s=bsd&~ck=anavml
    
### 3、使用收集日志：

    ./dell-dset-lx64-3.7.0.219.bin
    
    一直按回车，阅读完相关信息
    
    Do you agree to the above license terms? ('y' for yes | 'Enter' to exit): 
    
    输入： y
    
    Dell System E-Support Tool (DSET 3.7.0) Options:


    Choose an option:
    
    	1) View DSET Release Notes 
    	 Show the latest DSET release notes.
    
    	2) Create a One-Time Local System DSET Report 
    	 Creates a Local System DSET report.
    	 Note: This option does not permanently install DSET on the system.
    
    	3) Install/Upgrade DSET and Remote Provider (Recommended)
    	 Installs required components to generate reports for local and remote systems and also remote report collection
    	 from this system.
    
    	4) DSET 
    	 Installs required components to generate report for remote systems.
    
    	5) Remote Provider 
    	 Installs required components to allow reports to be generated from a remote system against this system.
    
    	6) Clear Dell Hardware Logs 
    	 Clears the Dell Hardware logs (ESM logs) from the system.
    	 Note: This option does not permanently install DSET on the system.
    
    	7) Quit
    	 Exits the installation process
    
    Enter option (1-7): 
    
    输入：2 <一次性收取即可,可以实际情况安装也行>
    
    Do you want to collect info for all hardware categories [y|n]:y

    Do you want to collect info for all storage categories [y|n]:y
    
    Do you want to collect info for all software categories [y|n]:y
    
    Do you want to collect linux log files [y|n]:y
    
    Do you want to collect advanced log files [y|n]:y
    
    Do you want to store this report in a default name and location [y|n]:y
    
    Do you want to enable report filtering (For more information, see the User's Guide) [y|n]:y
    
    Do you want to upload the report on request to Dell Technical Support after the report is generated [y|n]:y

    输入：一连串 y <根据实际情况选择，默认全y 即可>
    
    
    
    收集完成：家目录下会有一个zip 包，down下来邮件发送给dell 客服即可
                      也可以自行打开查看zip包内容，密码dell
                      
    包示例：DSET-Report-for-[SvcTag-XXX<序列号>-PE-R430]-on-01-17-2019-at-10.51-AM.zip
    
    
4、其他方式收集

    idrac收集       略
    bios-u盘收集    略      


    
        

    
    