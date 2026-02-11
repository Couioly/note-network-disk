
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



