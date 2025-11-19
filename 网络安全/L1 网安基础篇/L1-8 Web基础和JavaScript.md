
在网络安全培训过程中，由于没有学习到JavaScript的知识点，导致后续的xss漏洞挖掘都不能正常进行，为此特来学习JavaScript的基础知识点。

## 1 JavaScript介绍
---

JavaScript（简称“JS”）是当前最流行、应用最广泛的客户端脚本语言，用来在网页中添加一些动态效果与交互功能，在 Web 开发领域有着举足轻重的地位。

JavaScript 与 HTML 和 CSS 共同构成了我们所看到的网页

- HTML 用来定义网页的内容，例如标题、正文、图像等
- CSS 用来控制网页的外观，例如颜色、字体、背景等
- JavaScript 用来实时更新网页中的内容，例如从服务器获取数据并更新到网页中，修改某些标签的样式或其中的内容等，可以让网页更加生动。

## 2 JavaScript使用方法
---

### 2.1 文件内部

在body内部写script标签、在标签内写js代码

```html
<body>
  <script>
    // 写js逻辑代码
  </script>
</body>
```

### 2.2 文件外部

在head头部文件标签中通过script引入js文件

```html
<head>
  <!-- 网页采用utf-8的编码格式 -->
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <!-- 网页标题 -->
  <title>Document</title>
  <!-- 引入js文件 -->
  <script src="./index.js"></script>
</head>
```

## 3 JavaScript输出
---

### 3.1 控制台输出

`console.log`在控制台输出（一般在程序中用来调试代码，打印日志）

```html
<body>
  <script>
    console.log('hello JavaScript!');
  </script>
</body>
```

> 注意：必须打开网页控制台才能看到输出的内容

### 3.2 网页弹窗输出

在网页输出（一般在程序中用来调试代码，提示报错）

```html
<body>
  <script>
    alert('hello JavaScript!')
  </script>
</body>
```

> 注意：打开网页自动弹出

**拓展**  `prompt()` 函数：`prompt()` 函数会弹出一个对话框，提示用户输入信息，并返回用户输入的字符串。

```html
<body>
  <script>    
	let name = prompt("请输入您的姓名：");
    console.log("您输入的姓名是：" + name);
  </script>
</body>
```

## 4 JavaScript注释
---

### 4.1 单行注释

```html
<body>
  <script>
    // 这是注释内容
    alert('hello JavaScript!')
  </script>
</body>
```

### 4.2 多行注释

```html
<body>
  <script>
    /* 这是注释内容
    很多注释 */
    alert('hello JavaScript!')
  </script>
</body>
```

```
补充：
<!-- html注释 --> 
/* css注释（单行多行通用） */
// js注释
/* js多行注释*/
```

## 5 变量
---

### 5.1 变量声明

```js
    // 声明变量
    let 小皮;
```

### 5.2 变量赋值

```html
  <script>
    // 声明变量
    let 小皮;
    // 变量赋值
    小皮 = '我家的一条狗'
  </script>
```

### 5.3 变量声明并赋值

```html
<body>
  <script>
    let 小皮 = '我家的一条狗'
    console.log(小皮);
  </script>
</body>
```

## 6 JavaScript的数据类型
---

### 6.1 基本数据类型

- 字符串型 (String)
- 数值型 (Number)
- 布尔 (Boolean)
- undefined (Undefined)
- null (object)

### 6.2 String 字符串

String用于表示一个字符序列，即字符串。字符串需要使用 **单引号** 或 **双引号** 括起来。

```html
  <script>
   let a = '我是一个字符串a'
   let c = "我是一个字符串b"
  </script>
```

### 6.3 Number数值型

Number类型用来表示整数和浮点数，最常用的功能就是用来表示10进制的整数和浮点数。

```html
  <script>
    // 整数
    let a = 10
    // 浮点数
    let c = 10.25
  </script>
```

### 6.4 Boolean

布尔类型也被称为逻辑值类型或者真假值类型。

```html
  <script>
    // true 代表真
    let a = true
    // false 代表假
    let c = false
  </script>
```

### 6.5 Undefined

在使用let声明变量但未对其加以初始化时，这个变量的值就是undefined。

```html
  <script>
    // 变量声明 没有赋值 就会打印undefined
    let a;
    console.log(a);
  </script>
```

### 6.6 Null

空，一般用来初始化

```html
  <script>
    // 当变量需要为空的时候 就可以写null
    let a = null        
    let b = null
    // js中null 类似于 Python中的None
  </script>
```

### 6.7 typeof判断类型

