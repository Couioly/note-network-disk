
## 1 认识CSS
---
### 1.1 什么是CSS

css是一种样式设置的规则，用于**控制页面的外观样式**；css全称翻译是**层叠样式表**。

如果说HTML是网页的结构，那么css就是网页化妆师，它用于控制网页的布局、颜色、字体、间距等视觉表现，使网页内容与样式分离，从而提升开发效率和维护性。

**css的主要功能：**

1. 控制网页的样式；
2. 设置文本的字体，大小，颜色，对齐方式等；
3. 控制元素的背景调色，背景照片，边框对齐；
4. 调整元素的大小，内边距和外边距。

### 1.2 为什么使用CSS

1. 实现界面的精确控制，让页面更精美；
2. 实现内容与样式的分离，便于团队的开发；
3. 实现样式复用，便于网站的后期维护。

### 1.3 CSS的作用

页面外观的美化，实现特定的布局和定位。

**常见的css属性**

|       字体属性        |                                作用                                |                                            说明                                             |
| :---------------: | :--------------------------------------------------------------: | :---------------------------------------------------------------------------------------: |
|   `font-family`   |                            指定文本的字体系列                             |                                  font-family: Arial,宋体;                                   |
|    `font-size`    |                            指定文本的字体大小                             |                                      font-size: 16px                                      |
|   `font-weight`   |                            指定文本的字体粗细                             | normal：默认值，表示普通字体粗细。bold：表示粗体字体。bolder/lighter：相对于父元素更 粗 / 细 的字体。数值100-900，400普通字体，700粗体。 |
| `text-decoration` | 指定文本的装饰效果，如下划线、删除线等。可以使用关键字如：none、underline、line-through来设置装饰效果。 |                              text-decoration: line-through;                               |

|     文本属性      |  含义  |                       说明                       |         代码          |
| :-----------: | :--: | :--------------------------------------------: | :-----------------: |
|    `color`    |  颜色  |            可以使用颜色名称、十六进制值或RGB值来指定颜色            |     color: red;     |
| `line-height` |  行高  | 行之间的高度（当其值与div高度一致时，实现垂直对齐；可以结合text-align进行居中） |  line-height: 1.5;  |
| `text-align`  | 水平对齐 |        取值：left、right、<br>center、justify        | text-align: center; |

## 2 CSS书写规则
---

**css的三种书写位置**

- **行内**样式（写在style标签内）
- **内部**样式（直接写在head标签内）
- **外部**样式（使用外部.css文件）

### 2.1 行内样式

也称为嵌入样式，写在标签中，使用`HTML标签`的`style属性`定义；

只对设置`style属性`的标签起作用。

```html
<h1 style="在这里写css代码"></h1>
```

### 2.2 内部(联)样式

在标签下面建一个`style标签`写css代码，将页面内容与表现形式进行分离 ，方便对样式进行统一管理。

```html
<head> 
	<meta charset="UTF-8"> 
	<meta name="viewport" content="width=device-width, initial scale=1.0"> 
	<title>css书写规则</title> 
	<!-- 内部样式 --> 
	<style> 
		/*css特有的注释*/ 
		/*元素选择器，是为所有对应的标签进行设置，范围太广泛*/ 
		h2{ 
			color: rgb(255, 0, 0); 
			font-family: kaiti; 
			font-weight: 700; 
		} 
		/*class选择器（类选择器），可以为指定class名字的标签进行设置，范围会相对精准*/ 
		.name{ 
		color: aquamarine; 
		} /*id选择器（唯一）,可以为指定id名称的标签进行样式设置，范围最精确*/ 
		/*他跟class的区别是id选择器是唯一的只能指定一个标签，class可以指定多个标签*/ 
		#id1{ 
			background-color: aquamarine; 
		} 
	</style> 
</head> 
<body> 
	<h1>测试样例</h1> 
	<h1 class="name">测试样例</h1> 
	<h1 class="name">测试样例</h1> 
	<h1 id="id1">测试样例</h1> 
</body>
```

