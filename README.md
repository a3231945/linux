# linux 笔记

**记录了作者 运维工作学习相关的文档，
有疑问请与作者讨论，有什么使用上的错误，
也请您多多包涵。欢迎来沟通，互相学习！**

    联系人：     周先生
    微信：       18910734468
    联系邮箱：   zhouxulong@peter-zhou.com
 


* [简介](README.md)
* [硬件信息相关](ying-jian-xin-xi-xiang-guan.md)
  * [一、DELL-服务器常见问题技术手册](ying-jian-xin-xi-xiang-guan/yi-3001-dell-fu-wu-qi-chang-jian-wen-ti-ji-zhu-shou-ce.md)
  * [二、DELL-服务器错误代码](ying-jian-xin-xi-xiang-guan/er-3001-dell-fu-wu-qi-cuo-wu-dai-ma.md)
  * [三、DELL-Megacli命令](ying-jian-xin-xi-xiang-guan/san-3001-dell-megacli-ming-ling.md)
  * [四、DELL-racadm命令](ying-jian-xin-xi-xiang-guan/si-3001-dell-racadm-ming-ling.md)
  * [五、HP-hpasmcli命令](ying-jian-xin-xi-xiang-guan/wu-3001-hp-hpasmcli-ming-ling.md)
  * [六、查看LINUX本机公网IP](ying-jian-xin-xi-xiang-guan/liu-3001-cha-kan-linux-ben-ji-gong-wang-ip.md)
