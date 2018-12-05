

                ++++++++++++
                  +Client + eth0 192.168.1.1/24
                ++++++++++++
                    ｜
                    |
                    
                    ++++++++++++ up eth0 192.168.1.254/24
        |----------> +	GW +
        |         ++++++++++++ down eth1 1.1.1.254/24
        |             ｜
        |             |
        |         ++++++++++++ VIP eth0:1 1.1.1.1/24
        |         + Director +
        |         ++++++++++++ DIP eth0 1.1.1.100/24
        |                 |
        |__________________|_______________
            |                             |
            |                             |
        ++++++++++++             ++++++++++++
        + Real Server A +     + Real Server B +
        ++++++++++++             ++++++++++++
        eth0 1.1.1.10/24         eth0 1.1.1.20/24


**1、数据包走向**

    1.Client---------->GW
    sip:CIP         dip:VIP
    smac:Client_mac dmac:GW_up_mac
    
    2.GW-------------->Director
    sip:CIP          dip:VIP
    smac:GW_down_mac dmac:VIP_mac
    
    3.Director-------->Real Server
    ******************************************************************
    *	Director在给Real Server发包前要广播找Real Server mac	*
    *	sip:	DIP sip:	RIP	*
    *	smac:	DIP_mac dmac:	broadcast	*
    *	*
    *	sip:	RIP dip:	DIP	*
    *	smac:	RealServer_mac	dmac:	DIP_mac	*
    ******************************************************************
    sip:CIP        dip:VIP
    smac:DIP_mac   dmac:RealServer_mac
    
    4.Real Server----->GW
    sip:VIP              dip:CIP
    smac:RealServer_mac  dmac:GW_down_mac
    
    5.GW-------------->Client
    sip:VIP        dip:CIP
    smac:GW_up_mac dmac:Client_mac



**2、配置LVS VS/NAT模式**

Client:

    [root@localhost ~]# route add default gw 192.168.1.254 dev eth0


GW:

    [root@localhost ~]# echo 1 > /proc/sys/net/ipv4/ip_forward


Real Server A & Real Server B:

    [root@localhost ~]# yum install httpd
    [root@localhost ~]# ifconfig lo:1 1.1.1.1/32
    [root@localhost ~]# echo 1 > /proc/sys/net/ipv4/conf/eth0/arp_ignore
    [root@localhost ~]# echo 2 > /proc/sys/net/ipv4/conf/eth0/arp_announce


Director:

    [root@localhost ~]# yum install ipvsadm
    [root@localhost ~]# ipvsadm -A -t 1.1.1.1:80 -s rr
    [root@localhost ~]# ipvsadm -a -t 1.1.1.1:80 -r 1.1.1.10:80 -g
    [root@localhost ~]# ipvsadm -a -t 1.1.1.1:80 -r 1.1.1.20:80 -g
    [root@localhost ~]# ipvsadm -Ln
    [root@localhost ~]# ipvsadm -Ln --stats