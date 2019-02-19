

## 任务计划管理

**作用**:  计划任务主要是做一些周期性的任务，目前最主要的用途是定期备份数据


### 一、cron基础

        # ps aux |grep cron |grep -v 'grep'
        root      3078  0.0  0.0   5632  1108 ?        Ss   08:44   0:00 crond
        # chkconfig crond --list
        crond           0:关闭  1:关闭  2:启用  3:启用  4:启用  5:启用  6:关闭
        
        crond进程每分钟检查一次，以运行相应的任务
        crond日志文件/var/log/cron


### 二、系统级的计划任务  

更新whatis数据库
日志定期轮转
清理/tmp,/var/tmp
收集系统的状态信息
...
分时天月周

        # vim /etc/crontab
        01 * * * * root run-parts /etc/cron.hourly   		//run-parts 后面是目录
        02 4 * * * root run-parts /etc/cron.daily
        22 4 * * 0 root run-parts /etc/cron.weekly
        42 4 1 * * root run-parts /etc/cron.monthly
        # ls /etc/cron.hourly/
        # ls /etc/cron.daily/   		//下面都是一些脚本程序
        0anacron   cups        makewhatis.cron  prelink  rpm
        0logwatch  logrotate  mlocate.cron     rhsmd    tmpwatch
        # ls /etc/cron.weekly/
        0anacron  99-raid-check  makewhatis.cron
        # ls /etc/cron.monthly/
        0anacron


### 三、用户级的计划任务
        
        # crontab -e		//创建计划任务
        * * * * * /bin/ls
        # crontab -l			//查看计划任务
        * * * * * /bin/ls
        # ls /var/spool/cron/
        root
        # cat /var/spool/cron/root 
        * * * * * /bin/ls
        # tail /var/log/cron    	//查看日志
        
        作为root：
        # crontab -u alice -e



        时间表:
        *       *  	 *  	 * 	  *
        分	  时	   日	   月	   周
        0-59	0-23	1-31	1-12	 1-7	0,7表示周日
        
        00 02 * * * ls		//每天2:00整
        00 02 1 * * ls  	//每月1号2:00整
        00 02 14 2 * ls		//每年2月14号2:00整
        00 02 * * 7 ls  	//每周日2:00整
        00 02 14 2 7 ls 	//每年2月14号2:00整  或者  每周日2:00整，这两个时间都执行
        
        
        00 02 * * * ls		//每天2:00整
        * 02 * * * ls		//每天2:00中的每一分钟
        * * * * * ls		//每分钟执行ls
        * * 14 2 * ls		//2月14号的每分钟
        
        
        */5 * * * * ls		//每隔5分钟
        00 02 1,5,8 * * ls	//每月1,5,8号的2:00整
        00 02 1-8 * * ls	//每月1到8号的2:00整



**1、练习：备份etc目录**

        要求：
        1. 每天4:00备份/etc目录到/var/back
        2. 将备份命令写在脚本中，如/root/back.sh  加执行权限
        3. 每天备份的文件名包含当天的：日期，如2012-11-09.etc.tar.gz
        4. 计划任务执行时，屏幕不产生任何输出 &>/dev/null
        5. 只保留最近5天的备份 find /var/back -mtime +5 -exec rm -rf {} \;



- 编写脚本

        # vim /root/back.sh 
        tar -czf /var/back/`date +%F`.etc.tar.gz /etc   	//将/etc目录打包压缩成/var/back/以日期命名的tar.gz包
        find /var/back -mtime +5 -exec rm -rf {} \;


- 第二种版本

        #!/bin/bash
        filename=`date +%F`_etc.tar.gz
        back_dir=/var/back
        
        #判断备份文件存放目录是否存在
        if [ ! -d $back_dir ];then
           mkdir -p $back_dir
        fi
        
        #备份
        tar -czf ${back_dir}/$filename /etc &>/dev/null
        
        #删除修改时间超过2天的文件
        cd ${back_dir}
        find . -mtime +5 -exec rm -rf {} \;
        
        # chmod a+x back.sh
        
        
        手动测试脚本
        # /root/back.sh				//运行备份脚本
        # ll /var/back/
        总计 11720
        -rw-r--r-- 1 root root 11984709 09-14 17:59 2013-09-14_etc.tar.gz

        可以通过修改时间，继续测试
        date [-u|--utc|--universal] [MMDDhhmm[[CC]YY][.ss]]
        date -s 14:00   							//修改时间
        # date 09151200  			//修改日期，格式：月日时分年.秒
        
        # /root/back.sh 
        # ll /var/back/
        总计 23440
        -rw-r--r-- 1 root root 11984709 09-14 17:59 2013-09-14_etc.tar.gz
        -rw-r--r-- 1 root root 11984709 09-15 12:00 2013-09-15_etc.tar.gz


        配置cron执行脚本

        [root@station230 ~]# crontab -e
        0 4 * * * /root/back.sh 
        [root@station230 ~]# crontab -l         		//查看计划任务
        0 4 * * * /root/back.sh 

        测试：
                # date 09220359.50
                # date
                2013年 09月 22日 星期日 04:00:16 CST
                # ll /var/back/
                总计 11720
                -rw-r--r-- 1 root root 11984709 09-22 04:00 2013-09-22_etc.tar.gz
                
                # tail /var/log/cron
                Sep 20 12:03:01 yangs crond[5873]: (alice) CMD (ls)
                Sep 20 12:03:42 yangs crontab[5870]: (root) REPLACE (root)
                Sep 20 12:03:42 yangs crontab[5870]: (root) END EDIT (root)
                Sep 20 12:03:46 yangs crontab[5880]: (root) LIST (root)
                Sep 20 12:04:01 yangs crond[5882]: (alice) CMD (ls)
                Sep 20 12:05:01 yangs crond[5895]: (alice) CMD (ls)
                Sep 22 04:00:20 yangs crond[5909]: (alice) CMD (ls)
                Sep 22 04:00:20 yangs crond[5912]: (root) CMD (/root/back.sh)



        ====anacron====这个是“捡漏的”，就是出看每天的计划任务有没有执行，执行了就算了，没执行就按照延时时间去执行
        # vim /etc/anacrontab 
        1       65      cron.daily              run-parts /etc/cron.daily
        7       70      cron.weekly             run-parts /etc/cron.weekly
        30      75      cron.monthly            run-parts /etc/cron.monthly
        执行频率 延时	描述			执行的任务


**2、 让任务实现秒级执行**

        #通过计划任务实现
        
        每隔10秒执行命令date
        * * * * * date >/dev/pts/1
        * * * * * sleep 10; date >/dev/pts/1
        * * * * * sleep 20; date >/dev/pts/1
        * * * * * sleep 30; date >/dev/pts/1
        * * * * * sleep 40; date >/dev/pts/1
        * * * * * sleep 50; date >/dev/pts/1
        
        # 通过程序实现
        
        vim while01.sh
        #!/bin/bash		
        while :			
        do				
                echo "ok"		
                sleep 5		
        done			
        



