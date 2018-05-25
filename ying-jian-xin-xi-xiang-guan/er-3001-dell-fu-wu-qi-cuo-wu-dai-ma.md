###DELL服务器LCD信息代码的意思：
**E1114**

	Temp Ambient
	系统周围环境温度超出允许范围。
	
**E1116**

	Temp Memory
	内存已超过允许温度，系统已将其禁用以防止损坏组件。
**E1210**

	CMOS Batt
	缺少 CMOS 电池，或电压超出允许范围。
**E1211**
	
	ROMB Batt
	RAID 电池丢失、损坏或因温度问题而无法再充电。
**E12nn**

	XX PwrGd
	指定的稳压器出现故障。
**E1229**

	CPU # VCORE
	处理器 # VCORE 稳压器出现故障。
**E122B**

	0.9V Over Voltage
	0.9 V 稳压器电压已超过电压允许范围
**E122C**

	CPU Power Fault
	启动处理器稳压器之后，检测到稳压器故障
**E1310**

	RPM Fan ##
	指定的冷却风扇的 RPM 超出允许的运行范围。
**E1410**

	CPU # IERR
	指定的微处理器正在报告系统错误。
**E1414**

	CPU # Thermtrip
	指定的微处理器超出了允许的温度范围并已停止运行。
**E1418**

	CPU # Presence
	指定的处理器丢失或损坏，系统的配置不受支持。
**E141C**

	CPU Mismatch
	处理器的配置不受 Dell 支持。
**E141F**

	CPU Protocol
	系统 BIOS 已报告处理器协议错误。
**E1420**

	CPU Bus PERR
	系统 BIOS 已报告处理器总线奇偶校验错误。
**E1421**

	CPU Init
	系统 BIOS 已报告处理器初始化错误。
**E1422**

	CPU Machine Chk
	系统 BIOS 已报告机器检查错误。
**E1618**

	PS # Predictive
	电源设备电压超出允许范围；指定的电源设备安装错误或出现故障。
**E161C**

	PS # Input Lost
	指定的电源设备的电源不可用，或超出了允许范围。
**E1620**

	PS # Input Range
	指定的电源设备的电源不可用，或超出了允许范围。
**E1710**

	I/O Channel Chk
	系统 BIOS 已报告 I/O 通道检查错误。
**E1711**

	PCI PERR B## D## F##
	PCI PERR Slot #
	系统 BIOS 已报告组件的 PCI 奇偶校验错误，该组件所在的 PCI 配置空间位于总线 ##，设备 ##，功能 ##。
	系统 BIOS 已报告组件的 PCI 奇偶校验错误，该组件位于 PCI 插槽 #。
**E1712**

	PCI SERR B## D## F##
	PCI SERR Slot #
	系统 BIOS 已报告组件的 PCI 系统错误，该组件所在的 PCI 配置空间位于总线 ##，设备 ##，功能 ##。
	系统 BIOS 已报告组件的 PCI 系统错误，该组件位于插槽 #。
**E1714**

	Unknown Err
	系统 BIOS 已确定系统中存在错误，但无法确定错误来源。
**E171F**

	PCIE Fatal Err B## D## F##
	PCIE Fatal Err Slot #
	系统 BIOS 已报告组件的 PCIe 致命错误，该组件所在的 PCI 配置空间位于总线 ##，设备 ##，功能 ##。
	系统 BIOS 已报告组件的 PCIe 致命错误，该组件位于插槽 #。
	卸下并重置 PCI 扩充卡。如果问题仍然存在，请参阅排除扩充卡故障。
**E1913**

	CPU & Firmware Mismatch
	BMC 固件不支持 CPU。
**E2010**

	No Memory
	系统中没有安装内存。
**E2011**

	Mem Config Err
	检测到内存，但是内存不可配置。配置内存期间检测到错误。
**E2012**

	Unusable Memory
	已配置内存，但内存不可用。内存子系统出现故障。
**E2013**

	Shadow BIOS Fail
	系统 BIOS 无法将其快擦写映像复制到内存中。
**E2014**

	CMOS Fail
	CMOS 出现故障。CMOS RAM 未正常工作。
**E2015**

	DMA Controller
	DMA 控制器出现故障。
**E2016**

	Int Controller
	中断控制器出现故障。
**E2017**

	Timer Fail
	计时器刷新故障。
**E2018**

	Prog Timer
	可编程间隔计时器错误。
**E2019**

	Parity Error
	奇偶校验错误。
**E201A**

	SIO Err
	SIO 出现故障。
**E201B**

	Kybd Controller
	键盘控制器出现故障。
**E201C**

	SMI Init
	系统管理中断 (SMI) 初始化失败。
**E201D**

	Shutdown Test
	BIOS 关闭系统检测失败。
**E201E**

	POST Mem Test
	BIOS POST 内存检测失败。
**E201F**

	DRAC Config
	Dell 远程访问控制器 (DRAC) 配置失败。
**E2020**

	CPU Config
	CPU 配置失败。
**E2021**

	Memory Population
	内存配置不正确。内存安装顺序不正确。
**E2022**

	POST Fail
	视频后出现一般故障。
**E2110**

	MBE DIMM ## & ##
	“## & ##”指示的 DIMM 组中的一个 DIMM 发生内存多位错误 (MBE)。
**E2111**

	SBE Log Disable DIMM ##
	系统 BIOS 已禁用内存单位错误 (SBE) 记录，在重新引导系统之前，不会再记录更多的 SBE。“##”表示 BIOS 指示的 DIMM。
**E2112**

	Mem Spare DIMM ##
	系统 BIOS 确定内存中有太多错误，因此已将内存释放。“## & ##”表示 BIOS 指示的 DIMM 对。
**E2113**

	Mem Mirror DIMM ## & ##
	系统 BIOS 确定一半镜像中有太多错误，因此已将内存禁用。“## & ##”表示 BIOS 指示的 DIMM 对。
**E2118**

	Fatal NB Mem CRC
	北侧的全缓冲 DIMM (FBD) 内存子系统链接中的一个连接失败。
**E2119**

	Fatal SB Mem CRC
	南侧的 FBD 内存子系统链接中的一个连接失败。
**I1910**

	Intrusion
	主机盖被卸下。
**I1911**

	3 ERRs Chk Log
	LCD 溢出信息。
	LCD 上最多只能按顺序显示三条错误信息。第四条信息显示为标准的溢出信息。
**I1912**

	SEL Full
	系统事件日志中的事件已满，无法再记录更多事件。
**W1228**

	ROMB Batt < 24hr
	预先警告 RAID 电池只剩下不足 24 小时的电量。