场景：基本数据类型的判断

```html
  <script>
    let a = '我是字符串类型'
    let b = 10
    let c = undefined
    let d = null
    let e = true
    console.log(typeof a);  // string
    console.log(typeof b);  // number
    console.log(typeof c);  // undefined
    console.log(typeof d);  // 如果值是null 打印会为object类型
    console.log(typeof e);  // boolean
  </script>
```

## 7 运算符
---

### 7.1 算数运算符

案例：

```html
  <script>
    let a = 30
    let b = 20
    console.log(a+b);   // 加法  结果: 50
    console.log(a-b);   // 减法  结果：10
    console.log(a*b);   // 乘法  结果：600
    console.log(a/b);   // 除法  结果:1.5

    console.log(++a);   // 自增  结果：31

    console.log(--a);   // 自增  结果：29
  </script>
```

### 7.2 赋值运算符

赋值运算符（`=`、`+=`、`-=`、`*=`、`/=` ）

案例：

```js
 let a = 30
 let b = 20
 console.log(a=100);  // 结果: 100

 console.log(a+=b);   // a = a + b 结果50

 console.log(a-=b);   // a = a - b 结果10
```

### 7.3 比较运算符

- `==` 判断两边的值是否相等(但是不会验证数据类型是否一致) 如果相等返回true，否则返回false 只要值相等就相等

```html
  <script>
    let a = 30
    let b = '30'
    console.log(a==b); // 值相等就会相等 true
  </script>
```

- `!=` 用来判断两个值是否不相等 如果不相等返回true，否则返回false

```html
  <script>
    let a = 30
    let b = '30'
    console.log(a!=b); // 值相等就会相等 false
  </script>
```

- `===` 判断两边的值 和数据类型是否相等（值和数据类型都会进行验证）

```html
  <script>
    let a = 30
    let b = '30'
    console.log(a===b); // 值相等就会相等 false
  </script>
```

## 8 条件判断
---

**if判断语法框架**

```js
三个关键词汇: if  -- else  --  else if

语法结构:
    if(条件){
         执行语句
    }else if(条件){
         执行语句 
    }else{
         执行语句
    }
```

示例：

```html
  <script>
    let a = 30
    if(a == 30){
      console.log('条件成立 执行下面语句');
    }else{
      console.log('条件不成立 执行else的语句');
    }
  </script>
```

补充：

- js中的 并且符号 ： `&&` 类似于python中的`and`
- js中的 或者符号 ： `||` 类似于python中的`or`
- `;`不写也不会报错，也不影响使用，因为js有自动加`;`的机制

## 9 引用数据类型
---

### 9.1 数组

**数组**(Array)是一组按顺序排列的数据的集合（类似于python的列表），数组中的每个值都称为元素。
**数组中的每个元素都有一个唯一的索引，从0开始递增。**
数组中可以包含任意类型的数据（包括数字、字符串、布尔值、对象以及其他数组等）。
在 JavaScript 中定义数组需要使用方括号`[ ]`，数组中的每个元素使用逗号进行分隔，如下实例：

```js
  <script>
    // 创建一个空数组
    let array = [];
    // 创建一个包含初始值的数组
    let array = [1, 2, 3];
    // 直接创建
    let arr = [1, '2', 'hello world', [1,2,3]]
  </script>
```

通过索引取值

```js
    let arr = [1,2,3,4,5,6]
    console.log(arr[0]);
    console.log(arr[1]);
    console.log(arr[2]);
    console.log(arr[3]);
    console.log(arr[4]);
    console.log(arr[5]);
```

以下是一些常用的数组操作方法(对比python列表)：

- `array.length`：获取数组的长度。
- `array[index]`：通过索引获取数组中的元素。
- `array.push(element)`：向数组末尾添加一个元素。
- `array.pop()`：从数组末尾移除并返回一个元素。
- `array.unshift(element)`：向数组开头添加一个元素。
- `array.shift()`：从数组开头移除并返回一个元素。
- `array.splice(start, count, element1, element2, ...)`：从指定位置(start)开始删除指定数量(count)的元素，并可选地插入新的元素(element)。
- `array.concat(array1, array2, ...)`：合并一个或多个数组，返回一个新数组。
- `array.indexOf(element)`：返回指定元素在数组中首次出现的索引，如果不存在则返回-1。
- `array.includes(element)`：判断数组是否包含指定元素，返回布尔值。

### 9.2 对象

