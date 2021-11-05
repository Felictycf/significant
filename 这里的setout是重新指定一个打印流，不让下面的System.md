##### 这里的setout是重新指定一个打印流，不让下面的`System.out.println`到控制台里面输出，而是重新指定的是ps，

ps是输出流，输出到("D:\\IO\\text.txt")里去



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