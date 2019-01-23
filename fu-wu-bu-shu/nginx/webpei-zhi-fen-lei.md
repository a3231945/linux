#  web 功能分类


### 一、虚拟主机与请求的分发 ###
- listen
- server_name
- server_names_hash_bucket_size
- server_names_hash_max_size
- server_name_in_redirect
- location

### 二、文件路径的定义 ###
- root
- alias
- index
- error_page
- recursive_error_pages
- try_files

### 三、内存及磁盘资源的分配 ###
- client_body_in_file_only
- client_body_in_single_buffer
- client_header_buffer_size
- large_client_header_buffers
- client_body_buffer_size
- client_body_temp_path
- connection_poll_size
- request_poll_size

### 四、网络连接设置 ###
- client_header_timeout
- client_body_timeout
- send_timeout
- reset_timeout_connection
- lingering_close
- lingering_time
- lingering_timeout
- keepalive_disable
- keepalive_timeout
- keepalive_requests
- tcp_nodelay
- tcp_nopush

### 五、MIME类型的设置 ###
- type
- default_type
- types_hash_bucket_size
- types_hash_max_size

### 六、对客户端请求的限制  ###
- limit_except
- client_max_body_size
- limit_rate
- limit_rate_after

### 七、文件操作的优化 ###
- sendfile
- aio
- directio
- directio_alignment
- open_file_cache
- open_file_cache_errors
- open_file_cache_min_uses
- open_file_cache_valid

### 八、对客户端请求的特殊处理 ###
- ignore_invalid_headers
- underscores_in_headers
- if_modified_since
- log_not_found
- merge_slashes
- resolver address
- resolver_timeout
- server_tokens

### 九、负载均衡的基本配置 ###
- upstream
- server 
- ip_hash

### 十、反向代理 ###
- proxy_pass
- proxy_method
- proxy_hide_header
- proxy_pass_header
- proxy_pass_request_body
- proxy_pass_request_headers
- proxy_redirect
- proxy_next_upstream
- 