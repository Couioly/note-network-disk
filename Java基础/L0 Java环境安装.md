
# JDK下载与安装（开发工具）

>[!warning] **安装前注意事项**
>JDK 安装程序无法安装同一功能版本的多个版本。如果尝试在已安装的情况下进行安装，安装程序将卸载并重新安装，假如安装的是比当前已安装版本更老的版本，它将提示你将其卸载后再进行安装。

## JDK下载
---
#### 步骤1 打开官网

点击下列链接访问官方网址，打开官方网站！
**官方下载网址：[Java Downloads | Oracle](https://www.oracle.com/java/technologies/downloads/#jdk24-windows)**

![[Pasted image 20250815220741.png]]

#### 步骤2 选择版本

在打开的页面可以直观的看到 `JDK 21 is the latest Long-Term Support (LTS) release of the Java SE Platform.`，这句话的意思是 JDK是JavaSE平台的最新长期支持(LTS)版本，因此推荐安装长期支持版（因为LTS具有稳定性和可靠性）

![[Pasted image 20250815221041.png]]

#### 步骤3 下载安装包

接下来选择自己的操作系统，然后根据自己的处理器选择链接，这里我的是x64处理器，因此可以直接点击下载

![[Pasted image 20250815222236.png]]

**Windows操作系统下，JDK提供了三个JDK安装文件的下载：**

- **x64 Compressed Archive**：免安装版本，是一个压缩文件，下载后解压即可使用。
- **x64 Installer**：离线安装包版本，是一个可执行文件，包含一个图形用户界面的安装向导程序。
- **x64 MSI Installer**：也是离线安装包版本，通过MSI文件进行安装，提供了更丰富的安装选项。

**【区分处理器32位/64位的方法】** 键盘点击 `Win + R` 调出运行框，然后输入 `cmd` 回车打开dos命令窗口，在该窗口键入下列指令然后回车：

```shell
systeminfo
```

![[Pasted image 20250815223843.png]]

>[!warning] 注意
>JDK 11开始，Oracle就不再为32位Windows、Linux等操作系统提供官方的JDK二进制安装包，若位32位的用户可以考虑使用JDK 8等旧版本。

若需要其他版本的JDK可以前往JDK存档页面，<font color=blue>访问链接：</font> [Java Archive | Oracle](https://www.oracle.com/java/technologies/downloads/archive/)

![[Pasted image 20250815225034.png]]

## JDK安装
---

#### 步骤1 运行安装向导

打开下载文件夹，找到下载好的JDK安装向导，双击运行

![[Pasted image 20250815225450.png]]

#### 步骤2 安装向导

无脑式点击 `下一步` 即可完成安装！

![[Pasted image 20250815230309.png]]

## 疑惑解答
---
>**Q：为什么没有JRE相关的安装呢？**

A：在 JDK 8 及之前的版本中，JRE 是独立于 JDK 的组件：
- JDK（开发工具包）包含编译器（`javac`）、调试工具等开发工具，以及一个内置的 JRE；
- 用户若仅需运行 Java 程序，可单独安装 JRE（体积更小）。
但从**JDK 9 开始**，Java 引入了**模块系统（Module System）**，彻底改变了 JRE 的存在形式：
- JDK 不再包含单独的`jre`目录，而是通过模块（如`java.base`、`java.desktop`等）动态提供运行时所需的类和资源；
- JDK 本身集成了运行 Java 程序的全部必要组件（通过`java`命令即可直接运行.class 或.jar 文件），无需单独安装 JRE。
JDK 21 继承了这一设计，因此用户无需额外处理 JRE 的安装、配置或路径设置 ——**JDK 21 本身就是 “开发 + 运行” 的一体化工具包**。

>**Q：为什么没有环境变量相关的设置呢？**

A：手动配置环境变量（如`PATH`）的核心目的是让系统能找到 JDK 的`bin`目录（包含`java`、`javac`等命令）。JDK 21 的安装程序通过以下改进实现了自动配置：
- **`PATH`变量的自动添加**：在 Windows、MacOS 等系统中，JDK 21 的安装程序（如`.msi`、`.pkg`）默认包含 “添加到系统`PATH`” 的选项（通常默认勾选）。安装完成后，`bin`目录（如`C:\Program Files\Java\jdk-21\bin`）会被自动写入系统`PATH`，用户无需手动编辑环境变量，即可在命令行直接使用`java -version`、`javac`等命令。
- **`JAVA_HOME`的弱化与自动识别**：部分工具（如 IDE、构建工具 Maven/Gradle）依赖`JAVA_HOME`环境变量，但 JDK 21 的安装程序在部分系统中会自动设置`JAVA_HOME`（如 Windows 通过注册表记录安装路径，工具可自动读取）。即使未显式设置，多数场景下仅需`PATH`中的`bin`目录即可满足基础使用需求。

## 环境配置
---
**若觉得自动配置的环境不靠谱，那我们可以自己手动再配置一次！**

#### 步骤1 打开环境变量

再次按下 `Win + R` 键，打开“运行”对话框，输入以下命令：

```
SystemPropertiesAdvanced
```

#### 步骤2 配置JAVA_HOME

回车后将显示系统属性窗口，在窗口中点击 `环境变量` ，然后在系统变量中选择 `新建` ，输入下列变量名和变量值，然后点击 `确定`，如下所示：

```
变量名：JAVA_HOME
变量值：D:\ProgramEnviron\Java\jdk-11.0.2
```

![[Pasted image 20250915115807.png]]

#### 步骤3 配置PATH变量

系统变量中找到 `PATH` 双击打开(也可以选择点击编辑)，打开后选择新建，输入下列变量，然后依次点击三个确定进行保存修改。

```
%JAVA_HOME%\bin
```

![[Pasted image 20250816093024.png]]

#### 步骤4 验证配置

全部完成后点击 `Win + R` 键打开运行框，输入 `cmd` 回车打开终端，依次输入下列测试命令若能够正常运行则表示配置成功！

```shell
java
javac
java -version
```

![[Pasted image 20250816002324.png]]

# Intellij IDEA下载与安装（集成开发环境）

## Intellij IDEA下载
---
#### 步骤1 打开官网

点击下列链接访问官方网址，打开官方网站！
**官方下载网址：[Download | IntelliJ IDEA](https://www.jetbrains.com/idea/download)**

![[Pasted image 20250915121313.png]]

#### 步骤2 选择版本

根据需求进行版本的选择；

![[Pasted image 20250915121239.png]]

>[!bug] 注意
>自2025.3版本起都将专业版和社区版合并为统一版了！！！

#### 步骤3 下载安装包

点击`Download`进行安装包下载

![[Pasted image 20250915135301.png]]

## Intellij IDEA安装
---
#### 步骤1 允许安装向导

执行刚才下载好的程序，进行安装；

![[Pasted image 20250915135707.png]]

#### 步骤2 安装向导

![[Pasted image 20250915140305.png]]

#### 步骤3 验证安装

![[Pasted image 20250915141032.png]]


---
**JDK21官方使用文档：[Overview (Java SE 21 & JDK 21)](https://docs.oracle.com/en/java/javase/21/docs/api/index.html)**

