# 最简单微调工具（推荐）

我推荐 **Axolotl** 或 **LLaMA Factory**
此处以 **LLaMA Factory** 为例（图形界面 + 一键启动，最适合新手）

## 一键安装（Windows/Mac/Linux 通用）

```bash
git clone https://github.com/hiyouga/LLaMA-Factory.git
cd LLaMA-Factory
pip install -e ".[torch,metrics]"
```

## 启动图形界面

```bash
python src/webui.py
```

打开浏览器访问：

```plaintext
http://localhost:7860
```

---

# 微调 Qwen3.5-0.8B 完整步骤

## 1. 选择模型

```plaintext
Model name: Qwen2
Model path: qwen3.5:0.8b
```

## 2. 选择微调方式（新手必选）

```plaintext
微调方法：LoRA（最省资源）
```

## 3. 准备你的数据（最重要）

创建一个 `my_data.json` 文件，格式如下：

```json
[
  {
    "instruction": "你好",
    "input": "",
    "output": "你好呀！我是你微调后的专属AI~"
  },
  {
    "instruction": "介绍一下你自己",
    "input": "",
    "output": "我是基于Qwen3.5-0.8B微调的私人助手，只听你的话！"
  }
]
```

你想让它记住什么，就加什么对话。

## 4. 在界面选择这个数据集

点击：

```plaintext
Dataset → 上传 → 选择 my_data.json
```

## 5. 开始训练

点击 **Start**

等待 5~15 分钟，训练完成！

---

# 把微调后的模型导入 Ollama

## 1. 导出微调后的模型

LLaMA Factory 导出 → 选择 `GGUF` 格式

## 2. 创建 Ollama 模型文件

新建 `Modelfile`

```plaintext
FROM ./my-finetuned-model.gguf
```

## 3. 导入 Ollama

```bash
ollama create myqwen -f Modelfile
```

## 4. 运行

```bash
ollama run myqwen
```

✅ **你的私人微调模型就完成了！**

---

# 最关键的新手避坑

1. **0.8B 模型不要用全量微调，一定用 LoRA**
2. 数据集不要太大，**20~100 条效果最好**
3. 对话格式必须标准，否则训练无效
4. 微调完一定要导出 GGUF 才能给 Ollama 使用