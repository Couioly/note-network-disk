## 一、核心认知：requests 与 aiohttp+asyncio 的本质区别

### 1. 运行模式与并发能力

- **requests**：**同步阻塞**，单线程一次只能处理 1 个请求，等待响应时程序完全卡住；并发需依赖多线程 / 多进程，资源开销大、上限低。
- **aiohttp+asyncio**：**异步非阻塞**，基于协程 + 事件循环，单线程可同时处理上百个请求；等待 I/O 时自动切换任务，无阻塞、资源开销极低。

### 2. 核心概念对比（requests → aiohttp）

|requests 核心|aiohttp+asyncio 对应|关键差异|
|:--|:--|:--|
|`requests.get()`|`session.get()`（需 `async with`）|异步请求必须用 `ClientSession` 管理连接池|
|普通函数调用|`async def` 定义协程 + `await` 执行|协程需事件循环调度，不能直接调用|
|`response.text`/`json()`|`await response.text()`/`json()`|响应读取必须加 `await`，否则返回协程对象|
|无连接池管理|`ClientSession` 自动维护连接池|复用 TCP 连接，大幅减少握手开销|

### 3. 适用场景

- **requests**：少量请求、简单接口调用、同步业务逻辑、快速开发小脚本。
- **aiohttp+asyncio**：高并发爬虫、批量 API 调用、实时数据采集、异步 Web 服务、需要极致性能的 I/O 密集型任务。

## 二、环境准备

### 1. 安装依赖

```bash
# 核心异步HTTP库
pip install aiohttp
# 可选：加速DNS解析（强烈推荐）
pip install aiodns
```

### 2. 基础前置知识

- 掌握 `async/await` 语法：`async def` 定义协程函数，`await` 挂起当前协程等待异步操作完成。
- 理解事件循环：`asyncio.run()` 是 Python 3.7+ 启动事件循环的标准入口，负责调度所有协程任务。

## 三、从 requests 迁移：基础用法对照（可直接运行）

### 1. 单请求：requests 同步版 vs aiohttp 异步版

#### requests 同步（阻塞）

```python
import requests

def sync_request():
    # 发起同步GET请求，全程阻塞
    resp = requests.get("https://httpbin.org/get")
    print("状态码:", resp.status_code)
    print("响应文本:", resp.text[:100])
    # 解析JSON
    print("JSON数据:", resp.json())

if __name__ == "__main__":
    sync_request()
```

#### aiohttp 异步（非阻塞）

```python
import asyncio
import aiohttp

# 定义异步请求协程
async def async_request():
    # 1. 创建ClientSession（管理连接池、Cookie、Header，必须用async with）
    async with aiohttp.ClientSession() as session:
        # 2. 发起异步GET请求，await等待响应
        async with session.get("https://httpbin.org/get") as resp:
            print("状态码:", resp.status)
            # 3. 异步读取响应文本（必须await）
            text = await resp.text()
            print("响应文本:", text[:100])
            # 4. 异步解析JSON（必须await）
            json_data = await resp.json()
            print("JSON数据:", json_data)

if __name__ == "__main__":
    # 启动事件循环，执行异步任务
    asyncio.run(async_request())
```

### 2. 常用请求方法（GET/POST/PUT/DELETE）

#### GET（带参数）

```python
# requests
params = {"key": "value", "page": 1}
resp = requests.get("https://httpbin.org/get", params=params)

# aiohttp
async with session.get("https://httpbin.org/get", params=params) as resp:
    data = await resp.json()
```

#### POST（表单 / JSON）

```python
# requests（表单）
data = {"username": "test", "password": "123"}
resp = requests.post("https://httpbin.org/post", data=data)

# requests（JSON）
import json
resp = requests.post("https://httpbin.org/post", json=data)

# aiohttp（表单）
async with session.post("https://httpbin.org/post", data=data) as resp:
    pass

# aiohttp（JSON，推荐）
async with session.post("https://httpbin.org/post", json=data) as resp:
    pass
```

### 3. 自定义请求头、Cookie、超时

```python
# requests
headers = {"User-Agent": "Mozilla/5.0"}
cookies = {"session_id": "abc123"}
resp = requests.get("https://httpbin.org/headers", headers=headers, cookies=cookies, timeout=5)

# aiohttp
async with aiohttp.ClientSession(headers=headers, cookies=cookies) as session:
    # 超时配置：total总超时、connect连接超时
    timeout = aiohttp.ClientTimeout(total=10, connect=2)
    async with session.get("https://httpbin.org/headers", timeout=timeout) as resp:
        pass
```

