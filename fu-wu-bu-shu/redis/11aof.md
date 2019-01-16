### aof日志持久化

	aof的原理：set/append/写操作-->redis主进程-->后台日志进程-->aof文件
	1、每个命令重写一次aof?
	2、某key操作100次，产生100行记录，aof文件会很大怎么解决？


**aof配置：**

	appendonly no #是否打开aof日志功能（默认关闭）
	appendfsync always #每1个命令都立即同步到aof 安全，速度慢
	appendfsync everysec  #折中方案，每秒写一次
	appendfsync no        #写入工作交给操作系统，由操作系统判断缓冲大小，统一写入到aof 同步频率低，速度快
	
	no-appendfsync-no-rewrite yes   #正在导出rdb快照的过程中，要不要停止同步aof
	auto-aof-rewrite-percentage 100 #aof文件大小比起上次重写时的大小，增长率100%时，重写
	auto-aof-rewrite-min-size 64mb  #aof文件，至少超过64M时，重写
	
**几个问题：**
	
- 在dump rdb过程中aof如果停止同步，会不会丢失？ 
	
	  #不会，所有操作系统缓存在内存队列里，dump完成后，统一操作
	
- aof重写是指什么?（同一个key，操作100次）
	
	  #aof重写是指把内存中的数据，逆化成命令，写入到aof日志里，以解决aof日志过大的问题
	
- 如果rdb文件和aof文件都存在，优先级由谁来恢复？
	
	  #aof
	
- 2种是否能同时用?
	
	  #可以，而且推荐这么做
	
- 恢复时，rdb和aof那个恢复的快?
	
	  #rdb快，因为其数据的内存映射，直接载入到内存，而aof是命令，需要逐条执行


