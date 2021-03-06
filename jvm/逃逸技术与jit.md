[TOC]

在《[Java堆内存是线程共享的！面试官：你确定吗？](https://link.zhihu.com/?target=http%3A//mp.weixin.qq.com/s%3F__biz%3DMzI3NzE0NjcwMg%3D%3D%26mid%3D2650126638%26idx%3D1%26sn%3D5fe5146a66e14d9d39ff464f60ac58c7%26chksm%3Df36ba40fc41c2d19359e1db1e3340fd19b549e441c99a15b33722b79277b3daf9f180b2e7bb9%26scene%3D21%23wechat_redirect)》文章中，我们也介绍过，一个Java对象在堆上分配的时候，主要是在Eden区上，如果启动了TLAB的话会优先在TLAB上分配，少数情况下也可能会直接分配在老年代中，分配规则并不是百分之百固定的，这取决于当前使用的是哪一种垃圾收集器，还有虚拟机中与内存有关的参数的设置。

但是一般情况下是遵循以下原则的：

- 对象优先在Eden区分配

- - 优先在Eden分配，如果Eden没有足够空间，会触发一次Monitor GC

- 大对象直接进入老年代

- - 需要大量连续内存空间的Java对象，当对象需要的内存大于-XX：PretenureSizeThreshold参数的值时，对象会直接在老年代分配内存。

**但是，虽然虚拟机规范中是有着这样的要求，但是各个虚拟机厂商在实现虚拟机的时候，可能会针对对象的内存分配做一些优化。这其中最典型的就是HotSpot虚拟机中的JIT技术的成熟，使得对象在堆上分配内存并不是一定的。**

其实在《深入理解Java虚拟机》中，作者也提出过类似的观点，因为JIT技术的成熟使得"对象在堆上分配内存"就不是那么绝对的了。但是书中并没有展开介绍到底什么是JIT，也没有介绍JIT优化到底做了什么。那么接下来我们就来深入了解一下：

## **JIT 技术**

我们大家都知道，通过 javac 将可以将Java程序源代码编译，转换成 java 字节码，JVM 通过解释字节码将其翻译成对应的机器指令，逐条读入，逐条解释翻译。这就是传统的JVM的解释器（Interpreter）的功能。很显然，**Java编译器经过解释执行，其执行速度必然会比直接执行可执行的二进制字节码慢很多。为了解决这种效率问题，引入了 JIT（Just In Time ，即时编译） 技术。**

有了JIT技术之后，Java程序还是通过解释器进行解释执行，当JVM发现某个方法或代码块运行特别频繁的时候，就会认为这是“热点代码”（Hot Spot Code)。然后JIT会把部分“热点代码”翻译成本地机器相关的机器码，并进行优化，然后再把翻译后的机器码缓存起来，以备下次使用。

### **热点检测**

上面我们说过，要想触发JIT，首先需要识别出热点代码。目前主要的热点代码识别方式是热点探测（Hot Spot Detection），HotSpot虚拟机中采用的主要是基于计数器的热点探测

> 基于计数器的热点探测（Counter Based Hot Spot Detection)。采用这种方法的虚拟机会为每个方法，甚至是代码块建立计数器，统计方法的执行次数，某个方法超过阀值就认为是热点方法，触发JIT编译。

### **编译优化**

JIT在做了热点检测识别出热点代码后，除了会对其字节码进行缓存，还会对代码做各种优化。这些优化中，比较重要的几个有：逃逸分析、 锁消除、 锁膨胀、 方法内联、 空值检查消除、 类型检测消除、 公共子表达式消除等。

而这些优化中的逃逸分析就和本文要介绍的内容有关了。

## **逃逸分析**

逃逸分析(Escape Analysis)是目前Java虚拟机中比较前沿的优化技术。这是一种可以有效减少Java 程序中同步负载和内存堆分配压力的跨函数全局数据流分析算法。通过逃逸分析，Hotspot编译器能够分析出一个新的对象的引用的使用范围从而决定是否要将这个对象分配到堆上。

**逃逸分析的基本行为就是分析对象动态作用域：当一个对象在方法中被定义后，它可能被外部方法所引用，例如作为调用参数传递到其他地方中，称为方法逃逸。**

例如：

```text
public static String craeteStringBuffer(String s1, String s2) {

    StringBuffer sb = new StringBuffer();

    sb.append(s1);

    sb.append(s2);

    return sb.toString();

}
```

sb是一个方法内部变量，上述代码中并没有将他直接返回，这样这个StringBuffer又不会被其他方法所改变，这样它的作用域就只是在方法内部。我们就可以说这个变量并没有逃逸到方法外部。

有了逃逸分析，我们可以判断出一个方法中的变量是否有可能被其他线程所访问或者改变，那么基于这个特性，JIT就可以做一些优化：

