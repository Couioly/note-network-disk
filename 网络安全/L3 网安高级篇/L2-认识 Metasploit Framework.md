
>[!info] 学习目标
> 1. 认知| 说出 Exploit / Payload / Listener 四大积木是干嘛的
> 2. 技能| 能在 msfconsole 里独立完成「搜索-选用-配置-开枪」闭环
> 3. 思想| 牢记「**先授权、再测试、后报告**」

## MSF 概述
---

> **MSF = 黑客界的乐高工厂**  
> 把“漏洞”当积木，原材料是漏洞，生产线是模块，成品是 Meterpreter Shell。

## MSF 核心组件
---

|           组件类型           |              说明              |                                                                    示例                                                                     |                       常见文件/目录                        |
| :----------------------: | :--------------------------: | :---------------------------------------------------------------------------------------------------------------------------------------: | :--------------------------------------------------: |
| **Exploits（漏洞利用模块）——子弹** |      针对特定软件或系统漏洞编写的攻击代码      |                                              针对 Windows 系统 SMB 服务漏洞的 ms17_010_eternalblue 模块                                              |                 /usr/bin/msfconsole                  |
| **Payloads （载荷模块）——弹头**  | 漏洞触发后真正执行的代码（反弹 Shell、执行命令等） | 1. `Single Payload`：功能独立完整  2. `Stager Payload`：建立初始网络连接，如 reverse_tcp、bind_tcp        3. `Stage Payload`：功能强大无大小限制，如 Meterpreter Payload | /usr/share/metasploit-framework/modules/exploits/... |
| **Auxiliary（辅助模块）——侦察兵** |       扫描、爆破、信息收集，不触发漏洞       |                                       信息收集（auxiliary/scanner/portscan/tcp）、服务版本探测（smb_version 模块）等                                        | /usr/share/metasploit-framework/modules/payloads/... |
|  **Post（后渗透模块）——打扫战场**   |    拿到 Shell 后做提权、抓密码、横向移动    |                                                   权限提升（getsystem 模块）、数据收集（hashdump 模块）等                                                   |                /modules/auxiliary/...                |
|  **Encoders（编码器）——吉利服**  |    对 Payload 编码，绕过安全软件检测     |                                                            shikata_ga_nai 编码器                                                             |                  /modules/encoders                   |

## MSF 常用命令
---

|阶段|命令|作用|
|---|---|---|
|搜索|`search 关键字`|找模块|
|选用|`use 路径`|选中模块|
|看参数|`show options`|查缺什么|
|设置|`set 参数 值`|填坑|
|开火|`exploit` 或 `run`|执行|
|会话|`sessions -l` / `-i 1`|管理已拿到的 Shell|

## MSF 常用工具
---
#### `masscan` 端口扫描工具

```shell
masscan <目标> -p<端口> --rate<包/秒>
```

**使用示例：**

```shell
#指定扫描的端口范围
masscan 192.168.205.143 -p1-65535 --rate=10000
#指定扫描固定端口，多个端口用','分隔
masscan 192.168.0.11 -p80,443,22,3306, --rate=10000
```

|参数|作用|示例|
|---|---|---|
|`-p80,443,8000-9000`|端口列表/范围|`-p1-65535`|
|`--rate 10000`|每秒发包数|内网 1w，外网 1000|
|`--banners`|抓取 Banner|Web 服务识别|
|`-oG file.gnmap`|grepable 输出|后续管道处理|
|`--excludefile exclude.txt`|排除地址|保护关键资产|

#### nmap 端口探测工具

Nmap（Network Mapper）是一款功能强大的网络扫描工具

```shell
nmap [扫描类型] [选项] <目标>
```

**使用示例：**

```shell
nmap -sV -O -A 192.168.0.11 -p80,443
```

|参数|意义|场景|
|---|---|---|
|`-sS`|SYN 半开放|快且隐蔽|
|`-sV`|服务版本识别|找漏洞前要|
|`-O`|OS 指纹|资产画像|
|`-A`|全面扫描|一键打包|
|`-p-`|全端口|彻底摸家底|
|`-oX file.xml`|XML 输出|给报告工具|

#### nmap相关参数整合

**主机发现（Host Discovery）**