### 2.3 外部(联)样式

对**内联样式进行一步的抽离**，方便多个html页面公用一个样式；

用法：使用单独的`.css文件`定义，然后在页面中使用`link标签`引入。

```html
<head> 
	<meta charset="UTF-8"> 
	<meta name="viewport" content="width=device-width, initial scale=1.0"> 
	<title>css书写规则</title> 
	<!-- 外部样式，引入css文件 --> 
	<link rel="stylesheet" href="./外部连接样式.css"> 
</head> 
<body> 
	<h1>测试样例</h1> 
	<h1 class="name">测试样例</h1> 
	<h1 class="name">测试样例</h1> 
	<h1 id="id1">测试样例</h1> 
</body>
```

此时还存在调取的`外部连接样式.css`文件如下：

```css
/*css特有的注释*/ 
/*元素选择器，是为所有对应的标签进行设置，范围太广泛*/ 
h2{ 
	color: rgb(255, 0, 0); 
	font-family: kaiti; 
	font-weight: 700; 
} 
/*class选择器（类选择器），可以为指定class名字的标签进行设置，范围会相对精准*/ 
.name{ 
	color: aquamarine; 
} 
/*id选择器（唯一）,可以为指定id名称的标签进行样式设置，范围最精确*/ 
/*他跟class的区别是id选择器是唯一的只能指定一个标签，class可以指定多个标签*/ 
#id1{ 
	background-color: aquamarine; 
}
```

### 2.4 样式的优先级

**就近原则**：`行内样式 < 内部样式 < 外联样式`
**注意事项**：今后尽量不要对同格html进行重复样式书写

**如何选择三种样式**的写法：

1. 如果样式是**固定并且不修改**并且**很少**情况，使用**行内样式**
2. 如果样式**针对当前html页面**做的样式，并且**量比较大**的情况下，使用**内联样式**
3. 如果样式是**通用**，且css**代码很多**，使用**外联样式**，需要创建一个css文件，引入

### 2.5 选择器

css的选择器是css最基础也是最重要的一个知识点很重要，用于准确的选中元素，并赋予样式

**选择器权重**：元素选择器

#### 2.5.1 元素选择器

也称标签选择器，使用HTML标签作为选择器的名称

```html
<!--元素选择器，是为所有对应的标签进行设置，范围太广泛--> 
<head> 
	<style> 
		h2{ 
			<!--调节样式的代码--> 
			color: rgb(255, 0, 0);
			font-family: kaiti;
			font-weight: 700; 
		} 
	</style> 
</head> 
<boby> 
	<h1>内部样式</h1> 
	<h1>内部样式</h1> 
	<h1>内部样式</h1> 
	<h1>内部样式</h1> 
</boby>
```

#### 2.5.2 类选择器

使用**自定义的名称**，以.号作为前缀，然后再通过`html标签`的`class属性`调用类选择器

```html
<head> 
	<style> 
		/*class选择器（类选择器），可以为指定class名字的标签进行设置，范围会相对精准*/ 
		.name{ 
			color: aquamarine; /*调节样式的代码*/ 
		} 
	</style> 
</head> 
<boby> 
	<h1 class="name">内部样式</h1> 
	<h1 class="name">内部样式</h1> 
</boby> 
<!--class的名称要和在boby内输入的名称对上-->
```

#### 2.5.3 id选择器

使用**自定义名称**，以`#`作为前缀，然后通过`html标签`的`id属性`进行名称的匹配，id是**唯一的**

```html
<head> 
	<style> 
		/*id选择器（唯一）,可以为指定id名称的标签进行样式设置，范围最精确*/ 
		/*他跟class的区别是id选择器是唯一的只能指定一个标签，class可以指定多个标签*/ 
		#id1{ 
			background-color: aquamarine; /*调节样式代码*/ 
		} 
	</style> 
</head> 
<boby> 
	<h2 id="id1">内部样式2</h2> 
</boby> 
<!--class可以指定多个标签，id只能指定一个-->
```

