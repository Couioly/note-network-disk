
## 1 Java介绍
---
**Java可以实现的程序：**

- 企业级应用开发
- 移动应用开发
- Web服务与微服务开发
- 大数据与云计算
- 嵌入式系统与物联网
- 游戏开发
- 桌面GUI应用开发

### 1.1 Java概述

Java是一种**高级计算机语言**，它于1995年5月推出，是一种可以**编写跨平台应用软件**、**完全面向对象的程序设计**语言。Java语言简单易用、安全可靠，再计算机、移动通信、物联网、人工智能等领域中都发挥着重要的作用。

**Java之父：James Gosling**，Java的版本发展如下：

![[Pasted image 20250915211224.png]]

### 1.2 Java的分类

针对不同的开发市场，Java分为3个技术平台，分别是**JavaSE、JavaEE、JavaME**

#### 1.2.1 JavaSE

Java Platform Standard Edition，是为<font color=blue>开发普通桌面和商务应用程序提供的解决方案</font>。JavaSE平台中包括了<font color=blue>Java最核心的类库</font>，如集合、IO、数据库连接以及网络编程等。

#### 1.2.2 JavaEE

Java Platform Enterprise Edition，是为<font color=blue>开发企业级应用程序提供的解决方案</font>。JavaEE平台用于<font color=blue>开发、装配以及部署企业级应用程序</font>，主要包括Servlet、JSP、JavaBean、JDBC、EJB、Web Service等技术。

#### 1.2.3 JavaME

Java Platform Micro Edition，是为<font color=blue>开发电子消费产品和嵌入式设备提供的解决方案</font>。JavaME主要用于<font color=blue>小型数字电子设备上软件程序的开发</font>，提供HTTP等高级Internet协议，提供高效率的无线交流。

### 1.3 Java的特性

**简单**：Java语言是一种<font color=blue>相对简单</font>的编程语言。能够通过最基本的方法完成指定的任务。

**面向对象**：Java语言是一个纯粹的<font color=blue>面向对象程序设计语言</font>。具备<font color=blue>封装、继承、多态</font>的特性，支持类之间的单继承和接口之间的多继承。

**安全性**：Java语言<font color=blue>安全可靠</font>。Java<font color=blue>没有指针</font>，外界<font color=blue>不能通过伪造指针操作存储器</font>。Java编译器在编译程序时，不显示存储安排决策。

**跨平台性**：Java通过<font color=blue>JVM</font>（虚拟机）以及字节码实现<font color=blue>跨平台</font>。Java程序只要“<font color=blue>一次编写，就可到处运行</font>”。

**支持多线程**：Java语言<font color=blue>支持多线程</font>。程序中<font color=blue>多个任务可以并发执行</font>，多线程可以在很大程度上<font color=blue>提高程序的执行效率</font>。

**分布性**：Java是<font color=blue>分布式</font>语言。<font color=blue>支持各种层次的网络连接</font>。可以通过<font color=blue>Socket类</font>支持可靠的流（stream）进行网络连接。

## 2 Java知识储备
---
### 2.1 Java的运行机制

Java程序的执行是**JVM负责执行Java的字节码文件**（即 **.class** 文件）。要得到字节码文件，首先需要编写Java的源文件，再利用Java编译器将源文件编译成字节码文件。

![[Pasted image 20250915215249.png]]

Java程序的**跨平台特性**，有效的解决了程序设计语言在不同操作系统编译时产生不同机器代码的问题，大大**降低了程序开发和维护的成本**。

### 2.2 JDK、JRE、JVM的关系

![[Pasted image 20250915220207.png]]

![[20200709130053990.png]]

## 3 实例 输出Hello World
---
>[!bug] **Java程序结构：**
>- **工程/项目（project）**
>	- **包（package）**
>		- **类（class）**
>			- **方法（method）**

>[!info] 大体流程
><font color=red>编辑 --> 编译 --> 加载 --> 验证 --> 运行</font>
>
>1）编写 HelloWorld.java 文件
>2）使用 javac HelloWorld.java 对该文件进行编译，编译结束后将自动生成一个名为 HelloWorld.class 的字节码文件
>3）使用 java HelloWorld 命令启动Java虚拟机运行程序，Java虚拟机的类加载器（ClassLoader）会将编译好的字节码文件加载到内存，这个过程被称为类加载（类加载器会从指定的路径中查找对应的类文件，如类路径classpath所指定的路径）
>4）类加载到内存后，JVM会对字节码进行验证，确保字节码的正确性和安全性，验证包括格式验证、语义验证等多个方面，该过程为验证字节码
>5）经过验证的字节码会由JVM中的解释器或即时编译器（JIT）将其转换为特定平台的机器码，然后进行执行，输出运行结果

### 3.1 步骤1 编写程序代码

在任意目录下新建一个文本文档，将其重命名为`HelloWorld.java`。再使用记事本将该文件打开，输入Java代码，具体内容如下：

```java
class HelloWorld{
	public static void main(String[] args){
		System.out.println("HelloWorld!");
	}
}
```

>[!warning] 注意
>在编写程序时，程序中出现的**空格**、**括号**、**分号**等符号必须采用**英文半角格式**，否则将会**出错**。

>[!error] 重点
>**Java类名与文件名匹配规则：**
>1. Java源文件中，若**包含public修饰的类**，则该类的类名必须与文件名<font color=red>完全一致</font>（包括大小写）；
>2. 若文件中没有public类，文件名可以与任意一个类名相同，或自定义其他合法名称（建议与主类名相同，便于识别）；
>3. 违反规则会导致编译器报错，无法正常编译。

### 3.2 步骤2 编译程序

