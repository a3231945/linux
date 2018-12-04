#SVN 管理

###一、svn 库管理

**1、svn 数据库初始化**
   
    svnadmin create DIR

**2、svn 查看库**

    svn list file:///DIR

**3、svn import 初始化库**
   
    svn import dir file:///DIR/dir -m "init info"
    
    svn list file:///DIR

**4、svn 数据库检出**

    svn checkout file：///DIR
    OR

    #简写
    svn co  file:///DIR

    #指定其中一个库
    
    svn checkout file:///DIR/REPO
   
    #指定版本
    svn checkout -r VERSION file:///DIR 

**5、svn 数据库导出**
    
    svn export file:///DIR
    
    #指定版本
    svn export -r VESION file:///DIR
    
    #导出不包含.svn 目录
    指定版本号即可
        
###二、svn 客户端管理

**1、更新最新的工作拷贝**

    svn update
    #更新指定版本
    svn update -r VERSION

    
**2、修改**
    
    svn add
    svn delete
    svn copy
    svn move

**3、检查修改**
    
    svn status
    svn diff

**4、取消修改**
    
    svn revert

**5、解决冲突（合并别人的修改）**
    
    svn update
    svn resolved

**6、提交你的修改**
    
    svn commit

**7、状态表示**

    A  预定加入到版本的文件、目录或符号链接
    C  文件item发生冲突，在从服务器更新时与本地版本发生交迭
    D  文件、目录或是符号链item预定从版本库中删除
    M  文件item的内容被修改了

###三、svn 检验历史

    svn log      查看日志
    svn diff     显示特定修改
    svn cat      查看指定版本
    svn list     显示一个目录再某一个版本存在的文件

**实例：**

- **svn log**


    #查看版本二
    svn log -r 2

    #查看版本二，第5次修订
    svn log -r 2:5

    #查看单个文件/目录的日志
    svn log FILE/DIR

- **svn diff**


    #比较本地修改
    svn diff
    
    #比较和指定版本(3)
    svn diff -r 3 FILE/DIR

    #比较版本2 和版本3
    svn diff -r 2:3 FILE/DIR


- **svn cat**

    
    #查看版本2 的文件
    svn cat -r 2 FILE

    
- **svn list**
    
    
    #查看远程目录
    svn list file:///DIR
    svn list http://DIR
 
###四、svn清理
    
    svn cleanup
       