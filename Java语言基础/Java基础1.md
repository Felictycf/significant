



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
1.变量的分类
1.1 按数据类型分类

详细说明：
//1. 整型：byte(1字节=8bit) \ short(2字节) \ int(4字节) \ long(8字节)
		//① byte范围：-128 ~ 127
	// ② 声明long型变量，必须以"l"或"L"结尾
		// ③ 通常，定义整型变量时，使用int型。
       //④整型的常量，默认类型是：int型
//2. 浮点型：float(4字节) \ double(8字节)
		//① 浮点型，表示带小数点的数值
		//② float表示数值的范围比long还大
	//③ 定义float类型变量时，变量要以"f"或"F"结尾
	//④ 通常，定义浮点型变量时，使用double型。
	//⑤ 浮点型的常量，默认类型为：double
//3. 字符型：char (1字符=2字节)
		//① 定义char型变量，通常使用一对'',内部只能写一个字符
	//② 表示方式：1.声明一个字符 2.转义字符 3.直接使用 Unicode 值来表示字符型常量
//4.布尔型：boolean
	//① 只能取两个值之一：true 、 false
	//② 常常在条件判断、循环结构中使用
1.2 按声明的位置分类(了解)


2.定义变量的格式：
数据类型  变量名 = 变量值;
或
数据类型  变量名;
变量名 = 变量值;

3.变量使用的注意点：
   ① 变量必须先声明，后使用
   ② 变量都定义在其作用域内。在作用域内，它是有效的。换句话说，出了作用域，就失效了
   ③ 同一个作用域内，不可以声明两个同名的变量
