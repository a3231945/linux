###一、server端

    puppet   cert list                          #查看请求认证的证书
    puppet   cert list --all                    #查看所有证书
    puppet   cert --sign client1.puppet.com     #签发证书
    puppet   cert  --sign  -all                 #一次签发所有的证书
    puppet   cert  --revoke  puppet-test        #让puppet-test 这个证书过期
    puppet   cert --clean  puppet-test          #清除puppet-test 这个证书
   清除配置需要重启puppetmaster 服务
###二、client 端

- **删除已有的证书**    `cd   /var/lib/puppet/ssl   && rm -rf *`
- **重新申请证书 **   `puppet agent --server server.puppet.com --test`
    
###三、证书的认证过程  

puppet agent在第一次连接master的时候会向master申请证书，如果没有master没有签发证书，那么puppet agent和master的连接是否建立成功的，agent会持续等待master签发证书，并会每隔2分钟去检查master是否签发证书。

通过puppet agent --server= server.puppet.com --no-daemonize –verbose启动的时候能很清楚的查看到agent申请证书的过程