- `-sL`：仅进行 DNS 解析，不发送任何数据包到目标
- `-sn`：不进行端口扫描，仅进行主机发现（替代旧版`-sP`）
- `-Pn`：跳过主机发现，直接视为所有目标主机都在线
- `-PE/PP/PM`：使用 ICMP 回声 / 时间戳 / 网络掩码请求探测主机
- `-PS[端口]`：发送 TCP SYN 包到指定端口（默认 80,443 等）
- `-PA[端口]`：发送 TCP ACK 包到指定端口
- `-PU[端口]`：发送 UDP 包到指定端口
- `-PR`：使用 ARP 请求进行局域网扫描
- `-n/-R`：`-n`不进行 DNS 解析，`-R`始终进行 DNS 解析

**端口扫描（Port Scanning）**

- `-sS`：TCP SYN 扫描（半开放扫描，速度快且隐蔽）
- `-sT`：TCP 连接扫描（全连接扫描，易被检测）
- `-sU`：UDP 扫描（用于探测 UDP 端口）
- `-sN/sF/sX`：TCP 空扫描 / FIN 扫描 / Xmas 扫描（隐蔽性强）
- `-sA`：TCP ACK 扫描（用于探测防火墙规则）
- `-sW`：TCP 窗口扫描（通过窗口大小判断端口状态）
- `-sM`：TCP Maimon 扫描（类似 FIN 扫描）
- `-sY/sZ`：SCTP INIT/COOKIE-ECHO 扫描
- `-sO`：IP 协议扫描（探测目标支持的 IP 协议）
- `-p <端口范围>`：指定扫描的端口（如`-p 1-100`或`-p 80,443`）
- `-F`：快速扫描（扫描较少的端口）
- `-r`：不随机扫描端口，按顺序扫描

**服务与版本探测（Service/Version Detection）**

- `-sV`：探测端口上运行的服务及版本信息
- `--version-intensity <强度>`：设置版本探测强度（0-9，默认 7）
- `--version-light`：使用轻量级版本探测（强度 2）
- `--version-all`：尝试所有探测探针（强度 9）
- `--version-trace`：显示版本探测的详细过程

**操作系统探测（OS Detection）**

- `-O`：启用操作系统探测
- `--osscan-limit`：仅对确定在线的主机进行 OS 探测
- `--osscan-guess`：尝试猜测操作系统类型

**时间与性能控制（Timing and Performance）**

- `-T<0-5>`：设置扫描 timing 模板（0 最慢，5 最快）
    - `-T0`：偏执模式（避开 IDS）
    - `-T1`：沉默模式（缓慢扫描）
    - `-T2`：礼貌模式（减少网络负载）
    - `-T3`：默认模式
    - `-T4`：激进模式（快速扫描）
    - `-T5`：疯狂模式（极快，可能影响准确性）
- `-v`：增加详细程度（`-vv`更详细）
- `-d`：增加调试信息（`-dd`更详细）
- `--host-timeout <时间>`：设置主机扫描超时时间（如`--host-timeout 5m`）
- `--scan-delay/--max-scan-delay`：设置端口扫描的延迟时间

**防火墙 / IDS 规避（Firewall/IDS Evasion）**

- `-f`：使用分片 IP 数据包（避开简单防火墙）
- `-mtu <大小>`：指定 IP 分片大小（必须是 8 的倍数）
- `-D < decoys >`：使用诱饵扫描（掩盖真实 IP）
- `-S <源IP>`：伪造源 IP 地址
- `-e <接口>`：指定使用的网络接口
- `-g/--source-port <端口>`：使用指定的源端口
- `--data-length <长度>`：在数据包中添加随机数据
- `--ttl <值>`：设置 IP 的 TTL（生存时间）值
- `--spoof-mac <MAC地址/厂商>`：伪造 MAC 地址

**输出选项（Output Options）**

- `-oN <文件>`：正常输出到指定文件
- `-oX <文件>`：XML 格式输出到指定文件
- `-oS <文件>`：脚本 kiddie 格式输出
- `-oG <文件>`：Grepable 格式输出
- `-oA <前缀>`：同时输出三种格式（Normal/XML/Grepable）
- `-v`：显示详细输出
- `--stats-every <时间>`：定期显示扫描状态

**脚本扫描（Script Scanning）**

