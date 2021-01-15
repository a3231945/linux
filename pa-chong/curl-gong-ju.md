# curl

### 一、安装

```
yum -y install curl
```



### 二、curl使用

**1、指定ua**

```
curl -A 'test-user-agent'  http://httpbin.org/user-agent

curl -H 'User-Agent: ptest-user-agent' http://httpbin.org/user-agent
```



**2、发送cookies**

```
curl -b 'test=aaa'  http://httpbin.org/cookies

curl -b 'test1=aaa;test2=bbb' http://httpbin.org/cookies
```



**3、将获取的cookices 写入文件**

```
curl -c cookies.txt https://www.baidu.com
```



**4、指定POST请求的数据**

```
curl -d 'login=user&password=123' https://www.example.com/login

curl --data-urlencode 'login=user&password=123' https://www.example.com/login
```



**5、指定ref**

```
curl -e 'https://google.com' https://www.baidu.com/ 

curl -H 'Referer: https://google.com?q=example' https://www.example.com
```



**6、上传二进制文件**

```
curl -F 'file=@test.png;type=image/png' https://www.example.com

指定服务器收到的名称
curl -F 'file=@test.png;filename=test2.png' https://www.example.com
```



**7、跳过ssl检测**

```
curl -k https://www.example.com
```



**8、跟随重定向**

```
curl -L http://baidu.com -v
```



**9、限速**

```
curl --limit-rate 200k http://www.example.com/files.txt
```



**10、下载保存**

```
curl -o test.txt   http://www.example.com/files.txt

curl -O  http://www.example.com/files.txt
```



**11、隐藏错误信息**

```
curl -s -o /dev/null https://www.example.com
```



**12、只输出错误信息**

```
curl -S https://www.example.com
```



**13、debug**

```
curl -v  https://www.example.com
```



**14、指定代理**

```
curl -x user:pass@ip:port  https://www.example.com
```



**15、指定请求方式**

```
curl -XGET http://www.example.com

GET\POST\DELETE\PUT\HEAD\PATCH
```



**16、模拟跨域**

```
curl -H 'Origin: http://foo.example'  https://www.example.com -I
```



**17、获取dns解析时间、响应时间、传输时间**

```
curl -o /dev/null -s -w %{time_namelookup}:%{time_redirect}:%{time_pretransfer}:%{time_connect}:%{time_starttransfer}:%{time_total}:%{speed_download}  http://httpbin.org/get
```



### 三、常见错误码信息

| 错误码 | 错误描述                                                     |
| ------ | ------------------------------------------------------------ |
| 1      | Unsupported protocol. This build of curl has no support for this protocol. |
| 2      | Failed to initialize.                                        |
| 3      | URL malformed. The syntax was not correct.                   |
| 5      | Couldn't resolve proxy. The given proxy host could not be resolved. |
| 6      | Couldn't resolve host. The given remote host was not resolved. |
| 7      | Failed to connect to host.                                   |
| 8      | FTP weird server reply. The server sent data curl couldn't parse. |
| 9      | FTP access denied. The server denied login or denied access to the particular resource or directory you wanted to reach. Most often you tried to change to a directory that doesn't exist on the server. |
| 11     | FTP weird PASS reply. Curl couldn't parse the reply sent to the PASS request. |
| 13     | FTP weird PASV reply, Curl couldn't parse the reply sent to the PASV request. |
| 14     | FTP weird 227 format. Curl couldn't parse the 227-line the server sent. |
| 15     | FTP can't get host. Couldn't resolve the host IP we got in the 227-line. |
| 17     | FTP couldn't set binary. Couldn't change transfer method to binary. |
| 18     | Partial file. Only a part of the file was transferred.       |
| 19     | FTP couldn't download/access the given file, the RETR (or similar) command failed. |
| 21     | FTP quote error. A quote command returned error from the server. |
| 22     | HTTP page not retrieved. The requested url was not found or returned another error with the HTTP error code being 400 or above. This return code only appears if -f/--fail is used. |
| 23     | Write error. Curl couldn't write data to a local filesystem or similar. |
| 25     | FTP couldn't STOR file. The server denied the STOR operation, used for FTP uploading. |
| 26     | Read error. Various reading problems.                        |
| 27     | Out of memory. A memory allocation request failed.           |
| 28     | Operation timeout. The specified time-out period was reached according to the conditions. |
| 30     | FTP PORT failed. The PORT command failed. Not all FTP servers support the PORT command, try doing a transfer using PASV instead! |
| 31     | FTP couldn't use REST. The REST command failed. This command is used for resumed FTP transfers. |
| 33     | HTTP range error. The range "command" didn't work.           |
| 34     | HTTP post error. Internal post-request generation error.     |
| 35     | SSL connect error. The SSL handshaking failed.               |
| 36     | FTP bad download resume. Couldn't continue an earlier aborted download. |
| 37     | FILE couldn't read file. Failed to open the file. Permissions? |
| 38     | LDAP cannot bind. LDAP bind operation failed.                |
| 39     | LDAP search failed.                                          |
| 41     | Function not found. A required LDAP function was not found.  |
| 42     | Aborted by callback. An application told curl to abort the operation. |
| 43     | Internal error. A function was called with a bad parameter.  |
| 45     | Interface error. A specified outgoing interface could not be used. |
| 47     | Too many redirects. When following redirects, curl hit the maximum amount. |
| 48     | Unknown TELNET option specified.                             |
| 49     | Malformed telnet option.                                     |
| 51     | The peer's SSL certificate or SSH MD5 fingerprint was not ok. |
| 52     | The server didn't reply anything, which here is considered an error. |
| 53     | SSL crypto engine not found.                                 |
| 54     | Cannot set SSL crypto engine as default.                     |
| 55     | Failed sending network data.                                 |
| 56     | Failure in receiving network data.                           |
| 58     | Problem with the local certificate.                          |
| 59     | Couldn't use specified SSL cipher.                           |
| 60     | Peer certificate cannot be authenticated with known CA certificates. |
| 61     | Unrecognized transfer encoding.                              |
| 62     | Invalid LDAP URL.                                            |
| 63     | Maximum file size exceeded.                                  |
| 64     | Requested FTP SSL level failed.                              |
| 65     | Sending the data requires a rewind that failed.              |
| 66     | Failed to initialize SSL Engine.                             |
| 67     | The user name, password, or similar was not accepted and curl failed to log in. |
| 68     | File not found on TFTP server.                               |
| 69     | Permission problem on TFTP server.                           |
| 70     | Out of disk space on TFTP server.                            |
| 71     | Illegal TFTP operation.                                      |
| 72     | Unknown TFTP transfer ID.                                    |
| 73     | File already exists (TFTP).                                  |
| 74     | No such user (TFTP).                                         |
| 75     | Character conversion failed.                                 |
| 76     | Character conversion functions required.                     |
| 77     | Problem with reading the SSL CA cert (path? access rights?). |
| 78     | The resource referenced in the URL does not exist.           |
| 79     | An unspecified error occurred during the SSH session.        |
| 80     | Failed to shut down the SSL connection.                      |
| 82     | Could not load CRL file, missing or wrong format (added in 7.19.0). |
| 83     | Issuer check failed (added in 7.19.0).                       |
| XX     | More error codes will appear here in future releases. The existing ones are meant to never change. |