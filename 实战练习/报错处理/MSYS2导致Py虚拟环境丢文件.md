
### MSYS2安装后导致创建虚拟环境时缺失Scripts文件夹

当前环境：

```
系统Python路径：D:\ProgramEnviron\Python\Python311\python.exe （版本3.13.2）

MSYS2路径：D:\ProgramEnviron\MSYS2\

MSYS2下Python路径：D:\ProgramEnviron\MSYS2\mingw64\bin\python.exe （版本3.12.1）

存在问题的虚拟环境路径：D:\CodeFile\qt_env
```


![[Pasted image 20250721080026.png]]

#### 问题分析

**MSYS2 修改了系统环境变量**：

- 安装 MSYS2 时，它会将自己的路径（如 `D:\ProgramEnviron\MSYS2\mingw64\bin`）添加到系统 `PATH` 环境变量的**最前面**
- 这导致系统优先使用 MSYS2 自带的 Python（MinGW 编译版本），而不是你原本安装的 Python

**MSYS2 的 Python 与标准 Python 不兼容**：

- MSYS2 的 Python 是为 MinGW 环境编译的
- 它无法正常创建 Windows 原生的虚拟环境（缺少 `Scripts` 目录）
- 你看到的模块加载跟踪（`import _frozen_importlib` 等）是 MSYS2 Python 的启动过程

#### 解决方案

推荐在创建新环境前将存在问题的环境清理掉如下：

1）删除虚拟环境文件夹，命令如下：

```shell
rmdir /s /q D:\CodeFile\qt_env
```

2）清理 `pip` 缓存，命令如下：

```shell
pip cache purge
```

##### 方法 1：调整系统 PATH 变量（推荐）

**1）打开系统环境变量设置：**`Win + R` 输入 `sysdm.cpl` → "高级" → "环境变量"

![[Pasted image 20250721080713.png]]

**2）在系统变量中找到 `PATH`，点击编辑**

![[Pasted image 20250721080930.png]]

**3）将 MSYS2 的路径移到 Python 路径之后**：

![[Pasted image 20250721080836.png]]

**PATH 推荐变量顺序：**

```
D:\ProgramEnviron\Python\Python311\            # 系统Python路径
D:\ProgramEnviron\Python\Python311\Scripts\    # 系统Python路径
...                                            # 其他环境变量
D:\ProgramEnviron\MSYS2\mingw64\bin            # MSYS2环境变量
```

**4）保存后重启所有命令行窗口（若无效请重启电脑）**

![[Pasted image 20250721082450.png]]
##### 方法 2：使用完整路径调用系统 Python

```shell
# D:\ProgramEnviron\Python\Python311\python.exe为当前系统Python路径
# D:\CodeFile\qt_env为创建的虚拟环境的位置

D:\ProgramEnviron\Python\Python311\python.exe -m venv D:\CodeFile\qt_env
```

![[Pasted image 20250721083241.png]]

