##通用key命令操作

###一、查询

**1、查看所有key:** `keys *`
**2、匹配查看 *：** `keys sit*`
**3、单个字符匹配 ？：** `keys sit?`
**4、可选匹配 []:** `keys sit[e|y]`


###二、判断KEY类型
**1、随机返回一个KEY：** `randomkey`
**2、判断key 是否存在（0|1）：** `exists site #1 表示存在 0 表示不存在`
**3、返回KEY的类型：**	`type site #数据类型有 string ,link ,set ,order set ,hash`

###3、KEY的基本操作
**1、删除KEY:** `del site`

**2、重命名KEY:** `rename site web-site # site为旧名称 web-site 为新名称`

    #如果新名称已经存在会 覆盖已存在的key
    
**3、重命名返回值判断：** `renamex sit web-site # site为旧名称 web-site 为新名称`
    #带返回参数的改名，如果新名称已经存在会 不覆盖已存在的新名称 并返回0，如果新名称不存在，修改成功，并返回1

**4、移动KEY到其他库**： `move site 1 #将site 这个KEY移动到1号库。默认库0-15，验证 select 1 ;keys *`
    #默认开启了16个database ,redis.conf databases 关键字
    
**5、查询KEY的有效生命周期：**`ttl site # -1 代表永久有效 -2 代表不存在 返回时间以秒为单位pttl site #返回时间以毫秒为单位`

**6、修改KEY的有效生命周期：** `expire site 10 #设置生命周期为10 以秒为单位，请使用整数pexpire site 10 #设置时间以毫秒为单位`

**7、设置一个KEY为永久有效：** ` persist site`