**对象**(Object)类型是一组由键、值组成的无序集合，定义对象类型需要使用花括号`{ }`
**其中每个键都是字符串（不加引号），而值可以是任何数据类型，包括数字、字符串、布尔值、数组、函数等。**
对象是JavaScript中非常重要的数据结构之一，它提供了一种灵活的方式来组织和处理数据。

```js
  <script>
    let arr = {
      name:'不渝',
      age:'18'
    }
  </script>
```

通过对象名.键名字取值，通过.名字修改值

```js
  <script>
    let arr = {
      name:'Lily',
      age: '18'
    }
    console.log(arr.name);
    arr.name = "小柒"  
    console.log(arr.name);
    console.log(arr.age);
  </script>
```

还可以使用对象的方法来操作对象的属性和行为。例如，使用`Object.keys(obj)`可以获取对象的所有键，使用`Object.values(obj)`可以获取对象的所有值，使用`Object.entries(obj)`可以获取对象的所有键值对。

### 9.3 循环

循环就是重复做一件事，在编写代码的过程中，我们经常会遇到一些需要反复执行的操作，例如遍历一些数据、重复输出某个字符串等，如果一行行的写那就太麻烦了，对于这种重复的操作，我们应该选择使用循环来完成。

#### 9.3.1 for循环

循环适合在已知循环次数时使用

**语法说明**

- 形参1: 初始化计数器变量
- 形参2: 设置循环的次数
- 形参3：每次循环结束后更新（递增或递减）计数器的值

```js
for(形参1;形参2;形参3){
  //循环代码块;
}
```

```js
for(let a = 0; a < 10; a++){
  console.log(a);
}
```

```js
//遍历数组值   
let arr = [1,2,3,4,5,6]
for(let a = 0; a < arr.length; a++){
  console.log(arr[a]);
}
```

```js
// 99乘法表
for(let i = 1; i<10;i++){
    let x = "" ;   // 用于拼接每一行要打印的数据
    for(let j=1;j<=i;j++){
        // 在js中 console.log() 会自动换行
        // 在js中变量可以不用关键字申明
        // 但是最好要用关键词申明，推荐let
        let y = `${i}*${j}=${i*j}`
        x = x+" "+y
    }
    console.log(i,x);
}
```

#### 9.3.2 while循环

适合根据条件循环（未知循环次数满足条件的情况）

```js
    while(条件){
        执行代码
    }
```

```js
  <script>
    let a = 10
    while(a == 10){
      console.log('因为a等于10 满足条件 代码一直执行');
    }
  </script>
```

```js
// 在js用let关键词申明变量的时候，变量名不可重复申明
// while循环99乘法表
let i = 1;
while (i<10){
    let x = " " ;
    let j = 1 ;
    while (j<=i){
        let y = `${i}*${j}=${i*j} ` ;
        x = x+y
        j++;
    }
    console.log(x);
    i++;
}
//遍历数组值  
let list = ["A","B","C","D"] ;
i = 0 ;
while (i< list.length){
    console.log(`当前数组下标是${i},当前下标的值是${list[i]}`);
    i++;
}
```

- `break`：结束死循环

```js
  <script>
    let a = 10
    while(a == 10){
      console.log('因为a等于10满足条件  代码一直执行');
      break   // 结束循环
    }
  </script>
```

### 9.4 反引法

在JavaScript中，你可以使用模板字面量（Template literals）和`${}`语法来插入变量和表达式。这提供了一种方便的方式来格式化输出字符串。

```js
  <script>
    let a = 10
    console.log(`我今年${10}岁了`);
	//使用了反引号（``）来定义模板字面量，其中${}用于插入变量。
    //在${}内部，你可以放置任何有效的JavaScript表达式，包括变量、函数调用和运算符等。
	//模板字面量的好处是它提供了更直观、易读的方式来构建包含变量的字符串，而不需要使用字符串连接操作符（+）或复杂的字符串拼接方法。
  </script>
```

## 10 函数function
---

函数是一组执行特定任务（具有特定功能）的，可以重复使用的代码块

### 10.1 定义函数

```js
/*语法：*/
// 第一种方式：常规声明
function 函数名(){
      逻辑代码
}
// 第二种方式：箭头函数
let name = () =>{
    逻辑代码
}
```

### 10.2 调用函数

```js
let name = ()=>{

}
// 调用函数name()
name()
```

### 10.3 形参和实参

```js
let name = (形参1,形参2)=>{

}
name(实参1,实参2)
```

```js
let name = (x,y)=>{
  console.log(x,y);
}
name(10,20)
```

### 10.4 return

返回函数内部的值

```js
let name = (x,y)=>{
  return x + y
}
a = name(10,20)
console.log(a);
```

