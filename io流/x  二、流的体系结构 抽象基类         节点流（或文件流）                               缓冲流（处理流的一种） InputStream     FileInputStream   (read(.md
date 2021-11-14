```
 二、流的体系结构
* 抽象基类         节点流（或文件流）                               缓冲流（处理流的一种）
* InputStream     FileInputStream   (read(byte[] buffer))        BufferedInputStream (read(byte[] buffer))
* OutputStream    FileOutputStream  (write(byte[] buffer,0,len)  BufferedOutputStream (write(byte[] buffer,0,len) / flush()
* Reader          FileReader (read(char[] cbuf))                 BufferedReader (read(char[] cbuf) / readLine())
* Writer          FileWriter (write(char[] cbuf,0,len)           BufferedWriter (write(char[] cbuf,0,len) / flush()
```

FileReader和Filewriter是字符流。

如果用字节流来读字符流，就有可能一个字的字节没有读完，而出现乱码的情况，这样只需要使用转换流集合

在（数组变量名，0，一次读的长度）的时候，自动使用了flush（）方法。new line 可以进行换行，

详细使用请查看Javasenior的io流部分源码