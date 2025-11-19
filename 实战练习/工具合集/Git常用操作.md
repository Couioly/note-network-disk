## 创建 SSH 密钥并用于 GitHub 上传项目

### 步骤1：检查现有 SSH 密钥
---

在生成新的 SSH 密钥之前，应检查本地计算机中是否有现有密钥。

1）打开Git Bash（吉特狂欢）.

2）输入以查看是否存在现有的 SSH 密钥。`ls -al ~/.ssh`

```git
$ ls -al ~/.ssh
# 命令执行后将显示系统中 `~/.ssh` 目录下的文件列表
```

3）检查目录列表，看看您是否已经拥有公用 SSH 密钥。默认情况下，GitHub 支持的公钥的文件名为以下文件名之一。

- `_id_rsa.pub_`
- `_id_ecdsa.pub_`
- `_id_ed25519.pub_`

>提示：如果收到 `_~/.ssh_` 不存在的错误，则默认位置中没有现有的 SSH 密钥对。您可以在下一步中创建新的 SSH 密钥对。

4）生成新的 SSH 密钥或上传现有密钥。

- 如果您没有受支持的公钥和私钥对，或者不想使用任何可用的公钥和私钥对，请生成新的 SSH 密钥。
- 如果看到列出了要用于连接到 GitHub 的现有公钥和私钥对（例如 `_id_rsa.pub_` 和 `_id_rsa_`），则可以将密钥添加到` ssh-agent`。

### 步骤2：生成新的 SSH 密钥
---

您可以在本地计算机上生成新的 SSH 密钥。生成密钥后，可以在 GitHub.com 上将公钥添加到帐户，以便通过 SSH 对 Git作进行身份验证。

1） 打开Git Bash。

2） 粘贴下面的文本，将示例中使用的电子邮件替换为您的 GitHub 电子邮件地址。

```git
ssh-keygen -t ed25519 -C "your_email@example.com"
```

>注意：如果您使用的是不支持 `Ed25519` 算法的旧系统，请使用：

```git
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

执行后会出现提示：

- 首先询问密钥保存路径，直接按回车使用默认路径（`~/.ssh/id_ed25519`）

```git
Enter file in which to save the key (/c/Users/YOU/.ssh/id_ALGORITHM): 回车
```

- 然后会提示设置密码（可选），直接按回车跳过即可（无需密码）

```git
Enter passphrase (empty for no passphrase): 输入密码
Enter same passphrase again: 再次输入密码
```

生成成功后，再次执行 `ls -al ~/.ssh` 就能看到新生成的密钥文件：

- `id_ed25519`（私钥，务必保密，不要分享给任何人）
- `id_ed25519.pub`（公钥，需要添加到 GitHub 的文件）

### 步骤3：查看并复制公钥内容
---

执行下列命令将会显示一段以 `ssh-ed25519` 开头、以你的邮箱结尾的文本，**完整复制这段内容**（包括所有字符，不要遗漏或多复制）

```git
cat ~/.ssh/id_ed25519.pub
```

### 步骤4：将公钥添加到 GitHub 账户
---

1. 登录你的 GitHub 账号，点击右上角头像 → **Settings**
2. 在左侧菜单找到 **SSH and GPG keys** → 点击右上角 **New SSH key**
3. 在 **Title** 栏填写一个标识（例如 "我的笔记本电脑"）
4. 在 **Key** 栏粘贴刚才复制的公钥内容
5. 点击 **Add SSH key** 完成添加（可能需要输入 GitHub 密码确认）

### 步骤5：测试 SSH 连接
---

在git bash终端输入以下命令，测试是否配置成功：

```git
ssh -T git@github.com
```


首次连接会出现确认提示，输入 `yes` 后回车

```git
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

然后输入SSH密钥密码后回车

```git
Warning: Permanently added 'github.com' (ED25519) to the list of known hosts.
Enter passphrase for key '/c/Users/31245/.ssh/id_ed25519':
```

如果看到类似以下内容，说明配置成功：

```plaintext
Hi 你的GitHub用户名! You've successfully authenticated, but GitHub does not provide shell access.
```