### 10.5 函数形参默认值

y不传就默认为20 传了就以传过来的实参为准

```js
let name = (x,y=20)=>{
  return x + y
}
a = name(10)
console.log(a);
```

### 10.6 函数嵌套

```js
  <script>
    let a = () => {
      console.log('我是函数a')
      b()
    }
    let b = () => {
      console.log('我是函数b')
    }
    a()
  </script>
```

```js
/*课堂实例：*/
// 第一种方式：常规声明
function add(x,y){
console.log(`${x}+${y}=${x+y}`);
}
// 函数调用与python一样
add(1,1);  
console.log(add(3,3));

// 第二种方式：箭头函数(可以给名字也可以不给名字)
let add2 = (x,y)=>{
return `${x}+${y}=${x+y}` ;
}
console.log(add2(2,2));

// 设置函数默认参数值
let add3 = (x,y=2)=>{
    return `${x}%${y}=${x%y}` ;
}
console.log(add3(10));

// 函数嵌套
let x = () => {
    console.log("这是x函数");
    y();
}
let y = () => {
    console.log("这是y函数");
    z();
}
let z = () => {
    console.log("这是z函数");
}

x();
```

## 11 定时器

JavaScript 定时器，有时也称为“计时器”，用来在经过指定的时间后执行某些任务，类似于我们生活中的闹钟。
JavaScript 中提供了两种方式来设置定时器，分别是 `setTimeout()` 和 `setInterval()`，它们之间的区别如下：

|      方法       | 说明                                                                      |
| :-----------: | ----------------------------------------------------------------------- |
| setTimeout()  | 在指定的时间后（单位为毫秒），执行某些代码，代码只会执行一次                                          |
| setInterval() | 按照指定的周期（单位为毫秒）来重复执行某些代码，定时器不会自动停止，除非调用 clearInterval() 函数来手动停止或者关闭浏览器窗口 |

### 11.1 setTimeout()

`setTimeout()` 函数用来在指定时间后执行某些代码，代码仅执行一次。

语法：setTimeout（函数，时间） 时间以毫秒为单位

```js
  <script>
    setTimeout(()=>{
      console.log('3秒后此代码执行');
    },3000)
  </script>
```

### 11.2 setInterval()

`setInterval()` 函数可以定义一个能够重复执行的定时器，每次执行需要等待指定的时间间隔。

语法：setInterval（函数，时间）

```js
    setInterval(()=>{
      console.log('每个3秒 当前代码执行一次');
    },3000)


    // 时间函数封装
let time = () => {
    // Date是js内置时间模块，可以获取当前时间对象
    let myDate = new Date();
    // 获取完整的年份(4位,1970-????)
    let n = myDate.getFullYear(); 
    let y = myDate.getMonth() + 1; // 获取当前月份(0-11,0代表1月)
    let r = myDate.getDate(); // 获取当前日(1-31)
    let s = myDate.getHours(); // 获取当前小时数(0-23)
    let f = myDate.getMinutes(); // 获取当前分钟数(0-59)
    let m = myDate.getSeconds(); // 获取当前秒数(0-59)
    let arr = n+"年"+y+"月" + r+"日"+ s+"时"+ f+"分"+ m +"秒";
    return arr;
 };
// 实现定时提醒当前时间的效果
setInterval(()=>{
    console.log(time());
},1000)
```

## 12 Dom树
---

### 12.1 dom介绍

什么是dom？我们通过一张图赖了解

![[Pasted image 20250807190220.png]]

dom可以理解为一个个的标签（节点）

### 12.2 为什么要有dom？ 有什么作用

我们可以通过dom访问和修改操作让网页**动起来**

### 12.3 获取dom

document 代表整个网页，相当于dom树的入口
要获取 DOM 元素，可以使用 JavaScript 中的各种方法和属性。下面是几个方法介绍：
`getElementById`：**通过元素的 ID 获取dom**。

```js
// 语法：
let element = document.getElementById("elementId");
// elementId : 标签中的id值，类似于css中的id选择器
```

```js
// 举例
<!-- 定义一个p标签,设置id选择器p001 -->
<p id="p001">今天天气很好</p>

<script>
    // p 是我们获取的 id为p的<p>标签的dom节点
    let p1 = document.getElementById("p001");
    // 在控制台打印获取的 p 标签dom信息
    console.log("第一个",p1);
</script>
```

`getElementsByClassName`：通过类名获取元素集合（返回的是一个 HTMLCollection）。

