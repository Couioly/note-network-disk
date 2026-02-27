
## 设置代理

1.启动Burp，进入代理的设置页面，然后设置监听器为 `192.168.1.12`（此处是我本机电脑IP，端口自定义）

![](./images/file-20260227091850865.png)

2.设置电脑和手机的网络处于同一网段内，如我的电脑和手机连接的同一个WIFI，所以此时手机的IP为 `192.168.1.10` ，接下来进入手机端的WIFI设置页面，给手机设置代理，步骤如下：
- 设置 -> WLAN -> 与电脑相同的WIFI名（进入当前WiFi的详情页面）
- 修改代理：无 -> 手动
- 设置代理参数：主机名 -> 电脑IP（如 `192.168.1.12`）；端口 -> 监听器端口（如 `8899`）
- 保存

## 使用ADB连接手机

**官方精简版**：[https://developer.android.com/studio/releases/platform-tools（下滑找到「Download SDK Platform-Tools for Windows」）](https://developer.android.com/studio/releases/platform-tools)

>[!warning] 建议
>下载安装后建议配置环境变量，便于后续使用

