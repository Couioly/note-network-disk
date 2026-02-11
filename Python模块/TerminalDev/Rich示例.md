
可以在 REPL 中安装 Rich，这样 Python 数据结构会自动漂亮地打印并标注语法。具体做法如下：

```python
from rich import pretty
pretty.install()
["Rich and pretty", True]
```


利用 `rich` 库的 `Panel` 组件创建一个带边框的面板，并输出醒目的文本，代码如下：

```python
# 导入rich库的Panel组件和增强版print函数  
from rich.panel import Panel  
from rich import print  
  
# 创建并打印带样式的面板  
# Panel.fit() 会自适应内容宽度，border_style设置边框样式，文本使用加粗黄色  
print(Panel.fit("[bold yellow]Hi, I'm a Panel[/bold yellow]", border_style="red"))
```

![](./images/file-20260211113634572.png)

设置边框和文字颜色及样式效果：

```python
from rich.panel import Panel  
from rich import print  
from rich import box  
from rich.text import Text  
  
# 1. 基础边框样式（已验证能正常运行）  
print(Panel("圆角边框", border_style="green", expand=False, box=box.ROUNDED))  
print(Panel("粗边框", border_style="blue", expand=False, box=box.HEAVY))  
print(Panel("ASCII边框", border_style="yellow", expand=False, box=box.ASCII))  
  
# 文本+底色（仅文本）  
text = Text("渐变文本", style="bold red on yellow")  
panel = Panel(text, border_style="bright_cyan",expand=False)  
print(panel)  
  
# 模拟文字渐变：每个字符不同颜色  
text = Text()  
colors = ["red", "yellow", "green", "cyan", "blue"]  
for i, color in enumerate(colors):  
    text.append(f"渐{i}", style=color)  
print(Panel(text, border_style="white", expand=False))  
  
# 模拟边框渐变（多个Panel拼接）  
from rich.columns import Columns  
panels = [  
    Panel("左", border_style="red"),  
    Panel("中", border_style="yellow"),  
    Panel("右", border_style="green")  
]  
print(Columns(panels))
```

![](./images/file-20260211113534769.png)

