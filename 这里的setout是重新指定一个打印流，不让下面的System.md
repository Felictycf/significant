##### 这里的setout是重新指定一个打印流，不让下面的`System.out.println`到控制台里面输出，而是重新指定的是ps

ps是输出流，输出到("D:\\IO\\text.txt")里去

`close();`

`flush();`

<u>这在我以前的代码里有所体现，但是只知道这是清空缓冲区的作用，但是不知道具体什么是缓冲区，以及为什么要清空他，所以今天学习了一下，我简单举个例子你们就知道了：

首先，咱们设想要给鱼缸换水，所以需要一个水泵，水泵是连接鱼缸和下水道的，咱们的任务就是将鱼缸里面水全抽干，这时，我们就可以**把水管当做缓冲区**。如果咱们一见鱼缸里面水抽干了就**立马关**了水泵，这时会发现水管里还有来不及通过水泵流向下水道的**残留水**，我们可以把抽水当做读数据，排水当做写数据，水管当做缓冲区，这样就容易明白了。

那么这样一来我们如果**中途调用`close()`方法**，**输出区也还是有数据的**，就像水缸里有水，只是在缓冲区遗留了一部分，这时如果我们**先调用`flush()`方法**，就会**强制把数据输出**，缓存区就清空了，最后再关闭读写流调用close()就完成了</u>

```java
System类的setIn(InputStream is) / setOut(PrintStream ps)方式重新指定输入和输出的流。
```

```java
2. 打印流：PrintStream 和PrintWriter

2.1 提供了一系列重载的print() 和 println()
2.2 练习：



 */

@Test
public void test2() {
    PrintStream ps = null;
    try {
        FileOutputStream fos = new FileOutputStream(new File("D:\\IO\\text.txt"));
        // 创建打印输出流,设置为自动刷新模式(写入换行符或字节 '\n' 时都会刷新输出缓冲区)
        ps = new PrintStream(fos, true);
        if (ps != null) {// 把标准输出流(控制台输出)改成文件
            System.setOut(ps);
        }


        for (int i = 0; i <= 255; i++) { // 输出ASCII字符
            System.out.print((char) i);
            if (i % 50 == 0) { // 每50个数据一行
                System.out.println(); // 换行
            }
        }


    } catch (FileNotFoundException e) {
        e.printStackTrace();
    } finally {
        if (ps != null) {
            ps.close();
        }
    }

}
```