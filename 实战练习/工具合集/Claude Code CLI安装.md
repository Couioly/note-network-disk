
1. 打开 powershell，输入安装指令：

```shell
irm https://claude.ai/install.ps1 | iex
```

![](./images/file-20260405092202903.png)

2. 此时表示安装成功，但是目前无法直接使用，需要配置环境变量：

![](./images/file-20260405092516436.png)

3. 重启powershell窗口，执行启动Claudecode命令：

```bash
claude
```

Anthropic对中国大陆地区的服务限制 -> 报错示例：

![](./images/file-20260405093429194.png)

此处推荐一款免费的大模型 LongCat-Flash-Thinking-2601，因为它免费，适合入门学习，打开LongCat的API开放平台，登录注册后进入该页面；

![](./images/file-20260405094212340.png)

进入开放平台后就可以看到它每天都赠送 `50w Token` 的额度，每日更新；

![](./images/file-20260405094720597.png)

接下来创建一个API密钥，点击 APIKeys 创建页面进行创建；

![](./images/file-20260405095004145.png)

打开 接口文档，找到 Claudecode 的配置文件信息，复制该json文件内容；

![](./images/file-20260405095348099.png)

