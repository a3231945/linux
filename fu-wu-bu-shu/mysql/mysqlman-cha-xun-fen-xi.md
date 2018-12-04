# MySQL 慢查询分析 #
### 一、mysqldumpslow 分析工具 ###
**1、安装**

 mysql 源码包`scripts`目录下

**2、mysqldumpslow 命令使用**

查看命令帮助信息：
    
    [root@om scripts]# ./mysqldumpslow --help
    Usage: mysqldumpslow [ OPTS... ] [ LOGS... ]
    
    Parse and summarize the MySQL slow query log. Options are
    
      --verbose    verbose
      --debug      debug
      --help       write this text to standard output
    
      -v           verbose
      -d           debug
      -s ORDER     what to sort by (al, at, ar, c, l, r, t), 'at' is default
                    al: average lock time
                    ar: average rows sent
                    at: average query time
                     c: count
                     l: lock time
                     r: rows sent
                     t: query time  
      -r           reverse the sort order (largest last instead of first)
      -t NUM       just show the top n queries
      -a           don't abstract all numbers to N and strings to 'S'
      -n NUM       abstract numbers with at least n digits within names
      -g PATTERN   grep: only consider stmts that include this string
      -h HOSTNAME  hostname of db server for *-slow.log filename (can be wildcard),
                   default is '*', i.e. match all
      -i NAME      name of server instance (if using mysql.server startup script)
      -l           don't subtract lock time from total time
      ############################################################################
      -s 指定排序的方法：默认at
            al  平均锁时间
            ar  平均多少行数据
            at  平均查询时间
            c   查询次数
            r   返回行数
            l   锁时间
            t   时间
      -r    倒序
      -t    数量
      -g    正则匹配
      -h    主机

**查询结果分析：**
    
    Count: 734  Time=14.14s (10380s)  Lock=0.00s (0s)  Rows=24.6 (18072), zbxuser[zbxuser]@[127.0.0.1]
    
    Count:执行了 734次
    Time：最大执行时间是14.14秒 ，总共执行了10380秒
    Lock：最大锁时间是0秒，总共执行了0秒
    Rows：最大
        
**实例：**
*查看前10条平均锁时间最长的*
    
    ./mysqldumpslow -s al -t 10  /data/logs/services/mysql/slow.log

*查看前10条锁时间最长的*
        
    ./mysqldumpslow -s l -t 10 /data/logs/services/mysql/slow.log 

*查看前10条平均查询时间最长的（**默认**）*

    ./mysqldumpslow -s at -t 10 /data/logs/services/mysql/slow.log 

*查询前10条查询时间最长的*

    ./mysqldumpslow -s l -t 10 /data/logs/services/mysql/slow.log 

*查询前十条次数最多的*
    
    ./mysqldumpslow -s c -t 10 /data/logs/services/mysql/slow.log 

*查询前十条查询平均结果行数最多的*
    
    ./mysqldumpslow -s ar -t 10 /data/logs/services/mysql/slow.log

*查询前十条查询结果行数最多的*

     ./mysqldumpslow -s ar -t 10 /data/logs/services/mysql/slow.log
 
           

### 二、mysqlsla 慢查询分析工具 ###
**1、安装mysqlsla**
    
    安装依赖包
    yum -y install wget perl perl-DBI perl-DBD-MySQL
   
    yum install perl-ExtUtils-CBuilder perl-ExtUtils-MakeMaker  perl-Time-HiRes
   
    git clone https://github.com/daniel-nichter/hackmysql.com.git
    cd hackmysql.com/mysqlsla
    perl Makefile.PL 
    make && make install 