Java中提供了javac命令编译Java的源文件，使用javac命令进行编译的语法格式：

```shell
javac [options] <source files>
```

- `option`：可选参数，用于指定各种参数和设置
- `source files`：需要编译的Java源代码文件路径，多个文件使用空格分隔

>[!warning] 注意
>使用javac命令编译源代码文件时，需要输入完整的文件名称（含后缀.java）

对该实例进行编译：

1）打开终端进入到当前目录，右击选择终端；

![[Pasted image 20250916093914.png]]

2）输入javac命令对`HelloWorld.java`进行编译，命令如下：

```shell
javac HelloWorld.java
```

执行编译命令后将生成一个`.class`文件，结果如下图所示：

![[Pasted image 20250916094109.png]]

### 3.3 步骤3 运行程序

Java中提供了Java命令用于执行字节码文件，使用Java命令执行字节码文件的语法：

```shell
java [options] <classname> [args]
```

- `option`：可选参数，用于指定各种参数和设置
- `classname`：要执行的Java类的名称，该类应该包含`main()`方法为程序入口
- `args`：可选参数，作为`main()`方法的参数传入程序中

运行该实例：

在终端通过`java`命令执行HelloWorld程序，执行指令如下：

```shell
java HelloWorld
```

执行结果如下图所示：

![[Pasted image 20250916200635.png]]

>[!warning] 注意
>执行 java 语句时<font color=red>不需要加任何后缀</font>

## 4 使用Intellij IDEA实现实例

**实例任务：输出“贝壳餐厅点餐助手”**

### 4.1 步骤1 新建项目

1）启动IDEA后，单击`New Progect`选项进入到New Progect界面；
2）在该界面设置项目名称、项目存储位置、JDK的使用版本(左栏中选择Java)；

![[Pasted image 20250917180041.png]]

- `Name`：项目名称，用于唯一标识一个项目，这里设置为JavaSEProgect；
- `Location`：位置，用于指定项目的存储位置或文件的保存路径，这里设置为"G:/Java"；
- `Bulid system`：构建系统，用于管理项目构建和依赖管理的工具，这里使用IDEA自带的构建系统；
- `JDK`：选择当前项目基于的JDK，这里选择已安装的JDK11为例。

3）单击`Create`按钮完成项目的创建，进入项目结构界面。

![[Pasted image 20250917180212.png]]

- `.idea`目录：所有文件以及JavaSEProgect.iml文件都是IDEA开发工具使用的配置文件；
- `src`目录：用于保存程序的源文件；
- `External Libraries`：扩展类库，即Java程序编写和运行所依赖的类。

### 4.2 步骤2 创建class类

>[!warning] 注意
>一般在项目中通常需要用到包（package），然后再包里面再新建类（class），包名的命名约定：
>- 通常以目前所在公司单位的域名为依据，逆向命名，例如域名为`www.baidu.com`则包名为`com.baidu.自定义包名`等。

1）在项目结构界面，右击JavaSEProgect项目下的src目录，在弹出的菜单中依次选择 `New` --> `Java Class`，进入New Java Class选项界面；

![[Pasted image 20250917182730.png]]

2）在New Java Class选项界面，选择Class选项创建一个Java类，并在文本框中输入类名 Welcome，然后按`Enter`键完成Java类的创建。

![[Pasted image 20250917183002.png]]

3）Java类创建完成之后，src目录下面会生成一个`Welcome.java`文件，该文件会自动在右侧区域打开。

![[Pasted image 20250917183211.png]]

### 4.3 步骤3 编写Java程序代码

Welcome类创建完成之后，在类中创建main()方法，在方法内部编写餐厅助手欢迎语句，代码如下：

```java
public class Welcome {  
    public static void main(String[] args) {  
        System.out.println("欢迎来到贝壳餐厅点餐助手");  
    }  
}
```

>[!tips] 快捷方式
>- `main`：快速创建main方法框架语句
>- `sout`：快速创建输出语句`System.out.println()`

### 4.4 步骤4 运行程序

在IDEA的**集成开发环境中有内置的编译器和构建工具**，能够自动将源代码编译字节码文件，因此**无需手动进行编译**。编写好Java程序后，在文本编辑器视图中，单击第一行或第二行前面的<font color=red>绿三角形按钮</font>，或者在文本编辑区域右击选择`Run 'Welcome.main()'`，都可以运行Welcome程序，如图所示：

![[Pasted image 20250917184222.png]]

运行结果如下：

```
欢迎来到贝壳餐厅点餐助手
```

## 5 Intellij IDEA调试工具
---
### 5.1 步骤1 设置断点

在IDEA中，**断点**是开发人员为了**调试程序**而设置的一个**特殊标记点**，用于**暂停程序的执行**。通过**设置断点**，开发人员可以在**程序运行时**指定程序在**哪个位置暂停**，以便**观察程序的执行状态，检查变量的值和执行其他调试操作**。在IDEA中可以通过**单击需要设置断点的代码行左侧**来设置断点，设置成功的断点通常以一个**红色圆点**进行展示。

在`Welcome.java`文件的第3行代码左侧设置断点，如下图所示：

![[Pasted image 20250917185402.png]]

### 5.2 步骤2 Debug进行调试

单击IDEA菜单栏中右上方工具栏中的Debug按钮（绿色小虫子），以Debug模式启动程序，如下图所示：

![[Pasted image 20250917185913.png]]

**调试按钮的作用及快捷键**

![[Pasted image 20250917190254.png]]

由于welcome程序的方法中只有一行代码，所以按下`F9`则可以继续执行，如下图所示：

![[Pasted image 20250917190414.png]]