- 同步省略
- 标量替换
- 栈上分配

关于同步省略，大家可以参考我之前的《[深入理解多线程（五）—— Java虚拟机的锁优化技术](https://link.zhihu.com/?target=https%3A//mp.weixin.qq.com/s%3F__biz%3DMzI3NzE0NjcwMg%3D%3D%26mid%3D2650121186%26idx%3D1%26sn%3D248d37be27d3bbeb103464b2a96a0ae4%26scene%3D21%23wechat_redirect)》中关于锁消除技术的介绍。本文主要来分析下标量替换和栈上分配。

## **标量替换、栈上分配**

我们说，JIT经过逃逸分析之后，如果发现某个对象并没有逃逸到方法体之外的话，就可能对其进行优化，而这一优化最大的结果就是可能改变Java对象都是在堆上分配内存的这一原则。

对象要分配在堆上其实有很多原因，但是有一点比较关键的和本文有关的，那就是因为堆内存在访问上是线程共享的，这样一个线程创建出来的对象，其他线程也能访问到。

**那么，试想下，如果我们在某一个方法体内部创建了一个对象，并且对象并没有逃逸到方法外的话，那还有必要一定要把对象分配到堆上吗？**

其实就没有必要了，因为这个对象并不会被其他线程所访问到，生命周期也只是在一个方法内部，也就不用大费周折的在堆上分配内存，也减少了内存回收的必要。

那么，有了逃逸分析之后，发现一个对象并没有逃逸到放法外的话，通过什么办法可以进行优化，减少对象在堆上分配可能呢？

这就是栈上分配。在HotSopt中，栈上分配并没有正在的进行实现，而是通过标量替换来实现的。

所以我们重点介绍下，什么是标量替换，如何通过标量替换实现栈上分配。

### **标量替换**

标量（Scalar）是指一个无法再分解成更小的数据的数据。Java中的原始数据类型就是标量。相对的，那些还可以分解的数据叫做聚合量（Aggregate），Java中的对象就是聚合量，因为他可以分解成其他聚合量和标量。

**在JIT阶段，如果经过逃逸分析，发现一个对象不会被外界访问的话，那么经过JIT优化，就会把这个对象拆解成若干个其中包含的若干个成员变量来代替。这个过程就是标量替换。**

```text
public static void main(String[] args) {

   alloc();

}

private static void alloc() {

   Point point = new Point（1,2）;

   System.out.println("point.x="+point.x+"; point.y="+point.y);

}

class Point{

    private int x;

    private int y;

}
```

以上代码中，point对象并没有逃逸出alloc方法，并且point对象是可以拆解成标量的。那么，JIT就会不会直接创建Point对象，而是直接使用两个标量int x ，int y来替代Point对象。

```text
private static void alloc() {

   int x = 1;

   int y = 2;

   System.out.println("point.x="+x+"; point.y="+y);

}
```

可以看到，Point这个聚合量经过逃逸分析后，发现他并没有逃逸，就被替换成两个聚合量了。

**通过标量替换，原本的一个对象，被替换成了多个成员变量。而原本需要在堆上分配的内存，也就不再需要了，完全可以在本地方法栈中完成对成员变量的内存分配。**

## **实验证明**

**Talk Is Cheap, Show Me The Code**

**No Data, No BB；**

接下来我们就来通过一个实验，来看一下逃逸分析是否可以生效，生效后是否真的会发生栈上分配，而栈上分配又有什么好处呢？

我们来看以下代码：

```text
public static void main(String[] args) {

    long a1 = System.currentTimeMillis();

    for (int i = 0; i < 1000000; i++) {

        alloc();

    }

    // 查看执行时间

    long a2 = System.currentTimeMillis();

    System.out.println("cost " + (a2 - a1) + " ms");

    // 为了方便查看堆内存中对象个数，线程sleep

    try {

        Thread.sleep(100000);

    } catch (InterruptedException e1) {

        e1.printStackTrace();

    }

}

private static void alloc() {

    User user = new User();

}

static class User {

}
```

其实代码内容很简单，就是使用for循环，在代码中创建100万个User对象。

我们在alloc方法中定义了User对象，但是并没有在方法外部引用他。也就是说，这个对象并不会逃逸到alloc外部。经过JIT的逃逸分析之后，就可以对其内存分配进行优化。

我们指定以下JVM参数并运行：

```text
-Xmx4G -Xms4G -XX:-DoEscapeAnalysis -XX:+PrintGCDetails -XX:+HeapDumpOnOutOfMemoryError 
```

**其中-XX:-DoEscapeAnalysis表示关闭逃逸分析。**

在程序打印出 cost XX ms 后，代码运行结束之前，我们使用jmap命令，来查看下当前堆内存中有多少个User对象：

```text
➜  ~ jmap -histo 2809

 num     #instances         #bytes  class name
----------------------------------------------

   1:           524       87282184  [I

   2:       1000000       16000000  StackAllocTest$User

   3:          6806        2093136  [B

   4:          8006        1320872  [C

   5:          4188         100512  java.lang.String

   6:           581          66304  java.lang.Class
```

从上面的jmap执行结果中我们可以看到，堆中共创建了100万个StackAllocTest

在关闭逃逸分析的情况下（-XX:-DoEscapeAnalysis），虽然在alloc方法中创建的User对象并没有逃逸到方法外部，但是还是被分配在堆内存中。也就说，如果没有JIT编译器优化，没有逃逸分析技术，正常情况下就应该是这样的。即所有对象都分配到堆内存中。

接下来，我们开启逃逸分析，再来执行下以上代码。

```text
-Xmx4G -Xms4G -XX:+DoEscapeAnalysis -XX:+PrintGCDetails -XX:+HeapDumpOnOutOfMemoryError 
```

在程序打印出 cost XX ms 后，代码运行结束之前，我们使用jmap命令，来查看下当前堆内存中有多少个User对象：

```text
➜  ~ jmap -histo 2859 num     #instances         #bytes  class name
----------------------------------------------

   1:           524      101944280  [I

   2:          6806        2093136  [B

   3:         83619        1337904  StackAllocTest$User

   4:          8006        1320872  [C

   5:          4188         100512  java.lang.String

   6:           581          66304  java.lang.Class
```

从以上打印结果中可以发现，开启了逃逸分析之后（−XX:+DoEscapeAnalysis），在堆内存中只有8万多个StackAllocTestUser对象。也就是说在经过JIT优化之后，堆内存中分配的对象数量，从100万降到了8万。

除了以上通过jmap验证对象个数的方法以外，读者还可以尝试将堆内存调小，然后执行以上代码，根据GC的次数来分析，也能发现，开启了逃逸分析之后，在运行期间，GC次数会明显减少。正是因为很多堆上分配被优化成了栈上分配，所以GC次数有了明显的减少。

## **逃逸分析并不成熟**

前面的例子中，开启逃逸分析之后，对象数目从100万变成了8万，但是并不是0，说明JIT优化并不会完完全全的所有情况都进行优化。

关于逃逸分析的论文在1999年就已经发表了，但直到JDK 1.6才有实现，而且这项技术到如今也并不是十分成熟的。

**其根本原因就是无法保证逃逸分析的性能消耗一定能高于他的消耗。虽然经过逃逸分析可以做标量替换、栈上分配、和锁消除。但是逃逸分析自身也是需要进行一系列复杂的分析的，这其实也是一个相对耗时的过程。**

一个极端的例子，就是经过逃逸分析之后，发现没有一个对象是不逃逸的。那这个逃逸分析的过程就白白浪费掉了。虽然这项技术并不十分成熟，但是他也是即时编译器优化技术中一个十分重要的手段。

## **总结**

正常情况下，对象是要在堆上进行内存分配的，但是随着编译器优化技术的成熟，虽然虚拟机规范是这样要求的，但是具体实现上还是有些差别的。

如HotSpot虚拟机引入了JIT优化之后，会对对象进行逃逸分析，如果发现某一个对象并没有逃逸到方法外部，那么就可能通过标量替换来实现栈上分配，而避免堆上分配内存。

所以，对象一定在堆上分配内存，这是不对的。**最后，我们留一个思考题，我们之前讨论过了TLAB，今天又介绍了栈上分配。大家觉得这两个优化有什么相同点和不同点吗**



### 1.对象在栈上分配？

按照正常的套路对象不都是在堆上进行分配的吗？其实这句话说对了一半，一个对象除了可以在堆上分配在一定条件下还可以在栈（线程栈）上分配，如果非得给个原因：

假如在JVM中对象都是在堆上分配，那么当对象没有在引用链上就变成了"游离对象"，面临被GC回收的命运，问题是很多垃圾对象都是“临时工”，用了就会被直接回收掉，这种“临时工”对象一多起来，那还得了，很可能造成GC一直回收垃圾对象，这样就会影响程序性能。所以JVM设计者为了减少临时对象在堆内分配的数量，从而减少GC次数提升程序性能，JVM通过逃逸分析确定一个对象会不会被外部访问，如果不会被外部访问（也就是不会逃逸）就可以将该对象在栈上分配内存，这样这个对象所占用的内存空间就可以随栈帧出栈而销毁，减轻了垃圾回收的压力。

对象逃逸分析：就是分析对象动态作用域，当一个对象在方法中被定义后，它可能被外部方法所引用，例如被当成返回值给一个变量赋值或者作为调用参数传递到其他地方中。
————————————————
版权声明：本文为CSDN博主「1 Byte」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_40436854/article/details/109571649