**DELL 服务器有时会若硬件的改动，在开机以后会提示错误信息。信息一般会提示在显示模块上，以下为常见问题及解决方法：**



| **报错信息** | **原因** | 解决方法 |
| :--- | :--- | :--- |
| Alert! iDRAC6 not responding.Rebooting. | iDRAC6 未响应 BIOS 通信，一种原因是它未正常运行，另一种原因是它未完成初始化。系统将重新引导。 | 请等待系统重新引导。 |
| Alert! iDRAC6 not responding.Power required may exceed PSU wattage.Alert! Continuing system boot accepts the risk that system may power down without warning. | iDRAC6 挂起。系统在引导时，iDRAC6 被远程重设。在交流电恢复之后，iDRAC6 需要比正常情况下更长的时间来引导。 | 断开系统的交流电源 10 秒，然后重新启动系统。 |
| Alert! Node Interleaving disabled! Memory configuration does not support Node Interleaving. | 内存配置不支持节点交叉，或配置已更改（例如，内存模块出现故障），导致无法支持节点交叉。 系统将继续运行，但没有节点交叉功能。 | 请确保将内存模块安装在支持节点交叉的配置中。 |
| Alert! Power required exceeds PSU wattage.Check PSU and system configuration.Alert! Continuing system boot accepts the risk that system may power down without warning. | 电源设备可能不支持处理器、内存模块和扩充卡的系统配置。 | 如果某些系统组件刚刚进行了升级，请将系统恢复为以前的配置。 |
| Alert! Redundant memory disabled!Memory configuration does not support redundant | 虽然系统设置程序中已启用内存镜像功能，但当前配置不支持冗余内存。内存模块可能出现故障。 | 请检查内存模块是否出现故障。 |
| BIOS MANUFACTURING MODE detected.MANUFACTURING MODE will be cleared before the next boot.System reboot required for normal operation. | 系统处于生产模式。 | 请重新引导系统使其退出生产模式。 |
| BIOS Update Attempt Failed! | 远程 BIOS 更新尝试失败。 | 请重新尝试更新 BIOS。 |
| Caution!NVRAM\_CLR jumper is installed on system board | NVRAM\_CLR 跳线采用清除设置进行安装。CMOS 已被清除。 | 请将 NVRAM\_CLR 跳线移动到默认位置（插针 3 和 5）。 |
| CPU set to minimum frequency. | 处理器速度可能出于节能考虑而有意设得较低。 |  |
|  |  |  |
|  |  |  |
|  |  |  |



