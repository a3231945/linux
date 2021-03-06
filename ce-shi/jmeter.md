### 一、安装jmeter ###


#### 1、安装jdk

    略
#### 2、下载jmeter

地址：https://mirrors.tuna.tsinghua.edu.cn/apache/jmeter/binaries/apache-jmeter-5.1.1.zip

#### 3、目录介绍

    bin             核心可执行文件、包含配置
    extras          插件扩展包
    lib             核心的依赖包
    ext             核心包
    junit           单元测试
    docs            接口文档目录
    printable_docs  用户手册文档（最常用：usermanual/component_reference.html）

#### 4、启动方式

	linux:                 /bin/jmeter
	windows:			   /bin/jmeter.bat
	分布式压测(linux):		/bin/jmeter-server
	分布式压测(windows):		/bin/jmeter-server.bat

#### 5、配置文件

	用户配置:		  /bin/user.properties
	核心配置文件：		/bin/jmeter.properties
	日志配置文件：		/bin/log4j2.xml
	升级配置:         /bin/upgrade.properties
	报告生成配置；		/bin/reportgenerator.properties
	保存服务配置：		/bin/saveservice.properties
	系统属性配置：		/bin/system.properties

### 二、测试计划元件

#### 1、jmeter 入门脚本

#### 2、线程用户

- 线程数：表示请求的虚拟用户数量
- ramp-up:启动所有线程数所需要的时间
- 循环次数：线程数循环次数
- 调度器: 控制定时启动

#### 3、取样器

- 作用： 向服务器发送请求并且记录响应时间和响应内容

#### 4、逻辑控制器

- 作用：

#### 5、配置元件

- 作用：

#### 6、定时器

- 作用：

#### 7、前置处理器

- 作用：在取样器请求建立前执行的一些行为

#### 8、后置处理器

- 作用：在取样器请求建立后执行的一些行为

#### 9、断言

- 作用：判断取样器返回信息是否符合预期

#### 10、监听器 

- 作用：生成报表，结果查看等   

#### 11、插件管理

