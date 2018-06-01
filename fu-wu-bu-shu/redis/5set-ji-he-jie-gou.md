###set集合结构

	特点：无序性、唯一性、确定性
	
**1、给一个集合增加元素**  


	sadd gender male female 
	sadd gender yao yao  返回 只添加了一个（唯一性）
        
**2、查看集合内容**  `smembers gender      返回不是按照你添加的顺序（无序性）`

**3、删除一个元素**  `srem  gender yao     返回值为你删除的数据的个数`

**4、随机弹出一个元素并删除** `spop gender       (场景可以用来抽签)`

**5、随机弹出一个元素不删除**  `srandmember gender `

**6、判断集合里是否有这个元素**  `sismember gender a  返回1存在0不存在`

**7、返回集合总共有多少个元素**  `scard gender`

**8、移动一个集合里的元素到另一个集合**  
 
	sadd  upper A B C 
	sadd  lower a b c 
	smove upper lower A  将 upper 里的A 移动到lower
	
**9、求两个集合之间的并集** 			

	sadd lisi a b c d 
	sadd wang a c d e f 
	sadd poly a c d g 
	sinter lisi wang poly  求3个学生有哪些相同的课程
	
**10、查看并集**	 `sunion lisi wang poly  求3个学生一共有多少门课`

**11、求差集 **	 `sdiff lisi  wang       李四的课程那些王五没有`

**12、将他们的并集存放在一个新的集合里**  `sinterstore result lisi wang poly` 
