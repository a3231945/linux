     
                                ++++++++++++
                                +  Client  +   eth0 192.168.1.1/24
                                ++++++++++++
                                     |
                                     |
                                ++++++++++++   up   eth0:192.168.1.254/24
                                +     GW   +
                                ++++++++++++   down eth1:1.1.1.254/24
                                      |
                                      |
                                ++++++++++++   VIP eth0:1.1.1.1/24
                                + Director +
                                ++++++++++++   DIP eth1:172.16.1.254/24
                                     |        
                          ___________|____________
                         |                        | 
                         |                        |
             ++++++++++++                 ++++++++++++++
            + Real Server A  +         + Real Server B  +
             ++++++++++++                 ++++++++++++++
             eth0:172.16.1.1/24             eth0:172.16.1.2/24




**1、配置LVS VS/NAT模式**

Client:

     [root@localhost ~]# route add default gw 192.168.1.254 dev eth0


GW:

     [root@localhost ~]# echo 1 > /proc/sys/net/ipv4/ip_forward


Real Server A & Real Server B:

     [root@localhost ~]# yum install httpd
     [root@localhost ~]# route add default gw 172.16.1.254 dev eth0


Director:

     [root@localhost ~]# echo 1 > /proc/sys/net/ipv4/ip_forward
     [root@localhost ~]# route add default gw 1.1.1.254 dev eth0
     [root@localhost ~]# yum install ipvsadm
     [root@localhost ~]# ipvsadm -A -t 1.1.1.1:80 -s rr
     [root@localhost ~]# ipvsadm -a -t 1.1.1.1:80 -r 172.16.1.1:80 -m
     [root@localhost ~]# ipvsadm -a -t 1.1.1.1:80 -r 172.16.1.2:80 -m
     [root@localhost ~]# ipvsadm -Ln
     [root@localhost ~]# ipvsadm -Ln --stats


**2、数据包走向**

     1.Client---------->GW
     
     sip:CIP           dip:VIP
     smac:Client_mac 	dmac:GW_up_mac
     
     2.GW-------------->Director
     sip:CIP 		dip:VIP
     smac:GW_down_mac 	dmac:VIP_mac
     
     3.Director-------->Real Server (DNAT)
     sip:CIP 		dip:RIP
     smac:DIP_mac 	dmac:RealServer_mac
     
     4.Real Server----->Director
     sip:RIP                    dip:CIP
     smac:RealServer_mac        dmac:DIP_mac
     
     5.Director-------->GW
     sip:VIP 		dip:CIP
     smac:VIP_mac 	dmac:GW_down_mac
     
     6.GW-------------->Client
     sip:VIP 		dip:CIP
     smac:GW_up_mac 	dmac:Client_mac
