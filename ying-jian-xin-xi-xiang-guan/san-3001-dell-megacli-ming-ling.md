#常用命令

###一、raid信息 查看
**RAID 1**

    RAID Level : Primary-1, Secondary-0, RAID Level Qualifier-0      
**RAID 0**

    RAID Level : Primary-0, Secondary-0, RAID Level Qualifier-0      
**RAID 5**

    RAID Level : Primary-5, Secondary-0, RAID Level Qualifier-3

 **RAID 6**
 
    RAID Level : Primary-6, Secondary-0, RAID Level Qualifier-3
      
**RAID 10**
   
    RAID Level : Primary-1, Secondary-3, RAID Level Qualifier-0      

###二、常用命令
**查看硬盘当前状态：**

    /opt/MegaRAID/MegaCli/MegaCli64 -PDList -aAll|egrep  "Adapter|Enclosure Device ID|Slot Number|Foreign State:"  
**清除外来设备:**

    /opt/MegaRAID/MegaCli/MegaCli64 -cfgforeign -clear -a0   （在线更换硬盘或者增加硬盘后需要操作）
**创建raid方法：**

>raid 0:
 
    /opt/MegaRAID/MegaCli/MegaCli64 -CfgLdAdd -r0 [32:5,32:6,32:7,32:8,32:9] WB Direct -a0
>raid 1:

    /opt/MegaRAID/MegaCli/MegaCli64 -CfgLdAdd -r1 [32:4,32:5] WB Direct -a0
>raid5:

    /opt/MegaRAID/MegaCli/MegaCli64 -CfgLdAdd -r5 [32:2,32:3,32:4,32:5,32:6,32:7] WB Direct -a0
>raid 6:
    
    /opt/MegaRAID/MegaCli/MegaCli64 -CfgLdAdd -r6 [32:2,32:3,32:4,32:5] WB Direct -a0    
>raid 10:

    /opt/MegaRAID/MegaCli/MegaCli64 -CfgSpanAdd -r10 -Array0[32:2,32:3] -Array1[32:4,32:5] -Array2[32:6,32:7] WB Direct -a0

>raid5 + 热备盘

    /opt/MegaRAID/MegaCli/MegaCli64 -CfgLdAdd -r5 [32:2,32:3,32:4] WB Direct -Hsp[32:5] -a0

**删除raid:** 

    /opt/MegaRAID/MegaCli/MegaCli64  -cfglddel  -L1 -force -a0
**查看所有硬盘详细信息：**

    /opt/MegaRAID/MegaCli/MegaCli64 -PDList -aALL 
**查看所有raid信息：**

    /opt/MegaRAID/MegaCli/MegaCli64 -LDInfo -LALL -aAll 
**查看硬盘物理状态：**

    /opt/MegaRAID/MegaCli/MegaCli64 -PDList -aALL |grep "Firmware state" 
**外部设备导入raid信息：**

    /opt/MegaRAID/MegaCli/MegaCli64 -cfgforeign -import -a0
**修改硬盘模式为unconfigure good：**

    /opt/MegaRAID/MegaCli/MegaCli64 -PDMakeGood -PhysDrv[32:4] -force -a0  
**显示硬盘重建进度：**

    /opt/MegaRAID/MegaCli/MegaCli64 -PDRbld -ShowProg -PhysDrv [32:1] -a0  
**查看日志：**

    /opt/MegaRAID/MegaCli/MegaCli64 -FwTermLog dsply -a0
**查看raid卡电池状态：**

    /opt/MegaRAID/MegaCli/MegaCli64 -adpallinfo -a0|grep -i bbu
**查看raid卡的信息：**

    /opt/MegaRAID/MegaCli/MegaCli64 -AdpAllInfo -aALL
**查看磁盘缓存策略：**

    /opt/MegaRAID/MegaCli/MegaCli64 -LDGetProp -Cache -LALL -aALL
**设置磁盘缓存策略：**

    缓存策略解释：

    WT (Write through
    WB (Write back)
    NORA (No read ahead)
    RA (Read ahead)
    ADRA (Adaptive read ahead)
    Cached
    Direct
    例子：
    #/opt/MegaRAID/MegaCli/MegaCli64 -LDSetProp WT|WB|NORA|RA|ADRA -L0 -a0
    or
    #/opt/MegaRAID/MegaCli/MegaCli64 -LDSetProp -Cached|-Direct -L0 -a0
    or
    enable / disable disk cache
    #/opt/MegaRAID/MegaCli/MegaCli64 -LDSetProp -EnDskCache|-DisDskCache -L0 -a0
     
    
    