### order set有序集

**1、添加一个有序集合**  `zadd class 12 lily 13 lucy 18 lilei 6 poly` 

**2、按区间查询**  `zrange class  0 3   查第一个到第四个`

**3、按score的值查询** `zrangebyscore class 13 18   查看编号13到18`

**4、limit offset 查询**

	查看出编号1到20 跳过第一个 取2个值 zrangebyscore class 1 20 limit 1 2 
   
**5、查询出值并且查询出编号**   `zrange class 1 3 withscores`

**6、查看所有元素**    `zrange class 0 -1 `

**7、查询一个元素的排名 ** `zrank class poly   默认按从小到大排列 `

**8、查询一个元素的排名用从大到小 **   `zrevrank  class poly `

**9、按照编号删除元素**    `zremrangebyscore class 10 15` 

**10、按照排名来删除**      `zremrangebyrank  class  0 1` 

**11、按照值来删除**  	`zrem  class lucy`

**12、返回元素个数**    	`zcard class` 

**13、返回某个范围有多少人**   `zcount class 25 30`

**14、统计两个集合的集合**		

	zadd lisi 3 cat 5 dog 6 horse
	zadd wang 2 cat 6 dog 8 horse 1 dankey
	zintersotre resulte 2 lisi wang                默认求和
	zrange resulte 0 -1 
	zinterstore resulte 2 lisi wang aggregate min  求最小
	zinterstore resulte 2 lisi wang aggregate max  求最大
	zinterstore resulte 2 lisi wang aggregate sum  求最大
	zinterstore resulte 2 lisi wang weights 2 1 aggregate max  权重求最大
	
**15、统计两个集合的交集**      `zunionstore  同上`	