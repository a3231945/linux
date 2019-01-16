### string结构及命令

**1、添加字符串类型的key:** `set  name  longgege`

**2、添加字符串类型的key,并设置有效时间：** `set  name  longdidi  ex 10  # ex 秒 px 毫秒 nx 表示key 不存在，执行操作/xx表示存在，执行操作`

**3、一次性设置多个key:** `mset name  longe  age 23  #name  age  为key名`

**4、获取key的值：** `get  name`  

**5、一次性获取多个key:** `mget name ag`e 

**6、/*有疑问*/偏移修改**： `set world hello  setrange world 2 ?? #对world这个key 的第三个字符开始修改。替换2个问号  。结果为  he??o`

**7、偏移填充：** `set world hello  setrange world 6 ！ #缺省的部分会填充一个 \x00`

**8、追加字符串：**  `append world world     #在原来的key值的基础上添加一个world`

**9、获取值得一部分 ：** `getrange  world  0 3  #获取world这个key的第一个到第四个字符`

**10、获取旧值，同时设置新的值**： `getset  world  nihao` 

**11、数字加一：** `incr  num` 

**12、数字减一：** `decr  num`

**13、数字加指定大小：**  `incrby num 5`

**14、数字减指定大小：**  `decrby num 5`
