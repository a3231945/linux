### 一、下载x-pack
    cd /usr/local/src/
    wget https://artifacts.elastic.co/downloads/packs/x-pack/x-pack-6.1.3.zip
    

### 二、安装x-pack

- Elasticsearch 安装

        cd /usr/share/elasticsearch
        bin/elasticsearch-plugin install file:///usr/local/src/x-pack-6.1.3.zip

- Kibana 安装

        cd /usr/share/kibana
        bin/kibana-plugin install file:///usr/local/src/x-pack-6.1.3.zip

- logstash 安装

        cd /usr/share/logstash
        bin/logstash-plugin install file:///usr/local/src/x-pack-6.1.3.zip

### 三、配置x-pack
**1、启用x-pack**

生成密码串：`/usr/share/elasticsearch/bin/x-pack/setup-passwords auto`

        Changed password for user kibana
        PASSWORD kibana = Ef3Q!PRlJ5kzz?2zL79v
        
        Changed password for user logstash_system
        PASSWORD logstash_system = Pn6*Xn+3vfF~3k952Msd
        
        Changed password for user elastic
        PASSWORD elastic = ooPC^$-R=uUn0$IVdm_4

- Elasticsearch 配置

        略
- Kibana 配置

        #vim /etc/kibana/kibana.yml 
        41:elasticsearch.username: "kibana"
        42:elasticsearch.password: "Ef3Q!PRlJ5kzz?2zL79v"

- logstash  配置
        
        #vim /etc/logstash/logstash.yml  
        #增加
        
        xpack.monitoring.elasticsearch.username: logstash_system
        xpack.monitoring.elasticsearch.password: Pn6*Xn+3vfF~3k952Msd



**2、禁用x-pack**
- Elasticsearch
        
        #vim /etc/elasticsearch/elasticsearch.yml 
        xpack.security.enabled: false
- Kibana 
        #vim /etc/kibana/kibana.yml  

        xpack.security.enabled: false
- logstash

        #vim /etc/logstash/logstash.yml  
        xpack.security.enabled: false

### 四、重启服务生效
- Elasticsearch:`/etc/init.d/elasticsearch restart`
- Kibana:`/etc/init.d/kibana restart`
- logstash:`/etc/init.d/logstash restart`



