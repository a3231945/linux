### Link链表结构

**1、从左边插入一个值：** `lpush character a`

**2、从右边插入一个值：** `rpush character b`

**3、查看链表第一个值：** `lrange character 0 0`

**4、查看链表最后一个值：** `lrange character -1 -1`

**5、查看链表所有值：**	`lrange character 0 -1`

**6、查看链表制定位置：** `lrange character 1 3 #查看 2-4 的值`

**7、去除一个值，并删除：**	

    lpop character #从头取值，并从链表删除
    rpop character #从尾取值，并从链表删除

**8、删除一个值：**	

    lrem answer 1 b #从头开始从answer 这个链表中删除一个 值为b的KEY
    lrem anser -2 a #从尾部开始删除2个a ,值为正数代表从头开始删，值为负数代表从尾开始删

**9、剪切：**
	
    ltrim character 2 5 #从第3个值截取到第6个值
    ltrim character 1 -2 #从第2个值开始截取到倒数第2个

**10、查看指定下标的值：** `lindex character 0`

**11、查看链表长度**：	`llen character`

**12、插入值：**	

    linsert num before 3 2 #在num这个链表中 第一个值为3前面插入一个2
    linsert num after 9 10 #在 9 后面插入一个10
    linsert num after 1000 1001 #在1000 后面插入一个1001 如果1000 不存在插入失败

**13、前出，后入：**	`rpoplpush task job #将链表task中的第一个元素取出，插入到job`

**14、等待取值：**	`brpop job 20 #从 job里面取值，等待20秒。如果为0，则一直等待`