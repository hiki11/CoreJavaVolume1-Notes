# Java核心技术 卷1 笔记

## 第3章 Java的基本程序

### 3.3数据类型

Java是一种强类型语句。4种整型、2种浮点类型、1中表示Unicode码的字符单元的字符类型char和1种boolean类型。Java没有任何无符号类型（unsigned），并且数据范围固定。长整型后缀L，float后缀F，不加则为默认类型。

2、浮点数：不适用于禁止出现舍入误差的计算中，应该使用BigDecimal类。

3、char类型：在Java中，插入类型用UTF-16编码描述一个代码单元。强烈建议不要在程序中使用char类型，除非确实需要对UTF-16代码单元进行操作。最好将需要处理的字符串用抽象数据类型表示。

4.2、常量：在Java中，利用关键字final指示常量。一旦赋值就不能更改了。习惯上常量名用全大写。

希望某个常量可以再一个类中的多个方法中使用，即类常量。可以使用static final设置一个类常量。类常量定义于main方法的外部。

const是Java保留的关键字，但目前并没有使用。在Java中，必须使用final定义常量。

5、运算一般是截断计算，可以用strictfp关键字标记为严格的浮点计算。

5.3、位运算：&与、|或、^异或、~非；>>二进制位右移、<<二进制位左移、>>>用0填充高位，没有<<<。对于移位运算符，右侧参数要进行模32运算，若左侧操作数为long类型，则模64。

5.4、数学函数与常量：平方根`Math.sqrt(x);`，sqrt方法处理的不是对象，称之为静态方法。幂运算`Math.pow(x,a);`，参数和返回值都是double类型。

常用方法：三角函数，exp、log、log10，PI、E。

提示：可以`import static java.lang.Math.*;`，就不需要加前缀Math.了。

注释：在Math类中，为了达到高性能，所有方法都使用计算机浮点单元的例程。如果想得到一个完全可预测的结果，就使用StrictMath类。

5.5、类型的转换

![img](/Users/michael/Documents/Notes/Java核心技术笔记/image/屏幕快照 2020-11-01 下午3.33.23.png)

虚箭头表示可能会有精度的损失。二元操作时，会转换操作数为同一类型。

5.6、强制类型转换：(cast)，可能损失精度。摄入运算可以用Math.round方法。

警告：若把一个数强转另一个类型，而超出了另一个类型的表示范围，会被截断为完全不同的值。

5.7、括号与运算符级别：&&的优先级高于||；+=是右结合符号，从右向左计算。

5.8、枚举类型：也就是一个有限的集合。枚举类型包括有限个命名的值。例如：`enum Size { SMALL, MEDIUM, LARGE, EXTRA_LARGE};`。然后可以声明该类型的变量：`Size s = Size.MEDIUM;`。

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

### 3.8控制流程

1、块作用域：块（block）：一对花括号括起来的若干条简单的Java语句。块确定了变量的作用域。不能在嵌套的两个块中声明同名的变量。

4、确定循环（也就是for）：不成文的规定：for语句的3个部分应该对同一个计数器变量进行初始化、检测和更新。若不遵守这一规定，编写的循环常常晦涩难懂。

警告：在循环中，检测两个浮点数是否相等需要格外小心，如`for (double x = 10; i > 0; i--)`。由于0.1无法精确地用二进制表示，所以x会从9.99999....98跳到10.99999....98。

当在for语句第一部分声明了一个变量之后，这个变量的作用域就是for循环的循环体，而不能用于循环体外。若要在循环体外使用，那需要在外声明，如：

```java
int i;
for (i = 1; i <= 10; i++)
```

5、多重选择：switch语句。switch语句将从与选项值相匹配的case标签处开始执行直到遇到break语句，或者执行到switch语句的结束处位置。如果没有相匹配的case标签，而有default子句，就执行这个子句。

警告：有可能出发多个case分支。如果case分支语句的末尾没有break语句，那么就会接着执行。这种情况相当危险，常常会引发错误。为此，我们在程序中从不使用switch语句。（有优化方案）

case标签可以是：基本类型和封装类、枚举常量、字符串字面量（Java SE 7 开始）。

```java
switch (choice) {
	case 1:
		...
		break;
  case 2:...
  default:
  	...
  	break;
}
```



6、终端控制流程语句：标签break。用`xxx:`标记，然后就可以break xxx跳出，可以用于跳出多层循环。同样有标记continue。

### 3.9大数值

java.math的两个类：BigInteger和BigDecimal，可以处理任何长度数字序列的数值。BigInteger类实现了人已经堵的整数运算，BigDecimal实现了任意精度的浮点数运算。使用valueOf可以将普通数值转换为大数值。不能使用算术运算符处理大数值，而是用add和multiply方法，除此之外还有不少其他的API。

### 3.10数组

创建一个数组，数字数组会初始化为0，boolean初始化为false，对象数组初始化null。

1、foreach循环：foreach的collection必须是数组或者是实现了Iterator接口的类对象（例如ArrayList）。

打印数组：Arrays.toString();

2、数组初始化以及匿名数组

匿名数组初始化：`new int[] {17, 18, 23 ,29, 31 ,37}`，数组的大小就是初始值的个数。使用这种方法可以再不创建新变量的情况下重新初始化一个数组。

数组长度可以为0，但数组并不是null。

3、数组拷贝：`Arrays.copyOf(arr, length);`

4、命令行参数：main的String[] args，通过`java Message -g cruel world`运行程序时（程序名为Message），args[0]为-g，arg[1]为cruel，arg[2]为world。

5、