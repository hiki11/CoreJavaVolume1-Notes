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

5、数组排序：Arrays.sort()。

​	API：binarySearch(v)【二分法查找值v】；fill(v)【填充数组元素值为v】；equals(type[] a, type[] b)【数组大小相同，下标相同的元素都对应相等，返回true】

6、二维数组：打印二维数组，可以用`Arrays.deepToString();`

7、不规则数组：初始化。

```java
int[][] adds = new int[NMAX + 1][];
for (int n = 0; n <= NMAX; n++) {
	adds[n] = new int[n + 1];
}
```

## 第4章 对象与类

1、面向对象程序设计（OOP），Java是完全面向对象的。

传统的结构化设计：算法+数据结构=程序，数据结构是第一位，算法第二位。

OOP：数据结构是第一位，算法第二位。OOP适合解决规模较大的问题。

1.1、类（class）：构造对象的模板或蓝图。由类构造（construct）对象的过程称为创建类的实例（instance）。

封装（encapsulation）：将数据和行为组合在一个包，并对对象的使用者隐藏了数据的实现方式。对象中的数据称为实例域（instance field），操纵数据的过程称为方法（method）。

实现封装的关键在于绝对不能让类中的方法直接地访问其他类的实例域。程序仅通过对象的方法与对象数据进行交互。是提高重用性和可靠性的关键。

所有的类都源自一个“神通广大的超类”即Object。在扩展一个已有的类时，这个扩展后的新类具有所扩展的类的全部属性和方法。在新类中，只需提供适用于这个新类的新方法和数据域就可以了。通过扩展一个类建立另一个类的过程称为继承（inheritance）。

1.2、对象：

对象的三个主要特性：

- 对象的行为——可以对对象施加哪些操作，或可以对对象施加哪些方法？
- 对象的状态——当施加那些方法时，对象如何响应？
- 对象表示——如何辨别具有相同行为与状态的不同对象？

对象的特性是相互影响的。比如，状态影响它的行为。（一个订单已送货，那就拒绝条用增删订单条目的方法）。

1.3、识别类：首先从设计类开始，然后再往每个类中添加方法。（学习OOP的方式）

识别类的简单规则是在分析问题的过程中寻找名词，而方法对应着动词。

例如，在订单处理系统中，有着一些名词：项目（Item）、订单（Order）、送货地址（Shipping address）、付款（Payment）、账户（Account），     这些名词很可能成为类Item、Order等

接下来，查看动词：物品项目被添加到订单中，订单被发送或取消。订单货款被支付。对于每个动词：“添加”、“发送”、“取消”以及“支付”，都要标识出主要负责完成动作的对象。例如，当一个新的条目添加到订单中，那个订单对象就是被指定的对象，因为它知道如何存储条目以及对条目进行排序。也就是说，add应该是Order类的一个方法。

1.4、类之间的关系：依赖（“uses-a”）、聚合（“has-a”）、继承（“is-a”）

依赖（dependence），是最明显、常见的关系。例如，Order类使用Account类是因为Order对象需要访问Account对象查看信用状态。一个类的方法操纵另一个类的对象，就是一个类依赖与另一个类。应该尽可能减少相互依赖的类，即降低耦合度。

聚合（aggregation），也可以说是关联（关联的UML符号不易区分）。例如，一个Order对象包含一些Item对象。聚合关系意味着类A的对象包含类B的对象。

![img](/Users/michael/Documents/Notes/Java核心技术笔记/image/屏幕快照 2020-11-06 上午11.40.16.png)

继承（inheritance），用于表示特殊与一般关系。例如，RushOrder类由Order类继承而来。在具有特殊性的RushOrder类中包含一些特殊方法，其他一般方法由Order继承而来。

通过UML（Unified Modeling Language，统一建模语言）来描述类之间的关系。

2、使用预定义类：并不是所有类都具有面向对象特征。如Math.random。

2.1、对象与对象变量：想要使用对象，就必须先构造对象，并指定其初始化状态。然后，对对象应用方法。

Java中使用构造器（Constructor）构造新实例。构造器的名字应该与类名相同，是一种特殊的方法，比如Date类的构造器名为Date。要构造一个对象，需要在构造器前面加上new操作符`new Date()`。构造的对象可以存放在一个变量中`Date birthday = new Date();`。

