## 导包引起的错误

**报错截图**

![](./images/file-20260226091623984.png)

遇到的核心问题是 **Node.js 包的安装路径与 VSCode 终端的环境不一致**，导致 `base64-js` 包在 VSCode 中无法被找到，本质是不同终端 / 编辑器使用的 Node.js 环境（或包安装路径）不统一。

### 问题原因拆解

1. **全局安装的 “全局” 是相对的**：Node.js 的 `npm install -g` 会把包装到当前终端所关联的 Node.js 版本的全局目录下；
2. **编辑器终端的环境隔离**：PyCharm、VSCode 可能默认使用不同的终端（比如系统 cmd/PowerShell、Git Bash 或 WSL），甚至关联了不同版本的 Node.js（比如你装了多个 Node.js 版本），导致全局安装的包 “跨终端不可见”；
3. **局部安装优先级更高**：相比全局安装，把包装到项目本地（`node_modules` 目录）是更稳定的方式，能避免环境不一致问题。

### 解决方案（优先推荐局部安装，一劳永逸）

#### 步骤 1：在 VSCode 中打开项目终端，定位到项目根目录

确保终端的工作目录是你的 QQ 音乐项目根目录（能看到 `main.py`、`decrypt-data.js` 等文件的目录）。

#### 步骤 2：初始化项目的 `package.json`（首次执行）

```bash
npm init -y
```

这会在项目根目录生成 `package.json`，记录项目依赖，避免后续环境问题。

#### 步骤 3：局部安装 `base64-js` 包

```bash
npm install base64-js --save
```

安装后，项目根目录会生成 `node_modules` 文件夹（存放 `base64-js`），同时 `package.json` 会记录这个依赖。

#### 步骤 4：修改 JS 代码的导入方式（关键）

确保 JS 代码中导入 `base64-js` 的方式适配**局部安装**：

```javascript
// 正确的局部导入方式（推荐）
const base64js = require('base64-js');

// 如果你之前用的是 ES6 导入，也可以：
// import * as base64js from 'base64-js';
```

⚠️ 不要写绝对路径 / 相对路径（比如 `require('./node_modules/base64-js')`），让 Node.js 自动从项目本地的 `node_modules` 查找。

#### 步骤 5：验证安装成功

在 VSCode 终端执行以下命令，测试包是否能被找到：

```bash
node -e "const base64js = require('base64-js'); console.log('安装成功：', base64js)"
```

如果输出 `安装成功：[Object object]`，说明包已正确安装且可访问。

### 备选方案：统一 VSCode 的 Node.js 环境（解决全局安装失效问题）

如果不想局部安装，可确保 VSCode 终端使用的 Node.js 版本和你 “全局安装 base64-js” 的版本一致：

1. 打开 VSCode 的终端，执行 `which node`（Linux/Mac）或 `where node`（Windows），查看当前终端使用的 Node.js 路径；
2. 执行 `npm root -g`，查看该 Node.js 版本的全局包安装目录；
3. 确认 `base64-js` 存在于这个全局目录中（若不存在，在 VSCode 终端执行 `npm install -g base64-js`）；
4. 重启 VSCode，让终端加载最新的环境变量。

### 额外避坑提醒

1. **VSCode 的终端环境继承**：VSCode 默认继承系统终端的环境，但如果装了 nvm（Node.js 版本管理器），需确保 VSCode 终端加载了 nvm 环境（重启 VSCode 或手动执行 `nvm use 你的版本`）；
2. **Python 调用 JS 的路径**：如果你的 Python 代码是通过 `execjs`/`subprocess` 调用 Node.js 执行 JS 文件，需确保调用时的**工作目录是项目根目录**（否则 Node.js 找不到 `node_modules`）；

示例（Python 调用 JS 时指定工作目录）：

```python
import subprocess
import os

# 项目根目录（确保路径正确）
project_root = os.path.dirname(os.path.abspath(__file__))
# 调用 Node.js 执行 JS 文件，cwd 指定工作目录
result = subprocess.run(
	["node", "decrypt-data.js"],
	cwd=project_root,  # 关键：指定项目根目录
	capture_output=True,
	encoding="utf-8"
)
```

1. **忽略无关文件**：在项目根目录创建 `.gitignore`，添加 `node_modules/`，避免提交依赖文件夹到代码仓库。

### 总结

1. **核心解决方案**：用 `npm init -y` + `npm install base64-js` 做项目局部安装，适配所有编辑器 / 终端；
2. **关键要点**：JS 导入方式用默认的 `require('base64-js')`，Python 调用 JS 时确保工作目录是项目根；
3. **避坑核心**：局部安装依赖比全局安装更稳定，能彻底解决不同编辑器 / 终端的环境不一致问题。

按这个步骤操作后，VSCode 中就不会再报 `Cannot find module 'base64-js'` 的错误了。

## 编码引起的错误

**报错截图**

![](./images/file-20260226095139287.png)

你现在遇到的是**两个叠加的问题**：首先是 `subprocess` 读取 Node.js 输出时的**编码解码错误**（UnicodeDecodeError），导致 execjs 拿不到 JS 执行结果，进而返回 `None` 触发了 `replace` 方法的 AttributeError。核心原因是 Windows 系统默认编码（GBK）和 Node.js 输出的 UTF-8 编码不兼容，而非代码逻辑问题。

### 问题拆解

1. **根因：编码不匹配**

    execjs 底层调用 subprocess 执行 Node.js 时，默认用 Windows 系统编码（GBK）读取输出，但 Node.js 输出的是 UTF-8 编码的内容，包含 GBK 无法解码的字符（比如 0xaf），导致读取失败，最终返回 `None`。

