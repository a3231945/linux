    A<---------------------------->C (B)
               
                            C伪装B (CA,验证中心)




    SSL/TLS SSL(Secure Sockets Layer 安全套接层),及其继任者传输层安全（Transport Layer Security，TLS）是为网络通信提供安全及数据完整性的一种安全协议。
    TLS与SSL在传输层为数据通讯进行加密提供安全支持。 
    SSL 为Netscape所研发，用以保障在Internet上数据传输之安全，利用数据加密(Encryption)技术，可确保数据在网络上之传输过程中不会被截取及窃听。
    它已被广泛地用于Web浏览器与服务器之间的身份认证和加密数据传输。 

SSL协议可分为两层： 

SSL握手协议（SSL Handshake Protocol）：`它建立在SSL记录协议之上，用于在实际的数据传输开始前，通讯双方进行身份认证、协商加密算法、交换加密密钥等。 `
SSL记录协议（SSL Record Protocol）：`它建立在可靠的传输协议（如TCP）之上，为高层协议提供数据封装、压缩、加密等基本功能的支持。`

SSL协议提供的服务主要有： 

- 认证用户和服务器，确保数据发送到正确的客户机和服务器
- 加密数据以防止数据中途被窃取
- 维护数据的完整性，确保数据在传输过程中不被改变