- [jmeter -官方插件下载地址-  https://jmeter-plugins.org/install/Install/](https://jmeter-plugins.org/install/Install/)
- 下载安装至：jmeter/lib/ext

#### 12、jmeter元件的执行顺序

**组件的作用范围**

分类：

- 取样器的作用域的参照物，本身无所谓作用域的概念
- 以结果树为代表的大部分组件
  - 作用域：直接父级取样器+直接兄弟取样器
- 以逻辑控制器为代表的组件，只对子取样器有效

**元件执行顺序**

- 在同一作用域内，测试计划中的元件按照以下顺序执行
  - Config Elements  --  配置元件
  - Pre-porcessors   --  前置处理器
  - Timer -- 定时器
  - Sampler -- 取样器
  - Post-porcessors (除非 Sampler得到的返回结果为空) -- 后置处理器
  - Assirtions（除非Sampler 得到的返回结果为空） -- 断言
  - Listener（除非Sampler得到的返回结果为空） -- 监听器
- 注意：元件不会因为在脚本中的放置顺序而改变其执行的顺序，如果在一个脚本中有多个同类型的元件，他们的执行顺序是按照在脚本中的排序执行的

### 三、jmeter 运行原理 ###

    在jmeter 中是以线程的方式运行，jmter通过线程组来驱动多个线程组

### 四、jmeter测试计划要素

- 测试计划
- 在测试计划中至少有一个线程组
- 在线程组中至少有一个取样器
- 在测试计划中必须要有监听器

### 五、jemter取样器-HTTP请求

#### 1、http管理器

- 位置 配置元件
    - HTTP信息头管理器 --> HTTP 请求头
    - 主要使用Content-Type 参数
      - content-type 指请求参数的数据类型
- 在jmter 中，发送Http的post请求时，需要添加Http信息头管理器

#### 2、http请求配置

http 协议 默认端口80

https 协议 默认端口 443

http://www.baidu.com/index.html

- 协议 http
- 服务名称或IP： 接口的域名 www.baidu.com
- 端口号： 80
- 方法：Http协议请求方式 GET
- 路径（uri）： /index.html
- 内容编码： utf-8
- 参数：针对post 请求 x-www-from-urlencoded 格式和from-data格式
- 消息体数据：针对post请求中  请求数据

#### 3、http请求默认值

- 位置：
  - 线程组-- 添加--配置原件--http请求默认值
- 作用：
  - 将同一线程组下所有http请求的协议和服务器域名做一个统一的管理
  - 如果http请求中自己填写了协议和服务器域名，当发送请求时使用他本身的配置

### 六、jmeter 参数化

#### 1、jmeter 应用变量的方法

```
${变量名}
```

#### 2、csv数据实现参数化

- 建立一个csv格式的文件
  - 在excel 建立文件 另存为csv格式,使用notepad++ 转换为UTF-8格式（解决中文问题）
- 配置jmeter 中csv数据文件设置
  - 位置：线程组--添加--配置元件--csv数据文件设置
  - 文件名：选择准备好的csv文件
  - 文件编码：utf-8 
  - 变量名称：变量名1,变量2.等等（取决于csv数据列数）
  - 忽略首行：取决实际情况。如果csv首行写的是真实数据还是列名
  - 分隔符：取决文件以什么为分隔符
  - 其余选项按照默认即可
- 在http请求中引用参数
  - ${变量名}
- 如果使用csv中所有数据，需要设置线程组的线程数或循环次数

#### 3、用户参数

- 位置

  - 前置处理器--用户参数

- 使用方式

  - 针对那个http请求使用，就在那个http请求下添加
  - http请求--添加--前置处理器--用户参数

- 配置用户参数
  - 添加变量
    - 添加变量名称
  - 添加用户
    - 添加测试数据
    - 一个用户就是一组测试数据
- 使用用户参数
  - 在http请求中引用变量
  - 在线程组中设置线程数，线程数的值和用户参数中的用户数一致

####   4、用户定义变量

 -  位置
    	-  测试计划页面（一般情况下）
    	-  配置元件中--用户定义的变量
-  配置
   -  添加变量
-  使用
   -  凡是可以输入的位置，都可以使用变量

#### 5、jmeter参数化总结

- csv数据文件配置
  - 适用于大量测试数据时使用
- 用户参数
  - 适用于少量测试数据时
- 用户定义的变量
  - 适用于常量配置，数据库地址，测试环境地址，登录数据等

### 七、jmeter连接数据库

#### 1、连接mysql

- 在测试计划中导入数据库驱动jar包
- 在线程组添加JDBC Connection Configuration
- 在配置JDBC Conneciton Configuration
  - 在variable Name for created pool 填写要连接数据库名称映射名
  - 在Database Conneciton Configuration填写数据库相关参数
    - 数据库连接地址
      - jdbc:mysql://数据库地址:数据库端口/数据库名称
      - jdbc:mysql://127.0.0.1:3306/mysqltest
    - 选择数据库驱动
    - 数据库用户名
    - 数据库密码
- 在线程组中添加JDBC request
  - Variable Name Bound to Pool 中填写 连接数据库 名称的映射名
  - 写sql语句

```

#错误1
java项目连mysql时报错：java.sql.SQLException: Unknown character set index for field '255' received from server.

url = "jdbc:mysql://localhost:3306/xxx?useUnicode=true&characterEncoding=utf8";

#错误2
jmeter连接mysql提示Cannot create PoolableConnectionFactory（查看jmeter日志，提示SSL）

url = "jdbc:mysql://localhost:3306/xxx?useUnicode=true&useSSL=false";


```

#### 2、获取数据库查询结果

- sql语句中变量引用
  - ${变量名}
- 获取sql中查询结果
  - 在variable names 中填写变量名
  - 变量名指对查询结果的命名，接收sql结果，如果有多个字段，默认是第一个字段
- 调试取样器
  - 作用：主要显示脚本中变量的结果

### 八、jmeter关联

#### 1、xpath关联

- 使用场景
  - 在接口返回值为HTML或xml格式时，使用xpath提取器
- xpath提取器
  - 后置处理器中 -- xpath提取器
- 配置xpath 提取器
  - 引用名称：接收返回值数据的变量名
  - xpath qurey：xpath 表达式
  - 匹配数字：0表示随机选择，-1表示去所有
  - 缺省值：当我们没有找到对应数据的时候，显示缺省值内容
  - xml parsing options中选择Use Tidy和quiet

#### 2、json关联

- json提取器
  - Http请求--右键--添加--后置处理器--json提取器
- 使用场景
  - 在接口返回数据为json数据时使用
- 配置json提取器
  - 引用名称：接收返回值数据的变量名
  - json path expressions：jsonpath表达式 $.获取字段名
  - 匹配数字：0表示随机选择，-1表示去所有
  - 缺省值：当我们没有找到对应数据的时候，显示缺省值内容

#### 3、正则表达式关联

- 正则表达式提取器
  - Http请求--右键--添加--后置处理器--正则表达式提取器
- 使用场景
  - 适用于任何返回形式
- 表达式实例：
  - （.*?） 表示匹配所有
  - {"error_info":{"errno":1,"error":"\u7a0b\u5e8f\u6b63\u5e38\u6267\u884c"},"data":{"serveTime":1599026128.524}}
    - "data":{(.*?)} 提取 "serveTime":1599026128.524
- 配置正则表达式提取器
  - 前2个选项默认即可
  - 引用名称：接收返回值数据的变量名
  - 正则表达式："data":{(.*?)}
  - 模板：\$1\$ 表示取第一组数据
  - 匹配数字：0表示随机选择，1表示取第一个
  - 缺省值：当我们没有找到对应数据的时候，显示缺省值内容

### 九、断言

#### 1、响应状态码

- 断言状态码
  - 添加断言：那个几个口需要断言，在接口下添加断言
  - 配置断言
    - 添加 断言 -- 响应断言
    - 在测试字段 -- 选择响应代码
    - 在测试模式 -- 填写期望的状态码 200
- 断言文本值
  - 配置断言
    - 添加 断言 -- 响应断言
    - 添加后置处理器 -- bean shell postprocessor
      - 编写脚本 -- 将返回值中的中文解码
    - 在测试字段 -- 选择响应文本
    - 在测试模式 -- 填写要断言的具体字段和值

#### 2、json断言

- 作用范围： 返回值格式为json格式
- 添加json断言
  - 那个接口需要断言，在接口下添加断言
- 配置json断言
  - 添加 断言 -- json 断言
  - Assert JSON Path exists:获取返回结果的字段 -- 实际结果
  - Expencted Value: 预期结果，填写具体数据也可以引用参数

#### 3、大小断言

- 作用：断言返回值所占字节数的大小
- 如果填写 100 比较类型 > 表示，返回值的字节数>100

#### 4、持续时间断言

- 作用：断言接口响应时间是否<= 所期望的时间
- 如果填写100 ，如果请求时间 >100毫秒抛出断言错误



#### 5、JSR223断言

#### 6、Xpath断言

#### 7、Binary Response Assertion

#### 8、HTML断言

#### 9、MD5Hex断言

#### 10、SMIME断言

#### 11、XML断言

#### 12、XML Schema断言

#### 13、比较断言

#### 14、BeanShell断言

### 十、集合点

- Synchronizing Timer(同步定时器)  -- 集合点
  - 位置：定时器-- Synchronizing Timer(同步定时器)
- 配置集合点
  - 每次集合的用户数
  - 集合用户数所用时间（单位：毫秒）
    - 如果 设置为0，表示无线等待，直到线程数量==集合数量
    - 设置时长：在规定时间内启动已经集合线程
- 注意事项
  - 集合数最好能被线程数整除
  - 集合数的启动时间最好大于线程数启动时间

### 十一、函数

#### 1、使用函数步骤

- 1 点击jmeter 页面的函数助手
- 2 选择需要使用的函数
- 3 设置函数相关参数
- 4 点击生成
- 5 复制函数字符串
- 6 连接到需要使用的位置

#### 2、函数分类

- 获取信息函数
- 数据输入函数
- 数据计算函数
- 属性信息函数
- 字符串操作函数
- 脚本函数



#### 3、获取信息函数

-  __TestPlanName
  - 作用：返回当前测试计划的名称
- __threadGroupName
  - 作用：返回当前线程组的名称
- __threadNum
  - 作用：返回当前正在执行的线程编号
- __samplerName
  - 作用：返回当前请求的名称
- __log
  - 作用：输出日志信息
  - {__log(Message)} ：写入日志文件。”....thread Name：Message“
  - {__log(Message,OUT)}：写到控制台窗口。
  - ${__log(${VAR},,,VAR=)}：写入日志文件，"...thread Name VAR=value"
- __time
  - 作用：以多种格式返回当前时间
  - 属性值
    - YMD = yyyyMMdd
    - HMS = HHmmss
    - YMDHMS = yyyyMMdd-HHmmss
    - USER1 = JMeter属性time.USER1
    - USER2 = JMeter属性time.USER2
  - 实例
    - ${__time()}
    - ${__time(YMD,)}
    - ${__time(dd/MM/yyyy,)} 

#### 4、数据输出函数

- _StringFromFile
  - 作用：从文本文件中读取字符串，每次调用读取一样
- __FileToString
  - 作用：把文件读取成一个字符串，每次调用都是读取整个文件
- __CSVRead
  - 作用：
- __XPath
  - 作用：使用Xpath语法匹配XML文件

#### 5、数据计算函数

- __counter
  - 作用：计数器函数
- __intSum
  - 作用：对多个证书求和
- __longSum
  - 作用：长整型求和
- __Random
  - 作用：返回给定最大值和最小值之间的随机整数
- __RandomDate
  - 作用：返回给定开始时间和结束时间之间的随机日期
- _RandomString
  - 作用：根据给定的字符串生成指定长度的随机字符串
- __UUID
  - 作用：通用唯一标识符函数

#### 6、属性信息函数

- __isPropDefined
  - 作用：判断属性是否存在
- __property
  - 作用：对多个证书求和
- __P
  - 作用：简化的属性函数，
- __setProperty
  - 作用：简化的属性函数，用于与命令行上定义的属性一起使用

#### 7、字符串操作函数

- __split
  - 作用：根据分隔符拆分字符串为多个变量
- __changeCase
  - 作用：转换大小写
- __regexFunction
  - 作用：使用正则表达式解析之前的响应结果

#### 8、脚本函数

- __BeanShell
  - 作用：执行beanshell 脚本
- __javaScript
  - 作用：执行js脚本



**9、实例**

- 多线程组之间的数据传递

  - 操作步骤

    - 1、将原来的参数提升作用域 -- beanshell 取样器  使用函数：setProperty

    - 2、设置setProperty

      - 属性名称：指提升作用域后的变量名
      - value of Property：需要提升作用域的变量值 ${session}

    - 3、在线程组1 中添加一个新的取样器 -- Beanshell 取样器

    - 4、将设置好的setProperty函数复制粘贴到Bean shell 取样器中

    - 5、设置Property函数

      - 属性名称：新的变量名 -->提升作用域后的变量名称

    - 6、将设置好的Property函数复制链接到线程组2中

    - 7、设置线程组请求的先后顺序

      - 在线程组--调度器
      - 填写持续时间和启动延时

### 十二、jmeter分布式

#### 1、分布式概念

- 由多台电脑同时共同完成一个任务的部署，这种部署就叫做分不输

#### 2、jmeter分布式步骤

- 执行机（server）
  - 获取本机ip将ip写在jmeter配置文件中，remote_host=ip:1099
  - 配置网络允许 server ->agent 相互访问
- 控制机（agent）
  - 将执行机的ip写入到配置文件中remote_hosts=ip:1099,如果有多个，用逗号隔开
  - 启动jmeter-server
- 执行机（server）
  - 启动压测脚本

#### 3、非GUI模式运行

- agent：./jmeter-server -Djava.rmi.server.hostname=SERVERIP
- server：./jmeter -n -t test.jmx -r -l result.jtl -e -o tableresult



### 十三、逻辑控制器

#### 1、如果（if）控制器

- 位置：线程组--添加--逻辑控制器--如果（if）控制器
- 配置如果（if）控制器
  - 不勾选任何选项的使用方法
    - 字符串比较
      - 参数和值都需要引号，例如："${test}"="测试"
    - 数字比较
      - ${num}==1
    - 布尔值
      - ${flag}
      - 参数的值必须是小写tue/false
  - 勾选 “将条件接收为变量表达式”
    - 借助函数`_jexl3` 或 `groovy` 直接输出 true/false
    - ${__groovy("${test}"=="测试",)}
    - ${__jexl3("${test}"=="测试",)}
  - 全部勾选
    - 借助 ${JMeterThread.last_sample_ok}
    - 返回的是上一个取样器的执行结果，如果执行通过，返回true 那么下一个取样器才可以正常执行，反之下一个取样器不执行

#### 2、foreach控制器

- 位置：线程组 -- 添加 -- 逻辑控制器 -- foreach 控制器
- 使用方式
  - foreach 控制器 和用户定义变量配合使用
  - 1、设置用户定义变量
    - 变量名：前缀_数字  例如：test_1 \ test_2
    - 要求后缀的数字一定是连续的
  - 2、配置foreach控制器
    - 1、输入变量前缀： test
    - 2、开始循环字段（不包含）：0
    - 3、结束循环字段（包含）：根据实际情况配置 
    - 4、输出的变量名称：指用户定义的变量通过foreach控制器后，新名称
  - 3、使用foreach 控制器输出的变量 ${新名称}

#### 3、循环控制器

- 位置： 线程组 -- 添加 -- 逻辑控制器 -- 循环控制器
- 配置：循环次数 5 -- 在循环控制器 添加 取样器
  - 注意： 这里的循环只针对 包含的取样器，如果线程组设置了循环2次，那么该请求最终 会   （线程组循环次数）2*（循环控制器控制次数）5 = 10次

#### 4、while控制器

#### 5、switch控制器

#### 6、事务控制器

#### 7、include控制器

#### 8、Runtime控制器

#### 9、临界部分控制器

#### 10、交替控制器

#### 11、仅一次控制器

#### 12、录制控制器

#### 13、简单控制器

#### 14、随机控制器

#### 15、吞吐量控制器

#### 16、模块控制器



### 十四、jmeter性能测试

#### 1、什么是软件测试和性能测试

- 定义：软件的性能是软件的一种非功能特性，它关注的不是软件是否能够完成特定的功能，而是在完成改功能时展示出来的及时性
- 由定义可知性能关注的是软件的非功能特性，所以一般来说性能测试介入的时机是在功能测试完成之后。另外，由定义中的及时性可知性能也是一种指标，可以用时间或其他的指标来衡量，通常我们会使用某些工具或手段来检测软件的某些字标是否达到了要求，这就是性能测试
- 性能测试定义：指通过自动化的测试工具模拟多种正常、峰值以及异常负载调教对系统的各项性能指标进行测试

#### 2、不同人眼中的性能

- 用户
  - 还要让我等多久？ -- 响应时间
  - 为什么总是失败? -- 稳定性
- 开发
  - 架构设计是否合理？ -- 架构设计
  - 数据库设计是否合理？ -- 数据库设计
  - 代码是否存在性能问题？ -- 代码
  - 是否有不合理的内存使用？ -- 代码
  - 是否有不合理的线程同步操作？ -- 代码
  - 是否有不合理的资源竞争？ -- 代码
  - 代码算法是否还能有进一步提升？ -- 代码
- 运维
  - 服务器资源使用合理吗？ -- 资源利用率
  - 数据库使用合理吗？ -- 资源利用率
  - 环境是否实现扩展？ -- 可扩展性
  - 最大支撑多少用户访问？ -- 系统容量
  - 最大业务处理量？ -- 系统容量
  - 系统有哪些潜在的瓶颈？ -- 可扩展性
  - 更换哪些设备，添加哪些机器可以提高系统性能？ -- 可扩展性
  - 7X24 小时连续不间断业务访问？ -- 稳定性
- 测试
  - 全面的性能，包括用户、开发、管理员等各视角的性能

#### 3、性能测试类型和相关术语

- 负载：模拟业务操作对服务器造成压力的过程，比如模拟100个用户登录
- 基准测试：在给系统施压较低时，查看系统的运行状况记录相关数据作为基础参考
- 负载测试 Load Test：是指对系统不断地增加压力或增加一定压力下的持续时间，直到系统的某项或多项性能指标达到安全临界值，例如某种资源已经达到饱和状态等
- 压力测试Stress Test:压力测试是评估系统处于或超过预期负载时体系的运行情况，关注点在于系统的峰值负载或超过最大负荷情况下的处理能力
- 稳定性测试：在给系统施加一定业务压力的情况下，使系统运行一段时间，以此检测系统是否稳定
- 并发测试：测试多个用户同时访问同一应用、同一模块或者数据记录时是否存在死锁或者其他性能问题。

#### 4、性能测试应用场景（领域）

|              |                                             主要用途 | 典型场景                                                     | 特点                                                         | 常用性能测试方法               |
| ------------ | ---------------------------------------------------: | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------ |
| 能力验证     | 关注在给定的软硬件条件下，系统能否具有预期的能力表现 | 在要求平均响应时间小于2秒的前提下，如何判断系统是否能够支持50万用户/天的访问量？ | 要求在已确定的环境下运行、需要更具典型场景设计测试方案和用例，包括操作序列和并发用户量，需要明确的性能目标 | 负载测试、压力测试、稳定性测试 |
| 规划能力     |                 关注如何使系统具有我们要求的性能能力 | 某某系统计划在一年内用户量到XXX万，系统到时候是否能支持这么多用户量？如果不能需要如何调整系统的配置 | 它是一种探索性的测试、常用于了解系统性能和获得扩展性能的方法 | 负载测试、压力测试、配置测试   |
| 性能调优     |                               主要用于对系统性能调优 | 某某系统上线运行一段时间后响应速度越来越慢，此时应该如何办？ | 每次只改变一个配置、切忌无休止的调优                         | 并发测试、压力测试、配置测试   |
| 缺陷发现     |                         发现缺陷或问题重现、定位手段 | 某些缺陷只有在高负载情况下才能暴露出来，如线程锁、资源竞争或内存泄漏 | 作为系统测试补充，用来发现并发问题，或是对系统已经出现的问题进行重现和定位 | 并发测试、压力测试             |

**性能基准比较:**

- 常用于敏捷开发过程中，敏捷开发流程的特点是小步快走、快速试错、迭代周期短，需求变化频繁，很难定义完善的性能测试目标，也没有时间再每个迭代上开展详细的性能测试，可以通过建立性能基线，通过比较每次迭代中的性能表现变化，判断迭代是否达到了目标

#### 5、性能测试指标（重要）

- 性能测试指标概念及目的：

  - 概念：性能测试是通过测试工能局模拟多种正常、峰值及异常负载条件来对系统的各项性能指标进行测试
  - 目的：验证软件系统是否能够达到用户提出的性能指标，发现系统中存在的性能瓶颈并加以优化

- 性能指标分类：

  - 系统指标（与用户场景和需求相关指标）
  - 资源指标（与硬件资源消耗相关指标）

- 系统指标

  - 1、响应时间、平均响应时间

    ```
    对一个请求做出相应所需要的时间
    响应时间 = 网络响应时间 + 应用程序响应时间 
    
    平均响应时间：所有请求花费的平均时间
    如：如果有100个请求，其中 98个耗时为1ms，其他两个都为100ms
    平均响应时间： （98*1 + 2*100） /100 = 2.98 ms,但是2.98ms并不能反映服务器的整体效率，因为98个请求耗时才1ms，引申出 百分位数
    
    百分位数：以响应时间为例，指的是 99% 的请求响应时间，都处在这个值一下，更能体现整体效率。响应时间和负载的关系
    ```

  - 2、并发用户数

    ```
    并发主要是针对服务器而言，在同一时刻与服务器进行交互（指向服务器发出请求）的在线用户数
    
    并发用户数：某一物理时刻同时向系统提交请求的用户数，提交的请求可能是统一场景或功能，也可以是不同场景或功能
    在线用户数：某时间段内访问系统的用户数，这些用户并不一定会同时向系统提交请求
    系统用户数：系统注册的总用户数量
    
    三者之间的关系： 系统用户数 >= 在线用户数 >= 并发用户数
    
    并发用户数C ， 计算公式 C=nL/T  （经验公司）
    n: 每天访问系统的用户数
    L：在线用户从登陆到退出的时间
    T：用户每天使用系统大概多长时间
    峰值C1，最大并发数，计算公式 C1=C+3√C
    
    假设业务量=用户量
    1）业务量：40万*80% =32万
    2）时间段：24小时*20% = 4.8 小时
    3）每小时的业务量： 32万/4.8小时 = 6.67万/小时
    4）每秒的业务量 66700/3600=18.53笔/秒 即每个user迭代一次的时间：1/18.53 = 0.053秒
    5）每人每笔业务的处理时间要求=2秒 2/0.053 = 37.7 个user
    小结：系统要达到40万的业务访问量，需要38个并发用户持续运行4.8小时，如果满足则性能通过
    
    ```

  - 3、吞吐量、吞吐率

    ```
    衡量网络性能的重要指标
    
    吞吐量：网络传输的数据量（处理客户的请求数）throughput
    
    吞吐率：单位时间（可以是秒/分/时/天）内网络成功传输的数据量，如请求数/秒、页面数/秒
    
    从业务角度看，吞吐量可以用：请求数/秒、页面数/秒、人数/天 或 业务处理数/小时等单位来衡量
    从网络角度看，吞吐量可以用：字节/秒来衡量
    对于交互式应用来说，吞吐量指标反映的是服务器承受的压力，他能够说明系统的负载能力
    
    ```

  - 4、事务 、TPS、QPS

    ```
    事务：可以看作是一个动作或是一系列动作的集合，例如登录，从登陆开始到登陆结束为一个事务
    
    TPS：衡量系统处理事务或交易的能力，每秒处理的事务数
    QPS：服务器在疫苗的时间内处理了多少请求，也就是最大吞吐能力
    	单台服务器的QPS 一般是在 50 - 70 之间
    
    QPS(TPS) -- 并发数/平均响应时间
    	一个系统吞吐量通常由QPS（TPS）、并发数两个因素决定，每套系统这两个值都有一个相对极限值，在应用场景访问压力下，只要某一项达到系统最高值，系统的吞吐量就上不去了，如果压力继续增大，系统的吞吐量反而会下降，原因是系统超负荷工作，上下文切换、内存等等其他消耗导致系统性能下降
    ```

  - 点击量、点击率（Hits Per Second）
    - 点击数：指web Server收到的Http请求数
    - 点击率：单位时间每秒用户想Web Server 提交的Http请求数
    - 区分鼠标点击数：如请求一个网页，网页包含3张图片，向web Server 请求的点击数： 1 + 3 =4，而鼠标的一次点击就可以访问网络，点击数只有1次
  - PV 和UV
    - PV：访问一个URL，产生一个PV（Page View 页面访问量），每日每个网站的总PV量形容一个网站规模的重要指标
    - UV：作为一个独立的用户，访问站点的所有页面均算作一个UV（Unique Viaitor,用户访问）
      - 单台服务器每天PV计算
        - 公式1：每天总PV = QPS \*3600\*6
        - 公式2：每天总PV = QPS \*3600\*8

- 资源指标





#### 10、server_agent 性能监控步骤

- 使用插件 选择 Stepping Thread Group 线程组

- 配置 Stepping Thread Group 线程组

  - This Group will start -- 表示本次测试共执行的线程总是
  - First wait for  -- 表示 首次启动线程的等待时间
  - Then start -- 表示首次启动线程数
  - Next，add -- 每次增加的线程数 、threads every -- 每次增加的线程要运行时间
  - using range-up  -- 表示增加线程数的时间间隔
  - Then hold load for -- 表示所有线程都运行后，持续时间
  - Finally，stop -- 表示每次释放线程数、threads every -- 每次释放的线程要运行时间

- 遵循的原则

  - 给系统增加压力是，逐渐增加，释放压力，可以快速释放

- Stepping Thread Group线程组可以实现

  - 负载测试
  - 压力测试
  - 稳定性测试

- 监控服务器资源配置

  - 添加PerMon Metrics Collector 组件
  - 在服务器中启动server-Agent

- 配置PerMon Metrics Collector 组件

  - 点击Add Row 添加需要监控的内容
  - Hosts/Ip 一定是服务器的域名或IP
  - Port -- Server Agent 启动的端口号
  - Metrics to Collect  -- 选择监控服务器的具体内容
    - cpu/内存/网络/磁盘

  

  

  

### 十五、jmeter 实例

#### 1、http 测试

#### 2、ftp 测试

#### 3、数据库测试

#### 4、ldap 测试

#### 5、smtp测试

#### 6、websocket 测试

#### 7、dns 测试

#### 8、弱网测试

#### 9、安全测试

#### 10、jmeter网页爬虫

#### 11、jmeter全链路压测

#### 12、jmeter阶梯式压测

#### 13、固定吞吐量测试

#### 14、jmeter influxdb grafana 集成性能监控

#### 15、 jmeter ant jenkins 集成环境