```js
// 语法：
let elements = document.getElementsByClassName("className")
// className : 标签中的class值，类似于css中的class选择器
```

```js
// 举例:
<!-- 定义了一个p标签，设定class选择器名p002 -->
<p class="p002">这是我的第二个P标签</p>
<p class="p002">这是我的第三个P标签</p>
<p class="p002">这是我的第四个P标签</p>

<script>
    // 根据类名获取获取该类名对应的所有数据，返回的是一个数据集合对象
    let p2 = document.getElementsByClassName("p002") ;
    console.log("第二个",p2);
</script>
```

`querySelector`：通过选择器获取第一个匹配的元素。

```js
//语法：
let element = document.querySelector("selector");
//可以通过元素的ID、类名、标签名或其他CSS选择器来选择和获取元素。
```

```js
//举例：
//选择ID为"myElement"的元素：
var element = document.querySelector("#myElement");
//选择类名为"myClass"的第一个元素：
var element = document.querySelector(".myClass");
//选择标签名为"div"的第一个元素：
var element = document.querySelector("div");
// 可以打印dom在控制台来看看效果
```

### 12.4 修改样式

要修改DOM元素的样式，可以使用JavaScript中的`style`属性。`style`属性允许我们直接访问和修改元素的样式属性。

```js
// 实例：
//使用 style.属性名：通过直接设置元素的样式属性来修改样式。
let bon = document.querySelector('button')
// 通过style控制样式 color 为red 
bon.style.color = 'red'
// 修改button按钮的 字体大小bon.style.fontSize= '60px'
bon.style.fontSize= '60px'
```

### 12.5 获取文本内容

`innerHTML` 获取或设置元素的 `HTML` 内容

```js
<p>文本内容 哈哈</p>

<script>
// 获取
let p1 = document.querySelector('p')
console.log(p1.innerHTML);
// 修改
p1.innerHTML = "<p>呵呵呵</p>"
console.log(p1.innerHTML);
/*
innerHTML 属性不仅可以获取文本内容，还可以获取包括 HTML 标签在内的整个元素内容。
*/
</script>
```

- `value`获取到输入框中的内容（默认值内容）

```js
<input type="text" value="12312">
    
<script>
    // 获取
    let text = document.querySelector('input')
    console.log(text.value);
    // 修改
    text.value = "999999999"
    console.log(text.value);
    /* 
    需要注意的是，当获取输入框的值时，value 属性返回的是字符串类型。如果需要进行数值计算或其他操作，可能需要将其转换为适当的数据类型。
    */
</script>
```

### 12.6 事件

`addEventListener()` 是 JavaScript 中用于添加事件监听器的函数。它允许指定一个特定的事件类型，并在事件发生时执行相应的处理函数。

```js
// addEventListener()的语法如下：
dom对象.addEventListener(event,listener)
// event : 要监听的事件类型
// listener : 回调函数，事件发生时要执行的处理函数。
```

- `click` 点击事件的应用场景：点击后发生什么事情，如下案例：

```js
<button id="myButton">点击登录</button>

<script>
    let button  = document.getElementById('myButton')
    button.addEventListener('click',()=>{
        // 在点击事件发生时执行的处理函数
        alert("按钮被点击了！")
    })
</script>
```

通过点击事件，您可以在用户与页面进行交互时执行特定的操作，例如提交表单、打开链接、显示/隐藏元素等等。

- `change` 触发事件，是指在表单元素的值发生改变并失去焦点时触发的事件。应用场景：获取输入框值

```js
<div>
    请输入账号：
	<input id="myInput" type="text">
</div>
<script>
    // 获取 input 标签的 dom
    let userName = document.getElementById("myInput") ;
    // 添加点击监听事件
    userName.addEventListener('change',()=>{
        let x = userName.value;
        console.log("刚刚输入的用户名是："+x);
    })
</script>
```

- `input`触发事件是指在用户输入内容时实时触发的事件。它在输入框的值发生变化时立即触发。而不需要等待失去焦点（`change` 事件）

```js
<div>
    请输入密码：
	<input id="password" type="password">
</div>
<script>
    // 获取 密码输入框的 dom
    let password = document.getElementById("password") ;
    // 给密码输入框dom对象 添加一个事件监听器，监听input事件
    // 只要该事件发生，就在控制台打印密码输入框获取的值
    password.addEventListener('input',()=>{
        let y = password.value;
        console.log("当前输入的密码是："+y);
    })
</script>
```


---

上一篇：[[L1-7 Web基础和CSS]]                    下一篇：[[L1-9 PHP基础语法]]
