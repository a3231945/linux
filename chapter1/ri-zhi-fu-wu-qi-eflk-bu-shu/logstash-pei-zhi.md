# Logstash 配置

### 一、基础日志配置

    #
	input {
	     kafka  {
	     codec => "json"
	     topics => "elk"
	     bootstrap_servers => "xxx.xxx.xxx.xxx:9092"
	     client_id => "elk"
	    }
	}
	
	
	
	filter {
		#审计日志匹配规则
		
		
		if [source] == "/data/logs/audit.log" {
			grok {
				match => {
					"message" => "%{SYSLOGTIMESTAMP:run_time} %{WORD:audit_host} EEO_AUDIT\[%{NUMBER:audit_id}\]: \s*LOGINID=%{NUMBER:lo
	gin_id}\s*USER=%{WORD:run_user}\s*PID=%{NUMBER:pid}\s*PWD=%{UNIXPATH:run_pwd}\s*CMD=%{GREEDYDATA:run_cmd}"
				}
			}
		}
	
		#任务计划匹配规则
		if [source] == "/var/log/cron" {
			grok {
				match => {
					"message" => "%{SYSLOGBASE} \(%{USER:cron_user}\) %{CRON_ACTION:cron_action} \(%{DATA:cron_message}\)"
				}
			}
		}
	
	}
	
	
	output {
		
		##审计日志
		if [source] == "/data/logs/audit.log" {
			elasticsearch {
				       hosts => ["xxx.xxx.xxx.xxx:9200"]
				       index => "auth_elk_%{+YYYY.MM.dd}"
			}
			#终端调试
			#stdout { codec => rubydebug }
		}
	
		##系统日志
		if [source] == "/var/log/messages" {
			elasticsearch {
				       hosts => ["xxx.xxx.xxx.xxx:9200"]
				       index => "messages_elk_%{+YYYY.MM.dd}"
			}
		}
	
		##任务计划
		if [source] == "/var/log/cron" {
			elasticsearch {
				       hosts => ["xxx.xxx.xxx.xxx:9200"]
				       index => "crond_elk_%{+YYYY.MM.dd}"
			}
		}
		
		##硬件日志
		if [source] == "/var/log/dmesg" {
			elasticsearch {
				       hosts => ["xxx.xxx.xxx.xxx:9200"]
				       index => "dmesg_elk_%{+YYYY.MM.dd}"
			}
		}
	
		##安全日志
		if [source] == "/var/log/secure" {
			elasticsearch {
				       hosts => ["xxx.xxx.xxx.xxx:9200"]
				       index => "secure_elk_%{+YYYY.MM.dd}"
			}
		}
	}

