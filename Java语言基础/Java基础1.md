



## 1.进制的说明

```java
/*
计算机中不同进制的使用说明

对于整数，有四种表示方式：
> 二进制(binary)：0,1 ，满2进1.以0b或0B开头。
> 十进制(decimal)：0-9 ，满10进1。
> 八进制(octal)：0-7 ，满8进1. 以数字0开头表示。
> 十六进制(hex)：0-9及A-F，满16进1. 以0x或0X开头表示。此处的A-F不区分大小写。
    如：0x21AF +1= 0X21B0
```

### 2.标识符的规范，命名规范

```java
/*
标识符的使用
1.标识符：凡是自己可以起名字的地方都叫标识符。
   比如：类名、变量名、方法名、接口名、包名...

2.标识符的命名规则：--> 如果不遵守如下的规则，编译不通过！需要大家严格遵守

> 由26个英文字母大小写，0-9 ，_或 $ 组成  
> 数字不可以开头。
> 不可以使用关键字和保留字，但能包含关键字和保留字。
> Java中严格区分大小写，长度无限制。
> 标识符不能包含空格。

3. Java中的名称命名规范： --->如果不遵守如下的规范，编译可以通过！建议大家遵守

包名：多单词组成时所有字母都小写：xxxyyyzzz
类名、接口名：多单词组成时，所有单词的首字母大写：XxxYyyZzz
变量名、方法名：多单词组成时，第一个单词首字母小写，第二个单词开始每个单词首字母大写：xxxYyyZzz
常量名：所有字母都大写。多单词时每个单词用下划线连接：XXX_YYY_ZZZ

4.
注意1：在起名字时，为了提高阅读性，要尽量有意义，“见名知意”。
注意2：java采用unicode字符集，因此标识符也可以使用汉字声明，但是不建议使用。 
```

![image-20211115142714325](https://raw.githubusercontent.com/Felictycf/images/main/img/image-20211115142714325.png)

![image-20211115142846673](https://raw.githubusercontent.com/Felictycf/images/main/img/image-20211115142846673.png)

​     在起名字时，为了提高阅读性，要尽量意义，“见名知意”

### 3.字符串的认识

```java
/*
String类型变量的使用
1. String属于引用数据类型,翻译为：字符串
2. 声明String类型变量时，使用一对""
3. String可以和8种基本数据类型变量做运算，且运算只能是连接运算：+
4. 运算的结果仍然是String类型
```

### 4.java定义的数据类型

```java
Java定义的数据类型

一、变量按照数据类型来分：

   基本数据类型：
      整型：byte \ short \ int \ long
      浮点型：float \ double
      字符型：char
      布尔型：boolean


   引用数据类型：
      类(class)
      接口(interface)
      数组(array)
    //1. 整型：byte(1字节=8bit) \ short(2字节) \ int(4字节) \ long(8字节)
		//① byte范围：-128 ~ 127
    // ② 声明long型变量，必须以"l"或"L"结尾
		// ③ 通常，定义整型变量时，使用int型。
    //2. 浮点型：float(4字节) \ double(8字节)
		//① 浮点型，表示带小数点的数值
		//② float表示数值的范围比long还大
    //③ 定义float类型变量时，变量要以"f"或"F"结尾
    //④ 通常，定义浮点型变量时，使用double型。
    //3. 字符型：char (1字符=2字节)
		//① 定义char型变量，通常使用一对'',内部只能写一个字符
    //② 表示方式：1.声明一个字符 2.转义字符 3.直接使用 Unicode 值来表示字符型常量
    //4.布尔型：boolean
		//① 只能取两个值之一：true 、 false
		//② 常常在条件判断、循环结构中使用
    //2.编码情况2：
		//整型常量，默认类型为int型
		//浮点型常量，默认类型为double型
    
```

在我们进行实现某些算法的时候，一定要看好数据类型，如果计算的过程中可能会出现小数点时，一定要把所有参与的类型变成浮点型

### 5.成员变量和局部变量的区别

**1、在类中的位置不同**

成员变量：在类中方法外面

局部变量：在方法或者代码块中，或者方法的声明上（即在参数列表中）

**2、在内存中的位置不同，可以看看[Java程序内存的简单分析](http://www.cnblogs.com/huangminwen/p/5928315.html)**

成员变量：在堆中（方法区中的静态区）

局部变量：在栈中

成员变量与局部变量的区别

```java
 1 package demo;
 2 
 3 public class VariableDemo {
 4     String name = "成员变量";
 5 
 6     public static void main(String[] args) {
 7         new VariableDemo().show();
 8     }
 9 
10     public void show() {
11         String name = "局部变量";
12         System.out.println(name);
13     }
14 }
```

成员变量：随着对象的创建而存在，随着对象的消失而消失

局部变量：随着方法的调用或者代码块的执行而存在，随着方法的调用完毕或者代码块的执行完毕而消失

`且后面许多的代码需要把局部变量变成成员变量，因为加上某些方法和结构后后，就变成了局部变量，所有后面的量无法访问，就变成成员变量都可以访问，且在成员变量的时候，喜欢写成`

`FileOutputStream fff =null;`

`String name=null;`

以这样的方式来进行变成成员变量；

**4、初始值**

成员变量：有默认初始值

局部变量：没有默认初始值，使用之前需要赋值，否则编译器会报错（The local variable xxx may not have been initialized）

### 6.强制类型转换

```java
/*
基本数据类型之间的运算规则：

前提：这里讨论只是7种基本数据类型变量间的运算。不包含boolean类型的。

1. 自动类型提升：
    结论：当容量小的数据类型的变量与容量大的数据类型的变量做运算时，结果自动提升为容量大的数据类型。
   byte 、char 、short --> int --> long --> float --> double 

   特别的：当byte、char、short三种类型的变量做运算时，结果为int型
   
2. 强制类型转换：见VariableTest3.java


说明：此时的容量大小指的是，表示数的范围的大和小。比如：float容量要大于long的容量
3.精度缺失
/*
强制类型转换：自动类型提升运算的逆运算。
1.需要使用强转符：()
2.注意点：强制类型转换，可能导致精度损失。
举例：
double d1 = 12.9;
		//精度损失举例1
		int i1 = (int)d1;//截断操作
		System.out.println(i1);
		
		//没有精度损失
		long l1 = 123;
		short s2 = (short)l1;
		
		//精度损失举例2
		int i2 = 128;
		byte b = (byte)i2;
		System.out.println(b);//-128
```

### 8.关键字与标识符

1.

![image-20211115142608832](https://raw.githubusercontent.com/Felictycf/images/main/img/image-20211115142608832.png)

![image-20211115142626285](https://raw.githubusercontent.com/Felictycf/images/main/img/image-20211115142626285.png)

2.保留字：现Java版本尚未使用，但以后版本可能会作为关键字使用。
具体哪些保留字：goto 、const
注意：自己命名标识符时要避免使用这些保留字