**2、查看帮助信息**
    
    NAME
       mysqlsla - Parse, filter, analyze and sort MySQL slow, general and binary logs

    SYNOPSIS
           # Basic operation: parse a MySQL slow or general log
           mysqlsla --log-type slow LOG
           mysqlsla --log-type general LOG

           # Parse output from mysqlbinlog
           # mysqlsla cannot directly parse binary logs
           mysqlbinlog LOG | mysqlsla --log-type binary -

           # Parse a microslow patched slow log
           mysqlsla --log-type msl LOG

           # Replay a replay file
           mysqlsla --replay FILE

           # Parse a user-defined log specify its format
           mysqlsla --log-type udl --udl-format FILE

           # Let mysqlsla automatically determine the log type
           mysqlsla LOG

           ============================================================================================
           --log-type 日志类型，slow，general，binary，msl，udl
           
           --sort 指定排序规则，默认t_sum排序，t_sum 按总时间排序 c_sum 按总次数排序

           --statement-filter(-sf)[+-][TYPE]   过滤sql语句的类型，比如。select，update，drop，create，insert，例如“+select，insert”，不出现的默认是-，即不包括
    
           --databases db   指定数据库
 
**查询结果分析：**

    Count         : 5.83k  (5.10%)
    Time          : 93864.330161 s total, 16.114048 s avg, 10.001136 s to 60.819952 s max  (2.27%)
      95% of Time : 84733.609577 s total, 15.314225 s avg, 10.001136 s to 26.904785 s max
    Lock Time (s) : 498.24 ms total, 86 µs avg, 53 µs to 372 µs max  (0.17%)
      95% of Lock : 449.529 ms total, 81 µs avg, 53 µs to 139 µs max
    Rows sent     : 438 avg, 0 to 1.78k max  (0.25%)
    Rows examined : 3.19k avg, 0 to 55.39k max  (0.02%)
    Database      : 
    Users         : 
    	zbxuser@ 127.0.0.1 : 100.00% (5825) of query, 99.99% (114151) of all users
    
    Query abstract:
    SET timestamp=N; SELECT itemid,ROUND(N* MOD(cast(clock AS UNSIGNED)+N,N)/(N),N) AS i,COUNT(*) AS COUNT,avg(value) AS avg,MIN(value) AS MIN,MAX(value) AS MAX,MAX(clock) AS clock FROM history WHERE itemid='S' AND clock>='S' AND clock<='S' GROUP BY itemid,ROUND(N* MOD(cast(clock AS UNSIGNED)+N,N)/(N),N);
    
    Query sample:
    SET timestamp=1488943323;
    SELECT itemid,round(600* MOD(CAST(clock AS UNSIGNED)+2287,7200)/(7200),0) AS i,COUNT(*) AS count,AVG(value) AS avg,MIN(value) AS min,MAX(value) AS max,MAX(clock) AS clock FROM history WHERE itemid='33195' AND clock>='1488936113' AND clock<='1488943313' GROUP BY itemid,round(600* MOD(CAST(clock AS UNSIGNED)+2287,7200)/(7200),0);

    ===========================================================================================
    Count：sql的执行次数及占总的slow log数量的百分比
    Time:执行时间，包括总时间，平均时间，最小，最大时间，时间占到总慢sql的时间的百分比，
        95% of Time：去除最快和最慢的sql, 覆盖率占95%的sql的执行时间.  
    Lock Time(s):等待锁的总共时间，平均时间，最小到最大时间。所占百分比
        95% of Lock：95%的慢sql等待锁时间.  
    Rows sent：平均行数，最小到最大行数，所占所有百分比
    Rows examined： 扫描的行数量
    Database：   数据库
    Users:      用户
    Query abstract： 抽象后的sql
    Query sample：   sql语句

    
**实例：**


*使用默认分析*
    
    mysqlsla --log-type slow slow.log

*查询select 的top 10*

    mysqlsla --log-type slow  --statement-filter "+select" -top 10  slow.log

*查询指定数据库*
    
    mysqlsla --log-type slow  --database mysql -top 10 slow.log

*按次数排序*
    
    mysqlsla --log-type slow  --database mysql -top 10 --sort c_sum slow.log

        
    
        