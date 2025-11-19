
<font color=red>一个强大实用的黑客暴力破解字典建立工具 ——pydictor</font>

```
                          _ _      _
          _ __  _   _  __| (_) ___| |_ ___  _ __
         | '_ \| | | |/ _` | |/ __| __/ _ \| '__|
         | |_) | |_| | (_| | | (__| || (_) | |
         | .__/ \__, |\__,_|_|\___|\__\___/|_|
         |_|    |___/                         
```

## 1 为什么使用pydictor
---

#### 1.1 生成密码

  - 可以用pydictor生成普通爆破字典、基于网站内容的自定义字典、社会工程学字典等等一系列高级字典；
  - 可以使用pydictor的内置工具，对字典进行安全删除、合并、去重、合并并去重、高频词筛选；
  - 可以输入自己的字典，然后使用handler工具，对字典进行各种筛选，编码或加密操作。

#### 1.2 可定制性强

  可以通过修改多个配置文件、加入自己的字典、选用`leet mode`模式、长度选择、各类字符数量筛选、各类字符种类数筛选、正则表达式筛选，甚至可通过在`/lib/encode/ `目录下增加自己的脚本，完成自定义加密方法等高级操作；按照API编写标准，在`/plugins/`文件夹下添加自己的插件脚本，在`/tools/`目录下添加自己的工具脚本等。生成独一无二的高度定制、高效率和复杂字典，生成密码字典的好坏和你的自定义规则、能不能熟练使用pydictor有很大关系。
  
#### 1.3 强大灵活的配置解析功能

#### 1.4 兼容性强

 不管是使用的python 2.7版本还是python 3.4 以上版本，pydictor都可以在Windows、Linux 或者是Mac上运行。

## 2 声明
---

```
1. 在对方未授权的情况下，直接或间接利用pydictor工具攻击目标是违法行为.
2. pydictor 仅为安全研究和授权情况下使用，其使用人员有责任和义务遵守当地法律条规.
3. pydictor 的发布遵循GPL v3协议，开发人员对因误用该程序造成的资产损坏和损失概不负责.
```

## 3 部署
---

1. 克隆仓库指令

```bash
git clone --depth=1 --branch=master https://www.github.com/landgrey/pydictor.git
```

2. 进入目录

```bash
cd pydictor/
```

3. 运行脚本

```bash
python pydictor.py
```

 [旧版用法参考文档](https://github.com/LandGrey/pydictor/blob/3c855fecd0274205edef461096db6cdc4c008777/README_CN.md)

## 4 核心功能字典
---

#### 4.1 基础字典

```bash
python pydictor.py -base L --len 2 3 --encode b64
python pydictor.py -base dLc --len 1 3 -o /awesome/pwd
python pydictor.py -base d --len 4 4 --head Pa5sw0rd --output D:\exists\or\not\dict.txt
```

#### 4.2 自定义字符集字典

```bash
python pydictor.py -char "asdf123._@ " --len 1 3 --tail @site.com
```

#### 4.3 排列组合字典

```bash
python pydictor.py -chunk abc 123 "!@#" @ . _ " " --head a --tail @pass --encode md5
```

#### 4.4 语法引擎解析字典

```bash
python pydictor.py --conf              # 用默认的"/funcfg/build.conf"文件建立字典
python pydictor.py --conf /my/other/awesome.conf
python pydictor.py --conf "[0-9]{6,6}<none>[a-f,abc,123,!@#]{1,1}<none>" --encode md5 --output parsing.txt
```

#### 4.5 模式字典快速生成

```bash
# 注意: 
# 1. 使用 python3 会比 python2 更快
# 2. 仅一个元素支持单个字符，例如: [***]{1,1}<***>

# 生成模式: abc[%d][%d][%l][%d][%d][%l][%d][%d]

python3 pydictor.py --head abc --pattern "[0-9]{1,1}<none>[0-9]{1,1}<none>[a-z]{1,1}<none>[0-9]{1,1}<none>[0-9]{1,1}<none>[a-z]{1,1}<none>[0-9]{1,1}<none>[0-9]{1,1}<none>" -o output.txt
```

#### 4.6 规则扩展字典

```bash
python pydictor.py -extend bob --level 4 --len 4 12
python pydictor.py -extend liwei zwell.com --more --leet 0 1 2 11 21 --level 2 --len 6 16 --occur "<=10" ">0" "<=2" -o /possbile/wordlist.lst
```

#### 4.7 社会工程学字典

```bash
python pydictor.py --sedb
```

```
                              _ _      _
              _ __  _   _  __| (_) ___| |_ ___  _ __
             | '_ \| | | |/ _` | |/ __| __/ _ \| '__|
             | |_) | |_| | (_| | | (__| || (_) | |
             | .__/ \__, |\__,_|_|\___|\__\___/|_|
             |_|    |___/                         

                   Social Engineering Dictionary Builder
                                            Build by LandGrey
    ----------------------------[ command ]----------------------------
    [+]help desc             [+]exit/quit            [+]clear/cls
    [+]show option           [+]set option arguments [+]rm option
    [+]len minlen maxlen     [+]head prefix          [+]tail suffix
    [+]encode type           [+]occur L d s          [+]types L d s
    [+]regex string          [+]level code           [+]leet code
    [+]output directory      [+]run

    ----------------------------[ option ]----------------------------
    [+]cname                 [+]ename                [+]sname
    [+]birth                 [+]usedpwd              [+]phone
    [+]uphone                [+]hphone               [+]email
    [+]postcode              [+]nickname             [+]idcard
    [+]jobnum                [+]otherdate            [+]usedchar

pydictor SEDB>>
```

根据以下信息：

| information items        | value                 |
| :----------------------- | :-------------------- |
| chinese name             | 李伟                    |
| pinyin name              | liwei                 |
| simple name              | lw                    |
| simple name              | Lwei                  |
| english name             | zwell                 |
| birthday                 | 19880916              |
| used password            | liwei123456.          |
| used password            | liwei@19880916        |
| used password            | lw19880916_123        |
| used password            | abc123456             |
| phone number             | 18852006666           |
| used phone number        | 15500998080           |
| home phone               | 76500100              |
| company phone            | 010-61599000          |
| email account            | 33125500@qq.com       |
| email account            | 13561207878@163.com   |
| email account            | weiweili@gmail.com    |
| email account            | wei010wei@hotmail.com |
| home postcode            | 663321                |
| now place postcode       | 962210                |
| common nickname          | zlili                 |
| id card number           | 152726198809160571    |
| student id               | 20051230              |
| job number               | 100563                |
| father birthday          | 152726195910042816    |
| mother birthday          | 15222419621012476X    |
| boy/girl friend brithday | 152726198709063846    |
| friend brithday          | 152726198802083166    |
| pet name                 | tiger                 |
| crazy something          | games of thrones      |
| special meaning numbers  | 176003                |
| special meaning chars    | m0n5ter               |
| special meaning chars    | ppdog                 |

使用命令

```bash
python pydictor.py --sedb
set cname liwei
set sname lw Lwei
set ename zwell
set birth 19880916
set usedpwd liwei123456. liwei@19880916 lw19880916_123
set phone 18852006666
set uphone 15500998080
set hphone 76500100 61599000 01061599000
set email 33125500@qq.com
set email 13561207878@163.com
set email weiweili@gmail.com
set email wei010wei@hotmail.com
set postcode 663321 962210
set nickname zlili
set idcard 152726198809160571
set jobnum 20051230 100563
set otherdate 19591004 19621012
set otherdate 19870906 19880208
set usedchar tiger gof gamesthrones 176003 m0n5ter ppdog
```

查看当前配置然后生成字典

```
show
run
```

## 5 插件型字典(自己根据API文档开发)
---

#### 5.1 一段时间内生日字典

```bash
python pydictor.py -plug birthday 19800101 20001231 --len 6 8
```

#### 5.2 身份证后4/6/8位字典

```bash
python pydictor.py -plug pid4
python pydictor.py -plug pid6 --encode b64
python pydictor.py -plug pid8 --encode sha1 -o pid8.txt
```

#### 5.3 网页原始关键词字典

```bash
python pydictor.py -plug scratch  # 用/funcfg/scratch.sites 文件中的多行 url 作为输入
python pydictor.py -plug scratch http://www.example.com
```

## 6 内置工具(可自己根据API文档开发)
---

#### 6.1 字典合并工具

```bash
python pydictor.py -tool combiner /my/mess/dir
```

#### 6.2 字典比较工具

```bash
python pydictor.py -tool comparer big.txt small.txt
```

#### 6.3 词频统计工具

```bash
python pydictor.py -tool counter s huge.txt 1000
python pydictor.py -tool counter v /tmp/mess.txt 100
python pydictor.py -tool counter vs huge.txt 100 --encode url -o fre.txt
```

#### 6.4 字典处理工具

```bash
python pydictor.py -tool handler raw.txt --tail @awesome.com --encode md5
python pydictor.py -tool handler raw.txt --len 6 16 --occur "" "=6" "<0" --encode b64 -o ok.txt
```

#### 6.5 安全擦除字典工具

```bash
python pydictor.py -tool shredder        # 擦除当前输出目录下所有字典文件
python pydictor.py -tool shredder base 	 # 擦除当前输出目录下所有以"base"开头的字典文件
python pydictor.py -tool shredder /data/mess
python pydictor.py -tool shredder D:\mess\1.zip
```

#### 6.6 合并去重工具

```bash
python pydictor.py -tool uniqbiner /my/all/dict/
```

#### 6.7 字典去重工具

```bash
python pydictor.py -tool uniqifer /tmp/dicts.txt --output /tmp/uniq.txt
```

#### 6.8 多字典文件组合工具

```bash
python pydictor.py -tool hybrider heads.txt some_others.txt tails.txt
```

>使用pydictor有个窍门：时刻清楚你想要什么样子的字典

## 7 快速上手
---

### 7.1 pydictor可以生成的所有字典的类型及其说明

|   归属   |     类别     | 标识符 |       描述       |   支持功能代号    |
| :----: | :--------: | :-: | :------------: | :---------: |
|  core  |    base    | C1  |      基础字典      | F1 F2 F3 F4 |
|  core  |    char    | C2  |    自定义字符集字典    | F1 F2 F3 F4 |
|  core  |   chunk    | C3  |     排列组合字典     |     ALL     |
|  core  |    conf    | C4  |    配置语法生成字典    |     ALL     |
|  core  |  pattern   | C5  |    模式字典快速生成    |  F2 F3 F4   |
|  core  |   extend   | C6  |     规则扩展字典     |     ALL     |
|  core  |    sedb    | C7  |    社会工程学字典     |     ALL     |
|  tool  |  combiner  | T1  |     字典合并工具     |             |
|  tool  |  comparer  | T2  |    字典比较相减工具    |     ALL     |
|  tool  |  counter   | T3  |     词频统计工具     |     ALL     |
|  tool  |  handler   | T4  |   筛选处理原有字典工具   |     ALL     |
|  tool  | uniqbiner  | T5  |    先合并后去重工具    |     ALL     |
|  tool  |  uniqifer  | T6  |     字典去重工具     |     ALL     |
|  tool  |  hybrider  | T7  |   多字典文件组合工具    | F1 F2 F3 F4 |
|  tool  | printabler | T8  |   可打印字符过滤工具    |     ALL     |
| plugin |  birthday  | P1  |    生日日期字典插件    |     ALL     |
| plugin |    ftp     | P2  | 关键词生成ftp密码字典插件 |     ALL     |
| plugin |    pid4    | P3  |   身份证后四位字典插件   |     ALL     |
| plugin |    pid6    | P4  |   身份证后六位字典插件   |     ALL     |
| plugin |    pid8    | P5  |   身份证后八位字典插件   |     ALL     |
| plugin |  scratch   | P6  |  网页原始关键词字典插件   |     ALL     |

### 7.2 字典操作功能及说明对照表  

|   功能   | 功能代号 |          说明          |
| :----: | :--: | :------------------: |
|  len   |  F1  |        定义长度范围        |
|  head  |  F2  |         添加前缀         |
|  tail  |  F3  |         添加后缀         |
| encode |  F4  |      编码或自定义加密方法      |
| occur  |  F5  |  字母、数字、特殊字符出现次数范围筛选  |
| types  |  F6  |  字母、数字、特殊字符各种类数范围筛选  |
| regex  |  F7  |         正则筛选         |
| level  |  F8  |        字典级别筛选        |
|  leet  |  F9  |       1337 模式        |
| repeat | F10  | 字母、数字、特殊字符连续出现次数范围筛选 |

### 7.3 支持的编码或加密方式

|   方式   |           描述            |
| :----: | :---------------------: |
|  none  |      默认方式, 不进行任何编码      |
|  b16   |        base16 编码        |
|  b32   |        base32 编码        |
|  b64   |        base64 编码        |
|  des   |   des 算法, 需要根据情况修改代码    |
| execjs | 执行本地或远程js函数, 需要根据情况修改代码 |
|  hmac  |   hmac 算法, 需要根据情况修改代码   |
|  md5   |       md5 算法输出32位       |
| md516  |       md5 算法输出16位       |
|  rsa   |    rsa 算法 需要根据情况修改代码    |
|  sha1  |        sha-1 算法         |
| sha256 |       sha-256 算法        |
| sha512 |       sha-512 算法        |
|  url   |         url 编码          |
|  test  |      一个自定义编码方法的示例       |

## 8 功能模块
---

### 8.1 occur 功能

**用法：** `--occur [字母出现次数的范围] [数字出现次数的范围] [特殊字符出现次数的范围]`

```bash
--occur ">=4" "<6" "==0"
```

### 8.2 types 功能

**用法：** `--types [字母种类的范围] [数字种类的范围] [特殊字符种类的范围]`

```bash
--types "<=8" "<=4" "=0"
```

### 8.3 repeat 功能

**用法：**`--repeat [字母连续出现次数范围] [数字连续出现次数范围] [特殊字符连续出现次数范围]`

```bash
--repeat "<=3" ">=3" "==0"
```

### 8.4 regex 功能

**用法：**`--regex [正则表达式]`

```bash
--regex "^z.*?g$"
```

### 8.5 level 功能

**用法：**`--level [level]`

```
--level 4      /funcfg/extend.conf配置文件中level大于等于4的项目会被启用
```

### 8.6 leet功能

#### 8.6.1 默认置换表

`leet字符 = 替换字符，可以修改/funcfg/leet_mode.conf更改替换表`

```
a = 4
b = 6
e = 3
l = 1
i = 1
o = 0
s = 5
```

#### 8.6.2 模式代码

```
0               默认模式，全部替换
1               从左至右, 将第一个遇到的leet字符全部替换
2               从右至左, 将第一个遇到的leet字符全部替换
11-19           从左至右, 将第一个遇到的leet字符最多替换 code-10 个
21-29           从右至左, 将第一个遇到的leet字符最多替换 code-20 个
```

#### 8.6.3 代码作用表

|  代码  |      原字符串       |    被替换后的新字符串    |
| :--: | :-------------: | :-------------: |
|  0   | as a airs trees | 45 4 41r5 tr335 |
|  1   | as a airs trees | 4s 4 4irs trees |
|  2   | as a airs trees | a5 a air5 tree5 |
|  11  | as a airs trees | 4s a airs trees |
|  12  | as a airs trees | 4s 4 airs trees |
|  13  | as a airs trees | 4s 4 4irs trees |
|  14  | as a airs trees | 4s 4 4irs trees |
| ...  | as a airs trees | 4s 4 4irs trees |
|  21  | as a airs trees | as a airs tree5 |
|  22  | as a airs trees | as a air5 tree5 |
|  23  | as a airs trees | a5 a air5 tree5 |
|  24  | as a airs trees | a5 a air5 tree5 |
| ...  | as a airs trees | a5 a air5 tree5 |

## 9 pydictor API 开发者文档

### 9.1 插件

#### step 0

在`/plugins/`文件夹中创建一个`name.py`文件

#### step 1

导入以下模块并填写作者姓名

```python
#!/usr/bin/env python
# coding:utf-8
# author: LandGrey
"""
Copyright (c) 2016-2017 LandGrey (https://github.com/LandGrey/pydictor)
License: GNU GENERAL PUBLIC LICENSE Version 3
"""

from __future__ import unicode_literals

from lib.fun.fun import cool
from lib.fun.decorator import magic
from lib.data.data import pyoptions
```

#### step 2

定义`name_magic(args)`函数，并编写函数使用说明，获取参数值：

```python
def name_magic(*args):
    """[keyword1] [keyword2] ..."""
    args = list(args[0])
```

#### step 3

处理用户输入的异常参数：

```python
if len(args) == 1:
    exit(pyoptions.CRLF + cool.fuchsia("[!] Usage: {} {}".format(args[0], pyoptions.plugins_info.get(args[0]))))
```

#### step 4

使用`magic`装饰器，修改`name()`函数：

```python
@magic
def name():
```

#### step 5

在`name()`函数中，生成你的单词列表（Python列表类型）或者返回这些值：

```python
results = []

append something to results ...

retrun results
```

or

```python
results is python generator

for r in results:
    yield r
```

如果你想将自己创建的弱密码词库添加到最终的词库中，你可以将词库文件（在`/lib/data/data.py` 脚本中定义)放入以下一些文件夹中。

|       路径       |         变量          |
| :------------: | :-----------------: |
|   /wordlist    | paths.wordlist_path |
| /wordlist/App  | paths.applist_path  |
| /wordlist/IoT  | paths.iotlist_path  |
| /wordlist/NiP  | paths.niplist_path  |
| /wordlist/SEDB | paths.sedblist_path |
| /wordlist/Sys  | paths.syslist_path  |
| /wordlist/Web  | paths.weblist_path  |
| /wordlist/WiFi | paths.wifilist_path |

使用方法如下：
```
from lib.data.data import paths
from lib.fun.fun import walks_all_files

@magic
def name():
    for _ in walks_all_files(paths.weblist_path):
        yield "".join(_)
```

现在，你的脚本在Pydictor中支持了所有手部操作功能。

如果这是`/plugins/ftp.py`这个脚本，并且其名称必须为`ftp`，如下一个简单的示例：

```python
#!/usr/bin/env python
# coding:utf-8
# author: LandGrey
"""
Copyright (c) 2016-2017 LandGrey (https://github.com/LandGrey/pydictor)
License: GNU GENERAL PUBLIC LICENSE Version 3
"""

from __future__ import unicode_literals

from lib.fun.fun import cool
from lib.fun.decorator import magic
from lib.data.data import pyoptions


def ftp_magic(*args):
    """[keyword1] [keyword2] ..."""
    args = list(args[0])
    if len(args) == 1:
        exit(pyoptions.CRLF + cool.fuchsia("[!] Usage: {} {}".format(args[0], pyoptions.plugins_info.get(args[0]))))

    @magic
    def ftp():
        results = []
        default_password = ('ftp', 'anonymous', 'any@', 'craftpw', 'xbox', 'r@p8p0r+', 'pass', 'admin',
                            'lampp', 'password', 'Exabyte', 'pbxk1064', 'kilo1987', 'help1954', 'tuxalize')
        results += default_password
        weak_password = ('root', '123456', '111111', '666666', 'ftppass')
        results += weak_password
        for r in results:
            yield r

        tails = ['1', '01', '001', '123', 'abc', '!@#', '!QAZ', '1q2w3e', '!@#$', '!', '#', '.', '@123',
                 '2016', '2017', '2018', '@2016', '@2017', '@2018', ]
        for keyword in args:
            for tail in tails:
                yield keyword + tail
```

调用FTP插件，使用命令`python pydictor.py -plug ftp \[keyword1\] \[keyword2\] ...`

### 9.2 tool

与插件API大体相同，但有以下不同：

```
将脚本放入"/tools/"文件夹中
```

使用命令`python pydictor.py -tool name \[some_args\]`来调用该工具

### 9.3 encode

只需将Python脚本写在`/lib/encode/`文件夹中即可

```
1. 脚本文件名以 "_encode" 结尾, 例如 "name_encode.py"
2. 函数名必须与脚本文件名相同，例如"def name_encode(item)"
3. 函数"name_encode(item)"下一行必须是函数使用说明并进行换行，带着""
4. 编码完成后返回所编码的项目
```

**"base64"编码函数的一个实例**

在`/lib/encode/`文件夹中创建一个名为`b64_encode.py`的文件，并编写代码： 

```python
#!/usr/bin/env python
# coding:utf-8
#
"""
Copyright (c) 2016-2019 LandGrey (https://github.com/LandGrey/pydictor)
License: GNU GENERAL PUBLIC LICENSE Version 3
"""
from __future__ import unicode_literals

from base64 import b64encode


def b64_encode(item):
    """base64 encode"""
    try:
        return (b64encode(item.encode('utf-8'))).decode()
    except:
        return ''
```

使用命令`python pydictor.py --encode name`调用编码函数
