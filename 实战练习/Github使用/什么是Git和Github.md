
### 访问Github官网

**Github官网：** https://github.com

因为Github是国外网站，因此访问极不稳定，访问时需要借助一些额外的工具，接下来介绍一种加速器，可用于访问Github

#### WattToolkit下载

进入微软商店搜索 WattToolkit 进行下载安装，安装成功后进入主界面后点击左栏的`网络加速`选项，在该选项中找到`Github`并勾选，即可正常访问Github官网。

![[Pasted image 20251102163246.png]]

#### dev-sidecar下载

1）打开Github官网，搜索 `dev-sidecar` 项目，找到包含`开发者边车`字样的项目，点击进入；

![[Pasted image 20251102170109.png]]

2）进入后找到 `releases` 进入可以看到很多版本，找到最新版本一栏中的 `Assets`，根据自己电脑的版本进行下载；

![[Pasted image 20251102170544.png]]

3）下载完成后自行安装，接下来进行 `安装根证书`;

![[Pasted image 20251102163145.png]]

4）根证书安装成功后就可以快速的访问Github网站了。

![[Pasted image 20251102164448.png]]



### 配置双重身份验证

1）进入Github首页，点击头像，点击 `settings` 进入设置界面；

![[Pasted image 20251102171356.png]]

2）左栏中找到 `Password and authentication` 点击进入，下拉找到 `Enable two-factor authentication` 点击进入；

![[Pasted image 20251102171548.png]]

3）接下来会出现一个QR码，在手机端下载Authenticator软件，使用该软件进行QR码扫描，扫描后手机端会出现一个Github选项，进入该选项将会出现一个验证码，在网页端输入验证码即可得到恢复码，如下图：

![[Pasted image 20251102172826.png]]

4）最后点击 `I have saved my recovery codes`，点击 `Done` 即可完成二重验证的配置操作。

![[Pasted image 20251102172925.png]]

>[!warning] 注意
>有时验证的方式可能是手机号验证，但是地区中未包含 `+86` 的地区，此时可以尝试退出设置重新配置二重验证，有几率会换成QR码验证

### 存储库基本介绍

github命名规则：

![[Pasted image 20251102173857.png]]

README.md文件内容：

- 展示项目的基础信息
- 介绍项目的用途及使用方法

Releases信息：项目发布的版本信息

Issues讨论：可以发布新的讨论，Open表示正在进行的讨论，Closed表示已经结束的讨论。

![[Pasted image 20251102175248.png]]

### Github汉化

1）搜索`Github 汉化插件`，找到 `maboloshi/github-chinese` 项目，点击进入；

![[Pasted image 20251102181234.png]]

2）进入后根据自己的浏览器选择安装插件（此处我使用的为Chrome浏览器），所以点击 `Tampermonkey` 进行下载，下载完成后点击 `Github源【开发版】`进行下载；

![[Pasted image 20251102181854.png]]

3）对插件根据官方文档进行配置，最后刷新Github网页，即可完成汉化操作（汉化的是一些按钮），如图所示：

![[Pasted image 20251102182702.png]]

4）若需要对文档内容进一步翻译，可以使用 `沉浸式翻译` 这款插件。

### Github常见快捷键

1. `/`：快速搜索
2. `T`：快速定位到文件搜索栏
3. `L`：快速定位行号
4. `?`：打开快捷键快速搜查表
5. `.`：打开网页版的VScode

### Github常用界面

https://github.com/explore

- 新闻
- 每日热榜
- 个人推荐

https://github.com/search

- 搜索项目
- 搜索代码
- 高级搜索（Advanced search）

https://github.com/stars

所有点过 `stars` 的项目

### 开源协议介绍

- GPL许可证（修改源码后保持开源且使用相同许可证）
- LGPL许可证（修改源码后保持开源）
- Mozilla许可证（修改源码后保持开源且为源码修改处提供说明文档）
- Apache许可证（修改源码后可闭源但修改过的文件必须放置版权说明）
- MIT许可证（修改源码后可闭源且允许衍生软件使用你的名字促销）
- BSD许可证（修改源码后可闭源但不允许衍生软件使用你的名字促销）

### 回溯介绍

![[Pasted image 20251102201306.png]]

### 分支介绍

主干分支：main/master

`git checkout -b feature` 创建分支feature
`git commit` 进行提交（feature）

假设此时主干分支出现了BUG需要修复：

`git switch main` 基于当前分支切换到main分支
`git checkout -b bugfix` 创建一个用于修复BUG的分支
`git commit` 提交BUG的修复代码
`git switch main`
`git merge bugfix --no-ff` 合并bugfix分支到主干分支

>[!warning] 注意
>可以基于任何分支创建分支

### Pull Request(PR)