2. **表象：AttributeError**

    因为编码错误导致 JS 执行结果读取失败，execjs 拿到 `None`，后续调用 `replace` 方法就报错了。

### 解决方案（彻底修复编码 + execjs 运行时问题）

核心问题还是**subprocess 调用时未指定 UTF-8 编码**，导致读取 Node.js 输出时出现 GBK 解码错误。我帮你定位到需要修改的位置，按下面的步骤改就能彻底解决编码问题：

#### 第一步：找到需要修改的 2 个核心函数

你的源码里有两个执行 Node.js 命令的函数：`_exec_with_pipe` 和 `_exec_with_tempfile`，这两个函数里的 `Popen` 调用都没指定编码，是报错的根源。

#### 第二步：逐处修改（标注行号 + 修改内容）

##### 1. 修改 `_exec_with_pipe` 函数（源码第 92-103 行）

**原代码**：

```python
p = Popen(cmd, stdin=PIPE, stdout=PIPE, stderr=PIPE, cwd=self._cwd, universal_newlines=True)
input = self._compile(source)
if six.PY2:
    input = input.encode(sys.getfilesystemencoding())
stdoutdata, stderrdata = p.communicate(input=input)
ret = p.wait()
```

**修改后**（添加 `encoding='utf-8'` + 调整输入编码）：

```python
# 关键：添加 encoding='utf-8'，去掉 universal_newlines（新版Python用encoding替代）
p = Popen(
    cmd, 
    stdin=PIPE, 
    stdout=PIPE, 
    stderr=PIPE, 
    cwd=self._cwd, 
    encoding='utf-8',  # 强制UTF-8编码
    errors='ignore'    # 忽略偶发的解码错误
)
input = self._compile(source)
stdoutdata, stderrdata = p.communicate(input=input)
ret = p.wait()
```

##### 2. 修改 `_exec_with_tempfile` 函数（源码第 118-129 行）

**原代码**：

```python
p = Popen(cmd, stdout=PIPE, stderr=PIPE, cwd=self._cwd, universal_newlines=True)
stdoutdata, stderrdata = p.communicate()
ret = p.wait()
```

**修改后**（同样添加编码参数）：

```python
# 关键：添加 encoding='utf-8' 和 errors='ignore'
p = Popen(
    cmd, 
    stdout=PIPE, 
    stderr=PIPE, 
    cwd=self._cwd, 
    encoding='utf-8',  # 强制UTF-8读取输出
    errors='ignore'    # 容错：忽略无法解码的字符
)
stdoutdata, stderrdata = p.communicate()
ret = p.wait()
```

##### 3. （可选）补充：修复 `_extract_result` 函数的空值问题

源码第 164 行的 `_extract_result` 函数在 `output` 为 `None` 时会报错，我们加个兜底判断：

**原代码**：

```python
def _extract_result(self, output):
    output = output.replace("\r\n", "\n").replace("\r", "\n")
    output_last_line = output.split("\n")[-2]
    ret = json.loads(output_last_line)
```

**修改后**：

```python
def _extract_result(self, output):
    # 新增：兜底判断 output 不为空
    if not output:
        raise ProgramError("JS执行结果为空，可能是编码错误或Node.js执行失败")
    output = output.replace("\r\n", "\n").replace("\r", "\n")
    output_last_line = output.split("\n")[-2]
    ret = json.loads(output_last_line)
```

#### 第三步：修改后验证

1. 保存 `_external_runtime.py` 文件；
2. 重启 VSCode 终端（确保修改生效）；
3. 重新运行 `python main.py`，此时：
    
    - 不会再报 `UnicodeDecodeError`（编码问题解决）；
    - 不会再报 `AttributeError: 'NoneType' object has no attribute 'replace'`（因为输出能正常读取，`output` 不再是 `None`）。
    

#### 关键说明

1. **为什么要改这两处？**
    
    - 你的源码中 `Popen` 只加了 `universal_newlines=True`（等价于 `text=True`），但未指定 `encoding`，Python 会用系统默认编码（Windows 是 GBK）读取 Node.js 输出，而 Node.js 输出是 UTF-8，所以解码失败；
    - 手动添加 `encoding='utf-8'` 后，强制用 UTF-8 读取输出，完美匹配 Node.js 的编码，从根源解决解码错误。
    
2. **errors='ignore' 的作用？**
    
    偶尔 Node.js 输出中会有特殊字符，加这个参数可以忽略偶发的解码错误，避免程序直接崩溃（仅作为容错，核心还是靠 `encoding='utf-8'`）。

#### 为什么 PyCharm 没报错？

PyCharm 会自动给 subprocess 调用设置 UTF-8 编码（或终端默认编码是 UTF-8），而 VSCode 终端默认继承 Windows 系统的 GBK 编码，所以只有 VSCode 触发了这个编码错误。

#### 总结

1. **核心问题**：Windows 系统编码（GBK）与 Node.js 输出编码（UTF-8）不兼容，导致 execjs 读取结果失败返回 None；
2. **关键修复**：
- 方案 1（简单）：修改 execjs 的 `_external_runtime.py`，给 Popen 添加 `encoding='utf-8'`；
- 方案 2（推荐）：放弃 execjs，直接用 subprocess 调用 Node.js，手动指定 UTF-8 编码；

3. **验证方法**：修改后运行代码，若不再报 UnicodeDecodeError，且 `musics_data` 有值，说明修复成功。

按这个修改后，你的代码就能在 VSCode 中和 PyCharm 一样正常运行了～