- `-sC`：使用默认脚本集合（等同于`--script=default`）
- `--script <脚本名/类别>`：指定要运行的脚本（如`--script vuln`检测漏洞）
- `--script-args <参数>`：为脚本提供参数
- `--script-args-file <文件>`：从文件读取脚本参数
- `--script-trace`：显示脚本执行的详细过程

**其他常用参数**

- `-6`：启用 IPv6 扫描
- `-iL <文件>`：从文件读取目标列表
- `-iR <数量>`：随机选择指定数量的目标进行扫描
- `--exclude <目标>`：排除指定的目标
- `--excludefile <文件>`：从文件读取要排除的目标
- `-V`：显示 Nmap 版本信息
- `-h`：显示帮助信息

#### 工具对比

|  特征  | Masscan 🚀 |     Nmap 🔍     |
| :--: | :--------: | :-------------: |
| 设计定位 |   高速端口发现   |     全能信息收集      |
| 扫描速度 |   百万包/秒    |      千包/秒       |
| 默认模式 |  半开放 SYN   |  半开放 SYN（-sS）   |
| 结果细节 | 端口+Banner  |   端口+服务+OS+脚本   |
| 最佳场景 |  全网段快速找活口  |     小范围深度画像     |
| 输出格式 | -oG / ‑oX  | -oX / ‑oN / ‑oG |

## 永恒之蓝
---
#### 漏洞名片

| 项目       | 内容                                               |
| :------- | :----------------------------------------------- |
| 官方编号     | MS17-010（微软 2017-03-14 发布）                       |
| CVE 编号   | CVE-2017-0144                                    |
| 中文名      | 永恒之蓝（EternalBlue）                                |
| 影响协议     | **SMBv1**（Server Message Block v1）               |
| **关键端口** | **TCP 445**（主要）  <br>TCP 139（老系统兼容）              |
| 影响系统     | Windows XP / 7 / 8 / 10、Server 2003-2016 等未打补丁版本 |
| 危害等级     | 严重——远程代码执行 + 无需身份验证                              |

#### 漏洞原理

```shell
攻击机 ───特制SMB包──→ 445端口  
                         ↓  
   目标机 SMBv1 缓冲区溢出  
                         ↓  
   内核内存被覆盖 → 注入 Shellcode  
                         ↓  
   SYSTEM 权限 Meterpreter 到手
```

> [!tips] 一句话
> 向 445 端口丢一个超长 SMB 包，Windows 就“蓝”给你看

## 实战

#### 主机存活探测

```shell
nmap -sP 192.168.0.0/24
```

>[!question] '/24'是什么
>在使用nmap进行主机存活探测时，在IP地址后添加'/24'，这涉及到子网掩码和CIDR表示法（无类别域间路由），具体如下：
>CIDR是一种用于给IP地址块分配和表示的方法。'/24'是CIDR表示法的一部分，其中的数字'24'代表子网掩码中'1'的位数，常见的'255.255.255.0'转换为二进制是'11111111 11111111 11111111 00000000'，其中包含24个'1'，因此使用'/24'表示。
>>[!tips] 常见的CIDR表示
>>- '/16'：子网掩码为'255.255.0.0'，表示一个包含65534个可用主机的较大网段，nmap扫描范围是：'192.168.0.1' -- '192.168.255.254'
>>- '/8'：子网掩码为'255.0.0.0'，表示一个非常大的网段，包含约16777214个可用主机地址

