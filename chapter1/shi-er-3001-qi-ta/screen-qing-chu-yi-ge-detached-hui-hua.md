
### 1、查看会话

     # screen -ls
     There is a screen on:
     	185512.checkice	(Detached)
     1 Socket in /var/run/screen/S-root.



### 2、清除会话

**方法一：**

     # screen  -X -S 185512 quit
     
     验证结果：
     # screen -ls
     No Sockets found in /var/run/screen/S-root.
     
     
     语法： screen  -X -S ID quit
     
**方法二：**

      screen -r ID/NAME
      并利用exit退出并kiil掉session。
      
      例如：
      
          # screen  -ls
          There is a screen on:
          	5694.test	(Detached)
          1 Socket in /var/run/screen/S-root.
          
          # screen  -r 5694
          [screen is terminating]
          =====>进入screen会话输入：exit 或 按快捷键 ctrl + D 退出
          
          # screen  -ls     
          No Sockets found in /var/run/screen/S-root.



