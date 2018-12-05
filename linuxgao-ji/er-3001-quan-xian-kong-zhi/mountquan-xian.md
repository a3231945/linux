###文件系统的挂载选项

**1、rw	ro**
  
      
    [root@node0 test]# mount -o remount,ro /test/
    [root@node0 test]# echo "abc" >> /test/file1
    bash: /test/file1: Read-only file system


**2、exec 	noexec**
  
      
    [root@node0 ~]# mount /dev/hdb1 /test/
    [root@node0 ~]# echo "hello" > /test/file1
    [root@node0 ~]# cp /bin/cat /test/
    [root@node0 ~]# cd /test/
    [root@node0 test]# ./cat file1
    hello
    [root@node0 test]# mount -o remount,noexec /test/
    [root@node0 test]# ./cat file1
    bash: ./cat: Permission denied


**3、suid 	nosuid**
 
       
    [root@node0 test]# mount /dev/hdb1 /test/
    [root@node0 test]# ls
    cat cdrom file1 lost+found
    [root@node0 test]# chmod 640 file1
    [root@node0 test]# ls -l file1
    -rw-r----- 1 root root 6 Mar 3 16:04 file1
    [root@node0 test]# chmod u+s cat
    [root@node0 test]# su - user1
    [user1@node0 ~]$ cd /test/
    [user1@node0 test]$ ./cat file1
    hello
    [root@node0 test]# mount -o remount,nosuid /test/
    [root@node0 test]# su - user1
    [user1@node0 ~]$ cd /test/
    [user1@node0 test]$ ./cat file1
    ./cat: file1: Permission denied


**4、dev 	nodev**
 
       
    [root@node0 test]# ls -l /dev/cdrom
    lrwxrwxrwx 1 root root 3 Mar 3 14:47 /dev/cdrom -> hdd
    [root@node0 test]# ls -l /dev/hdd
    brw-rw---- 1 root disk 22, 64 Mar 3 14:47 /dev/hdd
    [root@node0 test]# mknod cdrom b 22 64
    [root@node0 test]# mount cdrom /mnt/
    mount: block device cdrom is write-protected, mounting read-only
    [root@node0 test]# umount /mnt/
    [root@node0 test]# mount -o remount,nodev /test/
    [root@node0 test]# ls -l cdrom
    brw-r--r-- 1 root root 22, 64 Mar 3 16:13 cdrom
    [root@node0 test]# mount cdrom /mnt/
    mount: block device cdrom is write-protected, mounting read-only
    mount: cannot mount block device cdrom read-only
    [root@node0 test]# ls /mnt/
    
    
**5、auto 	noauto**
 
       
    [root@node0 ~]# vim /etc/fstab
    /dev/hdb1 /test ext3 noauto 0 0
    [root@node0 ~]# mount -a
    
    
**6、async sync**
    
    
**7、atime noatime**
    
    Update inode access time for each access. This is the default.
    
**8、noacl	acl**

