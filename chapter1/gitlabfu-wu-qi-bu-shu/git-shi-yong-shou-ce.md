# Git 命令

### 快速使用 ###
 
1、hosts 修改 ，编辑 %systemroot%\System32\drivers\etc\hosts 文件

    #添加一行
    IP     gitlab
2、打开浏览器访问 http://gitlab ，点击注册

![](http://i.imgur.com/ctRwbP9.png)

3、填写相关信息

![](http://i.imgur.com/6IstPy8.png)

**注意：**
>1、密码为8位数以上字符串
>2、邮箱一定要填写正确邮箱。之后需要使用邮箱验证。

4、邮件验证完成，联系管理员获取相应代码库的权限

5、git 客户端安装
    
    #windows下载地址：
    https://github.com/git-for-windows/git/releases/download/v2.12.2.windows.2/MinGit-2.12.2.2-64-bit.zip    
    
    #mac安装：
    brew install git    

6、git 免秘钥
    
    #windows  打开 git Bash
    ssh-keygen -t rsa -C “你的邮箱” 

    系统盘用户目录会有一个 .ssh 目录。里面存放着公钥和私钥文件

-  登陆gitlab、选择配置设置
-  选择SSH KEYS
-  填写相应信息
    
![](http://i.imgur.com/8IPrTBW.png)
    
7、初始化本地代码库

    #设置你的名称
    git config --global user.name "XXX"
    #设置你的邮箱
    git config --global user.email "XXX"
    #设置SSL忽略
    git config --global http.sslVerify false
    #克隆完整代码库
    git clone https://gitlab/oma/doc.git
    



### 一、git 创建版本库

1、克隆远程版本库

    git clone [url] 

2、初始化本地版本库

    git init 

-----

### 二、修改和提交
1、查看状态

    git status

2、查看变更内容

    git diff
    
    git diff --cached
    
    git diff commitid:file1 commitid:file2

3、跟踪所有改动过的文件

    git add .

4、跟踪指定的文件

    git add <file\>

5、文件改名

    git mv <old\> <new\>

6、删除文件

    git rm <file\>

7、停止跟踪文件但不删除

    git rm --cache <file\>

8、提交所有更新过的文件

    git commit -m "commit message"

9、修改最后一次提交

    git commit --amend 

10、跳过缓存区提交
    
    git commit -a -m "commit message"
-----

### 三、查看提交历史

1、查看提交历史

    git log

2、查看指定文件的提交历史

    git log -p <file\>

3、以列表方式查看指定文件的提交历史

    git blame <file\>

-----

### 四、撤销
1、撤销缓存区域的文件

    git reset HEAD <file\>
    
2、撤销工作目录中所有未提交的文件的修改内容

    git reset --hard HEAD

3、撤销指定的未提交文件的修改内容

    git checkout HEAD <file\>

4、撤销指定的提交

    git revert <commit\>

5、取消对文件的修改

    git checkout -- <file\>


-----

### 五、分支与标签

1、显示所有本地分支

    git branch 

2、切换到指定分支或标签

    git checkout <branch/tag/>


3、创建新分支

    git branch <new-branch/>

4、删除本地分支

    git branch -d <branch/>

5、列出所有本地标签

    git tag

6、基于最新提交创建标签

    git tag <tagname\>

7、删除标签

    git tag -d <tagname\>

-----

### 六、合并与衍合

1、合并指定分支到当前分支

    git merge <branch\>

2、衍合指定分支到当前分支

    git rebase <branch\>

-----

### 七、远程操作

1、查看远程版本库信息

    git remote -v 

2、查看指定远程版本库信息

    git remote show <remote\>

3、添加远程版本库

    git remote add <remote\> <url\>

4、从远程代码库获取代码

    git fetch <remote\>

5、下载代码及快速合并

    git pull <remote\> <branch\>

6、上传代码及快速合并

    git push <remote\> <branch\>

7、删除远程分支或标签

    git push <remote\> :<branch/tag-name\>

8、上传所有标签

    git push --tags

### 八、其他
1、配置git命令补全

    #找到补全shell
    rpm -ql git | grep bash 
    /etc/bash_completion.d
    /etc/bash_completion.d/git
    /usr/share/doc/git-1.7.1/contrib/completion/git-completion.bash

    #拷贝补全bash
    cp /usr/share/doc/git-1.7.1/contrib/completion/git-completion.bash ~/.git-completion.bash
    
    #加载环境变量
    source ~/.git-completion.bash
    
2、配置git别名

    git config --global alias.co checkout
    git config --global alias.br branch
    git config --global alias.ci commit
    git config --global alias.st status    
    


    
    
