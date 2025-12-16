
1. 用于定义接口的关键字是 `interface`，用于实现接口的关键字是 `implements`.
2. 用于定义抽象类的关键字是 `abstract`，用于继承父类的关键字是 `extends`.

3. 下面代码中的m最后的运行结果为 `55`.

```java
int  a=10, b=3, m=5;
if( a==b )
   m+=a;
else
   m=++a*m; // 先算++a再算a*m
```

4. `static` 可以修饰成员变量，但**不能**修饰局部变量。
5. 关于数组作为方法的参数时，向方法传递的是**数组的引用**。
6. Java语言与平台无关，Java程序的运行结果与操作系统无关（宏观的角度来讲）。

7. 下面代码的运行结果是 `11-1`.

```java
int i=0, j=10;  
for(;i<=10;i++) {  
    j--;  
}  
System.out.println(i +""+ j);
```

8. 下列代码的运行结果是 `c`.

```java
Boolean b1 = new Boolean(true);  
Boolean b2 = new Boolean(true);  
Object obj1 = (Object)b1;  
Object obj2 = (Object)b2;  
if (obj1 == obj2)  
    if (obj1.equals(obj2))  
        System.out.println("a");  
    else  
        System.out.println("b");  
else   
if (obj1.equals(obj2))  
        System.out.println("c");  
    else  
        System.out.println("d");
```

>[!warning] 注意
>这道题通常考察的是if匹配，会将代码调整成以下方式：
>```java
>Boolean b1 = new Boolean(true);  
>Boolean b2 = new Boolean(true);  
>Object obj1 = (Object)b1;  
>Object obj2 = (Object)b2;  
>if (obj1 == obj2)  
>if (obj1.equals(obj2))  
>    System.out.println("a");  
>else  
  >  System.out.println("b");  
>else  
>if (obj1.equals(obj2))  
>    System.out.println("c");  
>else  
>    System.out.println("d");
>```

9. 在Java中 `javac` 用于编译源文件(.java)，而 `java` 用于执行编译后的字节码文件(.class)。
10. Java类中的属性可以是**简单变量**，也可以是一个**对象**。
11. 安装好JDK后，在它的bin目录下有许多exe可执行文件，其中`java.exe`命令的作用是**Java启动器**。
12. 在 Java 体系中，**JVM（Java 虚拟机）**、**JRE（Java 运行环境）**、**JDK（Java 开发工具包）** 是层层包含、各司其职的关系，核心逻辑是：`JVM < JRE < JDK`（JDK 包含 JRE，JRE 包含 JVM）。
13. 命令行参数个数异常 `ArrayIndexOutOfBoundException`.
14. 除数为零时异常 `ArithmeticException`.
15. 