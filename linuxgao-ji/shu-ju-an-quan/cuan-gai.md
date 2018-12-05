    A<--------------->C<------------>B
    
    C篡改A给B的信息 ( HASH,数据的完整性 )


hash 哈希 单向散列算法,生成hash值校验文件完整性

- 原信息不改相同的hash算法得到的值固定不变
- 不管原始信息多长多短hash值的长度是固定不变
- hash算法是无穷集合和有穷集合的映射（不可逆）

md5 Message-Digest Algorithm 5（信息-摘要算法）

    [root@localhost ~]# md5sum /etc/passwd
    bffc5857b2cf07ed48dd2a7535f11a31  /etc/passwd

sha1 Secure Hash Algorithm(安全哈希算法)

    [root@localhost ~]# sha1sum /etc/passwd
    fb3145817b02071c0ec19e2380385f1e47f1a92f  /etc/passwd
