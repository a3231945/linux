### sentinel运维监控

**监控配置：**

	sentinel monitor def_master 127.0.0.1 6379 2
	setinel auth-pass def_master 012_345^6789-90
	#master 被当前sentinel实例认定为“失效”的间隔时间
	#如果当前sentinel与master直接的通讯中，在指定时间内没有响应或者响应错误代码，那么
	#当前sentinel 就认为master 失效（Sdonw,"主观"失效）
	#<mastername><millseconds>
	#默认为30秒
	sentinel down-after-milliseconds def_master 30000
	#当前sentinel 实例是否允许实施“failover”(故障转移)
	#no表示当前sentinel为“观察者”（只参与“投票”，不参与实施failover）
	#全局至少有一个为yes
	sentinel can-failover def_master yes
	#sentinel notificatio-script mymaster /var/redis/notify.sh
	

