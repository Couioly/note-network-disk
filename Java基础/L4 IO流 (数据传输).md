
## 1 内容概述

**流**：Java中将**输入设备**和**输出设备**之间的数据传输抽象表述为**流**。Java的流都位于`java.io`包中，称为IO(输入输出)流。

#### IO流分类

根据**操作数据**：
- 字节流
- 字符流

根据**传输方向**：
- 输入流
- 输出流

## 2 文件管理

### 2.1 File类

**File类**是java中用于对磁盘上的文件和目录的抽象表示形式的类。通过实例化File对象，可以对文件或目录进行一些基本的操作，如**创建、删除、重命名、判断是否存在**等。

#### 创建File类对象

**File类的构造方法**

- `File(String pathname)`： 通过指定的字符串类型的文件路径创建File对象
- `File(String parent,String child)`：通过指定的字符串类型的父路径和子路径创建File对象
- `File(File parent, String child)`：通过指定的File类的父路径和字符串类型的子路径创建File对象

要理解这三种 `File` 类构造方法的使用，我们可以通过具体例子来逐一讲解：

1. `File(String pathname)` 示例

这种构造方法是直接传入**完整的文件或目录路径字符串**来创建 `File` 对象。

例如，假设我们要操作电脑上的一个文件 `test.txt`，它位于 `D:\java\files` 目录下，代码可以这样写：

```java
import java.io.File;

public class FileDemo {
    public static void main(String[] args) {
        // 传入完整路径字符串，创建File对象
        File file = new File("D:\\java\\files\\test.txt");
        // 后续可以通过这个file对象做很多操作，比如判断文件是否存在、获取文件名等
        System.out.println("文件是否存在：" + file.exists());
        System.out.println("文件名：" + file.getName());
    }
}
```

2. `File(String parent, String child)` 示例

这种构造方法是将**父路径**和**子路径**分别作为字符串传入，由这两部分拼接成完整路径来创建 `File` 对象。

还是以 `D:\java\files\test.txt` 为例，我们可以把父路径 `D:\java\files` 和子路径 `test.txt` 分开传入：

```java
import java.io.File;

public class FileDemo {
    public static void main(String[] args) {
        String parent = "D:\\java\\files"; // 父路径字符串
        String child = "test.txt";        // 子路径字符串
        File file = new File(parent, child);
        System.out.println("完整路径：" + file.getPath());
        System.out.println("文件是否存在：" + file.exists());
    }
}
```

3. `File(File parent, String child)` 示例

这种构造方法是先创建一个**表示父路径的 `File` 对象**，再传入子路径字符串，从而拼接成完整路径。

同样针对 `D:\java\files\test.txt`，代码如下：

```java
import java.io.File;

public class FileDemo {
    public static void main(String[] args) {
        File parentFile = new File("D:\\java\\files"); // 先创建父路径的File对象
        String child = "test.txt";
        File file = new File(parentFile, child);
        System.out.println("完整路径：" + file.getPath());
        System.out.println("文件大小（字节）：" + file.length());
    }
}
```

通过这三个例子可以看出，三种构造方法的核心都是为了确定**文件或目录的具体路径**，只是路径的拆分和传递方式不同，开发者可以根据场景选择最方便的方式来创建 `File`对象。

#### File类的常用方法

- `boolean exists()`：判断File对象对应的文件或目录**是否存在**
- `boolean isFile()`：判断File对象对应的**是否是文件**而非目录（当文件不存在时也为false
- `boolean isDirectory()`：判断File对象对应的**是否是目录**而非文件（当目录不存在时也为false
- `boolean isAbsolute()`：判断File对象对应的文件或目录**是否是绝对路径**
- `boolean canRead()`：判断File对象对应的文件**是否可以读取**
- `boolean canWrite()`：判断File对象对应的文件**是否可以修改**
- `String getName()`：返回File对象的**文件或目录的名称**
- `long length()`：返回文件内容的**长度**（单位为字节）
- `long lastModified()`：返回1970-1-1的0时0分0秒到**文件最后修改时间的毫秒值**
- `String getPath()`：返回File对象对应的**路径名字符串**
- `String getAbsolutePath()`：返回File对象对应的**绝对路径**(注意区别操作系统)
- `String getParentFile()`：返回File对象对应的**目录的父目录**(不包含最后一级)
- `boolean delete()`：对当前的文件**进行删除操作并返回删除状态**

#### 遍历目录下的文件

- `String[] list()`：获取当前目录下所有的一级目录名称和文件名称到一个字符串数组中
- `File[] listFiles()`：获取当前目录下所有的一级目录名称和文件对象到一个文件对象数组中

**`String[] list()`示例**

```java
public void file(){  
    File dir = new File("D:\\CodeFile\\...");  
    // 调用list()方法，获取当前目录下所有一级目录和文件的名称字符串数组  
    String[] names = dir.list();  
    if (names != null) {  
        for (String name : names) {  
            System.out.println(name);  
        }  
    }  
}
```

为了实现能够获取符合指定条件的文件，File类提供了一个重载的list(FilenameFilter)方法，该方法接收一个FilenameFilter类型的参数，用于**过滤文件名**。

>[!warning] 提示
>FilenameFilter是一个接口，被称为**文件过滤器**，其中定义了一个抽象方法`accept(File file, String name)`，用于依次对指定File的所有子目录或文件进行迭代。

**`list(FilenameFilter filter)`方法的工作原理如下**：

1. 调用`list(FilenameFilter filter)`方法，传入`FilenameFilter`文件过滤器对象；
2. 遍历当前File对象所对应目录的所有子目录和文件；
3. 把代表当前目录的File对象和子目录或文件的名称作为参数，调用`accept(File file,String name)`方法；
4. 若`accept(File file,String name)`方法返回`true`，就将当前子目录或文件添加入数组，否则不进行添加。
