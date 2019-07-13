<!-- GFM-TOC -->

- [Java 版本](#Java 版本)
- [名词解释](#名词解释)

<!-- GFM-TOC -->

# Java 简介

#### Java 版本

- Java SE：Standard Edition，标准版，包含标准的 JVM 和标准库，是整个Java平台的核心。

- Java EE：Enterprise Edition，企业版，在Java SE的基础上加上了大量的API和库，以便方便开发Web应用、数据库、消息服务等。

- Java ME：Micro Edition，针对嵌入式设备的“瘦身版”，其虚拟机和库都与 Java EE不同。

  ![1562896856294](E:\GitHub\CS-Learning\docs\java\pics\1562896856294.png)

#### 名词解释

- JDK：Java Development Kit，包含 JRE 和 Java 的开发工具（编译器、调试器等）。

- JRE：Java Runtime Environment，运行 Java 字节码的虚拟机。

  ![1562897337667](E:\GitHub\CS-Learning\docs\java\pics\1562897337667.png)

#### 安装 JDK

[Oracle 官网下载](https://www.oracle.com/technetwork/java/javase/downloads/index.html)

安装完成后配置环境变量和

##### 配置环境变量

`JAVA_HOME=%安装路径%\Java|jdk` 

JAVA_HOME 指向JDK的安装目录，Eclipse/Tomcat 等 JAVA 开发的软件通过搜索 JAVA_HOME 变量来找到并使用安装好的 JDK。

`PATH=%JAVA_HOME%\bin`

PATH 指向搜索命令路径，bin 路径下包含了常用的可执行文件，如 javac/ java 等。

`CLASSPATH:.;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar `

CLASSPATH指向类搜索路径，与import, package 关键字有关。

如配置成功，输入命令`java -version`，会得到如下输出：

```
$ java -version
java version "11.0.2" 2019-01-15 LTS
Java(TM) SE Runtime Environment 18.9 (build 11.0.2+9-LTS)
Java HotSpot(TM) 64-Bit Server VM 18.9 (build 11.0.2+9-LTS, mixed mode)
```

PATH 下的常用可执行文件

- javac：这是 Java 的**编译器**，它用于把 Java 源码文件（以`.java`后缀结尾）编译为 Java 字节码文件（以 `.class` 后缀结尾）；
- java：启动 JVM，即运行 Java 程序，让 JVM 执行指定 Java 字节码文件；
- jar：用于把一组 `.class `文件打包成一个 `.jar` 文件，便于发布；
- javadoc：用于从Java源码中自动提取注释并生成文档；
- jdb：Java 调试器，用于开发阶段的运行调试。

##### 运行 Java 程序

1. 在文本编辑器中输入 Java 代码，保存为 `类名.java` 的格式

2. 使用 `javac` 可以将 `.java` 源码编译成 `.class`字节码；

   ```
   $ javac Hello.java
   ```

3. 使用 `java` 运行一个已编译的 Java 程序，参数是类名。

   ```
   $ java Hello
   Hello, world!
   ```

##### IDE

IDE是集成开发环境 (Integrated Development Environment) 的缩写，可以把编写代码、组织项目、编译、运行、调试等放到一个环境中运行，能极大地提高开发效率。

这里使用 Eclipse 。

### Java 程序基础

#### 基本类型



#### 引用类型

###### 字符和字符串

Java的字符类型 `char` 是基本类型，字符串类型 `String` 是引用类型；

基本类型的变量是“持有”某个数值，引用类型的变量是“指向”某个对象；

引用类型的变量可以是空值 `null`；

空字符串是一个有效的字符串对象，它不等于 `null` 。

###### 字符串不可变

```java
public class Main {
    public static void main(String[] args) {
        String s = "hello";//字符串变量 s 指向"hello"
        String t = s;//t = "hello"
        s = "world";//s = "world"
        System.out.println(t); // t = "hello"
    }
}
```



##### 数组

数组一旦创建后，大小就不可变；数组元素可以是值类型（如int）或引用类型（如String），但数组本身是引用类型；

```java
//数组元素为值类型
ns = new int[] { 68, 79, 91, 85, 62 };
```

![1562911998948](E:\GitHub\CS-Learning\docs\java\pics\1562911998948.png)

```java
//数组元素为引用类型
String[] names = {
    "ABC", "XYZ", "zoo"
};
```

![1562912100211](E:\GitHub\CS-Learning\docs\java\pics\1562912100211.png)

<div align="center"> <img src="https://dreamwhigh.github.io/CS-Learning/#/java/pics/1562912100211.png" width="600"/> </div><br>

<div align="center"> <img src="https://gitee.com/CyC2018/CS-Notes/raw/master/docs/pics/474e5579-38b1-47d2-8f76-a13ae086b039.jpg"/> </div><br>

#### **关键字**

- var 用于省略变量类型

  ```java
  var sb = new StringBuilder();
  //编译器会自动判断为下面的语句
  StringBuilder sb = new StringBuilder();
  ```

  

#### 作用域



#### 整数运算

##### 溢出

整数计算结果超出了范围，就会产生溢出，而溢出**不会出错**，但会得到一些奇怪的答案。

这里需要了解[反码、补码](<https://www.cnblogs.com/flowerslip/p/5933833.html>)

正数：补码 = 反码 =原码

负数：反码 = 原码（符号位不变）其余位取反

​	    补码 = 反码 + 1

最小值的特殊情况：

以 byte 为例，一个字节，8 bit，取值范围是 -128 ~ 127，但是**-128只有补码** **（1000 0000）**

##### 运算符号

###### 自增/自减

`++n` 表示先加 1 再引用 n，`n++` 表示先引用 n 再加 1。

###### 移位运算

<< : 左移运算符，符号位不动，num << 1,相当于 num 乘以2

\>> : 右移运算符，符号位不动，num >> 1,相当于 num 除以2

\>>> : 无符号右移，忽略符号位，空位都以0补齐

[移位运算比乘除运算的性能更高](<https://blog.csdn.net/zhou_zhou_gogo1/article/details/84246328>)，故与 2^k^ 的乘除法运算可以转换为移位运算。

除法运算向 0 取整，右移运算向 下取整，如下所示，对于正数 a，所得结果一样；对于负数 b，结果是不同的。

```java
    public static void main(String[] args) {
        int a = 7;
        int b = -7; 
        int a1 = a >> 1;
        int b1 = b >> 1;
        System.out.println("a / 2 = " + a/2);//a / 2 = 3
        System.out.println("a >> 1 = " + a1 );//a >> 1 = 3
        System.out.println("b / 2 = " + b/2);//b / 2 = -3
        System.out.println("b >> 1 = " + b1 );//b >> 1 = -4        
    }
```

###### 逻辑运算符

&（与），&&（短路与），|（或），||（短路或），^（异或）

布尔运算的一个重要特点是**短路运算**。如果一个布尔运算的表达式能提前确定结果，则后续的计算不再执行，直接返回结果。

因为`false && x`的结果总是`false`，无论`x`是`true`还是`false`，因此，与运算在确定第一个值为`false`后，不再继续计算，而是直接返回`false`。

##### 输出输入

###### 输出

`print` 表示输出后不换行；

`println` 是print line的缩写，表示输出并换行；

`printf`表示格式化输出，通过使用占位符`%?`，把后面的参数格式化成指定格式。

Eclipse 中输入 syso ,再按 alt + '/' 可快速输入 System.out.println( ) 。

| 占位符 | 说明                             |
| :----- | :------------------------------- |
| %d     | 格式化输出整数                   |
| %x     | 格式化输出十六进制整数           |
| %f     | 格式化输出浮点数                 |
| %e     | 格式化输出科学计数法表示的浮点数 |
| %s     | 格式化字符串                     |

###### 输入

[java中从键盘输入的三种方法](<https://blog.csdn.net/u012249177/article/details/49586383>)

1. System.in 和 System.out 方法
2. InputStreamReader 和 BufferedReader 方法
3. Scanner 类中的方法（首先要创建一个 Scanner 对象，再调用方法）

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in); // 创建Scanner对象
        String name = scanner.nextLine(); // 读取一行输入并获取字符串
        int age = scanner.nextInt(); // 读取一行输入并获取整数
    }
}
```

##### if 判断

###### 浮点数的判断

浮点数在计算机中常常无法精确表示，并且计算可能出现误差，因此，判断浮点数相等不能直接用 `==` 判断。正确的方法是利用差值小于某个临界值来判断：

```java
public class Main {
    public static void main(String[] args) {
        double x = 1 - 9.0 / 10;
        if (Math.abs(x - 0.1) < 0.00001) {
            System.out.println("x is 0.1");
        } else {
            System.out.println("x is NOT 0.1");
        }
    }
}
```

###### 判断引用类型相等

`==` 表示引用类型是否指向**同一个对象**。

`equals()` 方法判断引用类型的**变量内容**是否相等。

注意：执行语句 `s1.equals(s2)` 时，如果变量 `s1` 为 `nul ` ，会报 `NullPointerException` 。

```java
//正确的做法加上 s1 != null
public class Main {
    public static void main(String[] args) {
        String s1 = null;
        if (s1 != null && s1.equals("hello")) {
            System.out.println("hello");
        }
    }
}

```

##### switch

`case` 语句具有 ”穿透性“ ，漏写 `break` 会造成严重的逻辑错误，而且不易在源代码中发现错误。从 Java 12 开始，`switch` 语句升级为更简洁的表达式语法，使用类似模式匹配（Pattern Matching）的方法，保证只有一种路径会被执行，没有穿透效应，故不需要 `break` 语句：

```java
public class Main {
    public static void main(String[] args) {
        String fruit = "apple";
        switch (fruit) {
        case "apple" -> System.out.println("Selected apple");
        case "pear" -> System.out.println("Selected pear");
        case "mango" -> {
            System.out.println("Selected mango");
            System.out.println("Good choice!");
        }//多条语句用 {} 括起来
        default -> System.out.println("No fruit selected");
        }
    }
}
```

### 面向对象

面向对象的基本概念，包括：

- 类
- 实例
- 方法

面向对象的实现方式，包括：

- 继承
- 多态

Java语言本身提供的机制，包括：

- package
- classpath
- jar

以及Java标准库提供的核心类，包括：

- 字符串
- 包装类型
- JavaBean
- 枚举
- 常用工具类

##### 方法

封装性，隐藏对象的属性和实现细节，只提供公共访问方式

```java
public class Main {
    public static void main(String[] args) {
        Person ming = new Person();
        ming.setBirth(2008);
        System.out.println(ming.getAge());
    }
}

class Person {
    private String name;//外部无法访问
    private int birth;//外部无法访问

    public void setBirth(int birth) {
        this.birth = birth;
    }

    public int getAge() {
        return calcAge(2019); // 调用private方法
    }

    // private方法，外部无法调用
    private int calcAge(int currentYear) {
        return currentYear - this.birth;
    }
}
```

##### 构造方法

构造方法的名称就是类名。构造方法的参数没有限制，在方法内部，也可以编写任意语句。但是，和普通方法相比，构造方法没有返回值（也没有 `void` ），调用构造方法，必须用 `new` 操作符。

如果一个类没有定义构造方法，编译器会自动为我们生成一个默认构造方法，它没有参数，也没有执行语句。

可以同时存在多个构造方法，方法的**重载**。

```java
class Person {
	//默认的构造方法
    public Person() {
    }
    //自定义构造方法
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

##### 重载与重写

存在于同一个类中，指一个方法与已经存在的方法名称上相同，但是参数类型、个数、顺序至少有一个不同。

应该注意的是，返回值不同，其它都相同不算是重载。



##### 继承

任何类，除了 `Object`，都会继承自某个类，未明确注明 `extends` 的类，编译器会自动加上 `extends Object`。

任何`class`的构造方法，第一行语句必须是调用父类的构造方法。如果没有明确地调用父类的构造方法，编译器会帮我们自动加一句`super();`

子类*不会继承*任何父类的构造方法。子类默认的构造方法是编译器自动生成的，不是继承的。

##### 多态

子类可以覆写父类的方法（Override），覆写在子类中改变了父类方法的行为。

Java 的实例方法调用是基于**运行时的实际类型的动态调用**，而非变量的声明类型。

```java
public class Main {
    public static void main(String[] args) {
    //引用类型为 Person，实际类型为 Student
    	Person p = new Student();
        p.run(); // 打印Student.run
    }
}

class Person {
    public void run() {
        System.out.println("Person.run");
    }
}

class Student extends Person {
    @Override
    public void run() {
        System.out.println("Student.run");
    }
}
```

**多态的特性：运行期才能动态决定调用的子类方法。**

```java
//无法确定传入参数 p 的实际类型是 Person 还是 Student 
public void runTwice(Person p) {
    //无法确定调用的是哪个类的 run()方法
    p.run();
    p.run();
}
```

##### 抽象类

如果父类的方法本身不需要实现任何功能，仅仅是为了定义方法签名，目的是让子类去覆写它，那么，可以把父类的方法声明为抽象方法：

```java
class Person {
    public abstract void run();
}
```

使用 `abstract` 修饰的类就是抽象类。我们无法实例化一个抽象类，抽象类本身被设计成只能用于被继承。

通过 `abstract` 定义的方法是抽象方法，它只有定义，没有实现。抽象方法定义了子类必须实现的接口规范。

通过`abstract`定义的方法是抽象方法，它只有定义，没有实现，不能被实例化。抽象方法定义了子类必须实现的接口规范；

```java
public class Main {
    public static void main(String[] args) {
        Person p = new Student();
        p.run();
    }
}

abstract class Person {
    public abstract void run();
}

class Student extends Person {
    @Override
    public void run() {
        System.out.println("Student.run");
    }
}
```

###### 面向抽象编程

是一种引用高层类型，避免引用实际子类型的方式。

- 上层代码只定义规范（例如：`abstract class Person`）；
- 具体的业务逻辑由不同的子类实现，调用者并不关心。
- 只关心抽象方法的定义，不关心子类的具体实现。

```java
Person s = new Student();
// 不关心Person变量的具体子类型:
s.run();
```

##### 接口

接口中不能有字段，定义的方法默认都是 `public abstract` 。

一个类只能继承另一个类，但可以实现多个接口，使用关键字 `implements` ；

一个接口可以继承另一个接口，使用关键字 `extends` 。

```java
interface Hello {
    void hello();
}
//一个接口继承另一个接口, extends
interface Person extends Hello {
    void run();
}
//一个类实现多个接口,implements
class Student implements Person, Hello {
    private String name;

    public Student(String name) {
        this.name = name;
    }

    @Override
    public void hello() {
        System.out.println(this.name + " hello");
    }
    
    @Override
    public void run() {
        System.out.println(this.name + " run");
    }
    
}
```

术语区分：Java 的接口特指 `interface` 的定义，表示一个接口类型和一组方法签名，而编程接口泛指接口规范，如方法签名，数据格式，网络协议等。

**[default方法](<https://blog.csdn.net/qq_35835624/article/details/80196932>)**

从 JDK 1.8 之后，接口可以定义 `default` 方法，实现类可以不必覆写`default` 方法；当同时继承的两个接口中都定义了相同 `default` 方法，不知道调用哪个接口中的 `default` 方法，需要在实现类中重新实现该方法；当继承的父类和一个接口中定义了同一个方法，这时候实现类调用父类的方法，因为**类优先于接口**。

`default`方法和抽象类的普通方法是有所不同的。因为`interface`没有字段，`default`方法无法访问字段，而抽象类的普通方法可以访问实例字段。

##### 静态字段和静态方法

###### 静态字段

```java
class Person {
    //定义实例字段 name:
    public String name;
    // 定义静态字段 number:
    public static int number;
}
```

- 实例字段：在 class 中定义的字段，每个实例都有独立的字段，各个实例的同名字段互不影响。
- 静态字段：用 `static` 修饰的字段，所有实例共享同一个静态字段。修改一个实例的静态字段，会改变所有实例的静态字段，因为本质上都是同一个字段。
- 不推荐用 `实例变量.静态字段` (hong.number) 去访问静态字段，因为在Java程序中，实例对象并没有静态字段；推荐用 `类名.静态字段` (Person.number) 来访问。把静态字段理解为描述 `class` 本身的字段（非实例字段）。

```java
public class Main {
    public static void main(String[] args) {
        Person ming = new Person("Xiao Ming", 12);
        Person hong = new Person("Xiao Hong", 15);
        ming.number = 88;
        System.out.println(hong.number);//88
        hong.number = 99;
        System.out.println(ming.number);//99
    }
}

class Person {
    public String name;
    public int age;

    public static int number;//静态变量

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

###### 静态方法

用 `static` 修饰的方法

调用实例方法必须通过一个实例变量，而调用静态方法直接通过类名就可以调用。

因为静态方法属于`class`而不属于实例，因此，静态方法内部，无法访问`this`  变量，也无法访问实例字段，它只能访问静态字段。

```java
public class Main {
    public static void main(String[] args) {
        //直接通过 类名.方法名 调用静态方法
        Person.setNumber(99);
        System.out.println(Person.number);
    }
}

class Person {
    public static int number;
    //静态方法
    public static void setNumber(int value) {
        number = value;
    }
}
```

###### 接口的静态字段

接口不能定义实例字段，但可以有静态字段，默认为 `public static final` 类型。

##### 包

Java内建的`package`机制是为了避免`class`命名冲突；

JDK的核心类使用`java.lang`包，编译器会自动导入；

JDK的其它常用类定义在`java.util.*`，`java.math.*`，`java.text.*`，……；

包名推荐使用倒置的域名，例如`org.apache`。

##### 作用域

一个`.java`文件只能包含一个`public`类，但可以包含多个非`public`类。如果有`public`类，文件名必须和`public`类的名字相同。

#### Java 核心类

###### 拼接字符串

StringJoiner (CharSequence delimiter, CharSequence prefix,  CharSequence suffix) 的第二个和第三个参数分别是拼接后的字符串的前缀和后缀。