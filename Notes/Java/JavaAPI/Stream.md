## Java流(Stream)、文件(File)和IO

### 读取控制台输入

为了获得一个绑定到控制台的字符流，你可以把 System.in 包装在一个 BufferedReader 对象中来创建一个字符流。

```java
BufferedReader br = new BufferReader(new
					InputStreamReader(System.in));
```

BufferedReader对象创建后，可以用read()方法从控制台读取一个字符，或者用readLine()方法读取一个字符串。

### 控制台输出

控制台的输出由 print( ) 和 println() 完成。这些方法都由类 PrintStream 定义，System.out 是该类对象的一个引用。

### 读写文件

#### FileInputStream

用于从文件读取数据，他的对象可以用关键字new来创建。

```java
File f = new File("C:/java");
// 用文件对象构造
InputStream is = new FileInputStream(f);
// 用文件名构造
is = new FileInputStream("C:/java");
```

|方法|描述|
|---|---|
|read(int r)|从InputStream对象读取指定字节的数据，返回为整数值。返回下一字节数据，如果到结尾则返回-1|
|read(vyte[] r)|从输入流读取r.length长度的字节。返回的读取的字节数，如果是文件结尾则返回-1|
|avaiable()|返回下一次对此输入流调用的方法可以不受阻塞地从此输入流读取的字节|
|close()|关闭此文件输入流并释放与此流有关的所有系统资源。|
|finalize()|这个方法清除与该文件的连接。|

### FileOutputStream

|方法|描述|
|---|---|
|close()|关闭此文件输入流并释放与此流有关的所有系统资源。|
|finalize()|这个方法清除与该文件的连接。|
|write(int w)|把指定的字节写道输出流中|
|write(byte[] w)|把指定数组中w.length长度的字节写道OutputStream中|

### Scanner

可利用Scanner类来获取用户的输入

可以通过Scanner类的next()与nextLine()方法获取输入的字符串

```java
Scanner scanner = new Scanner(System.in);

// 判断是否有输入
if(scanner.hasNext()) {
	String str = scan.next();
}
// 关闭Scanner
scanner.close();
```