对象与对象变量存在一个重要的区别，例如：`Date deadlin`只是一个对象变量，可以引用Date类型的对象，但它不是一个对象，实际上也没有引用。此时不能讲任何Date方法应用在这个变量上。

一定要认识到：一个对象变量并没有实际包含一个对象，仅仅是引用一个对象。

在Java中，任何对象变量的值都是对存储在另外一个地方的一个对象的引用。new操作符的返回值也是一个引用。如：`Date deadline = new Date();`有两个部分。表达式new Date()构造了一个Date类型的对象，并且它的值是对新创建对象的引用。这个引用存储在变量deadline中。

可以显式地将对象变量设置为null，表名这个对象变量目前没有引用任何对象。局部变量不会自动初始化为null，必需调用new或设置为null来初始化。

所有的Java对象都存储在堆中。当一个对象包含另一个对象变量时，这个变量依然包含着指向另一个堆对象的指针。使用一个未初始化的指针，运行系统会产生一个运行时错误。

2.2、Java类库中的GregorianCalendar类：标准Java类库分别包含了两个类：一个是用来表示时间点的Date类；另一个用来表示大家熟悉的日历表示法的GregorianCalendar类，GregorianCalendar类扩展了一个更加通用的Calendar类，这个类描述了日历的一般属性。

将时间和日历分开是一中很好的面向对象设计。通常，最好使用不同的类表示不同的概念。Date类只提供了少量的方法用来比较两个时间点，例如before和after方法表示是否早于或晚于一个时间点。

使用new GregorianCalendar()构建新对象，还可以通过提供年月日表示日历对象，怪异的事，月份从0开始计数。

2.3、更改器方法与访问器方法：...

3、用户自定义类：学习设计复杂应用所需要的主力类（workhorse class）。通常这些类没有main方法，却又自己的实例域和实例方法。想要创建一个完整的程序，应该将若干类组合在一起，其中只有一个类有main方法。

3.1、Employee类

在Java中，最简单的类定义形式为：

```java
class ClassName {
  field1
  field2
  ...
  constructor1
  constructor2
  ...
  method1
  method2
  ...
}
```

一个简单的Employee类：

```java
class Employee {
  // instance fields
  private String name;
  private double salary;
  private Date hireDay;
  
  // constructor
  public Employee(String n, double s, int year, int month, int day) {
    name = n;
    salary = s;
    GregorianCalendar calendar = new GregorianCalendar(year, month - 1, day);
  }
      
  // a method
  public String getName() {
    return name;
  }

  // more method
  ...
}
```

3.2、多个源文件的使用：许多程序员习惯于将每一个类存在一个单独的源文件中。编译EmployeeTest.java，其使用了Employee类，那么Java编译器就会自动搜索Employee.java进行编译。并且Employee.java若版本较已有Employee.java版本新，那么Java编译器会自动重新编译这个文件。

3.3、剖析Employee类：方法可以有4中访问级别，实例（name、salary...）是private关键字，确保只有类自身的方法才能访问这些实例域，其他类的方法不能够读写这些域。（并不建议public实例域，这会破坏封装，任何类的任何方法都可以修改public域）

3.4、构造器：构造器与类同名，在构造 类的对象时，构造器会运行，以便将实例域初始化为所希望的状态。构造器与其他的方法有一个重要的不同，构造器总是伴随着new操作符的执行被调用，而不能对一个已经存在的对象调用构造器来达到重新设置实例域的目的，这个将会产生编译错误。

构造器的主要点：①构造器与类同名②每个类可以有一个以上的构造器③构造器可以有0个、1个或多个参数④构造器没有返回值⑤构造器总是伴随着new操作一起调用

警告：不要在构造器中定义与实例域重名的局部变量（这些变量会屏蔽同名的实例域）。

3.5 隐式参数与显式参数：salary称为隐式（implicit）参数，byPercent称为显式（explicit）参数，位于方法名后面括号中的数值，并且有方法声明。

关键字this表示隐式参数，隐式参数没有出现在方法声明中。

```java
public void raiseSalary(double byPercent) {
  double raise = this.salary * byPercent / 100;
  this.salary += raise;
}
```

