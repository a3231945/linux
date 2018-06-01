###redis运维常用命令
**1、命令：**

	time  			查看时间戳与微秒数
	dbsize 			查看当前数据库有多少key
	bgrewriteaof 	后台进程重写aof
	bgsave     		后台保存rdb快照
	save 			保存rdb快照
	lastsave 		上次保存时间

	slaveof master-host port   把当前实例设置为master的slave
	flushall 		清空所有库所有键
	flushdb			清空当前库所有键
	shutdown [""|save|nosave] 断开链接，关闭服务器
	slowlog get			显示慢查询
	info			显示服务器信息
	config get 		获取配置信息
	config set 		设置配置信息
	monitor			打开控制台
	sync			主从同步
	client list	 	客户端列表
	client kill		关闭某个客户端
	client setname	为客户端设置名字
	client getname  获取客户端名字

**2、问题**

	如果不小心运行了flushall ，立即shutdown nosave 关闭服务器，然后手动编辑aof文件，去掉文件中的"flashall"相关行，然后开启服务器，就可以导入回原来数据
	#如果flushall之后，系统恰好bgrewriteaof了，那么aof就清空了，数据丢失
	
	slowlog显示慢查询
	多慢才叫慢？
	#由slowlog-log-slower-than 10000,来指定（单位是微秒）
	
	服务器存储多少条慢查询的记录？
	#由 slowlog-max-len 128  来做限制


**3、redis运维时需要注意的参数**
- **内存**

      memory
      used_memory:859192 数据结构的空间
      used_memory:7634944 实占空间
      mem_fragmentaion_ratio:8.89 前2者的比例，1 N为最佳，如果此值过大，说明redis的内存的碎片化严重，可以导出再导入一次

- **主从复制**

      replication
      role:slave
      master_host:xxx.xxx.xxx.xxx
      master_port:6379
      master_link_status:up
      
- **持久化**

      persistence
      rdb_change_since_last_save:0
      rdb_last_save_time:1375224063
      
- **fork耗时**

      status
      latest_fork_usec:936 上次导出rdb快照，持久化花费微秒
      注意：如果实例有10G内容，导出需要2分钟
      每分钟写入10000次，导致不断的rdb导出，磁盘处于高IO状态

- **慢日志**

      config set/get slowlog-log-slower-than
      config get/set slowlog-max-len
      slowlog get 获取慢日志