* [基础服务](chapter1.md)
  * [一、NTP服务](chapter1/ntpfu-wu-qi-bu-shu.md)
    * [Ntpd](chapter1/ntpfu-wu-qi-bu-shu/ntpd.md)
    * [Chrony](chapter1/ntpfu-wu-qi-bu-shu/chrony.md)
    * [Ntp Client](chapter1/ntpfu-wu-qi-bu-shu/ntp-client.md)
    * [使用案例](chapter1/ntpfu-wu-qi-bu-shu/shi-yong-an-li.md)
  * [二、DNS服务](chapter1/dnsfu-wu-qi-bu-shu.md)
    * [bind yum 安装](chapter1/dnsfu-wu-qi-bu-shu/bind-yum-an-zhuang.md)
    * [bind 编译安装](chapter1/dnsfu-wu-qi-bu-shu/bind-bian-yi-an-zhuang.md)
    * [bind 故障处理](chapter1/dnsfu-wu-qi-bu-shu/bind-gu-zhang-chu-li.md)
    * [客户端配置](chapter1/dnsfu-wu-qi-bu-shu/ke-hu-duan-pei-zhi.md)
    * [Dnsmasq 安装](chapter1/dnsfu-wu-qi-bu-shu/dnsmasq-an-zhuang.md)
  * [三、SSH服务](chapter1/san-3001-ssh-fu-wu.md)
    * [SSH server安装](chapter1/san-3001-ssh-fu-wu/ssh-serveran-zhuang.md)
    * [SSH client 安装](chapter1/san-3001-ssh-fu-wu/ssh-client-an-zhuang.md)
    * [SSH FORWARD 转发](chapter1/san-3001-ssh-fu-wu/ssh-forward.md)
    * [SSH agent 代理](chapter1/san-3001-ssh-fu-wu/ssh-agent-dai-li.md)
    * [Pssh 使用](chapter1/san-3001-ssh-fu-wu/pssh-shi-yong.md)
    * [Rsync 服务](chapter1/san-3001-ssh-fu-wu/rsync-fu-wu.md)
  * [四、YUM服务](chapter1/yumyuan-fu-wu-qi-bu-shu.md)
    * [内部yum源同步公网源](chapter1/yumyuan-fu-wu-qi-bu-shu/nei-bu-yum-yuan-tong-bu-gong-wang-yuan.md)
    * [自定义yum源](chapter1/yumyuan-fu-wu-qi-bu-shu/zi-ding-yi-yum-yuan.md)
    * [rpmbuild 制作rpm包](chapter1/yumyuan-fu-wu-qi-bu-shu/zi-ding-yi-rpm-bao.md)
    * [fpm 制作rpm包](chapter1/yumyuan-fu-wu-qi-bu-shu/fpm-zhi-zuo-rpm-bao.md)
  * [五、共享服务](chapter1/ftpfu-wu.md)
    * [VSFTP 服务](chapter1/ftpfu-wu/vsftp-fu-wu.md)
    * [ProFTP 服务](chapter1/ftpfu-wu/proftp-fu-wu.md)
    * [Samba 服务](chapter1/ftpfu-wu/samba-fu-wu.md)
  * [六、NFS服务](chapter1/liu-3001-nfs-fu-wu.md)
    * [NFS 安装部署](chapter1/liu-3001-nfs-fu-wu/nfs-an-zhuang-bu-shu.md)
    * [NFS 客户端](chapter1/liu-3001-nfs-fu-wu/nfs-ke-hu-duan.md)
  * [七、日志服务](chapter1/ri-zhi-fu-wu-qi-eflk-bu-shu.md)
    * [Rsyslog 服务](chapter1/ri-zhi-fu-wu-qi-eflk-bu-shu/rsyslog-fu-wu.md)
    * [EFLK 部署](chapter1/ri-zhi-fu-wu-qi-eflk-bu-shu/eflkbu-shu.md)
    * [Logstash 配置](chapter1/ri-zhi-fu-wu-qi-eflk-bu-shu/logstash-pei-zhi.md)
    * [安装 grokbug 环境](chapter1/ri-zhi-fu-wu-qi-eflk-bu-shu/an-zhuang-grokbug-huan-jing.md)
  * [八、版本控制服务](chapter1/gitlabfu-wu-qi-bu-shu.md)
    * [SVN 服务](chapter1/gitlabfu-wu-qi-bu-shu/svn-fu-wu.md)
    * [Git 服务](chapter1/gitlabfu-wu-qi-bu-shu/git-fu-wu.md)
    * [Git 使用手册](chapter1/gitlabfu-wu-qi-bu-shu/git-shi-yong-shou-ce.md)
    * [GitBook部署](chapter1/gitbookbu-shu.md)
  * [九、虚拟化服务](chapter1/jiu-3001-xu-ni-hua-fu-wu.md)
    * [KVM服务](chapter1/kvmfu-wu-qi-bu-shu.md)
    * [LXC](chapter1/lxc.md)
    * [VirtualBox](chapter1/virtualbox.md)
    * [Vagrant](chapter1/vagrant.md)
    * [Openstack](chapter1/openstack.md)
    * [Docker](chapter1/docker.md)
      * [1、基础学习](chapter1/docker/13001-ji-chu-xue-xi.md)
        * [docker 私有镜像库](chapter1/docker/13001-ji-chu-xue-xi/docker-si-you-jing-xiang-ku.md)
        * [Dockerfile 指令详解](chapter1/docker/13001-ji-chu-xue-xi/dockerfile-zhi-ling-xiang-jie.md)
        * [nginx-Dockerfile](chapter1/docker/13001-ji-chu-xue-xi/nginx-dockerfile.md)
        * [php5.6-Dockerfile](chapter1/docker/13001-ji-chu-xue-xi/php56-dockerfile.md)
      * [2、图形化管理工具（三剑客）](chapter1/docker/23001-tu-xing-hua-guan-li-gong-ju-ff08-san-jian-ke-ff09.md)
        * 1、Compose
        * [2、Machine](chapter1/docker/23001-tu-xing-hua-guan-li-gong-ju-ff08-san-jian-ke-ff09/1machine.md)
        * 3、Swarm
      * 3、K8s学习
  * [十、自动化](chapter1/shi-3001-zi-dong-hua.md)
    * [Ansible](chapter1/shi-3001-zi-dong-hua/ansible.md)
      * [1、ansible安装](chapter1/shi-3001-zi-dong-hua/ansible/ansiblean-zhuang.md)
      * [2、ansible基本使用](chapter1/shi-3001-zi-dong-hua/ansible/2ansibleji-ben-shi-yong.md)
      * [3、常见模块](chapter1/shi-3001-zi-dong-hua/ansible/33001-chang-jian-mo-kuai.md)
      * [4、playbook-YAML](chapter1/shi-3001-zi-dong-hua/ansible/4playbook-yaml.md)
      * [5、ansible基础元素](chapter1/shi-3001-zi-dong-hua/ansible/5ansibleji-chu-yuan-su.md)
      * [6、playbook的组成结构](chapter1/shi-3001-zi-dong-hua/ansible/6playbookde-zu-cheng-jie-gou.md)
      * [报错信息处理](chapter1/shi-3001-zi-dong-hua/ansible/bao-cuo-xin-xi-chu-li.md)
      * [Ansible - windows](chapter1/shi-3001-zi-dong-hua/ansible/ansible-windows.md)
    * [Saltstack](chapter1/shi-3001-zi-dong-hua/saltstack.md)
    * [Puppet](chapter1/shi-3001-zi-dong-hua/puppet.md)
      * [puppet安装](chapter1/shi-3001-zi-dong-hua/puppet/puppetan-zhuang.md)
      * [puppet自动注册](chapter1/shi-3001-zi-dong-hua/puppet/puppetzi-dong-zhu-ce.md)
      * [puppet证书管理](chapter1/shi-3001-zi-dong-hua/puppet/puppetzheng-shu-guan-li.md)
      * [puppet文件管理](chapter1/shi-3001-zi-dong-hua/puppet/puppetwen-jian-guan-li.md)
      * [puppet软件包管理](chapter1/shi-3001-zi-dong-hua/puppet/puppetruan-jian-bao-guan-li.md)
      * [puppet用户管理](chapter1/shi-3001-zi-dong-hua/puppet/puppetyong-hu-guan-li.md)
      * [puppet任务计划管理](chapter1/shi-3001-zi-dong-hua/puppet/puppetren-wu-ji-hua-guan-li.md)
      * [puppet exce管理](chapter1/shi-3001-zi-dong-hua/puppet/puppet-exceguan-li.md)
      * [puppet service 管理](chapter1/shi-3001-zi-dong-hua/puppet/puppet-service-guan-li.md)
      * [puppet高级用法（编程语法）](chapter1/shi-3001-zi-dong-hua/puppet/puppetgao-ji-yong-fa-ff08-bian-cheng-yu-fa-ff09.md)
      * [puppet高级用法（类和模块）](chapter1/shi-3001-zi-dong-hua/puppet/puppetgao-ji-yong-fa-ff08-lei-he-mo-kuai-ff09.md)
      * [错误](chapter1/shi-3001-zi-dong-hua/puppet/cuo-wu.md)
      * [常用命令帮助](chapter1/shi-3001-zi-dong-hua/puppet/chang-yong-ming-ling-bang-zhu.md)
      * [puppet-dashboard安装](chapter1/shi-3001-zi-dong-hua/puppet/puppet-dashboardan-zhuang.md)
    * [Fabric](chapter1/shi-3001-zi-dong-hua/fabric.md)
  * [十一、监控](chapter1/shi-yi-3001-jian-kong.md)
    * [Zabbix](chapter1/shi-yi-3001-jian-kong/zabbix.md)
    * [Grafana](chapter1/shi-yi-3001-jian-kong/grafana.md)
  * [十二、其他](chapter1/shi-er-3001-qi-ta.md)
    * [SS 翻墙服务](chapter1/shi-er-3001-qi-ta/ss-fan-qiang-fu-wu.md)
    * [PXE 批量装机服务](chapter1/shi-er-3001-qi-ta/pxe-pi-liang-zhuang-ji-fu-wu.md)
    * [Redmin 服务](chapter1/shi-er-3001-qi-ta/redmin-fu-wu.md)
    * jira
    * [nali 使用](chapter1/shi-er-3001-qi-ta/nali-shi-yong.md)
