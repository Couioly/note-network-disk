
## 1 认识HTML
---
### 1.1 什么是HTML

**HTML**的全称是**超文本标记语言**，是一种用于创建网页结构和内容的标记语言，他可以使用各种标签描述网页的元素，这些元素包括文本图像，链接，表格，音频，视频等，将这些元素组合在一起，形成一个完整的网页结构。

### 1.2 HTML的作用
·
用来开发网页

### 1.3 网页的组成部分

**网站**：浏览器打开的，由很多个网页组成的一个整体

**网页**：浏览器打开的一个网页

网页由**HTML+css+js**组成

1. HTML：网页的骨架（确认网页的元素）
2. css：网页的皮囊（能够让网页炫酷好看起来，可以结合html控制网页排版，或者网页内容的颜色样式）
3. javascipt：网页的动作（javascipt能够让网页实现复杂的逻辑）

### 1.4 工具介绍

VScode

打开软件工具，创建一个专属的文件夹，用VScode打开文件夹。新建一个`.html`结尾的文件，使用快捷键`!+回车`可以快速生成网页框架。

```html
<!-- 原始代码生成：！+回车键 --> 
<!--文档的类型--> 
<!DOCTYPE html> 
<!-- 告诉浏览器整个文档的内容是用英语编写的。这有助于浏览器更好地理解和处理文档内容。 --> 
<!-- lang="en"英文 lang="zh-CN"简体中文，通常用于中国大陆地区。 --> 
<html lang="en"> 
<!-- html编码中，所有的代码全是用尖括号包裹的，并且大部分都是成对出现的 --> 
<!-- head在编码中式是头部的意思 网页的头部 --> 
<head> 
	<meta charset="UTF-8"> <!-- 编码的格式 --> 
	<meta name="viewport" content="width=device-width, initial-scale=1.0"> 
	<title>第一次做网页</title> <!-- title中间的内容是网页的标题 --> 
</head> 
<body> 

</body> 
</html>
```

![[Pasted image 20250806221421.png]]

## 2 HTML的基础
---
### 2.1 HTML的标题

HTML标题是通过标签来定义的

```html
<h1>这里是一级标题<h1> 
<h2>这里是二级标题<h2> 
<h3>这里是三级标题<h3> 
...
```

### 2.2 HTML的段落

HTML段落是通过标签来定义的

```html
<p>这里是一个段落</P>
```

主要是用于将一段内容包裹起来，便于后期的统一设置p标签本身没有的效果，标签内的内容和标签外的内容空一行 自动换行的

### 2.3 HTML链接

HTML的链接是通过标签来定义的

```html
<a href="https://www.baidu.com">这是前往百度的链接</a>
```

### 2.4 HTML图像

HTML图像是通过标签的来定义的

```html
<img src="/images/logo.png" width="200" height="100" /> <!-- width调节照片的宽度 height调节照片的高度-->
```

### 2.5 HTML的属性

HTML元素可以设置属性
属性可以在元素中添加附加信息
属性一般描述于开始标签
属性总是以 `名称/值对` 的形式出现，比如：`name=“value”`

|  属性   |   特殊符号   |      描述       |
| :---: | :------: | :-----------: |
|  id   |    #     |    id是唯一的     |
| class |    .     | class可以同时存在多个 |
| style | 无（普通的属性） |  用于对标签样式进行设置  |

#### 2.5.1 HTML样式

背景颜色background-color：

```html
<h1 style="background-color: #ff359b">这是一个标题</h1> 
<p style="background-color: #00d2ff">这是一个段落</p> 
<!-- style="background-color: 颜色;" 设置背景颜色 --> 
<!-- style="color: 颜色;" 设置标题字体颜色 -->
```

#### 2.5.2 字体样式

样式font-family：

```html
<h1 style="font-family：kaiTi">这是一个标题</h1> 
<!-- 通用 
宋体：SimSun 
黑体：SimHei 
微软雅黑：Microsoft YaHei 
微软正黑体：Microsoft JhengHei 
新宋体：NSimSun 
新细明体：PMingLiu 
细明体:MingLiU 
标楷体：DFKai-SB 
仿宋：FangSong 
楷体：KaiTi 
# style="color: red;font-family: kaiTi" 
在背景设置颜色的基础上把字体样式更改成了楷体 -->
```

#### 2.5.3 字体的大小

大小font-size：

```html
<p style="font-size: 18px">这是一个段落</p> <!--px:是一个单位-->
```

#### 2.5.4 文本对齐

文本对齐text-align：

```html
<!--使用text-align（文字对齐）属性指定文本的水平垂直对齐方式--> 
<h1 style="text-align: center">这是一个标题</h1>
```

多个属性添加在一段代码上的时候可以直接在；后边直接接要增加的属性

```html
<h3 style="font-family: kaiTi;font-size: 30px;text-align: center;color: aqua;">这是一个标题</h3> 
<!-- 对这标题里的字段进行了楷体字、字体加大、字体对齐、加颜色等多个属性设置 -->
```

### 2.6 HTML表格

HTML表格用于在网页上以行和列形式展示数据，表格由标签定义，包含行（tr），表头（th）和单元格（td）代码说明：

```html
<table> 表格标签 可以加border参数增加表格的边框 
<tr> 行 
<td> 列 
<th> 表格头/标题 

!+回车键生成源码 
<table> 
中间添加表格内的内容 
</table> 

border-collapse: collapse //合并单元格边框，使表格看起来更整洁 
padding：8px //设置单元格内边距，使内容与边框之间有空间 
text-align：left //设置文本左对齐 -->
```

