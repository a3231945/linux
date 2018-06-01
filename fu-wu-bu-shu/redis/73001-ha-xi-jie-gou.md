###hash结构

**1、添加一个hash结构数据** 
   
	hset user1 name  lisi 
	hset user1 age 28
	hset user1 height 175 

**2、查看hash所有数据**       `hgetall user1`

**3、一次添加一个hash 结构多个域与值**  `hmset user2 name wang age 10 height 100` 

**4、查看单个域和值 ** 			`hget user1 name`

**5、查看多个域和值**           `hget user1 name age` 

**6、删除单个域和值 **			`hdel user1 name `

**7、查看hash有几个域** 			`hlen user1 `

**8、查看hash是否有某个域**	`hexists user1 name` 

**9、域的值自增长**     `hincrby user1 age 1   #user1 的年龄增长1`

**10、域的值浮点型自增长**   `hincrbyfloat user1 age 0.5 #user1 增长0.5`

**11、返回所有域 ** 	`hkeys user2` 

**12、返回所有值**	`hvals user2`