* [应用服务](fu-wu-bu-shu.md)
  * [WEB服务](fu-wu-bu-shu/webfu-wu.md)
    * [一、NGINX](fu-wu-bu-shu/nginx.md)
    * [二、PHP](fu-wu-bu-shu/php.md)
    * [三、TOMCAT](fu-wu-bu-shu/tomcat.md)
  * [数据库服务](fu-wu-bu-shu/shu-ju-ku-fu-wu.md)
    * [一、MYSQL](fu-wu-bu-shu/mysql.md)
    * [二、REDIS](fu-wu-bu-shu/redis.md)
      * [1、安装](fu-wu-bu-shu/redis/an-zhuang.md)
      * [2、通用key命令操作](fu-wu-bu-shu/redis/23001-tong-yong-key-ming-ling-cao-zuo.md)
      * [3、string结构及命令](fu-wu-bu-shu/redis/3stringjie-gou-ji-ming-ling.md)
      * [4、Link链表结构](fu-wu-bu-shu/redis/4linklian-biao-jie-gou.md)
      * [5、set 集合结构](fu-wu-bu-shu/redis/5set-ji-he-jie-gou.md)
      * [6、order set有序集合](fu-wu-bu-shu/redis/6order-setyou-xu-ji-he.md)
      * [7、哈希结构](fu-wu-bu-shu/redis/73001-ha-xi-jie-gou.md)
      * [8、redis事务及锁应用](fu-wu-bu-shu/redis/8redisshi-wu-ji-suo-ying-yong.md)
      * [9、redis频道发布与消息订阅](fu-wu-bu-shu/redis/9redispin-dao-fa-bu-yu-xiao-xi-ding-yue.md)
      * [10、rdb快照持久化](fu-wu-bu-shu/redis/10rdbkuai-zhao-chi-jiu-hua.md)
      * [11、aof日志持久化](fu-wu-bu-shu/redis/11aof.md)
      * [12、redis主从复制](fu-wu-bu-shu/redis/12rediszhu-cong-fu-zhi.md)
      * [13、redis运维常用命令](fu-wu-bu-shu/redis/13redisyun-wei-chang-yong-ming-ling.md)
      * [14、sentinel 运维监控（哨兵）](fu-wu-bu-shu/redis/14sentinel-yun-wei-jian-kong-ff08-shao-bing-ff09.md)
      * [15、案例（位图法统计活跃用户）](fu-wu-bu-shu/redis/153001-an-li-ff08-wei-tu-fa-tong-ji-huo-yue-yong-hu-ff09.md)
    * [三、MONGO](fu-wu-bu-shu/mongo.md)
    * 四、Memcache
    * 五、SSDB
  * [缓存服务](fu-wu-bu-shu/huan-cun-fu-wu.md)
    * NGINX
    * [SQUID](fu-wu-bu-shu/huan-cun-fu-wu/squid.md)
    * [ATS](fu-wu-bu-shu/huan-cun-fu-wu/ata.md)
    * Varnish
  * [负载均衡-高可用服务](fu-wu-bu-shu/fu-zai-jun-8861-gao-ke-yong-fu-wu.md)
    * LVS
    * [Haproxy](fu-wu-bu-shu/fu-zai-jun-8861-gao-ke-yong-fu-wu/haproxy.md)
    * Nginx
    * [Heartbeat](fu-wu-bu-shu/fu-zai-jun-8861-gao-ke-yong-fu-wu/heartbeat.md)
    * Keepalive
  * [中间件服务](fu-wu-bu-shu/zhong-jian-jian-fu-wu.md)
  * [大数据服务](fu-wu-bu-shu/da-shu-ju-fu-wu.md)
