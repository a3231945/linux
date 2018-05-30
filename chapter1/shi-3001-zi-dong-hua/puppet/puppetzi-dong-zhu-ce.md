###一、配置自动注册规则

**server端：**

          vim /etc/puppet/autosign.conf
          *.puppet.com                       #指定自动注册的域名范围（IP没验证，不确定）
###二、清除认证配置

          puppet cert  list --all  查看
          puppet cert  --clean   client1.puppet.com  # 清除制定配置
###三、清理客户端配置

          cd   /var/lib/puppet/ssl   && rm -rf *
          重启服务
