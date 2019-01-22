### 一、 明确搜索仓库标题、仓库描述、README
**1、查找库名称**

    语法:
        in:name  关键词
        
    例如：
        in:name  lvs
**2、查找库描述**

    语法:
        in:descripton 关键词
        
    例如：
        in:descripton lvs 
        
**3、查找readme**

    语法:
        in:readme 关键词
    例如：
        in:readme lvs   
         
### 二、 明确搜索star、fork 条件
**1、查找stars 条件判断(大于、小于、范围)**

    语法:
        stars:>数字 关键词
    例如：
        stars:>500 lvs
 
    语法:
        stars:<数字 关键词
    例如：
        stars:<500 lvs      
    
    语法:
        stars:N..M 关键词
    例如：
        stars:10..20 lvs 
    
        
**2、查找fork 条件判断(大于、小于、范围)**
    
    同理：stars              

### 三、明确搜索仓库大小

    语法:
        size:>=大小 关键词
    例如：
        size:>=5000 lvs
        
    注意：5000 代表5M
    
### 四、明确仓库是否还在更新维护

    语法:
        pushed:>时间戳 关键词
    例如：
        pushed:>2018-01-01 lvs

### 五、明确搜索仓库的LICENSE

    语法:
        license:协议 关键词
    例如：
        license:apache-2.0 kubernetes   

### 六、明确搜索的语言

    语法:
        language:语言  关键词
    例如：
        language:c lvs

### 七、明确搜索个人或组织的仓库
 
    语法:
        user:用户|组织
    例如：
        user:alibaba   
### 八、其他
    
    注意：以上的语法可以组合使用 用空格隔开
    
    例如: 
         language:c lvs user:alibaba
    