###redis事务及锁应用

**1、redis 支持简单的事务（不支持回滚**）

**2、redis 与 MySQL对比：**

			MySQL               redis
	开启	start transaction   mutil
	语句    普通sql 			普通命令
	失败	Rollback回滚		Discard 取消
	成功	Commit				exec
	
	注：rollback 域 discard 的区别
	如果已经成功执行了2条语句，第3条语句出错
	rollback后，前2条的语句影响消失
	Discard 只是本次事务，前2条语句造成的影响仍然在
	
	在mutil后面的语句中，语句出错可能有2种情况
	1、语句就有问题:这种，Exec时报错，所有语句得不到执行
	2、语句本身没错，但适用对象有问题，比如zadd 操作link对象
	exec之后，会执行正确的语句，并跳过不适当的语句
	
	set wang 200
	set zhao 700
	multi 
	decrby zhao 100
	incrby wang 100
	exec 
	抢票{
		set wang 300
		set lisi 300
		set ticket 1
	wang:
		watch ticket    #watch 可以监听多个key当其中一个key有变化，就会取消事务  取消 unwatch 
		multi 
		decr ticket 
		decrby wang 100
		exec 
	lisi:在王之前把票买走了。	