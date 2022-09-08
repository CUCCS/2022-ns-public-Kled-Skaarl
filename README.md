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
  
  - [x] 靶机可以直接访问攻击者主机
  - [x] 攻击者主机无法直接访问靶机
  - [x] 网关可以直接访问攻击者主机和靶机
  - [x] 靶机的所有对外上下行流量必须经过网关
  - [x] 所有节点均可以访问互联网

### 实验步骤

- ##### 虚拟硬盘配置成多重加载

打开虚拟机进入虚拟介质管理界面，选中对应的虚拟硬盘改为多重加载，并在释放盘片后重新加载虚拟硬盘即可

<img src="https://s1.328888.xyz/2022/09/07/57E5B.png" title="" alt="" data-align="center">

<img src="https://s1.328888.xyz/2022/09/07/5Q3jC.png" title="" alt="" data-align="center">

- ##### 搭建满足要求的虚拟机拓扑网络

共有六台虚拟机系统，分别为两台XP系统，两台Debian系统以及两台Kali系统，其中一台Debian系统作为网关，一台Kali作为攻击者，其余四台分别分在两个独立局域网中作为靶机.

1.配置网关所需网卡：NAT网络，Host-only网络，两块内部网络

<img src="https://img1.imgtp.com/2022/09/08/2tYZLJXo.png" title="" alt="" data-align="center">

此时网关主机的ip地址如下

![](https://img1.imgtp.com/2022/09/08/n4HJ2kJy.png)

2.配置攻击者kali所需网卡

<img src="https://img1.imgtp.com/2022/09/08/1HFTgQjH.png" title="" alt="" data-align="center">

3.配置靶机

- Victim-XP-1

<img src="https://img1.imgtp.com/2022/09/08/a8twT8Ko.png" title="" alt="" data-align="center">

<img src="https://img1.imgtp.com/2022/09/08/Ua0lDuXU.png" title="" alt="" data-align="center">

- Victim-kali-1

<img src="https://img1.imgtp.com/2022/09/08/l8yti79f.png" title="" alt="" data-align="center">

<img src="https://img1.imgtp.com/2022/09/08/Qk30eFil.png" title="" alt="" data-align="center">

- Victim-XP-2

<img src="https://img1.imgtp.com/2022/09/08/bpKEx282.png" title="" alt="" data-align="center">

<img src="https://img1.imgtp.com/2022/09/08/1YRn85CI.png" title="" alt="" data-align="center">

- Victim-Debian-2

<img src="https://img1.imgtp.com/2022/09/08/wRAmmUiF.png" title="" alt="" data-align="center">

<img src="https://img1.imgtp.com/2022/09/08/AFitwfiu.png" title="" alt="" data-align="center">

IP地址汇总

| 虚拟机             | ip             |
|:---------------:|:--------------:|
| Attacker-kali   | 10.0.2.15      |
| Victim-XP-1     | 172.16.111.112 |
| Victim-kali-1   | 172.16.111.121 |
| Victim-XP-2     | 172.16.222.104 |
| Victim-Debian-2 | 172.16.222.130 |

- ##### 网络连通性测试

1.靶机可以直接访问攻击者主机

<img src="https://img1.imgtp.com/2022/09/08/hoFtSULI.png" title="" alt="" data-align="center">2.攻击者主机无法直接访问靶机

<img src="https://img1.imgtp.com/2022/09/08/U40MUTLh.png" title="" alt="" data-align="center">

3.网关可以直接访问靶机和攻击者主机

<img src="https://img1.imgtp.com/2022/09/08/Qj4GYnCH.png" title="" alt="" data-align="center">

<img src="https://img1.imgtp.com/2022/09/08/YRHSobc2.png" title="" alt="" data-align="center">

4.靶机的所有上过流量必须经过网关

首先需安装linux下抓包工具tcpdump

```bash
sudo apt update && apt install tcpdump
```

之后在靶机上进行网络请求，通过网关进行抓包

抓包命令：

```bash
sudo tcpdump -i [希望抓取的网卡名称] -n -w [数据保存为的文件名称]

#-i表示指定监听的网络接口
#-n表示不把网络地址转换成名字
#-w表示直接将分组写入文件中，而不是不分析并打印出来
```

<img src="https://img1.imgtp.com/2022/09/08/ll4GdbSK.png" title="" alt="" data-align="center">

本地使用wireshark分析抓包文件

<img src="https://img1.imgtp.com/2022/09/08/CP2WInvN.png" title="" alt="" data-align="center">

分析发现靶机流量均经过网关

5.所有节点都可以访问互联网

<img src="https://img1.imgtp.com/2022/09/08/RcgvSM0O.png" title="" alt="" data-align="center">

<img src="https://img1.imgtp.com/2022/09/08/1VYYgFur.png" title="" alt="" data-align="center">

<img src="https://img1.imgtp.com/2022/09/08/Khznfaa6.png" title="" alt="" data-align="center">

### 问题及解决

安装虚拟机后启动报错

<img src="https://img1.imgtp.com/2022/09/08/4yDbjAYK.png" title="" alt="" data-align="center">

原因是虚拟机处理器数量默认为1，过少导致系统无法正常启动

<img src="https://img1.imgtp.com/2022/09/08/rFybc1cf.png" title="" alt="" data-align="center">

将其改为4即可正常启动

<img src="https://img1.imgtp.com/2022/09/08/OhuzqZGK.png" title="" alt="" data-align="center">

### 参考资料

[Linux抓包工具tcpdump](http://www.ha97.com/4550.html)