![image.png](https://note.youdao.com/yws/public/resource/32ab6a45f53f97e2c9ae5b3bda1713db/WEBRESOURCEa47726125cecd2b2fd923692bc3ad82d?ynotemdtimestamp=1757325709790)

#### 端口存活探测

```shell
masscan -p1-65535 192.168.0.11 --rate=1000
```

![image.png](https://note.youdao.com/yws/public/resource/32ab6a45f53f97e2c9ae5b3bda1713db/WEBRESOURCE58c54a30207959df1a44ca3b23ee3eed?ynotemdtimestamp=1757325709790)

#### 端口服务探测

```shell
nmap -sV -p 135,5357,445,139 -A 192.168.0.11
```

![image.png](https://note.youdao.com/yws/public/resource/32ab6a45f53f97e2c9ae5b3bda1713db/WEBRESOURCE2292e28137297e095da1511bcb53bd35?ynotemdtimestamp=1757325709790)

#### 漏洞检测

- 输入 `msfconsole`，进入MSF

![image.png](https://note.youdao.com/yws/public/resource/32ab6a45f53f97e2c9ae5b3bda1713db/WEBRESOURCEd183f54b2b3170e435990a87280d0b7c?ynotemdtimestamp=1757325709790)

- `search ms17-010` 搜索关键字查询漏洞检测模块

![image.png](https://note.youdao.com/yws/public/resource/32ab6a45f53f97e2c9ae5b3bda1713db/WEBRESOURCEbea0f53b8de3cb5c7165adf252db1e6f?ynotemdtimestamp=1757325709790)

- `use 24` 使用这个检测模块

![image.png](https://note.youdao.com/yws/public/resource/32ab6a45f53f97e2c9ae5b3bda1713db/WEBRESOURCE01bc866c7694ed17146c22896ddadc9f?ynotemdtimestamp=1757325709790)

- `show options` 查看需要设置哪些选项

![image.png](https://note.youdao.com/yws/public/resource/32ab6a45f53f97e2c9ae5b3bda1713db/WEBRESOURCE00565fbb7442094152dc522580828baa?ynotemdtimestamp=1757325709790)

- 看到只剩 `RHOSTS` 需要设置了

![image.png](https://note.youdao.com/yws/public/resource/32ab6a45f53f97e2c9ae5b3bda1713db/WEBRESOURCE6824f8e6755013322a045a3f9939b155?ynotemdtimestamp=1757325709790)

- `RHOSTS` 设置为目标地址

![image.png](https://note.youdao.com/yws/public/resource/32ab6a45f53f97e2c9ae5b3bda1713db/WEBRESOURCE88c9a7f31d815f2a4aaf9b3ceaeb321e?ynotemdtimestamp=1757325709790)

- 最后我们直接 `run` 或者 `exploit` 都可以

```shell
Host is likely VULNERABLE to MS17-010!主机可能容易受到 MS17-010 的攻击！
```

![image.png](https://note.youdao.com/yws/public/resource/32ab6a45f53f97e2c9ae5b3bda1713db/WEBRESOURCE9630c86785ffcf79bf6e031754454a1f?ynotemdtimestamp=1757325709790)

![image.png](https://note.youdao.com/yws/public/resource/32ab6a45f53f97e2c9ae5b3bda1713db/WEBRESOURCEb4a49bd6c13151cb4962974c99931264?ynotemdtimestamp=1757325709790)

#### 漏洞利用

- `search ms17-010` 搜索关键字查询漏洞利用模块

![image.png](https://note.youdao.com/yws/public/resource/32ab6a45f53f97e2c9ae5b3bda1713db/WEBRESOURCE1326f3361c7aa00c0567a8e257c60dff?ynotemdtimestamp=1757325709790)

- `use 0` 使用当前这个漏洞利用模块

![image.png](https://note.youdao.com/yws/public/resource/32ab6a45f53f97e2c9ae5b3bda1713db/WEBRESOURCEdaeac41bb8d1d25d3fd6db9c4e01605d?ynotemdtimestamp=1757325709790)

- `show options` 查看需要设置哪些选项

![image.png](https://note.youdao.com/yws/public/resource/32ab6a45f53f97e2c9ae5b3bda1713db/WEBRESOURCE28dfeef497d8b54bbfd01820a1ba4fcd?ynotemdtimestamp=1757325709790)

- 看到只剩 `RHOSTS` 需要设置了，`RHOSTS` 设置为目标地址

![image.png](https://note.youdao.com/yws/public/resource/32ab6a45f53f97e2c9ae5b3bda1713db/WEBRESOURCEdc8152b501a2204bd4fd197a2881bb29?ynotemdtimestamp=1757325709790)

- 最后我们直接 `run` 或者 `exploit` 开始攻击

![image.png](https://note.youdao.com/yws/public/resource/32ab6a45f53f97e2c9ae5b3bda1713db/WEBRESOURCE0896d7e66c94e9a2530a8ea06058e27f?ynotemdtimestamp=1757325709790)

## 比扫描更快的是罚单

> [!warning] 注意
> **再高的扫描速率也赶不上《网络安全法》里罚款数字的爬升速度——你扫的是端口，它扫的是你的账户余额；**
> 
> **你按毫秒提速，它按百万加码；当0day还在缓冲区里排队，罚款单已经光速签收了。**

