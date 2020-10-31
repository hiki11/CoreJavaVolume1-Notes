# Java核心技术 卷1 笔记

### 3.6字符串

从概念上讲，Java字符串就是Unicode字符序列。

1、子串：substring(0, 3)，表示从0（包括0）开始到3（但不包括3）。

2、拼接：用+号

3、不可变字符串：由于不能修改Java字符串中的字符，所以称之为不可变字符串。正如数字3永远是数字3，字符串“Hello”永远包含字符H、e、l、l和o的代码单元序列，而不能修改其中的任何一个字符。当然，可以修改字符串变量，让它引用另一个字符串，如同可以把存放3的数值变量改为存放4一样。

这样修改字符串效率确实不高，但特点是编辑器可以让字符串**共享**。

4、检测字符串是否相等：equals()；equalsIgnoreCase()可以忽略大小写。一定不能使用==运算符，它只确定是否放置在同一个位置上。substring和+产生的结果并不是共享的。也可以使用compareTo进行比较，类似于C语言的strcmp

5、空串是""，长度为0内容为空。和Null串不同，Null表示没有任何对象与该变量关联。

6、代码点与代码单元：Java字符串由char序列组成，而char数据类型是一个采用UTF-16编码表示Unicode代码点的代码单元。大多数的常用Unicode字符用一个代码单元就能表示，二辅助字符需要一对代码单元表示。

length方法可以返回采用UTF-16编码表示的给定字符串所需要的代码单元数量。

7、字符串API

8、阅读联机API文档：浏览器指向安装JDK的docs/api/index.html子目录，可以打开API文档

9、构建字符串使用StringBuilder类。这个类是JDK5.0引用的，前身是StringBuffer。StringBuilder单线程效率高，StringBuffer多线程效率较之低。常用方法：builder.append()构建，builder.toString()转换为字符串，insert()方法插入字符串或代码单元Char，delete()删除一段代码单元，setCharAt修改某个代码单元。

### 3.7输入输出

1、读取输入

```java
Scanner in = new Scanner(System.in);
System.out.print("What is your name? ");
String name = in.nextLine();
```

读取单词用next（空格为分隔符），读取行nextLine，读取整数nextInt，浮点数是nextDouble。hasNext(), hasNextInt()...是否有下一个。

Scanner类定义在java.util包。当使用的类不是定义在基本java.lang包中时，一定要用import指示字把包加载进来。

注释：因为输入是可见的，所以Scanner类不适合用于读取密码。可以使用Console类实现。

```java
Console cons = System.console();
String username = cons.readLine("User name: ");
char[] passwd = cons.readPassword("Password: ");
```

为了安全起见，返回的密码存放在一位字符数组中，而不是字符串中。在对密码进行处理之后，应该马上用一个填充值覆盖数组元素。

2、格式化输出

Java SE 5.0 沿用了 C语言库函数中的printf方法，进行格式化。例如`System.out.printf("%8.2f",x);`可以用8个字符的宽度和小数点后两个字符的精度打印x。

参数索引值是从1开始的，比如%1$...对第1个参数格式化。

![img](/Users/michael/Documents/Notes/Java核心技术笔记/image/屏幕快照 2020-10-31 下午9.24.54.png)

3、文件输入与输出

对文件进行读取，需要用File对象构造一个Scanner对象，如：`Scanner in = new Scanner(Paths.get("myfile.txt"));`然后用之前的Scanner方法读取。

如果文件名包含反斜杠，需要再加一个反斜杠，如"c:\\\mydirectory\\\myfile.txt"。也可以使用相对路径。

需要写入文件则构造一个PrintWriter对象`PrintWriter out = new PrintWriter("myfile.txt");`。如果文件不存在，则创建该文件。

警告：如果用的是字符串参数`Scanner in = new Scanner("myfile.txt");`，那么Scanner不会把字符串当做文件名，而是作为数据：'m', 'y', 'f' ...。

注释：如果可能读取一个不存在的文件，那么需要告知编辑器，抛出“找不到文件”的异常`throws FileNotFoundException`。

