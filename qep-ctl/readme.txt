---------
qep-ctl
---------
QDMA Ethernet Platform (QEP) 配置工具包. 

编译:
-------
	gcc qep_ctl.c -o qep-ctl

支持的命令:
------------------
qep-ctl 支持以下命令.

	1. help : 此命令展示简短的用法.输入如下.
		./qep-ctl CMD [ options ]
		  CMD = [ config | show | read | write | help ]
		  Options =  -d BDF
					 -b bar_num
					 -c [ tx | rx ]
					 -m mac_addr
					 -v VID,DEI,PCP
					 -e [ mac,vlan,discard ] [ disable ]
	2. config : 此命令配置以太网 MAC 地址与 802.1Q VLAN 域. 
		-m: 此选项用于更新硬件的以太网 MAC 地址.
			MAC 地址在 qep 驱动 probe 的时候用到. 
			示例： ./qep-ctl config -m 01:02:03:03:04:06

		-v: VLAN 标识符(VID), Drop eligible indicator(DEI)and Priority code
			point(PCP) are provided with (-v) option. The values are comma 
			separated in order of VID, DEI, PCP respectively. DEI and PCP are 
			optional. 
			e.g. ./qep-ctl config -v 234,0,3
		
		-e: CLI supports upto four inputs with (-e) option. 
			->'mac': This enables MAC update in tx direction. The source MAC 
				address ofall outgoing packets will be updated with programmed 
				MAC address.It also enables MAC filtering in rx direction. 
				Incoming packets whose destination MAC address matches the  
				programmed MAC will only be accepted.
			->'vlan': This enables VLAN tag insertion in tx direction. For all 
				outgoing packets, a standard 802.1q VLAN TAG is inserted with 
				programmed values. It also enables VLAN ID based packet filtering
				in rx direction. The device only accepts an incoming packet if 
				it contains a valid VLAN tag and VLAN ID matches with programed 
				VLAN ID.
			->'discard': This input drops all outgoing packets for tx and all 
				incoming packets for rx.
			->'disable': This input disables the security block and clears all
				enable flags.
		Comma separated list of values can be provided for enabling multiple 
		features and direction can sleceted using general option.
		e.g.  ./qep-ctl config -e mac,vlan -c tx
		
	3. show: This command displays the current configuration of the security 
		block.
		e.g.  ./qep-ctl show
	
	4 read: This command is used to read a register directly. The offset must be
		the first parameter to read command. By default, it uses first found 
		device and user BAR i.e bar number 2. To read from different device 
		and BAR use general options.
		e.g.  ./qep-ctl read 0x0010000
	
	5. write: This command is used to write to a register directly. It 
		takes offset and value as next two parameters. By default, it uses first
		found device and user BAR i.e bar number 2. To write to different device
		and BAR use general options.
		e.g.  ./qep-ctl write 0x0010000 0xef
	
常规选项:
----------------
qep-ctl 提供以下常规选项. 

 -d: 此选项用于选择设备，有效BDF必须此规范。如果提供 (-d) 选项， CLI 就会查询你 Xilinx PCI device 
	(0x7000) 的BDF. 如果设备有不同的 PCI 设备 ID，那么源码中的 PCIE_DEV_ID_MGMT_PF 宏必须更新
	才能检测到设备. 防止特殊情况，含有多个设备的系统，每条命令都必须提供 BDF. 

 -b: 此选项用于指定 direct register 读写用的 PCIe BAR 数量. 默认情况下，每个用户 BAR 的 BAR 数量为2. 
        默认用户 BAR 可以通过源码中的 DEFAULT_USER_BAR 宏进行更新.

 -c:此选项用于指定传输方向. 可选参数为 'rx' 或 'tx' . 如果选项没有指定默认双向.