### 步骤6：上传项目到 GitHub
---

1）先在 GitHub 上创建一个新仓库（Repository），获取仓库的 SSH 地址（格式类似 `git@github.com:用户名/仓库名.git`）

2） 在本地项目文件夹中执行：

```bash
# 初始化仓库（如果尚未初始化）
git init

# 添加文件到暂存区
git add .

# 提交文件
git commit -m "首次提交"

# 关联远程仓库
git remote add origin git@github.com:用户名/仓库名.git

# 推送到GitHub
git push -u origin main
```

完成后，你的项目就成功上传到 GitHub 了。以后每次修改后，只需使用 `git add .`、`git commit -m "描述"` 和 `git push` 即可更新代码。


## 换行符警告

当执行`git add .`命令时出现警告提示，如下图示例：

```
warning: in the working copy of 'Crawl_Tools.py', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'README.md', LF will be replaced by CRLF the next time Git touches it
```

### 原因
---

在 Windows 系统中，默认的换行符是 `CRLF`（`\r\n`），而在 Unix/Linux/MacOS 系统中，默认的换行符是 `LF`（`\n`）。当你在 Windows 上使用 Git 并且没有配置好换行符的处理方式时，Git 可能会在添加文件（`git add`）或检出文件（`git checkout`）时，对换行符进行自动转换，从而给出这样的警告。

### 解决方法
---

你可以通过配置 Git 的换行符处理策略来解决这个问题，有以下两种常见的配置方式：

#### 方式一：让 Git 自动处理换行符

在 Git 仓库所在的目录下，打开终端（如 Git Bash），执行以下命令：

```bash
git config core.autocrlf true
```

这个配置的作用是：

- 当你执行 `git add` 时，Git 会将文件中的 `CRLF` 转换为 `LF` 后再存入版本库。
- 当你执行 `git checkout` 时，Git 会将版本库中的 `LF` 转换为 `CRLF` 后再检出到工作目录。

#### 方式二：保持换行符不变（适合跨平台协作场景）

如果你希望保持文件中的换行符不变（比如项目需要在多个平台上协作，希望统一使用 `LF` 作为换行符），可以执行以下命令：

```bash
git config core.autocrlf input
```

这个配置的作用是：

- 当你执行 `git add` 时，Git 会将文件中的 `CRLF` 转换为 `LF` 后再存入版本库。
- 当你执行 `git checkout` 时，Git 不会对换行符进行转换，保持版本库中的 `LF` 不变。

配置完成后，再执行 `git add .` 时，就不会再出现这样的警告了。

## 后续新增文件的提交流程

1）进入项目根目录

```bash
cd /文件路径
```

2）添加文件到暂存区

```bash
git add 所需上传的文件名
```

3）提交更改（使用明确的提交信息）

```bash
git commit -m "docs: 添加项目***文件"
```

4）推送远程仓库（默认分支通常为main或master）

```bash
git push origin main  # 若分支不同，请将main替换为你的分支名称
```

5）刷新 GitHub 仓库页面，文件会自动显示在仓库主页，文件会出现在仓库文件列表中。

## 后续修改文件的提交流程

1）首先修改所需修改的文件

2）修改完成后，可以使用`git status`来查看已修改的文件是否正确，命令和执行结果如下：

```bash
git status
```

此处示例运行结果表示修改的文件为`README.md`和`main.py`

```
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md
        modified:   main.py

no changes added to commit (use "git add" and/or "git commit -a")
```

3）使用`git add`命令将所修改的文件添加至暂存区

```bash
git add README.md main.py
```

4）使用`git commit -m`提交更改的信息，如下命令：

```bash
git commit -m "修改内容：
- 修复 README.md 文件中的启动命令错误
- 修改 main.py 文件的头部信息参数错误"
```

也可以使用明确的提交信息进行提交，如下列命令：

```bash
git commit -m "fix: 同时修复 README.md 的命令错误和 main.py 的参数错误"
```

**提交信息规范建议：**

- `feat`：新功能
- `fix`：错误修复
- `docs`：文档更新
- `style`：样式调整

5）将修改文件推送至Github上，命令如下：

```bash
git push origin main
```