## 3 CSS盒子模型
---

**盒子模型**（box model）是**css中用于描述元素布局和渲染**的基本概念，它将每个html元素视为一个矩形的盒子，这个盒子包含了元素的内容，内边距，边框和外边距。（**可以理解为，网页上每个可见的html元素都是一个盒子**）

### 3.1 盒子模型的组成

- **内容区**：**盒子的实际内容**，例如文本，图像或其他子元素；
- **内边距区**：内边距是**位于内容区与边框之间的空白区域**，他**提供了元素内容与边距之间的间距**；
- **边框区**：**边框是位于内容区周围的线条或边界**，他用于**定义元素的边界和外观**；
- **外边距区**：**外边距是位于边框与相邻元素之间的空白区域**，他**提供了元素与其他元素之间的间距**。

这四个部分形成了一个完整的盒子，他们相互影响和叠加，决定了元素在页面中的布局和尺寸，盒子模型在网页设计和布局中起着重要的作用，它使我们能够灵活的控制元素的外观和相互之间的关系；

**在css中，可以使用相关的盒子属性（如widht,height,padding,border,margin）来控制每个部分的大小，样式和间距，通过调整这些属性的值，可以对元素的布局，外观和空间分配进行精准的控制。**

![[Pasted image 20250807095742.png]]

### 3.2 盒子模型演示

```html
<head> 
	<meta charset="UTF-8"> 
	<meta name="viewport" content="width=device-width, initial scale=1.0"> 
	<title>盒子模型</title> 
	<style> 
		#box1{ 
			/*盒子模型属性的设置*/ 
			/*盒子模型是一个概念,我们可以把网页中的所有标签看作是一个个的小盒子 既然是盒子的话我们就可以为他去设置一些特点,这些特点我们就叫盒子属性*/ 
			background-color: aqua; /*字体颜色的设置*/ 
			width: 500px; /*设置宽度*/ 
			height: 500px; /*设置高度*/ 
			padding: 80px 100px; /*设置内边距 , 就是内容和边框之间的空白区域*/ 
			margin-top: 50px; /*设置外边距 就是元素周围空白的区域,也可以指定上下左右的值*/ 
			margin-left: 100px; /*top上 left左*/ 
			border-radius: 10%; /*边框圆角*/ 
		} 
		#box2{ 
			background-color: aquamarine; 
			width: 200px; /*设置密码框宽度*/ 
			height: 50px; /*设置密码框高度*/ 
			border-radius: 20%; /*设置密码框圆角*/ 
		} 
	</style> 
</head> 
<body> 
	<!-- 包裹标签 --> 
	<div id="box1">哈哈哈哈,今天天气真好</div> 
	<input id="box2" type="text" placeholder="请输入密码"> 
	<!-- input 也可以加盒子属性的--> 
</body>
```

### 3.3 盒子成分的分析

**margin**

盒子的**外边距**，是**完全透明**的，开发者只可以设置他的边距

margin包含了上下左右四条边，开发者可以单独设置每一条边的边距

- `margin-top`：上边距
- `margin-bottom`：下边距
- `margin-left`：左边距
- `margin-right`：右边距

注意：

1. `margin属性`后只跟一个值，同时设置四条边的边距相等
2. `margin属性`后跟两个值，第一个值设置上下边距，第二个值设置左右边距
3. `margin属性`后跟三个值，第一个设置上边距，第二个是设置左右边距，第三个值设置下边距

**padding**

表示盒子的**内边距**（填充）

与外边距不同，padding不是只能完全的透明，**可以设置背景颜色和图片**

与margin类似，padding包含了上下左右四条边，开发者**可以单独设置**每一条的边距