## 四、核心进阶：高并发批量请求（aiohttp 核心优势）

### 1. 批量并发请求（asyncio.gather）

```python
import asyncio
import aiohttp

# 单个URL请求协程
async def fetch(session, url):
    try:
        async with session.get(url, timeout=10) as resp:
            # 仅返回状态码+URL，避免内存占用过大
            return {"url": url, "status": resp.status, "data": await resp.text()}
    except Exception as e:
        return {"url": url, "error": str(e)}

# 批量请求主协程
async def batch_request(urls):
    # 复用一个session，所有请求共享连接池（关键优化）
    async with aiohttp.ClientSession() as session:
        # 创建任务列表
        tasks = [fetch(session, url) for url in urls]
        # 并发执行所有任务，await等待全部完成
        results = await asyncio.gather(*tasks)
        return results

if __name__ == "__main__":
    # 测试URL列表
    test_urls = [
        "https://httpbin.org/get",
        "https://httpbin.org/status/200",
        "https://httpbin.org/status/404",
        "https://example.com"
    ]
    # 执行批量请求
    final_results = asyncio.run(batch_request(test_urls))
    for res in final_results:
        print(res)
```

### 2. 控制并发数（避免服务器过载）

使用 `asyncio.Semaphore` 限制最大并发数，防止请求量过大导致被封禁或服务器崩溃：

```python
import asyncio
import aiohttp

# 定义信号量，最大并发数设为10
MAX_CONCURRENT = 10
semaphore = asyncio.Semaphore(MAX_CONCURRENT)

async def fetch_with_limit(session, url):
    # 获取信号量，超过上限则等待
    async with semaphore:
        return await fetch(session, url)  # 复用之前的fetch函数

async def batch_request_with_limit(urls):
    async with aiohttp.ClientSession() as session:
        tasks = [fetch_with_limit(session, url) for url in urls]
        results = await asyncio.gather(*tasks)
        return results
```

## 五、响应处理与异常捕获

### 1. 常用响应方法（必须 await）

- `await resp.text()`：获取文本内容（支持指定编码，如 `encoding="utf-8"`）。
- `await resp.json()`：解析 JSON 为 Python 字典（最常用）。
- `await resp.read()`：获取二进制数据（用于下载文件、图片）。
- `resp.status`：HTTP 状态码（如 200、404、500）。
- `resp.headers`：响应头（字典格式）。

### 2. 异常处理（异步任务必备）

```python
async def safe_fetch(session, url):
    try:
        async with session.get(url, timeout=10) as resp:
            # 主动抛出非200状态码异常
            resp.raise_for_status()
            return await resp.json()
    except aiohttp.ClientError as e:
        # 捕获aiohttp客户端异常（连接错误、超时等）
        return f"客户端错误: {e}"
    except asyncio.TimeoutError:
        return "请求超时"
    except Exception as e:
        # 捕获其他未知异常
        return f"未知错误: {e}"
```

## 六、最佳实践（从 requests 迁移必看）

1. **必须复用 `ClientSession`**：不要为每个请求创建新 session，否则会频繁建立 / 断开 TCP 连接，性能大幅下降。
2. **避免在异步代码中混用同步 I/O**：如在 `async def` 中调用 `requests.get()`，会阻塞整个事件循环，异步优势完全失效。
3. **合理设置超时与并发数**：超时防止请求无限等待，并发数控制避免资源耗尽或被目标服务器封禁。
4. **异常处理全覆盖**：异步任务中未捕获的异常会导致整个事件循环崩溃，必须对每个请求做异常捕获。
5. **逐步迁移**：现有 requests 代码可保留，新功能 / 高并发模块用 aiohttp 实现，无需一次性全量重构。

## 七、官方与优质学习资源

1. **aiohttp 官方文档（权威）**：[https://docs.aiohttp.org/en/stable/](https://docs.aiohttp.org/en/stable/)docs.aiohttp.org
2. **asyncio 官方文档**：[https://docs.python.org/3/library/asyncio.html](https://docs.python.org/3/library/asyncio.html)
3. **中文实战教程**：[https://blog.csdn.net/gitblog_00895/article/details/151809149](https://blog.csdn.net/gitblog_00895/article/details/151809149)
4. **GitHub 源码**：[https://github.com/aio-libs/aiohttp](https://github.com/aio-libs/aiohttp)