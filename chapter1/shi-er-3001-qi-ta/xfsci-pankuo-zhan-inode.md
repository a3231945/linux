# xfs文件系统磁盘扩展inode值

**现象：**

    磁盘inode数量被占满了，磁盘又没有办法清理
    
**扩展inode值：**

    # df -i
         /dev/sdb       195297280  195283626    195283626   100% /data

         
    # xfs_growfs -m 10 /data  
         meta-data=/dev/sdb               isize=512    agcount=4, agsize=122060800 blks
                  =                       sectsz=512   attr=2, projid32bit=1
                  =                       crc=1        finobt=0 spinodes=0
         data     =                       bsize=4096   blocks=488243200, imaxpct=5
                  =                       sunit=0      swidth=0 blks
         naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
         log      =internal               bsize=4096   blocks=238400, version=2
                  =                       sectsz=512   sunit=0 blks, lazy-count=1
         realtime =none                   extsz=4096   blocks=0, rtextents=0
         inode max percent changed from 5 to 10

         
    # df -i | grep data
    /dev/sdb       1171783680  195283626    1171770026    17% /data



    