表格常用属性

```html
ccolspan //合并列，<td colspan="2"> 表示该单元格横跨两列 
rowspan //合并行，<td rowspan="2"> 表示该单元格纵跨两行 
<caption> //添加表格标题，显示在表格上方 
<thead>,<tbody>,<tfoot> //用于分组表格的内容，分别表示表头，主题和页脚
```

创建表格示例：

```html
<!DOCTYPE html> 
<html lang="en"> 
<head> 
	<meta charset="UTF-8"> 
	<meta name="viewport" content="width=device-width, initial-scale=1.0"> 
	<title>第一次做表格</title> #//网页 
</head> 
<body> 
	<!-- 表格 tr行 td列 --> 
	<table border="3"> <!--在内容周围生成一个边框 --> 
		<tr> 
			<th>姓名</th> <!--生成一个表格头和标题 --> 
			<th>年龄</th> 
		</tr> 
		<tr> 
			<td>张三</td> <!--表格内的列内容 --> 
			<td>18</td> 
		</tr> 
		<tr> 
			<td>李四</td> 
			<td>20</td> 
		</tr> 
		<tr> 
			<td>王五</td> 
			<td>25</td> 
		</tr>
	</table> 
</body> 
</html>
```

### 2.7 HTML列表

HTML可以无序列表也可以有序列表。

无序列表使用：

标签，在每行元素开头，用粗体原点（典型小黑圆圈）进行标记 有序列表始于标签，在每行元素的开头，使用数字来标记。

```html
<!-- <ul>标签 在每行元素开头，是典型的小黑圆圈的点来进行标记 --> 
<ul> 
	<li>无序列表第一列</li> 
	<li>无有序列表第二列</li> 
	<li>无序列表第三列</li> 
</ul> 
<!-- <ol>标签 在每行的元素开头，用数字来标记 --> 
<ol> 
	<li>有序列表第一行</li> 
	<li>有序列表第二行</li> 
	<li>有序列表第三行</li> 
</ol>
```

### 2.8 HTML区块标签

和将元素包裹起来

div和span的区别：div自动换行，span不自动换行

```html
<!--会自动换行--> 
<div> hello! </div> 
<div> hello! </div> 
<!--不会自动换行--> 
<span> 你好世界 </span> 
<span> 你好世界 </span>
```

### 2.9 HTML的表单和输入

**type属性**用于定义文本框元素的类型，该属性指定了输入字段所期望的数据类型或输入的方式
**placeholder属性**，用于输入字段中提供默认提示的文本

-  **test** 默认值，定义单行输入的字段；**placeholder**是提示文字，内容比较多了可以使用“换行”

```html
<input type="test" placeholder="请输入姓名">
```

- **password**：定义密码字段

```html
<input type="password" placeholder="请输入密码">
```

- **date**：定义选择日期

```html
<input type="date">
```

前三项的效果加起来是这样：

```html
<input type="text" placeholder="请输入账号"> 
<input type="password" placeholder="请输入密码"> 
<input type="date">
```

![[Pasted image 20250806225212.png]]

- **redio**：定义单选框

```html
<!--选择按钮：redio单选框--> 
<input type="radio"><span>同意</span> 
<!--span是单选框后边接的文字 
但是这样显示起来不是很明显 我们可以写下边的语气看起来更明显一点 --> 
<input type="radio"><span>同意</span><!--span是单选框后边接的文字 --> 
<br> 
<p>选择性别</p> 
<input type="radio"><span>A男生</span> 
<br> 
<input type="radio"><span>B女生</span> 
<!-- 
	在男生和女生前面加一个input type="radio"参数（单选框） 
	但是这样的效果是两个都可以选的 因为他们不属于一个组 需要在一个组里 
	在radio后加参数name可以解决同时选择的问题 
-->
```

![[Pasted image 20250806225714.png]]

- **checkbox**：定义复选框

```html
<!--复选框--> 
<h3>这是一个复选框</h3> 
<input type="checkbox"><span>选项A</span> 
<br> 
<input type="checkbox"><span>选项B</span> 
<br> 
<input type="checkbox"><span>选项C</span> 
<!-- 复选框也可以加name参数 -->
```

![[Pasted image 20250806225733.png]]

- **button**：按钮

```html
<!-- 按钮键位 value按钮名称--> 
<input type="button" value="按钮"> 
<br> <!--br表示换行--> 
<button>一个按钮</button>
<br> 
<!--reset 重置按钮 ， submit 提交按钮--> 
<input type="reset"> <br> <!-- 重置按钮 --> 
<input type="submit"> <!-- 提交按钮 -->
```

![[Pasted image 20250806225904.png]]

- **file**：文件上传

```html
<!--文件上传--> 
<input type="file">
```

![[Pasted image 20250806230040.png]]

- 下拉列表

```html
<!-- select下拉列表 option下拉列表中的内容 --> 
<select> 
	<option>我的微信</option> 
	<option>我的账号</option> 
	<option>我的密码</option> 
</select>
```

![[Pasted image 20250806230143.png]]

- 文本域

```html
<!--文本域--> 
<textarea style="width: 200px;height: 100px;resize: none">记录一下...</textarea> 
<!-- 中间输入的是 文本里边的内容 -->
```

![[Pasted image 20250806230349.png]]


---

上一篇：[[L1-5 网络基础协议]]                    下一篇：[[L1-7 Web基础和CSS]]
