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