### 二、服务日志
**1、nginx**

	#
	input {
	     kafka  {
	     codec => "json"
	     topics => "elk"
	     bootstrap_servers => "xxx.xxx.xxx.xxx:9092"
	     client_id => "elk"
	    }
	}
	
	filter {
		#nginx 日志匹配规则
		if [source] == "/var/log/nginx/www.test.com_user.access.log"  or [source] == "/var/log/nginx/www.test.com_inner.access.log" or [source] == "/var/log/nginx/www.test.com_ecdn.access.log" or [source] == "/var/log/nginx/www.test.com_tcdn.access.log" or [source] == "/var/log/nginx/www.test.com_acdn.access.log" or [source] == "/var/log/nginx/m_eeo_cn.access.log"
		{
			grok {
				match => {
					"message" => '\[%{HTTPDATE:timestamp}\] (%{IPV4:xreal_ip}|-) (%{IPV4:remote_ip}|-) (?:%{IPV4:upstream_ip}\:%{IPORHOST:upstream_port}|-) %{WORD:http_method} %{NUMBER:http_code} (?:%{NUMBER:http_bytes}|-) HTTP/%{NUMBER:http_version} %{WORD:http_type}://%{NOTSPACE:http_hostname} "%{NOTSPACE:http_uri}" "(?:%{NOTSPACE:http_refer}|-)" (%{QS:agent}|-) (?:%{BASE10NUM:request_time}|-)\s*(?:%{BASE10NUM:upstream_response_time}|-) (?:%{NUMBER:upstream_status}|-) (?:%{WORD:upstream_cache_status}|-)'			
				}	
				#remove_field => ['type','_id','input_type','beat','version','offset','beats_input_codec_plain_applied']
			}
	
			date {
				match => ["timestamp", "yyyy-MM-dd HH:mm:ss,SSS"]
				target => "@timestamp"
	    		}	
	
			if [source_ip] != "-" {
				geoip {
					source => "source_ip"
					target => "geoip"
					database => "/etc/logstash/GeoLite2-City.mmdb"
					add_field => [ "[geoip][coordinates]", "%{[geoip][longitude]}" ]
					add_field => [ "[geoip][coordinates]", "%{[geoip][latitude]}"  ]
				}
			}
	
			mutate {
				split => ["http_uri", "?action="]
				add_field => ["api","%{http_uri[1]}"]
	                       	convert =>  ["http_code", "integer"]
	                       	convert =>  ["http_bytes", "integer"]
	                       	convert =>  ["request_time", "float"]
				convert => [ "[geoip][coordinates]", "float"]
	        }
		}
	
		#错误日志匹配规则
		if [source] == "/var/log/nginx/www.test.com_user.error.log"  or [source] == "/var/log/nginx/www.test.com_inner.error.log" or [source] == "/var/log/nginx/www.test.com_ecdn.error.log" or [source]== "/var/log/nginx/www.test.com_tcdn.error.log" or [source] == "/var/log/nginx/www.test.com_acdn.error.log" or [source] == "/var/log/nginx/m_eeo_cn.error.log"
		{
			grok {
				match => {
				"message" => '%{YEAR:year}/%{NUMBER:month}/%{NUMBER:day} %{TIME:time} \[%{WORD:loglevel}\] %{GREEDYDATA:info}'
				}	
				#remove_field => ['type','_id','input_type','beat','version','offset','beats_input_codec_plain_applied']
			}
		
		}
	}
	output {
		
		#访问日志
		if [source] == "/var/log/nginx/www.test.com_user.access.log" {
			elasticsearch {
				       hosts => ["xxx.xxx.xxx.xxx:9200"]
				       index => "www.test.com_user.access_elk_%{+YYYY.MM.dd}"
			}
			#终端调试
			#stdout { codec => rubydebug }
		}
		
		if [source] == "/var/log/nginx/www.test.com_inner.access.log" {
				elasticsearch {
							   hosts => ["xxx.xxx.xxx.xxx:9200"]
							   index => "www.test.com_inner.access_elk_%{+YYYY.MM.dd}"
				}
		}
	
		if [source] == "/var/log/nginx/www.test.com_ecdn.access.log" {
				elasticsearch {
							   hosts => ["xxx.xxx.xxx.xxx:9200"]
							   index => "www.test.com_ecdn.access_elk_%{+YYYY.MM.dd}"
				}
		}
	
		if [source] == "/var/log/nginx/www.test.com_tcdn.access.log" {
				elasticsearch {
							   hosts => ["xxx.xxx.xxx.xxx:9200"]
							   index => "www.test.com_tcdn.access_elk_%{+YYYY.MM.dd}"
				}
		}
	
		if [source] == "/var/log/nginx/www.test.com_acdn.access.log" {
				elasticsearch {
							   hosts => ["xxx.xxx.xxx.xxx:9200"]
							   index => "www.test.com_acdn.access_elk_%{+YYYY.MM.dd}"
				}
		}
	
		######################################################################
		#错误日志
		if [source] == "/var/log/nginx/www.test.com_user.error.log" {
			elasticsearch {
				       hosts => ["xxx.xxx.xxx.xxx:9200"]
				       index => "www.test.com_user.error_elk_%{+YYYY.MM.dd}"
			}
		}
		if [source] == "/var/log/nginx/www.test.com_inner.error.log" {
				elasticsearch {
							   hosts => ["xxx.xxx.xxx.xxx:9200"]
							   index => "www.test.com_inner.error_elk_%{+YYYY.MM.dd}"
				}
		}
	
		if [source] == "/var/log/nginx/www.test.com_ecdn.error.log" {
				elasticsearch {
							   hosts => ["xxx.xxx.xxx.xxx:9200"]
							   index => "www.test.com_ecdn.error_elk_%{+YYYY.MM.dd}"
				}
		}
	
		if [source] == "/var/log/nginx/www.test.com_tcdn.error.log" {
				elasticsearch {
							   hosts => ["xxx.xxx.xxx.xxx:9200"]
							   index => "www.test.com_tcdn.error_elk_%{+YYYY.MM.dd}"
				}
		}
	
		if [source] == "/var/log/nginx/www.test.com_acdn.error.log" {
				elasticsearch {
							   hosts => ["xxx.xxx.xxx.xxx:9200"]
							   index => "www.test.com_acdn.error_elk_%{+YYYY.MM.dd}"
				}
		}
	
	}


**2、php**

	#
	input {
	     kafka  {
	     codec => "json"
	     topics => "elk"
	     bootstrap_servers => "xxx.xxx.xxx.xxx:9092"
	     client_id => "elk"
	    }
	}
	
	filter {
		#php 日志匹配规则
		if [source] == "/var/log/php-fpm/php-fpm.log" {
			grok {
				match => {
				"message" => "\[%{WORD:day}-%{WORD:month}-%{WORD:year} %{TIME:time}\] %{LOGLEVEL:loglevel}:%{GREEDYDATA:info}"
				}	
				add_field => {
				"timestamp" =>"%{day}-%{month}-%{year} %{time}"
				}
			}
		}
	}
	output {
		
		if [source] == "/var/log/php-fpm/php-fpm.log" {
			elasticsearch {
				       hosts => ["xxx.xxx.xxx.xxx:9200"]
				       index => "php-fpm_elk_%{+YYYY.MM.dd}"
			}
		}
	}
**3、mysql**
	
	略
**4、squid**

	略

### 三、业务日志

	略