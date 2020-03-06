# Xilinx QDMA Ethernet Platform (QEP) 驱动

## QDMA以太网平台（QDMA Ethernet Platform）

QEP 设计方案对基于QDMA的流平台添加了以太网支持。以太网子系统被添加到shell的静态区域。此平台有三种物理层功能，其中两种功能是设备管理(PF0)与计算加速(PF1),另一个物理功能是网络加速 (PF2)。以太网子系统通过PF2接入主机。
Linux内核驱动与 DPDK 驱动都可以运行在PCI Express root port 主机PC上与QEP终端IP通过PCIE交互。qep-ctl 是一个QDMA 以太网平台配置工具。

### 起步文档

* [QEP 驱动详细文档](https://xilinx.github.io/qep-drivers/)

* [QEP 控制程序用户指南](qep-ctl/readme.txt)
