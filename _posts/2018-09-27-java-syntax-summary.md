---
title: "Java 基础语法关键点总结"
date: 2018-09-27 11:03:07 +0800
category: Java
excerpt: 总有一些语法细节，当你再看书时发现很多地方是比较容易遗忘或是没有注意到的，本文旨在将这些知识点整理出来，方便复习。
---

## Java 文档

命令：javadoc -help

## 分隔符、标识符和关键字

`分隔符`：分号 (;)、花括号 ({})、方括号 ([])、圆括号 (())、空格 ( )、圆点 (.)。

Java 采用分号 (;) 作为语句的分隔，语句可跨行书写，但一个字符串、变量名不能跨多行书写。

```java
// good
String words = "Hello " +
    "Java";

// error
String words = "Hello
    Java";
String word
    s = "Hello Java";
```

`标识符`：用于命名的符号，必须以字母、下划线 (\_)、美元符 ($) 开头，后可跟任意字母、数字、下划线和美元符。

标识符不能是 Java 关键字和保留字。

`关键字`：特殊用途的单词，**所有的关键字都是小写的**。

## 数据类型

Java 数据类型分为：基本类型和引用类型。

![Java Data Type](https://www.z4a.net/images/2018/10/23/DataType.png)

char 代表字符型，用单引号 (') 表示，存的是字符的编码，所以实际上也是一种整数类型，相当于无符号整数类型。

引用数据类型就是对一个对象的引用，对象包括实例和数组两种。空类型就是 null 值的类型，这种类型没有名称，所以不可能声明一个 null 类型的变量或者转换到 null 类型。

**空引用只能被转换成引用类型，不能转换成基本类型，因此不要把一个 null 值赋给基本数据类型的变量。**

Java 中整数值的 4 种表达方式：十进制、二进制 (以 0b 或 0B 开头)、八进制 (**以 0 开头**) 和十六进制 (以 0x 或 0X 开头)。

**数值可用下划线分隔**，可以更加直观地分辨数值包含的位数，如下：

```java
int bina = 0B1000_0000_0000_0000_0000_0000_0000_0011;
double pi = 3.14_15_92_65_36;
```

**Java 中浮点类型默认是 double 类型**，因此，如果希望把一个浮点类型值当成 float 类型处理，其后应紧跟 f 或 F。

```java
// normal
double pi = 3.14;

// error
float pi = 3.14;

// good
float pi = 3.14f;
```

### 基本类型的类型转换

由小到大的类型转换为自动类型转换，由大到小的类型转换为强制类型转换，需在转换数值前加转换类型，其语法为：`(TargetType) value`。

强制类型转换容易引起精度、信息的丢失及溢出错误。

**表达式类型的自动提升**：当一个算术表达式中含多个基本类型的值时，整个算术表达式的数据类型将发生自动提升，提升规则如下

 - 所有的 byte 类型、short 类型和 char 类型将被提升到 int 类型。
 - 整个算术表达式的数据类型自动提升到表达式中最高等级操作数的类型。

```java
// 输出 Hello!a7
System.out.println("Hello!" + 'a' + 7);

// 输出 104Hello!
System.out.println('a' + 7 + "Hello!");
```

### 直接量

直接量是指在程序中通过源代码直接给出的值，例如 `int a = 18;` 中为 a 所分配的初始值 18 就是一个直接量。

char 类型的直接量有三种：字符 ('a')、转义字符 ('\n')、Unicode 值表示的字符 ('\u0061')。

String 类型的直接量为双引号括起来的字符序列，不能赋给其他类型的变量。String 类是典型的不可变类，String 对象创建出来就不可能被改变。

关于字符串直接量，当程序第一次使用某个字符串直接量时，Java 会直接使用常量池 (constant pool) 中的字符串直接量。常量池指的是在编译期被确定，并被保存在已编译的 `.class` 文件中的一些数据。它包括关于类、方法、接口中的常量，也包括字符串直接量。

```java
String s0 = "hello";
String s1 = "hello";
String s2 = "he" + "llo";

// true
System.out.println(s0 == s1);
// true
System.out.println(s0 == s2);
```

Java 会确保每个字符串常量只有一个，不会产生多个副本。s0 和 s1 中的 "hello" 都是字符串常量，在编译期就被确定量，所以第一个返回 true，而 "he" 和 "llo" 也都是字符串常量，当一个字符串由多个字符串常量连接而成时，它本身也是字符串常量，s2 也在编译期就被解析为一个字符串常量，所以 s2 也是常量池中 "hello" 的引用，所以第二个也返回 true。

## 运算符

单目运算符自加和自减中运算符在操作数左边和右边的区别：当运算符在操作数左边时，操作数先进行单目运算，再将操作数代入表达式中运算；当运算符在操作数右边时，先将操作数代入表达式中运算，再对操作数进行单目运算。

```java
// case 1
int a = 5;
int b = a++ + 6;
// 输出 a 为 6，b 为 11
System.out.println(a + "\n" + b);

// case 2
int a = 5;
int b = ++a + 6;
// 输出 a 为 6，b 为 12
System.out.println(a + "\n" + b);
```

**赋值表达式是有值的**，其值就是右边被赋的值，因此，赋值运算符支持连续赋值，但这种写法使程序可读性降低，不推荐。

```java
int a;
int b;
int c;

// 同时为 a, b, c 赋值 18
a = b = c = 18;
```

推荐使用扩展后的赋值运算符如：`+=`、`-=`、`*=`、`&=`、`|=`、`^=` 等，这种运算符不仅具有更好的性能，而且程序会更加健壮。

```java
byte a = 5;
// a 为 byte 类型，a + 5 由表达式类型的自动提升
// 其结果为 int 类型，因此下面语法会出错
a = a + 5;

byte b = 5;
// 下面使用扩展后的赋值运算符不会出错
b += 5;
```

逻辑运算符中的短路与不短路：逻辑与 `&&` (短路与) 或 `&` (不短路与) 和逻辑或 `||` (短路或) 或 `|` (不短路或) 的区别在于，**短路逻辑运算符如果在第一个表达式的结果就能得出最终表达式的结果，将不再进行后续表达式的运算**，相反，不短路的逻辑运算符不管怎样，都会计算各个表达式。

```java
int a = 5;
int b = 10;
if (a > 4 || b++ > 10) {
    // 输出：a = 5, b = 10
    System.out.println("a = " + a + ", b = " + b);
}

int c = 5;
int d = 10;
if (c > 4 | d++ > 10) {
    // 输出：c = 5, d = 11
    System.out.println("c = " + c + ", d = " + d);
}
```

## 数组类型

Java 中的数组要求所有的元素具有相同的数据类型，一旦数组的初始化完成，数组在内存中所占的空间将被固定下来，因此数组的长度将不可改变。即使把某个数组元素的数据清空，但它所占据的空间依然被保留，依然属于该数组，数组的长度依然不变。

Java 的数组可以存储基本类型的数据，也可存储引用类型的数据，只要所有的数组元素具有相同的类型即可 (父类定义的类型，可存放其继承而来的不同类型子类)。

数组也是一种数据类型，它本身是一种引用类型。例如 `int` 是一个基本类型，但 `int[]` 就是一种引用类型。

数组的定义支持两种语法：

```java
type[] arrayName;
type arrayName[];
```

但推荐第一种语法，不仅具有更好的语意，而且更具可读性，`type[]` 确实是一种新类型 (引用类型)，符合定义变量的语法。第二种定义数组的语法方式，是一种糟糕的方式，越来越多的语言都不再支持这种定义语法了。

定义数组时不能指定数组的长度，也不能使用，只有对数组进行初始化后才可以使用。

### 数组初始化

数组初始化分两种：
 - 静态初始化：由程序员显式指定每个数组元素的初始值，由系统决定数组长度。
 - 动态初始化：初始化时程序员只指定数组长度，由系统为数组元素分配初始值。

```java
// 定义数组类型变量
int[] intArr;
// 静态初始化
intArr = new int[]{5, 6, 7, 8};

// 静态初始化简洁语法
// 只有在定义和初始化同时执行时才支持使用这种语法
int[] a = {5, 6, 7, 8};

int[] prices;
// 动态初始化
// 由系统分配初始值
prices = new int[5];
// 定义和初始化同时执行
Object[] books = new String[4];
```

动态初始化，系统分配初始值规则如下：

 - 基本类型中整数类型 (byte、short、int、long) 为 0。
 - 基本类型中浮点类型 (float、double) 为 0.0。
 - 基本类型中字符类型 (char) 为 '\u0000'。
 - 基本类型中布尔类型 (boolean) 为 false。
 - 引用类型 (类、接口、数组) 为 null。

**不要同时使用静态初始化和动态初始化，也就是说，不要在进行数组初始化时，既指定数组长度，又为每个数组元素分配初始值**。

### foreach 循环

使用 foreach 循环遍历数组和集合元素时，无须获得数组和集合长度，无须根据索引来访问数组元素和集合元素，foreach 循环自动遍历数组和集合的每个元素。**使用 foreach 循环时，通常不要对循环变量进行赋值，没有太大的实际意义**，因为 foreach 循环中的循环变量相当于一个临时变量，系统会把数组元素依次赋给这个临时变量，而这个临时变量并不是数组元素，它只是保存了数组元素的值。因此，如果希望改变数组元素的值，不能使用这种 foreach 循环。

```java
String[] books = {"Git", "Java", "Python"};

for (String book: books) {
    book = "itsCoder";
    System.out.println(book);
}

System.out.println(books[0]);
```

输出结果如下：

```
itsCoder
itsCoder
itsCoder
Git
```

### 内存中的数组

初始化一个数组后，实际数组对象数据被存储在堆 (heap) 内存中，引用该数组对象的数组引用变量，如果是一个局部变量，被存储在栈 (stack) 内存中，可以理解为 C 语言中的指针，数组引用变量存放的是数组在堆内存中的地址，通过 `arrayName[index]` 的方式实现数据的访问。

![Array Memory](https://www.z4a.net/images/2018/10/23/ArrayMemory.png)

通过设置 `arrayName = null` 的方式切断与实际数组间的引用关系，当实际数组无引用指向时，在堆中就成了垃圾，等待回收。

**类的对象的存储和引用也是一样的。**

## 面向对象

### 对象的 this 引用

Java 提供的 this 关键字总是指向调用该方法的对象，根据 this 出现的位置不同，this 作为对象的默认引用有两种情形。

 - 构造器中引用该构造器正在初始化的对象。
 - 在方法中引用调用该方法的对象。

this 可以代表任何对象，当 this 出现在某个方法中时，它所代表的对象是不确定的，但它的类型是确定的，它所代表的对象只能是当前类，只有这个方法被调用时，它所代表的对象才能被确定下来：谁在调用这个方法，this 就代表谁。

```java
public class Dog {
    public void jump() {
        System.out.println("jump");
    }

    public void run() {
        this.jump();
        System.out.println("run");
    }
}

public class DogTest {
    public static void main(String[] args) {
        Dog dog = new Dog();
        dog.run();
    }
}
```

因为当程序调用 `run()` 方法时，一定会提供一个 Dog 对象，这样就可以直接使用这个对象了而无须重新创建一个对象了，使用 this 来指代这个对象，从而调用 Dog 类的实例方法。Java 中对象里的这种方法依赖关系很常见，因此，Java 允许对象的一个成员直接调用另一个成员，可以省略 this 前缀。

而对于 static 修饰的方法 (称为类方法或静态方法)，可以直接使用类来调用，所以，若在类方法中使用 this 关键字，此时这个关键字无法指向合适的对象。因此，static 修饰的方法不能访问不使用 static 修饰的普通成员，即：**静态成员不能直接访问非静态成员**。

### 对象访问类成员的陷阱

**当需要修改类变量时，应使用类作为调用者来操作**，虽然 Java 中允许使用这个类的实例对象访问类成员，但这是一个「错误」的设计，应避免这种调用方法，才不会产生歧义，提高程序的可读性。

当第一次使用一个类时，会进行初始化操作，此时对类成员变量系统进行自动赋值，其类成员变量在堆中是存放于一块独立的内存中。当对象赋给实例变量时，其实例变量在堆中开辟另一块独立的内存，所以多个实例间，类成员变量是共有的，实例变量是各自私有的，通过任何一个实例对象访问修改类成员变量，都会对其他实例访问造成影响。

例如一个 Person 类：

```java
class Person {
    public static int eyeNum;
    public String name;
}

public class Test {
    public static void main(String[] args) {
        Person p1 = new Person();
        p1.name = "张三";
        p1.eyeNum = 2;

        Person p2 = new Person();
        p2.name = "李四";
        // 输出 2
        System.out.println(p2.eyeNum);
    }
}
```

其类初始化内存分配及各实例初始化后内存分配情况如下：

![Class Memory](https://www.z4a.net/images/2018/10/23/ClassMemory.png)

### 方法的参数传递机制

**Java 里方法的参数传递方式只有一种：「值传递」**。所谓值传递，就是将实际参数值的副本 (复制品) 传入方法内，而参数本身不会受到影响。

```java
public class Test {
    public static void swap(int a, int b) {
        int tmp = a;
        a = b;
        b = tmp;
        System.out.println("swap 方法里，a 的值是 "
            + a + "; b 的值是 " + b);
    }

    public static void main(String[] args) {
        int a = 6;
        int b = 9;
        swap(a, b);
        System.out.println("交换后，变量 a 的值是 "
            + a + "; 变量 b 的值是 " + b);
    }
}
```

输出结果是：

```
swap 方法里，a 的值是 9; b 的值是 6
交换后，变量 a 的值是 6; 变量 b 的值是 9
```

在 `main` 方法的栈区里，存在 a，b 两个值，而在 `swap` 方法的栈区中，存储的是 `main` 方法里 a，b 变量值的副本，所以在 `swap` 方法中对 a，b 进行操作，修改的是 `swap` 方法栈区的值，不会影响到 `mian` 方法栈区中的 a，b 变量。

同理，对于引用类型也是一样的，但是，因为引用类型变量存储的是「地址」，所以即使是值传递，当对其数据操作时，地址指向的还是同一个堆里的对象数据，因此，引用类型的操作是「有效的」。

### 形参个数可变的方法

**形参个数可变的参数，本质上就是一个数组参数**。

其语法为：`[修饰符] 方法返回值类型 functionName(Type... parameterName)`。

方法内部可用 foreach 循环取得可变参数的各个值，一个方法中最多包含一个长度可变的形参，且长度可变的形参只能置于形参列表的最后。

```java
public static void test(int a, int b, String... books) {

    System.out.println(a);
    System.out.println(b);

    for (String book: books) {
        System.out.println(book);
    }
}
```

### 方法重载

方法重载的要求：同一个类中方法名相同，参数列表不同，**至于方法的其他部分，如方法返回值类型、修饰符等，与方法重载没有任何关系**。

### 静态导入语法

除了导入类之外，Java 还支持导入指定类的某个静态成员变量、方法或全部的静态成员变量、方法。

其语法是：

```
import static package.subpackage....ClassName.fieldName|methodName/ClassName.*;
```

```java
import static java.lang.System.*;
import static java.lang.Math.*;

public class StaticImportTest {
    public static void main(String[] args) {
        out.println(PI);
        out.println(sqrt(256));
    }
}
```

### 方法重写

在使用继承时，子类重写父类方法，遵循『两同两小一大』原则：**方法名、形参列表相同；子类方法返回值类型、子类方法声明抛出的异常类都应比父类的更小或相等；子类方法的访问权限应比父类方法的访问权限更大或相等**。而且覆盖方法和被覆盖方法都只能同时是类方法或实例方法，不能一个为类方法，一个为实例方法。

### 类的初始化顺序

当类第一次被加载使用时，系统会在类准备阶段为该类所有静态成员变量分配内存，在初始化阶段，先会对静态成员变量初始化，即：由子类回溯到最高父类，执行静态初始化块或声明类变量时指定的初始值，然后向下依次执行直至当前类。其次是普通初始化块、声明实例变量和构造器，也是由最高父类向下依次执行直至当前类。

用一个例子说明：

```java
class Root {
    static {
        System.out.println("Root 的静态初始化块");
    }

    {
        System.out.println("Root 的普通初始化块");
    }

    public Root() {
        System.out.println("Root 的无参构造器");
    }
}

class Mid extends Root {
    static {
        System.out.println("Mid 的静态初始化块");
    }

    {
        System.out.println("Mid 的普通初始化块");
    }

    public Mid() {
        System.out.println("Mid 的无参构造器");
    }

    public Mid(String msg) {
        this();
        System.out.println("Mid 的带参构造器，其参数值：" + msg);
    }
}

class Leaf extends Mid {
    static {
        System.out.println("Leaf 的静态初始化块");
    }

    {
        System.out.println("Leaf 的普通初始化块");
    }

    public Leaf() {
        super("itsCoder");
        System.out.println("Leaf 的无参构造器");
    }
}

public class Test {
    public static void main(String[] args) {
        new Leaf();
        new Leaf();
    }
}
```

其输出结果是 (虚线为手动添加，为了更直观分析)：

```
Root 的静态初始化块
Mid 的静态初始化块
Leaf 的静态初始化块
-------------------------
Root 的普通初始化块
Root 的无参构造器
Mid 的普通初始化块
Mid 的无参构造器
Mid 的带参构造器，其参数值：itsCoder
Leaf 的普通初始化块
Leaf 的无参构造器
-------------------------
Root 的普通初始化块
Root 的无参构造器
Mid 的普通初始化块
Mid 的无参构造器
Mid 的带参构造器，其参数值：itsCoder
Leaf 的普通初始化块
Leaf 的无参构造器
```

静态初始化块和声明静态成员变量时所指定的初始值都是该类的初始化代码，它们的执行顺序和源程序中的排列顺序相同。

```java
public class StaticInitTest {
    static {
        a = 6;
    }

    static int a = 9;

    public static void main(String[] args) {
        // 输出 9
        System.out.println(StaticInitTest.a);
    }
}
```

> 实际上初始化块是一个『假象』，使用 `javac` 命令编译 Java 类后，该 Java 类中的初始化块会消失 —— 初始化块中的代码会被『还原』到每个构造器中，且位于构造器所有代码的前面。

### 自动装箱的特别情形

自动装箱，即可以直接把一个基本类型值赋给一个包装类实例，这种情况下可能会出现一些特别的情形。

```java
Integer ina = 2;
Integer inb = 2;
// 输出 true
System.out.println("两个 2 自动装箱后是否相等：" + (ina == inb));

Integer biga = 128;
Integer bigb = 128;
// 输出 false
System.out.println("两个 128 自动装箱后是否相等：" + (biga == bigb));
```

这是因为 Integer 类的设计中，对 -128 到 127 间的数据做了缓存：

```java
// java.lang.Integer

// 定义一个长度为 256 的 Integer 数组
static final Integer[] cache = new Integer[-(-128) + 127 + 1];
static {
    // 执行初始化，创建 -128 到 127 的 Integer 实例，并放入 cache 数组中
    for(int i = 0; i < cache.length; i++)
        cache[i] = new Integer(i - 128);
}
```

所以如果把一个 -128 ~ 127 之间的整数自动装箱成一个 Integer 实例时，实际上是直接指向对应的数组元素。即引用的是 cache 数组中的同一个数组元素，所以 ina 和 inb 相等。

### `==` 和 `equals` 方法

当使用 `==` 来判断两个变量是否相等时：如果两个变量是基本类型变量，且都是数值类型 (不一定要求数据类型严格相同)，则只要两个变量的值相等，就将返回 true；如果两个变量是引用类型变量，只有它们指向同一个对象时，判断才返回 true。**`==` 不可用于比较类型上没有父子关系的两个对象，否则引发编译错误**。

```java
int it = 18;
float fl = 18.0f;
// 输出 true
System.out.println("18 和 18.0f 是否相等？" + (it == fl));

char ch = 'A';
// 输出 true
System.out.println("65 和 'A' 是否相等？" + (65 == ch));

String str1 = new String("hello");
String str2 = new String("hello");
// 输出 false
System.out.println("str1 和 str2 是否相等？" + (str1 == str2));
// 输出 true
System.out.println("str1 是否 equals str2？" + (str1.equals(str2)));

// java.lang.String 与 EqualTest 类没有继承关系
// 引发编译错误
System.out.println("hello" == new EqualTest());
```

`equals()` 方法是 Object 类提供的一个实例方法，因此所有引用变量都可调用该方法来判断是否与其他引用变量相等，**但是这个方法判断两个对象相等的标准与使用 == 运算符没有区别，同样要求两个引用变量指向同一个对象才会返回 true**。因此，这个 Object 类提供的 `equals()` 方法没有太大的实际意义，如果希望采用自定义的相等标准，则可采用重写 equals 方法来实现。

**String 已经重写了 Object 的 `equals()` 方法**，String 的 `equals()` 方法判断两个字符串相等的标准是：只要两个字符串所包含的字符序列相同，通过 `equals()` 比较将返回 true，所以 `str1.equals(str2)` 才返回 true。

### final 修饰符

final 成员变量 (包括实例变量和类变量) **必须**由程序员**显式初始化**，**系统不会对 final 成员进行隐式初始化**。

final 修饰基本类型变量和引用类型变量的区别：当使用 final 修饰基本类型变量时，不能对基本类型变量重新赋值，因此基本类型变量不能被改变。对于引用类型变量而言，它保存的仅仅是一个引用，final 只保证这个引用类型变量所引用的地址不会改变，即一直引用同一个对象，但这个对象完全可以发生改变。

### 抽象方法和抽象类

抽象类不能用于创建实例，只能被其他子类继承，抽象类的构造器主要是用于被其子类调用。抽象方法是只有方法签名，没有方法实现 (没有方法体，而非空方法体) 的方法。

**有抽象方法的类只能被定义成抽象类，抽象类里可以没有抽象方法**。

### 自我克隆

Java 的 Object 类还提供了一个 protected 修饰的 `clone()` 方法，用于帮助其他对象来实现「自我克隆」，得到当前对象的一个副本，二者之间完全隔离，但这种克隆是一种「简单克隆/浅克隆」—— 它只克隆该对象的所有成员变量值，不会对引用类型的成员变量值所引用的对象进行克隆，即引用类型对象克隆的是地址值，指向同一个内存地址。如果要进行「深克隆」，则需要自己进行「递归克隆」。

自定义类要实现「克隆」需要实现 Cloneable 接口，这是一个标记性的接口，里面没有定义任何方法。其次需要实现自己的 `clone()` 方法，在实现这个方法时，通过调用 `super.clone()` 方法来得到该对象的副本，并将其返回。

```java
class Address {
    String detail;
    public Address(String detail) {
        this.detail = detail;
    }
}

class User implements Cloneable {
    int age;
    Address address;

    public User(int age) {
        this.age = age;
        address = new Address("itsCoder");
    }

    public User clone() throws CloneNotSupportedException {
        return (User)super.clone();
    }
}

public class Test {
    public static void main(String[] args) throws CloneNotSupportedException {
        User u1 = new User(29);
        User u2 = u1.clone();
        // 返回 false
        System.out.println(u1 == u2);
        // 返回 true
        System.out.println(u1.address == u2.address);
    }
}
```

### Objects 类

使用 Java 7 新增的 Objects 工具类来操作对象，这些工具方法大多是「空指针」安全的。

```java
public class ObjectTest {
    static ObjectTest obj;

    public static void main(String[] args) {
        // 输出一个 null 对象的 hashCode 值，输出 0
        System.out.println(Objects.hashCode(obj));
        // 输出一个 null 对象的 toString，输出 null
        System.out.println(Objects.toString(obj));
        // 要求 obj 不能为 null，如果 obj 为 null 引发空指针异常
        System.out.println(Objects.requireNonNull(obj, "obj 参数不能是 null!"));
    }

    public test(Object obj) {
        // 赋值时用于校验参数，为 null 引发异常
        this.obj = Objects.requireNonNull(obj);
    }
}
```

### String、StringBuffer 和 StringBuilder

**String** 类是不可变类，一旦一个 String 对象被创建以后，包含在这个对象中的字符序列是不可改变的，直至这个对象被销毁。因为是不可变的，所以会额外产生很多临时变量，如下：

```java
String str = "java";
str = str + "struts";
str = str + "spring";
```

上面程序除了使用了 3 个字符串直接量之外，还会额外产生 2 个字符串直接量 —— "javastruts" 和 "javastrutsspring"，程序中的 str 依次指向 3 个不同的字符串对象。使用 StringBuffer 或 StringBuilder (追加方法) 就可以避免这个问题。

**StringBuffer** 代表一个字符序列可变的字符串，提供有 `append()`、`insert()`、`reverse()`、`setCharAt()`、`setLength()` 等方法用来改变字符串对象的字符序列。

**StringBuilder** 是 JDK 1.5 新增的类，和 StringBuffer 基本类似，只是 StringBuffer 是线程安全的，而 StringBuilder 则没有实现线程安全功能，所以性能略高。通常情况下，如需创建一个内容可变的字符串对象，则应优先考虑使用 StringBuilder 类。

## 集合

> 『善用集合工具类 [Collections](https://docs.oracle.com/javase/7/docs/api/java/util/Collections.html)』

Java 集合大致可分为 Set、List、Queue 和 Map 四种体系，其中 Set 代表无序、不可重复的集合；List 代表有序、重复的集合；而 Map 代表具有映射关系的集合，Java 5 又增加了 Queue 体系集合，代表一种队列集合实现。

**集合类主要由两个接口派生而来 —— 「Collection 和 Map」**，二者是集合框架的根接口，包含一些子接口和实现类。继承树体系如下所示：

![Collection Map](https://www.z4a.net/images/2018/10/23/CollectionMap.png)

图中灰色背景为最常用实现类：HashSet、TreeSet、ArrayList、ArrayDeque、LinkedList、HashMap 和 TreeMap。

### HashSet

HashSet 判断两个元素相等的标准：`equals()`、`hashCode()` 均相等。「相等元素」使用 `add()` 方法添加时返回 false 并不加入集合中。

HashSet 中每个能存储元素的「槽位」(slot) 通常称为「桶」(bucket)，如果有多个元素的 hashCode 值相同，但它们通过 `equals()` 方法比较返回 false，就需要在一个「桶」里放多个元素，这样会导致性能下降。所以重写 `hashCode()` 方法的一些基本规则如下：

 - 在程序运行过程中，同一个对象多次调用 hashCode() 方法应该返回相同的值。
 - 当两个对象通过 equals() 方法比较返回 true 时，这两个对象的 hashCode() 方法应返回相等的值。
 - 对象中用作 equals() 方法比较标准的实例变量，都应该用于计算 hashCode 值。

### TreeSet

TreeSet 是 SortedSet 接口的实现类，可以确保集合元素处于排序状态 (按元素的实际值大小进行排序)。与 HashSet 集合采用 hash 算法来决定元素的存储位置不同，TreeSet 采用红黑树的数据结构来存储集合元素。

TreeSet 支持两种排序方法：自然排序 (默认) 和定制排序。

**自然排序**：TreeSet 会调用集合元素的 `compareTo(Object obj)` 方法来比较元素之间的大小关系，然后将集合元素按升序排序。所以将一个对象添加到 TreeSet 时，则该对象必须实现 Comparable 接口，否则程序将会抛出异常。一些常用类都实现了 Comparable 接口，其比较规则如下：

- BigDecimal、BigInteger 以及所有数值型对象的包装类，按它们对应的数值大小比较。
- Character、String 类，按字符 (/字符串中的字符) 的 UNICODE 值进行比较。
- Boolean 类,其 true 对应的包装类实例大于 false 对应的包装类实例。
- Date、Time 类，后面的时间、日期大于前面的时间、日期。

> 当向 TreeSet 集合中添加第一个元素时，这个元素可以没有实现 Comparable 接口，因为只有一个元素时，无须进行比较，也就不用调用当前元素的 `compareTo()` 方法，但这不是一种好做法，当试图从 TreeSet 中取出元素时，会引发 ClassCastException 异常。而又因为只有相同类的两个实例才会比较大小，所以 TreeSet 只能添加同一种类型的对象，否则依然会引发 ClassCastException 异常。

总结一下就是，当把一个对象加入 TreeSet 集合时，TreeSet 调用该对象的 `compareTo(Object obj)` 方法与容器中的其他对象比较大小，然后根据红黑树结构找到它的存储位置。如果两个对象通过 `compareTo(Object obj)` 方法比较相等，则新对象将无法添加到 TreeSet 集合中。

**定制排序**：如需实现定制排序，例如降序排序，则可通过 Comparator 接口 (函数式接口) 的帮助，该接口包含一个 `int compare(T O1, T O2)` 方法，使用示例如下：

```java
class M {
    int age;

    public M(int age) {
        this.age = age;
    }

    public String toString() {
        return "M[age: " + age + "]";
    }
}

public class TreeSetTest {
    public static void main(String[] args) {
        //函数式接口，可使用 Lambda 表达式
        TreeSet ts = new TreeSet((o1, o2) -> {
            M m1 = (M) o1;
            M m2 = (M) o2;
            // 年龄大的对象反而小
            return m1.age > m2.age ? -1 : m1.age < m2.age ? 1 : 0;
        });

        // 使用定制排序的添加对象类无须实现 Comparator 接口
        ts.add(new M(5));
        ts.add(new M(-3));
        ts.add(new M(9));
        System.out.println(ts);
    }
}

// 输出:
// [M[age: 9], M[age: 5], M[age: -3]]
```

### HashSet、TreeSet 性能分析

HashSet 的性能总是比 TreeSet 好 (特别是最常用的添加、查询元素等操作)，因为 TreeSet 需要额外的红黑树算法来维护集合元素的次序。只有当需要一个保持排序的 Set 时，才应该使用 TreeSet，否则都应该使用 HashSet。

HashSet 还有一个子类：LinkedHashSet，对于普通的插入、删除操作，LinkedHashSet 比 HashSet 要略微慢一点，这是由于维护链表所带来的额外开销造成的，但由于有了链表，遍历 LinkedHashSet 会更快。

由于 Set 的 HashSet、TreeSet 这两个实现类都是线程不安全的，因此可以通过 Collections 工具类的 synchronizedSortedSet 方法来「包装」该 Set 集合，使用语法如下：

```java
SortedSet s = Collections.synchronizedSortedSet(new TreeSet(...));
```

### List 集合

List 集合判断两个对象相等的标准是通过 equals() 方法返回 true 即可。

```java
class A {
    public boolean equals(Object obj) {
        return true;
    }
}

public class ListTest {
    public static void main(String[] args) {
        List books = new ArrayList();
        book.add(new String("itsCoder"));
        book.add(new String("Java"));
        book.add(new String("Kotlin"));
        System.out.println(books);
        // String 类的 equals 方法比较字符串序列值，删除成功
        books.remove(new String("Kotlin"));
        System.out.println(books);
        // 删除 A 对象，将导致第一个元素被删除
        books.remove(new A());
        System.out.println(books);
    }
}

// 输出：
// [itsCoder, Java, Kotlin]
// [itsCoder, Java]
// [Java]
```

Java 8 为 List 集合增加了 `sort()` 和 `replaceAll()` 两个常用的默认方法，其中 `sort()` 方法需要一个 Comparator (函数式接口) 对象来控制元素排序，而 `replaceAll()` 方法则需要一个 UnaryOperator (函数式接口) 来替换所有集合元素。

```java
public class ListTest {
    public static void main(String[] args) {
        List books = new ArrayList();
        book.add(new String("itsCoder"));
        book.add(new String("Java"));
        book.add(new String("Kotlin"));
        book.add(new String("JavaScript"));
        System.out.println(books);

        books.sort((o1, o2) -> ((String) o1).length() - ((String) o2).length());
        System.out.println(books);

        books.replaceAll(ele -> ((String) ele).length());
        System.out.println(books);
    }
}

// 输出：
// [itsCoder, Java, Kotlin, JavaScript]
// [java, Kotlin, itsCoder, JavaScript]
// [4, 6, 8, 10]
```

List 与 Set 只提供了一个 `iterator()` 方法不同，此外还提供了一个 `listIterator()` 方法，该方法返回一个 ListIterator 对象，ListIterator 接口继承了 Iterator 接口，在其基础上增加了几个专门操作 List 的方法：

- `boolean hasPrevious()`: 返回该迭代器关联的集合是否还有上一个元素。
- `Object previous()`: 返回该迭代器的上一个元素。
- `void add(Object o)`：在指定位置插入一个元素。

**即 List 集合可实现向前迭代功能，并且在迭代期间可执行 add 操作并生效。**

### ArrayList 和 Vector 实现类

二者都是基于数组实现的 List 类，即封装了一个动态的、允许再分配的 Object[] 数组。其对象使用 initalCapacity 参数来设置该数组长度，当添加元素超出时，该参数自动增加。**在创建空的这两个集合时不指定 initalCapacity 参数，则其数组默认长度为 10**。

如果要向集合中添加大量元素的话，可使用 `void ensureCapacity(int minCapacity)` 方法一次性地增加 initalCapacity 大小，以减少重分配次数，提高性能。

同时，还提供了 `void trimToSize()` 方法用来调整数组长度为当前元素个数，减少集合对象占用的存储空间。

ArrayList 和 Vector 在用法上几乎完全相同，但由于 **Vector 是一个古老的集合** (从 JDK 1.0 就有了)，那时候 Java 还没有提供系统的集合框架，这就使得 Vector 里提供了一些方法名很长的方法，例如 `addElement(Object obj)`，实际上这个方法与 `add(Object obj)` 没有任何区别。从 JDK 1.2 后，Java 提供了系统的集合框架，就将 Vector 改为实现 List 接口，作为 List 的实现之一，从而导致 Vector 里有一些方法功能重复。(实际上，Vector 具有很多缺点，通常尽量少用 Vector 实现类)

此外，还有一个显著的区别就是，**ArrayList 是线程不安全的，而 Vector 集合是线程安全的** (也因此 Vector 的性能比 ArrayList 的性能要低)，同样地，使用 Collections 工具类也可以将 ArrayList 变成线程安全的。

Vector 还提供了一个 Stack 子类，用于模拟「栈」这种数据结构，由于 Stack 继承了 Vector，因此它也是一个非常古老的 Java 集合类，它同样是线程安全的、性能较差的，应尽量少用 Stack 类，如需使用「栈」这种数据结构，可以考虑 ArrayDeque (既实现了 List 接口 —— 「底层基于数组实现，性能也很好」，也实现了 Deque 接口 —— 「可以作为栈来使用」)。

### Queue 集合

Queue 是用于模拟队列这种数据结构，队列即通常指「先进先出」的容器。Queue 接口有一个 PriorityQueue 实现类。此外，Queue 还有一个 Deque 接口，Deque 代表一个「双端队列」，可当成队列使用，也可当成栈使用，Java 为 Deque 提供了 ArrayDeque 和 LinkedList 两个实现类。

### PriorityQueue 实现类

PriorityQueue 是一个『比较标准』(而非绝对标准) 的队列实现类，因为该队列保存元素的顺序并不是按加入队列的顺序，而是按队列元素的大小进行重新排序，所以当调用 `peek()` 方法或 `poll()` 方法取出元素时，并不是最先进入的那个元素，而是最小的那一个。从这来看，它已经违反了队列最基本的规则：先进先出。

```java
class PriorityQueueTest {
    public static void main(String[] args) {
        PriorityQueue pq = new PriorityQueue();
        pq.offer(6);
        pq.offer(-3);
        pq.offer(20);
        pq.offer(18);
        System.out.println(pq.poll());
        System.out.println(pq.poll());
        System.out.println(pq.poll());
        System.out.println(pq.poll());
    }
}

// 输出：
// -3
// 6
// 18
// 20
```

当然，PriorityQueue 有两种排序方式：自然排序和定制排序，其要求及方法与前面介绍的 TreeSet 一致。

### ArrayDeque 实现类

ArrayDeque 从名称就可看出，它是一个基于数组实现的双端队列，创建 Deque 时同样可指定一个 numElements 参数，该参数用于指定 Object[] 数组的长度，如果不指定，**默认 Deque 底层数组的长度为 16**。当程序中需要使用「栈」这种数据结构时，尽量避免使用 Stack，推荐使用 ArrayDeque，但同时，它不仅可以作为栈使用，不要忘了，最基本的，它还可以当成队列使用。

### LinkedList 实现类

LinkedList 类是 List 接口的实现类，这意味着它是一个 List 集合，可以根据索引来随机访问集合元素，同时，它还实现了 Deque 接口，这意味着它也能当成双端队列来使用，因此，它既能当作「栈」来使用，也能当作「队列」来使用。这是一个功能十分强大的集合类。

LinkedList 内部以链表的形式来保存元素，其随机访问集合元素性能相比 ArrayList、ArrayDuque 这类以数组形式存储元素的集合类要差，但在插入、删除元素时性能比较出色。而 Vector 虽然也是以数组形式存储元素，但因其实现了线程同步功能 (而且实现机制也不好)，所以各方面性能都比较差。

### Map 集合

Map 用于保存具有映射关系的数据，因此 Map 集合里保存着两组值，一组值用于保存 Map 里的 key，另一组用于保存 value，key 和 value 都可以是任何引用类型的数据。

Map 的 key 不允许重复，任何两个 key 通过 equals 方法比较总是返回 false，如果把 Map 里的所有 key 放在一起来看，它们就组成了一个 Set 集合 (所有的 key 没有顺序，不能重复)。其实 Map 的实现类和子接口中的 key 集的存储形式和对应 Set 集合中元素的存储形式完全相同。

Set 与 Map 间的关系非常密切，如果把 key-value 对中的 value 当成 key 的附庸，就可以像对待 Set 一样来对待 Map 了。而事实上 Map 提供了一个 Entry 内部类来封装 key-value 对，而计算 Entry 存储时则只考虑 Entry 封装的 key。**从 Java 源码来看，Java 是先实现了 Map，然后通过包装一个所有 value 都为 null 的 Map 就实现了 Set 集合**。

从另一个角度来看，如果把 Map 里的所有 value 放一起来看，它们又像一个 List：元素可重复，可通过索引来查找，只不过 Map 中的索引不再使用整数值，而是以另一个对象作为索引，即 key 索引。所以，Map 有时也被称为「字典」或者「关联数组」。

Map 集合最经典的用法就是成对地添加、删除 key-value 对，当添加 key-value 对时，如果 Map 中已有重复的 key，那么新添加的 value 会覆盖该 key 原来的 value 值，该方法将返回被覆盖的 value 值。

### HashMap 和 Hashtable 类

二者都是 Map 接口的典型实现类，它们之间的关系完全类似于 ArrayList 和 Vector 的关系 (同 Vector 一样，Hashtable 也是一个从 JDK 1.0 就已经出现的古老的 Map 实现类，『甚至于它的命名都没遵守 Java 的命名规范...』)。

Hashtable 是一个线程安全的 Map 实现，HashMap 是线程不安全的实现，但也因此性能高于 Hashtable。

Hashtable 不允许使用 null 作为 key 和 value，否则引发空指针异常，但 HashMap 可以使用 null 作为 key 或 value。

### Properties 读写属性文件

Properties 类是 Hashtable 类的子类，可以把 Map 对象和属性文件关联起来。由于属性文件里的属性名、属性值只能是字符串类型，所以 Properties 相当于一个 key、value 都是 String 类型的 Map。

### SortedMap 接口和 TreeMap 实现类

二者关系同 SortedSet 接口和 TreeSet 实现类一样。

### WeakHashMap 实现类

与 HashMap 类似，区别在于 HashMap 的 key 保留了对实际对象的强引用，只要该 HashMap 对象不被销毁，该 HashMap 的所有 key 引用的对象就不会被垃圾回收，HashMap 也不会自动删除这些 key 所对应的 key-value 对。

而 WeakHashMap 的 key 只保留了对实际对象的弱引用，这些 key 所引用的对象可能被垃圾回收，WeakHashMap 会自动删除该 key 对应的 key-value 对。

```java
WeakHashMap whm = new WeakHashMap();
whm.put(new String("Sun"), new String("sun"));
whm.put(new String("Mon"), new String("rain"));
whm.put(new String("Tue"), new String("wind"));
whm.put("Fri", new String("sun"));
System.out.println(whm);
// 通知系统垃圾回收
System.gc();
System.runFinalization();
System.out.println(whm);

// 输出 (一般情况下第二次输出就只有一个键值对):
// {Sun=sun, Mon=rain, Tue=wind, Fri=sun}
// {Fri=sun}
```

### IdentityHashMap 实现类

实现机制与 HashMap 类似，但它在处理两个 key 相等时判断机制不同，只有当两个 key 严格相等时 (key1 == key2)，才认为二个 key 相等，而对于普通 HashMap 而言，只要 `equals()` 方法返回 true 以及二者 hashCode 值相等即可。

## 泛型

Java 增加泛型支持很大程度上是为了让集合能记住元素的数据类型。在没有泛型之前，把一个对象「丢进」集合中，集合就会忘记对象类型，把所有对象当成 Object 类型处理，当需要取出时需要进行强制类型转换，不仅使代码臃肿，而且容易引发 ClassCastExeception 异常。

> 泛型设计的原则：只要代码在编译时没有出现警告，就不会遇到运行时 ClassCastException 异常。

### 泛型语法

在 Java 5 后引入了「参数化类型」即「泛型」，其语法为：

```java
List<String> strList = new ArrayList<String>();
```

但 Java 7 后，简化了语法，无须在构造器后带泛型信息，可自动推断，被称之为「菱形语法」：

```java
List<String> strList = new ArrayList<>();
```

### 类型通配符

> 如果 Foo 是 Bar 的一个子类或子接口，而 G 是具有泛型声明的类或接口，`G<Foo>` 并不是 `G<Bar>` 的子类型

为了表示各类泛型 List 的父类，可以使用类型通配符，将一个 `?` 作为类型实参传给 List 集合，语法为：`List<?>` (意为元素类型未知的 List)。这个 `?` 被称为通配符。

看下面一个例子 (Foo 是 Bar 的一个子类或子接口)：

```java
public void test(List<Bar> barList) {
    for (Bar bar : barList) {
        ...
    }
}

List<Foo> fooList = new ArrayList<>();
test(fooList);
```

由于 `List<Foo>` 并不是 `List<Bar>` 的子类型，上述代码将引发编译错误。为了表示 `List<Foo>` 的父类，可以考虑使用 `List<?>` 通配符来表示所有类型。

```java
public void test(List<?> barList) {
    for (Object obj : barList) {
        Bar bar = (Bar) obj;
        ...
    }
}

List<Foo> fooList = new ArrayList<>();
test(fooList);
```

但是这种实现方法显得臃肿繁琐，使用了泛型还要进行强制类型转换，通过**设定通配符上限**用以解决此类问题。

```java
public void test(List<? extends Bar> barList) {
    for (Bar bar : barList) {
        ...
    }
}

List<Foo> fooList = new ArrayList<>();
test(fooList);
```

对应的，有通配符上限就有通配符下限，语法为 `<? super SubClass>`。

### 泛型方法

先引出一个问题，下面方法将一个 Object 数组所有元素添加至 Collection 集合：

```java
static void fromArrayToCollection(Object[] a, Collection<Object> c) {
    for (Object o : a) {
        c.add(o);
    }
}
```

但是这个方法功能有限，它只能将 Object[] 数组的元素复制到元素为 Object (其子类也不行) 的 Collection 集合中：

```java
String[] str = {"a", "b"};
List<String> strList = new ArrayList<>();
// 引发编译错误
fromArrayToCollection(str, strList);
```

上述方法的参数类型不能使用 Collection<String>，即使使用通配符 Collection<?> 也不行，因为 Java 不允许把对象放进一个未知类型的集合中，Java 5 提供的泛型方法即可解决这个问题。

```java
static <T> void fromArrayToCollection(T[] a, Collection<T> c) {
    for (T o : a) {
        c.add(o);
    }
}
```

泛型方法的语法为：类型形参声明放置于方法修饰符和方法返回值类型之间。

## 异常处理

Java 将异常分为两种，Checked 异常和 Runtime 异常，并认为 Checked 异常都是可以在编译阶段被处理的异常，所以它强制要求程序处理所有的 Checked 异常，而 Runtime 异常则无须处理。

异常捕获处理应先处理小异常，再处理大异常。

### 声明抛出异常

使用 throws 声明抛出异常，表示当前方法不知如何处理这种类型的异常，并交由上一级调用者处理，若 main 方法也不知如何处理，通过 throws 声明抛出异常后，将交由 JVM 处理，即打印异常的跟踪栈信息，并终止程序运行。

调用声明抛出异常的方法时，要么将该方法放在 try 块中显式捕获该异常，要么放在另一个带 throws 声明抛出的方法中。

```java
public class ThrowsTest {
    public static void main(String[] args) throws Exception {
        test();
    }

    public static void test() throws IOException {
        FileInputStream fis = new FileInputStream("a.txt");
    }
}
```

> throws 声明抛出只能在方法签名中使用

### 多异常捕获

Java 7 提供了多异常捕获，一个 catch 块可以捕获多种类型的异常，各类异常用 `|` 分隔，**但要注意捕获多种异常时，异常变量有隐式的 final 修饰，因此程序不能对异常变量重新赋值。**

```java
try {
    ...
} catch(IndexOutOfBoundsException|NumberFormatException|ArithmeticException ie) {
    // 以下代码错误，多异常捕获其异常变量默认有 final 修饰
    ie = new ArithmeticException("test");
} catch(Exception e) {
    // 单异常捕获，异常变量没有 final 修饰，代码正确
    e = new RuntimeException("test");
}
```

### 自动关闭资源的 try 语句

Java 7 增强了 try 语句功能，使得无须手动在 finally 块中显式关闭一些资源，这种增强型的 try 语句相当于包含了隐式的 finally 块。其语法是在 try 后紧跟圆括号，里面可以声明、初始化一个或多个资源 (即需要在程序结束时显式关闭的资源，如数据库、网络连接等)，try 语句在该语句结束时自动关闭这些资源。

```java
public class AutoCloseTest {
    public static void main(String[] args) throws IOException {
        try (
            BufferedReader br = new BufferedReader(new FileReader("AutoCloseTest.java"));
            PrintStream ps = new PrintStream(new FileOutputStream("a.txt"));
            ) {
                System.out.println(br.readLine());
                ps.println("itsCoder");
            }
    }
}
```

需要指出的是，为保证 try 语句可以正常关闭资源，这些资源实现类必须实现了 AutoCloseable 接口或其子接口 Closeable，即实现其接口的 `close()` 方法，而 Closeable 接口里的 `close()` 方法声明抛出了 IOException 异常，因此它的实现类在实现 `close()` 时只能声明抛出 IOException 或其子类，而 AutoCloseable 接口里的 `close()` 方法声明抛出 Exception 异常，因此它的实现类在实现 `close()` 时可声明抛出任何异常。

> Java 7 几乎把所有的「资源类」进行了改写，都实现了 AutoCloseable 或 closeable 接口。

## 注解

Annotation (即注解) 为代码里的一种特殊标记，这种标记**可以在编译、类加载、运行时被读取，并执行相应的处理**。通过使用注解，可以在不改变原有逻辑的情况下，在源文件中嵌入一些补充信息。代码分析工具、开发工具和部署工具可以通过这些补充信息进行验证或者部署。

Annotation 不影响程序代码的执行，无论增加、删除 Annotation，代码都始终如一地执行。如果希望让程序中的 Annotation 在运行时起一定的作用，只有通过某种配套的工具对 Annotation 中的信息进行访问和处理，**访问和处理 Annotation 的工具统称 APT (Annotation Processing Tool)**。

### 自定义注解

定义一个注解使用 `@interface` 关键字，与定义一个接口类似：

```java
// 定义一个 @Test 注解
public @interface Test {

}

public class MyClass {
    // info() 方法使用 @Test 注解
    @Test
    public void info() {
        ...
    }
}
```

如果注解带有成员变量，其声明方式同接口声明方法类似，方法名代表变量名，返回值类型代表变量类型。而一旦指定成员变量，使用该注解时就应该指定相应的值，除非使用 default 指定了默认初始值：

```java
public @interface MyTag {
    String name();
    int age() default 18;
}

public class MyClass {
    @MyTag(name="showzeng")
    public void info() {
        ...
    }
}
```

Java 根据有无定义成员变量将注解分为两类：

| 类别 | 有无定义成员变量 | 作用 |
|:--:|:--:|:--:|
| 标记 Annotation | 无 | 利用自身的存在与否来提供信息 |
| 元数据 Annotation | 有 | 可以接收更多的元数据进行一些操作 |

### 基本注解

Java 提供了 5 个基本的注解，其中 `@SafeVarargs` 是 Java 7 新增的，`@FunctionalInterface` 是 Java 8 新增的，这 5 个基本的注解都定义在 java.lang 包下。

| 注解名 | 作用 |
|--|--|
| @Override | 强制子类必须覆盖父类方法，注解会告诉编译器检查这个被注解的方法，保证父类一定包含了该方法重写的方法，以避免一些如方法签名及方法名拼写错误等低级错误 |
| @Deprecated | 标识此方法已经过时，会引发编译器的警告 |
| @SuppressWarnings | 抑制一些编译器警告，其中有一个 value 成员变量 |
| @SafeVarargs | 抑制堆污染警告 |
| @FunctionalInterface | 为 Java 8 的 Lambda 表达式所准备的，检查是否为规范的函数式接口 |

> 当 Annotation 的成员变量名为 value 时，可以直接在 Annotation 后的括号里指定该成员变量的值，无须使用「name=value」的形式

```java
// 关闭整个类里的编译器警告，两种用法均可
// @SuppressWarnings("unchecked")
@SuppressWarnings(value="unchecked")
public class Test {
    ...
}
```

### JDK 的元 Annotation

JDK 除了提供有 5 个基本注解外，还在 java.lang.annotation 包下提供了 6 个 Meta Annotation (元 Annotation)，**所谓元注解，就是用来修饰其他自定义注解的注解**。

| 注解名 | value 成员变量值 | 作用 |
|--|--|--|
| @Retention | RetentionPolicy.CLASS | 默认值，该注解被记录在 class 文件中，运行 Java 程序时，JVM 不可获取注解信息 |
| - | RetentionPolicy.RUNTIME | 注解记录在 class 文件中，运行时 JVM 可获取注解信息，程序可通过反射来获取注解信息 |
| - | RetentionPolicy.SOURCE | 只保留在源代码中，编译器直接丢弃这种注解 |
| @Target | ElementType.ANNOTATION_TYPE | 指定该注解只能用来修饰注解 |
| - | ElementType.CONSTRUCTOR | 指定该注解只能用来修饰构造器 |
| - | ElementType.FIELD | 指定该注解只能用来修饰成员变量 |
| - | ElementType.LOCAL_VARIABLE | 指定该注解只能用来修饰局部变量 |
| - | ElementType.METHOD | 指定该注解只能用来修饰方法定义 |
| - | ElementType.PACKAGE | 指定该注解只能用来修饰包定义 |
| - | ElementType.PARAMETER | 指定该注解只能用来修饰参数 |
| - | ElementType.TYPE | 指定该注解可以修饰类、接口 (包括注解)或枚举定义 |
| @Documented | - | 指定该注解将被 javadoc 工具提取成文档 |
| @Inherited | - | 指定该注解修饰的类，其子类将默认继承该注解 |
| @Repeatable | - | 指定该注解可重复修饰程序元素 |

例如，定义一个自定义注解，该注解用来修饰成员变量，并能够保留到运行时，通过反射获取信息：

```java
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
public @interface BindView {
    int value();
}

public class Activity {
    @BindView(R.id.btn_login)
    Button buttonLogin;

    ...
}
```

### 重复注解

重复注解是 Java 8 新增的一个特性，在此前同一个程序元素最多只能使用一个相同类型的注解，如果需要使用多个，必须使用「Annotation 容器」，例如在 Struts 2 开发中，需在 Action 类上使用多个 @Result 注解：

```java
@Results({@Result(name="failure", location="failed.jsp"),
        @Result(name="success", location="succ.jsp")})
public Action FooAction { ... }
```

其实重复注解也是使用注解容器，只是简化了使用时的写法。在使用 @Repeatable 修饰注解成为可重复注解时，必须为 value 成员变量指定值，该成员变量值即为「容器注解」—— 该容器注解可包含多个当前注解，如下示例所示：

```java
// 可重复注解 @MyTag
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@Repeatable(MyTags.class)
public @interface MyTag {
    String name() default "itsCoder";
    int age();
}

// @MyTag 的注解容器
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface MyTags {
    MyTag[] value();
}

// 使用可重复注解 @MyTag 修饰程序元素，写法简化
@MyTag(age=3)
@MyTag(name="showzeng", age=18)
public class Test { ... }
```

### 提取注解信息

使用注解修饰了类、方法、成员变量等成员后，注解不会自己生效，必须由开发者提供相应的工具来提取并处理注解信息。

Java 使用 Annotation 接口来代表程序元素面前的注解，**该接口是所有注解的父接口**。在 java.lang.reflect 包下主要包含一些实现反射功能的工具类。

而从 Java 5 开始，java.lang.reflect 包提供的反射 API 增加了读取运行时 Annotation (定义时使用了 `@Retention(RetentionPolicy.RUNTIME)` 修饰) 的能力，此外还新增了 AnnotatedElement 接口，该接口代表程序中可以接受注解的程序元素，主要有以下几个实现类：

- Class：类定义
- Constructor：构造器定义
- Field：类的成员变量定义
- Method：类的方法定义
- Package：类的包定义

也由于 AnnotatedElement 接口是所有程序元素 (如 Class、Method、Constructor 等) 的父接口，所以程序可通过反射来获取某个类的 AnnotatedElement 对象 (如 Class、Method、Constructor 等) 之后，调用该对象的一些方法来访问注解信息。

| 方法 | 作用 |
|--|--|
| \<A extends Annotation\> A getAnnotation(Class\<A\> annotationClass) | 返回该程序元素上存在的指定类型的注解，不存在返回 null |
| \<A extends Annotation\> A getDeclaredAnnotation(Class\<A\> annotationClass) | Java 8 新增方法，获取直接修饰该程序元素、指定类型的注解，不存在返回 null |
| Annotation[] getAnnotations() | 返回该程序元素上存在的所有注解 |
| Annotation[] getDeclaredAnnotations() | 返回直接修饰该程序元素的所有注解 |
| boolean isAnnotationPresent(Class\<? extends Annotation\> annotationClass) | 判断该程序元素上是否存在指定类型的注解 |
| \<A extends Annotation\> A[] getAnnotationsByType(Class\<A\> annotationClass) | 与 getAnnotation() 方法相似，但因 Java 8 新增了重复注解功能，需使用此方法获取修饰该程序元素、指定类型的多个注解 |
| \<A extends Annotation\> A[] getDeclaredAnnotationsByType(Class\<A\> annotionClass) | 与 getDeclaredAnnotations() 方法相似，同理同上 |

下面是一个提取『Button id』的例子：

```java
// ~/annotationtest/BindView.java
import java.lang.annotation.*;

@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
public @interface BindView {
    int value();
}

// ~/annotationtest/Activity.java
import java.lang.annotation.*;

public class Activity {

    @BindView(18)
    Button button;

    public static void main(String[] args) throws ClassNotFoundException, NoSuchFieldException {
        Annotation[] annotationArray = Class.forName("Activity").getDeclaredField("button").getAnnotations();
        for (Annotation annotation : annotationArray ) {
            if (annotation instanceof BindView) {
                System.out.println("Button's id is: " + ((BindView) annotation).value());
            }
        }
    }
}

class Button { }
```

编译运行可得以下结果：

```
➜ annotationtest $ javac Activity.java BindView.java
➜ annotationtest $ java Activity                    
Button's id is: 18
```

### 编译时处理注解

很多时候运行时通过反射来获取注解，并进行一些处理，性能可能不是很好，如果在编译时就对注解进行一些操作是不是就要好一些呢。Java 的编译工具 javac 有一个 `-processor` 选项，用于指定一个「注解处理器」，那么我们就可以通过自定义一个注解处理器来实现在编译时对注解进行处理操作，用来生成源文件或其他的附属文件等。要实现一个注解处理器需要实现 javax.annotation.processing 包下的 Processor 接口，不过实现该接口需实现的方法过多，所以通常会采用继承 AbstractProcessor 的方式来实现注解处理器。

在编译时处理的注解，就不需要同反射一样存留时间保持到运行时，只需要在源文件中存留即可：

```java
@Retention(RetentionPolicy.SOURCE)
```

同时，注解处理器是通过 RoundEnvironment 来获取注解信息的，具体使用方法可查阅官方文档。
