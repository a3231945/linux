**1、dns 区域文件乱码**
    
    zone "91.32.10.in-addr.arpa" IN {	
    	type slave;
        masters { XXX.XXX.XXX.XXX; };
        masterfile-format text;
    	file "10.32.91.zone";
    };
    
    ###配置传送格式
    masterfile-format text;

