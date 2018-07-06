#Goaccess 日志分析


###一、Goaccess 安装
**1、配置epel 源**

    yum -y install epel-release

**2、安装goaccess**

    yum -y install goaccess geoip-devel libmaxminddb-devel tokyocabinet-devel openssl-devel ncurses-devel



###二、Goaccess 使用
**1、日志分析**
    #分析日志
    goaccess LOGFILE 

    #实时分析
    goaccess LOGFILE -c

    #导出html
    goaccess LOGFILE -o report.html

    #导出csv
    goaccess LOGFILE -o csv > report.csv

    #导出json
    goaccess LOGFILE -o json > report.json

    #server实时监听
    goaccess LOGFILE -o report.html --real-time-html

###三、nginx2Goaccess 脚本使用

**1、安装 nginx2Goaccess**

    git clone https://github.com/stockrt/nginx2goaccess.git



**2、生成goaccess nginx 配置文件**

    nginx2goaccess.sh <log_format>

    