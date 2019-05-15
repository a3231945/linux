### 头指针分离

**问题描述：**

    master 指针和 HEAD 指针 不同步移动

**出现原因：**
    
    checkout 了某个具体的 commit（再提交修改后发现master 指针与HEAD指针分离）
    
**解决办法：**

    # 强制将 master 分支指向当前头指针的位置
    git branch -f master HEAD
    
    # 检出 master 分支
    git checkout master    