* [Linux基础](linuxji-chu.md)
  * [一、linux目录结构](linuxji-chu/yi-3001-linux-mu-lu-jie-gou.md)
  * [二、文件类型和文件扩展名](linuxji-chu/er-3001-wen-jian-lei-xing-he-wen-jian-kuo-zhan-ming.md)
  * [三、用户和用户组管理](linuxji-chu/san-3001-yong-hu-he-yong-hu-zu.md)
  * [四、软链接和硬链接](linuxji-chu/si-3001-ruan-lian-jie-he-ying-lian-jie.md)
  * [五、文件和目录管理](linuxji-chu/wu-3001-wen-jian-he-mu-lu-guan-li.md)
  * [六、进程管理](linuxji-chu/liu-3001-jin-cheng-guan-li.md)
  * [七、软件管理](linuxji-chu/qi-3001-ruan-jian-guan-li.md)
  * [八、任务计划管理](linuxji-chu/ba-3001-ren-wu-ji-hua-guan-li.md)
* [Linux高级](linuxgao-ji.md)
  * [一、防火墙配置-iptables](linuxgao-ji/iptables-pei-zhi.md)
  * [二、权限控制](linuxgao-ji/er-3001-quan-xian-kong-zhi.md)
  * [三、数据安全](linuxgao-ji/shu-ju-an-quan.md)
  * [四、调优](linuxgao-ji/diao-you.md)
    * [1、CPU](linuxgao-ji/er-3001-quan-xian-kong-zhi/1cpu.md)
    * [2、MEM](linuxgao-ji/er-3001-quan-xian-kong-zhi/2mem.md)
    * [3、IO](linuxgao-ji/er-3001-quan-xian-kong-zhi/3io.md)
    * [4、策略路由](linuxgao-ji/er-3001-quan-xian-kong-zhi/43001-ce-lve-lu-you.md)
    * [5、网卡绑定](linuxgao-ji/er-3001-quan-xian-kong-zhi/53001-wang-qia-bang-ding.md)

