- `padding-top`：上部填充
- `padding-bottom`：下部填充
- `padding-left`：左部填充
- `padding-right`：右部填充

border

border表示盒子的边界，他可以设置成可见的，样式多样的

border像margin和padding一样可以分别对每一条边进行设置

- `border-top`：上边界
- `border-bottom`：下边界
- `border-left`：左边界
- `border-right`：右边接

```html
<!--以内部样式为例--> 
<head> 
	<style> 
		#box{ 
			/*使用简写属性，同时设置四条边界，四条边界的宽度，样式和颜色*/ 
			border: 2px solid green; 
			/*下面的样式与上面的样式等价*/ 
			border-top: 2px solid green; 
			border-bottom: 2px solid green; 
			border-left: 2px solid green; 
			border-right: 2px solid green; 
		} 
	</style> 
</head>
```

除了可以单独对每一条边进行样式设置之外，还可以分别对边界的宽度，样式和颜色进行设置（上边的表格有）

- 边界的宽度：`border-widht`
- 边界的样式：`border-style`
- 边界的颜色：`border-color`

常见的边框样式：

- 实线边框：`solid`
- 虚线边框：`dashed`
- 点状边框：`dotted`
- 双实线边框：`double`

### 3.4 背景

示例：

![[Pasted image 20250807101328.png]]
```html
<!--以内部样式为例--> 
<head> 
	<style> 
		#box{ 
			widht: 500px ;
			height:300px ;
			background-image:url(./图片.jpg); 
			backgroud-size:500px 300px; 
			/*图片不重复*/ 
			background-posittion：center; 
		} 
	</style> 
</head>
```

## 4 flex布局
---

2009年，W3C提出了一种新的方案-Flex布局，可以简便，完整，响应式的实现各种页面布局，目前，它已经得到了所有浏览器的支持。

**何为Flex布局**

即**弹性布局**，为盒装模型提供最大的灵活性，任何一个容器都可以指定为Flex布局

### 4.1 如何使用flex

#### 4.1.1 步骤一 创建flex容器

使用flex布局，首先需要创建一个flex容器，即一个元素的父级元素，在父级元素上应用`display:flex`样式，已**将其设置为flex容器**

例如：

```html
<style> 
	#b1{ 
		display:flex; 
	} 
</style> 
	<div id="a1"> 
		<div id="a101">flex项目1</div> 
		<div id="a101">flex项目2</div> 
	</div>
```

#### 4.1.2 步骤二 定义flex项目

将要放置flex容器内的元素称为`flex项目`，这些项目将根据flex容器的规则进行布局；

**默认**的情况下，`flex项目`会**水平排列**，可以**通过在flex容器上应用其他css属性来更改**flex项目的布局方式，如`flex-direction`，`justify-content`和`align-items`等。

### 4.2 flex布局容器的一些属性

**c（默认值：row）用于定义flex项目的排列方向**

可选值包括：

- `row水平方向值（默认值）`，从左到右排列
- `row-reverse`：水平方向，从右到左排列
- `column`：垂直方向，从上到下排列
- `column-reverse`：垂直方向，从下到上排列

**justify-content（默认值：flex-start）用于定义flex项目在主轴上的对齐方式**

可选值包括：

- `flex-start`：在主轴起始位置对齐
- `flex-en`：在主轴末尾位置对齐
- `center`：在主轴中心位置对齐o
- `space-between`：在主轴上均匀分布，首个项目放在起始位置，末尾项目放在末尾位置
- `space-around`：在主轴上均匀分布，项目之间和两侧均有空间
- `space-evenly`：在主轴上均匀发布，包括首个项目和末尾目两侧的空间

**align-items（默认值：stretch）用于定义flex项目在侧轴上的对齐方式**

可选值包括：

- `stretch`：默认值，如果项目未设置固定的侧轴尺寸，则会拉伸以填满容器
- `flex-start`：在侧轴起始位置对齐
- `flex-end`：在侧轴末尾位置对齐
- `center`：在侧轴中心位置对齐
- `baseline`：以基线对齐，适用于文本等

