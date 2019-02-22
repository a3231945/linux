## tcprstat分析服务的响应时间
 
### 一、tcprstat 安装

    wget http://github.com/downloads/Lowercases/tcprstat/tcprstat-static.v0.3.1.x86_64
    
    mv tcprstat-static.v0.3.1.x86_64 tcprstat
    chmod +x tcprstat 
    

### 二、tcprstat 使用
**1、查看帮助**

	# ./tcprstat  --help
	tcprstat 0.3.1, libpcap version 1.1.1.
	Usage: tcprstat [--port <port>] [--format=<format>] [--interval=<sec>]
	             [--header[=<header>] | --no-header] [--iterations=<it>]
	             [--read=<file>]
	       tcprstat --version | --help
	
		--read <file>, -r    Capture from pcap file <file>, not live.
		--local <addresses>, -l
		                     <addresses> is a comma-separated list of ip
		                     addresses, which are used as the local list of
		                     addresses instead of pcap getting the list.
		                     This is useful when working with a pcap file got
		                     from another host with different addresses.
		--port <port>, -p    Capture traffic only for tcp/<port>.
		--format <format>, -f
		                     Output format. Argument is a string detailing
		                     how the information is presented. Accepted codes:
		                         %n - Response time count
		                         %a - Response time media in microseconds
		                         %s - Response time sum
		                         %x - Response time squares sum
		                         %m - Minimum value
		                         %M - Maximum value
		                         %h - Median value
		                         %S - Standard deviation
		                         %v - Variance (square stddev)
		                         %I - Iteration number
		                         %t - Timestamp since iteration zero
		                         %T - Unix timestamp
		                         %% - A literal %
		                     Default is:
	    "%T\t%n\t%M\t%m\t%a\t%h\t%S\t%95M\t%95a\t%95S\t%99M\t%99a\t%99S\n".
		                     Statistics may contain a percentile between
		                     the percentage sign and the code: %99n, %95a.
		--header[=<header>], --no-header
		                     Whether to output a header. If not supplied, a
		                     header is created out of the format. By default,
		                     the header is shown.
		--interval <seconds>, -t
		                     Output interval. Default is 10.
		--iterations <n>, -n
		                     Output iterations. Default is 1, 0 is infinity
	
		--help               Shows program information and usage.
		--version            Shows version information.
		
**2、参数解释**

	命令行参数    简短形式   类型      描述                    默认值
	--format    -f        字符串     输出格式化字符串  ”%T\t%n\t%M\t%m\t%a\t%h\t%S\t%95M\t%95a\t%95S\t%99M\t%99a\t%99S\n” 
	--help                          显示帮助信息
	--interval  -t        数字      监控多少秒输出一次统计     10
	--iterations  -n      数字      共输出几次统计信息         1
	--local       -l      字符串    本级ip地址列表
	--port        -p      数字      服务端口
	--read        -r      字符串    pcap文件路径
	--version                      显示版本信息
	--no-header           字符串    输出不显示头信息
	--header              字符串    指定输出的头信息

**3、使用实例**

	1) 抓取 80 端口 间隔1秒 5次
	# ./tcprstat -p 80 -t 1 -n 5
	timestamp	count	max	min	avg	med	stddev	95_max	95_avg	95_std	99_max	99_avg	99_std
	1550821771	1	21750	21750	21750	21750	0	0	0	0	0	0	0
	1550821772	0	0	0	0	0	0	0	0	0	0	0	0
	1550821773	2	22666	18295	20480	22666	2190	18295	18295	0	18295	18295	0
	1550821774	1	21579	21579	21579	21579	0	0	0	0	0	0	0
	1550821775	0	0	0	0	0	0	0	0	0	0	0	0

	timestamp	时间戳
	count 		次数
	max   		最大响应时间（微秒）
	min   		最小响应时间（微秒）
	avg   		平均响应时间（微秒）
	med		中值
	stddev		标准差
	95_max		95峰值最大响应时间
	95_avg		95峰值平均响应时间
	95_std		95峰值标准差
	99_max		99峰值最大响应时间
	99_avg		99峰值平均响应时间
	99_std		99峰值标准差

	2) 分析pacp<tcpdump文件>
	# tcpdump -i eth0 -nn port 80 -w ./tcpdump.log
	tcpdump: listening on eth0, link-type EN10MB (Ethernet), capture size 65535 bytes


	# ./tcprstat  -t 2 -n 5 -r ./tcpdump.log 
	timestamp	count	max	min	avg	med	stddev	95_max	95_avg	95_std	99_max	99_avg	99_std
	1550822415	22	687	36	93	53	132	120	62	26	126	65	29

	

	
    




    