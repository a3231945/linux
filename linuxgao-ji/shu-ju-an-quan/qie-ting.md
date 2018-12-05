	A<-------------------------------->B
				|
				C 
		 窃听 ( 加密,数据机密性)



**1、加密（解决数据机密性**）

加密由2部分组成：算法&密钥 （算法要够复杂，密钥要够安全）


**对称加密:(Symmetric encryption)**

	采用单钥密码系统的加密方法，同一个密钥可以同时用作信息的加密和解密，这种加密方法称为对称加密，也称为单密钥加密。
	需要对加密和解密使用相同密钥的加密算法。由于其速度，对称性加密通常在消息发送方需要加密大量数据时使用。对称性加密也称为密钥加密。

	DES (Data Encryption Standard数据加密标准)
	3DES (Triple DES 三重DES)
	AES （Advanced Encryption Standard 高级加密标准)

**非对称加密:(Asymmetric encryption)**

	与对称加密算法不同，非对称加密算法需要两个密钥：公开密钥（publickey）和私有密钥（privatekey）。
	公开密钥与私有密钥是一对，如果用公开密钥对数据进行加密，只有用对应的私有密钥才能解密；如果用私有密钥对数据进行加密，那么只有用对应的公开密钥才能解密。因为加密和解密使用的是两个不同的密钥，所以这种算法叫作非对称加密算法。
	
		RSA 
		DSA



**2、openssl 对称加密**

_加密，使用des算法，密钥123_

	[root@node1 tmp]# openssl enc -e -des -in /etc/passwd -out /tmp/passwd.des
	enter des-cbc encryption password:123
	Verifying - enter des-cbc encryption password:123

_解密_

	[root@node1 tmp]# openssl enc -d -des -in file1
