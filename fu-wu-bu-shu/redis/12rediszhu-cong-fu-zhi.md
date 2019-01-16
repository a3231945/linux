### redis主从复制

**1、集群的作用：**

		主从备份，防止主机宕机
		读写分离，分担master的任务
		任务分离，如从服务器分别分担备份工作和计算工作
	
**2、redis集群两种方式:**


		   <--slave1
	master                     
		   <--slave2
		
	master<--slave1<--slave2
	
		
**3、主从通信过程**

			<-------------------sync[自动]-----------------
	master  -------------------dump出rdb------------------>   	slave1
			----缓冲的aof(dump的过程中又有数据过来了)----->
			---replicationFeedSlaves(进程保持)------------>
	
**4、redis集群配置**

- Master配置： 
 
      1、关闭Rdb快照（备份工作交给Slave）
      2、可以开启aof

- Slave配置：

      1、声明slaveof
      2、配置密码【如果master有密码】
      3、【某一个】slave打开rdb快照功能
      4、配置是否自读【slave-read-only】
	
**5、redis主从复制的缺陷**

    缺陷：  每次slave断开后（无论是主动断开，还是网络故障）再连接master
		       都要master全部dump出来rdb,再aof（即同步的过程都要重新执行一遍）
				
    切记：多台slave不要同时启动，否则master IO剧增0