## 5 案例：制定qq邮箱视图框架
---

```html
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <meta name="viewport" content="width=device-width, initial-scale=1.0">  
    <title>Document</title>  
    <style>        #body{  
            padding: 0px;  
            margin: 0px;  
        }  
        #a1{  
            width: 100%;  
            height: 100vh;  
            background-color: aqua;  
            display: flex;  
            flex-direction: column;  
  
        }  
        #b1{  
            flex: 5;  
            background-color: brown;  
            display: flex;  
        }  
        #c1{  
            flex: 85;  
            background-color: blanchedalmond;  
            justify-content: center;  
            align-items: center;  
            display: flex;  
        }  
        #d1{  
            flex: 10;  
            background-color: blue;  
            display: flex;  
        }  
        #b01{  
            flex: 10;  
            background-color: aqua;  
        }  
        #b02{  
            flex: 50;  
            background-color: white;  
        }  
        #b03{  
            flex: 2;  
            background-color: aqua;  
        }  
        #b04{  
            flex: 2;  
            background-color: rgb(10, 146, 26);  
        }  
        #b05{  
            flex: 2;  
            background-color: rgb(150, 4, 111);  
        }  
        #b06{  
            flex: 2;  
            background-color: rgb(223, 216, 17);  
        }  
        #c2{  
            background-color: black;  
            width: 800px;  
            height: 500px;  
            display: flex;  
        }  
        #c01{  
            flex: 2;  
            background-color: aquamarine;  
        }  
        #c02{  
            flex: 1;  
            background-color: white;  
            display: flex;  
            justify-content: center;  
            align-items: center;  
        }  
        #c002{  
            background-color: white;  
            display: flex;  
            flex-direction: column;  
            justify-content: center;  
            align-items: center;  
        }  
        #d01{  
            flex: 2;  
            background-color: white;  
        }  
        #d02{  
            flex: 6;  
            background-color: aqua;  
        }  
        #d03{  
            flex: 2;  
            background-color: white;  
        }  
        #c003{  
            flex: 1;  
            padding-bottom: 80px;  
        }  
        #c004{  
            flex: 1;  
        }  
    </style>  
</head>  
<body>  
    <div>        
	    <div id="a1">  
            <div id="b1">  
                <div id="b01">自愿加班征集网</div>  
                <div id="b02"></div>  
                <div id="b03">用户</div>  
                <div id="b04">语言</div>  
                <div id="b05">介绍</div>  
                <div id="b06">更多</div>  
            </div>            <div id="c1">  
                <div id="c2">  
                    <div id="c01">  
                        <img src="https://haowallpaper.com/link/common/file/previewFileImg/16614696358169984" width="532" height="500">  
                    </div>                    
                    <div id="c02">  
                        <div id="c002">  
                            <div id="c003">  
                                <img src="https://haowallpaper.com/link/common/file/previewFileImg/16449967419411840" width="80" height="80"><br>  
                            </div>                            
                            <div id="c004">  
                                <input type="text" placeholder="请输入账号"><br>  
                                <input type="password" placeholder="请输入密码"><br>  
                                <input type="button" value="注册">  
                                <input type="button" value="登录">  
                            </div>                            
                            </div>  
                    </div>                
                </div>            
            </div>            
            <div id="d1">  
                <div id="d01"></div>  
                <div id="d02">  
                    <div>                       
	                    <a href="https://www.csdn.com">我的博客主页--点击跳转</a><br>  
                        家里蹲有限公司，版权归公司所有！  
                    </div>  
                </div>                
                <div id="d03"></div>  
            </div>        
        </div>    
    </div>
</body>  
</html>
```


---

上一篇：[[L1-6 Web基础和HTML]]                    下一篇：[[L1-8 Web基础和JavaScript]]