4.基本数据类型变量间运算规则
4.1 涉及到的基本数据类型：除了boolean之外的其他7种
4.2 自动类型转换(只涉及7种基本数据类型）
结论：当容量小的数据类型的变量与容量大的数据类型的变量做运算时，结果自动提升为容量大的数据类型。
	byte 、char 、short --> int --> long --> float --> double 
	特别的：当byte、char、short三种类型的变量做运算时，结果为int型
说明：此时的容量大小指的是，表示数的范围的大和小。比如：float容量要大于long的容量

4.3 强制类型转换(只涉及7种基本数据类型）：自动类型提升运算的逆运算。
1.需要使用强转符：()
2.注意点：强制类型转换，可能导致精度损失。
4.4 String与8种基本数据类型间的运算
1. String属于引用数据类型,翻译为：字符串
2. 声明String类型变量时，使用一对""
3. String可以和8种基本数据类型变量做运算，且运算只能是连接运算：+
4. 运算的结果仍然是String类型
避免：
String s = 123;//编译错误
String s1 = "123";
int i = (int)s1;//编译错误


    
```

在我们进行实现某些算法的时候，一定要看好数据类型，如果计算的过程中可能会出现小数点时，一定要把所有参与的类型变成浮点型

![image-20211115144441713](https://raw.githubusercontent.com/Felictycf/images/main/img/image-20211115144441713.png)

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

### 6.关键字与标识符

1.

![image-20211115142608832](https://raw.githubusercontent.com/Felictycf/images/main/img/image-20211115142608832.png)

![image-20211115142626285](https://raw.githubusercontent.com/Felictycf/images/main/img/image-20211115142626285.png)

2.保留字：现Java版本尚未使用，但以后版本可能会作为关键字使用。
具体哪些保留字：goto 、const
注意：自己命名标识符时要避免使用这些保留字

### 7.进制

1.编程中涉及的进制及表示方式：

![image-20211115144728387](https://raw.githubusercontent.com/Felictycf/images/main/img/image-20211115144728387.png)

2.二进制的使用说明：
2.1 计算机底层的存储方式：所有数字在计算机底层都以二进制形式存在。
2.2 二进制数据的存储方式：所有的数值，不管正负，底层都以补码的方式存储。
2.3 原码、反码、补码的说明：
正数：三码合一
负数：

![image-20211115144744805](https://raw.githubusercontent.com/Felictycf/images/main/img/image-20211115144744805.png)

3.进制间的转换

![image-20211115144756722](https://raw.githubusercontent.com/Felictycf/images/main/img/image-20211115144756722.png)

![image-20211115144804675](https://raw.githubusercontent.com/Felictycf/images/main/img/image-20211115144804675.png)

![image-20211115144811301](https://raw.githubusercontent.com/Felictycf/images/main/img/image-20211115144811301.png)

![image-20211115144818691](https://raw.githubusercontent.com/Felictycf/images/main/img/image-20211115144818691.png)

3.3 图示十进制转换为二进制：

![image-20211115144904241](https://raw.githubusercontent.com/Felictycf/images/main/img/image-20211115144904241.png)

3.4 二进制与八进制、十六进制间的转换：

![image-20211115144926862](https://raw.githubusercontent.com/Felictycf/images/main/img/image-20211115144926862.png)

![image-20211115144932767](https://raw.githubusercontent.com/Felictycf/images/main/img/image-20211115144932767.png)

### 8.算术运算符

1.算术运算符： + - + - * / % (前)++ (后)++ (前)-- (后)-- + 
【典型代码】
		//除号：/
		int num1 = 12;
		int num2 = 5;
		int result1 = num1 / num2;
		System.out.println(result1);//2
		// %:取余运算
		//结果的符号与被模数的符号相同
		//开发中，经常使用%来判断能否被除尽的情况。
		int m1 = 12;
		int n1 = 5;
		System.out.println("m1 % n1 = " + m1 % n1);

		int m2 = -12;
		int n2 = 5;
		System.out.println("m2 % n2 = " + m2 % n2);
	
		int m3 = 12;
		int n3 = -5;
		System.out.println("m3 % n3 = " + m3 % n3);
	
		int m4 = -12;
		int n4 = -5;
		System.out.println("m4 % n4 = " + m4 % n4);
		//(前)++ :先自增1，后运算
		//(后)++ :先运算，后自增1
		int a1 = 10;
		int b1 = ++a1;
		System.out.println("a1 = " + a1 + ",b1 = " + b1);
		
		int a2 = 10;
		int b2 = a2++;
		System.out.println("a2 = " + a2 + ",b2 = " + b2);
		
		int a3 = 10;
		++a3;//a3++;
		int b3 = a3;
		//(前)-- :先自减1，后运算
		//(后)-- :先运算，后自减1
		
		int a4 = 10;
		int b4 = a4--;//int b4 = --a4;
		System.out.println("a4 = " + a4 + ",b4 = " + b4);
【特别说明的】
1.//(前)++ :先自增1，后运算
 //(后)++ :先运算，后自增1
2.//(前)-- :先自减1，后运算
  //(后)-- :先运算，后自减1
3.连接符：+：只能使用在String与其他数据类型变量之间使用。

2.赋值运算符

2.赋值运算符：=  +=  -=  *=  /=  %= 
【典型代码】
		int i2,j2;
		//连续赋值
		i2 = j2 = 10;
		//***************
		int i3 = 10,j3 = 20;
		int num1 = 10;
		num1 += 2;//num1 = num1 + 2;
		System.out.println(num1);//12

		int num2 = 12;
		num2 %= 5;//num2 = num2 % 5;
		System.out.println(num2);
	
		short s1 = 10;
		//s1 = s1 + 2;//编译失败
		s1 += 2;//结论：不会改变变量本身的数据类型
		System.out.println(s1);

【特别说明的】
1.运算的结果不会改变变量本身的数据类型
2.
		//开发中，如果希望变量实现+2的操作，有几种方法？(前提：int num = 10;)
		//方式一：num = num + 2;
		//方式二：num += 2; (推荐)
		
		//开发中，如果希望变量实现+1的操作，有几种方法？(前提：int num = 10;)
		//方式一：num = num + 1;
		//方式二：num += 1; 
		//方式三：num++; (推荐)

【特别说明的】
1.运算的结果不会改变变量本身的数据类型
2.
		//开发中，如果希望变量实现+2的操作，有几种方法？(前提：int num = 10;)
		//方式一：num = num + 2;
		//方式二：num += 2; (推荐)
		

		//开发中，如果希望变量实现+1的操作，有几种方法？(前提：int num = 10;)
		//方式一：num = num + 1;
		//方式二：num += 1; 
		//方式三：num++; (推荐)

3.三元运算符

6.三元运算符：(条件表达式)? 表达式1 : 表达式2
【典型代码】
1.获取两个整数的较大值
2.获取三个数的最大值
【特别说明的】
1. 说明
  ① 条件表达式的结果为boolean类型
  ② 根据条件表达式真或假，决定执行表达式1，还是表达式2.
    如果表达式为true，则执行表达式1。
    如果表达式为false，则执行表达式2。
  ③ 表达式1 和表达式2要求是一致的。
  ④ 三元运算符可以嵌套使用
2. 
  凡是可以使用三元运算符的地方，都可以改写为if-else
  反之，不成立。
3. 如果程序既可以使用三元运算符，又可以使用if-else结构，那么优先选择三元运算符。原因：简洁、执行效率高。



