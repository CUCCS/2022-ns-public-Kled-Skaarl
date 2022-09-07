# 实验一 基于VirtualBox的网络攻防基础环境搭建

### 实验目的

- 掌握 VirtualBox 虚拟机的安装与使用；
- 掌握 VirtualBox 的虚拟网络类型和按需配置；
- 掌握 VirtualBox 的虚拟硬盘多重加载；

### 实验环境

- VirtualBox 虚拟机
- 攻击者主机（Attacker）：Kali Rolling 2019.2
- 网关（Gateway, GW）：Debian Buster
- 靶机（Victim）：From Sqli to shell / xp-sp3 / Kali

### 实验要求

- 虚拟硬盘配置成多重加载，效果如下图所示；

<img src="https://c4pr1c3.github.io/cuc-ns/chap0x01/attach/chap0x01/media/vb-multi-attach.png" title="" alt="" data-align="center">

- 搭建满足如下拓扑图所示的虚拟机网络拓扑；

<img title="" src="https://c4pr1c3.github.io/cuc-ns/chap0x01/attach/chap0x01/media/vb-exp-layout.png" alt="" data-align="center">

- 完成以下网络连通性测试；
  
  - [ ] 靶机可以直接访问攻击者主机
  - [ ] 攻击者主机无法直接访问靶机
  - [ ] 网关可以直接访问攻击者主机和靶机
  - [ ] 靶机的所有对外上下行流量必须经过网关
  - [ ] 所有节点均可以访问互联网

### 实验步骤

- ##### 虚拟硬盘配置成多重加载

打开虚拟机进入虚拟介质管理界面，选中对应的虚拟硬盘改为多重加载，并在释放盘片后重新加载虚拟硬盘即可

<img src="https://s1.328888.xyz/2022/09/07/57E5B.png" title="" alt="" data-align="center">

<img src="https://s1.328888.xyz/2022/09/07/5Q3jC.png" title="" alt="" data-align="center">

- ##### 搭建满足要求的虚拟机拓扑网络

共有六台虚拟机系统，分别为两台XP系统，两台Debian系统以及两台Kali系统，其中一台Debian系统作为网关，一台Kali作为攻击者，其余四台分别分在两个独立局域网中作为靶机.

1.配置网关所需网卡：NAT网络，Host-only网络，两块内部网络

<img src="https://s1.328888.xyz/2022/09/07/5Q9MI.png" title="" alt="" data-align="center">

2.攻击者所需网卡：
