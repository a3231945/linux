### 一、nicstat 安装
**1、下载源码**

    wget wget -c http://nchc.dl.sourceforge.net/project/nicstat/nicstat-1.92.tar.gz

**2、编译安装**

    tar xf nicstat-1.92.tar.gz
    cd nicstat-1.92
    cp Makefile.Linux Makefile
    vim Makefile
    17 #CFLAGS =       $(COPT)
    
    make

### 二、nicstat 使用
**1、查看帮助**

    # ./nicstat.sh -h
    USAGE: nicstat [-hvnsxpztual] [-i int[,int...]]
       [-S int:mbps[,int:mbps...]] [interval [count]]
    
         -h                 # help
         -v                 # show version (1.92)
         -i interface       # track interface only
         -n                 # show non-local interfaces only (exclude lo0)
         -s                 # summary output
         -x                 # extended output
         -p                 # parseable output
         -z                 # skip zero value lines
         -t                 # show TCP statistics
         -u                 # show UDP statistics
         -a                 # equivalent to "-x -u -t"
         -l                 # list interface(s)
         -M                 # output in Mbits/sec
         -S int:mbps[fd|hd] # tell nicstat the interface
                            # speed (Mbits/sec) and duplex
    eg,
       nicstat              # print summary since boot only
       nicstat 1            # print every 1 second
       nicstat 1 5          # print 5 times only
       nicstat -z 1         # print every 1 second, skip zero lines
       nicstat -i hme0 1    # print hme0 only every 1 second

**2、字段解释**

    选项：
        -h	#显示简单的用法
        -v	#显示nicstat版本
        -n	#只统计非本地（即非回环）接口
        -s	#显示摘要输出（只是接收和发送的数据量）
        -x	#显示扩展的输出
        -M	#以Mbps显示吞吐量,而不是默认的KB/s
        -p	#以解析后的输出格式显示
        -z	#跳过采样周期内是零流量的接口
        -t	#tcp流量统计
        -u	#ucp流量统计
        -a	#等同于'-x -t -u'
        -l	#只显示端口状态
        -i interface[,interface...]	#指定接口
        
     Time				#抽样结束的时间 
     Int				#网卡名
     rKB/s,InKB			#每秒读的千字节数(received)
     wKB/s,OutKB			#每秒写的千字节数(transmitted)
     rMbps,RdMbps			#每秒读的百万字节数K(received)
     wMbps,WrMbps			#每秒写的百万字节数M(transmitted)
     rPk/s,InSeg,InDG	        #每秒读的数据包
     wPk/s,OutSeg,OutDG             #每秒写的数据包
     rAvs				#平均读的数据包大小
     wAvs				#平均写的数据包大小
     %Util				#接口的利用率百分比
     Sat				#每秒的错误数，接口接近饱和的一个指标
    
**3、使用实例**

    1) 一次性查看网络流量
    # ./nicstat.sh 
    Time      Int   rKB/s   wKB/s   rPk/s   wPk/s    rAvs    wAvs %Util    Sat
    15:02:35       lo   46.75   46.75    7.92    7.92  6041.4  6041.4  0.00   0.00
    15:02:35     eth0    0.19    0.12    2.12    0.13   92.97   920.7  0.00   0.00
    
    
    2) 间隔1秒刷新一次
    # ./nicstat.sh 1
        Time      Int   rKB/s   wKB/s   rPk/s   wPk/s    rAvs    wAvs %Util    Sat
    15:03:34       lo   46.75   46.75    7.92    7.92  6041.4  6041.4  0.00   0.00
    15:03:34     eth0    0.19    0.12    2.12    0.13   92.97   920.7  0.00   0.00
        Time      Int   rKB/s   wKB/s   rPk/s   wPk/s    rAvs    wAvs %Util    Sat
    15:03:35       lo    0.00    0.00    0.00    0.00    0.00    0.00  0.00   0.00
    15:03:35     eth0    0.13    0.35    2.00    1.00   65.00   358.0  0.00   0.00
        Time      Int   rKB/s   wKB/s   rPk/s   wPk/s    rAvs    wAvs %Util    Sat
    15:03:36       lo    0.00    0.00    0.00    0.00    0.00    0.00  0.00   0.00
    15:03:36     eth0    0.93    0.35   15.00    1.00   63.73   358.0  0.01   0.00
        Time      Int   rKB/s   wKB/s   rPk/s   wPk/s    rAvs    wAvs %Util    Sat
    15:03:37       lo    0.00    0.00    0.00    0.00    0.00    0.00  0.00   0.00
    15:03:37     eth0    0.46    0.35    7.00    1.00   67.71   358.0  0.01   0.00
    
    3) 间隔1秒刷新一次，总共获取3次
    # ./nicstat.sh 1 3
        Time      Int   rKB/s   wKB/s   rPk/s   wPk/s    rAvs    wAvs %Util    Sat
    15:05:04       lo   46.75   46.75    7.92    7.92  6041.4  6041.4  0.00   0.00
    15:05:04     eth0    0.19    0.12    2.12    0.13   92.97   920.7  0.00   0.00
        Time      Int   rKB/s   wKB/s   rPk/s   wPk/s    rAvs    wAvs %Util    Sat
    15:05:05       lo    0.00    0.00    0.00    0.00    0.00    0.00  0.00   0.00
    15:05:05     eth0    0.67    0.35   10.99    1.00   62.27   358.0  0.01   0.00
        Time      Int   rKB/s   wKB/s   rPk/s   wPk/s    rAvs    wAvs %Util    Sat
    15:05:06       lo    0.00    0.00    0.00    0.00    0.00    0.00  0.00   0.00
    15:05:06     eth0    0.40    0.35    6.00    1.00   68.33   358.0  0.01   0.00

    4) 指定网卡
    # ./nicstat.sh -i eth0
        Time      Int   rKB/s   wKB/s   rPk/s   wPk/s    rAvs    wAvs %Util    Sat
    15:05:45     eth0    0.19    0.12    2.12    0.13   92.97   920.7  0.00   0.00

    5) 指定统计单位为M
    # ./nicstat.sh -M 
        Time      Int   rMbps   wMbps   rPk/s   wPk/s    rAvs    wAvs %Util    Sat
    15:07:06       lo    0.37    0.37    7.92    7.92  6041.4  6041.4  0.00   0.00
    15:07:06     eth0    0.00    0.00    2.12    0.13   92.97   920.6  0.00   0.00
    
    6) 统计tcp
    # ./nicstat.sh -t 1
    15:08:48    InKB   OutKB   InSeg  OutSeg Reset  AttF %ReTX InConn OutCon Drops
    TCP         0.00    0.00    4.37    4.37  0.00  0.00 0.000   0.00   0.00  0.00
    15:08:49    InKB   OutKB   InSeg  OutSeg Reset  AttF %ReTX InConn OutCon Drops
    TCP         0.00    0.00    1.00    1.00  0.00  0.00 0.000   0.00   0.00  0.00
    15:08:50    InKB   OutKB   InSeg  OutSeg Reset  AttF %ReTX InConn OutCon Drops
    TCP         0.00    0.00    1.00    1.00  0.00  0.00 0.000   0.00   0.00  0.00
    
    7) 统计udp
    # ./nicstat.sh -u 1
    15:09:21                    InDG   OutDG     InErr  OutErr
    UDP                         2.49    2.49      0.00    0.00
    15:09:22                    InDG   OutDG     InErr  OutErr
    UDP                         0.00    0.00      0.00    0.00




