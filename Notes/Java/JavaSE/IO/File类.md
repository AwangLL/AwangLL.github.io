# File类

## 构造方法

|方法声明|功能描述|
|---|---|
|File(String pathname)|通过指定的一个字符串类型的文件路径来创建一个新的File对象
|File(String parent, String child)|根据指定的一个字符串类型的父路径和一个字符串类型的子路径（包括文件名称）创建一个File对象
|File(File parent,String child)|根据指定的File类的父路径和字符串类型的子路径（包括文件名称）创建一个File对象

```java
/* 绝对地址 */
File f = new File("D:\\file\\a.txt");
/* 相对地址 */
File f1 = new File("src\\Hello.java");
```

## File类常用方法

|方法声明|功能描述|
|---|---|
|boolean exists()|判断File对象对应的文件或目录是否存在，若存在则返回true，否则返回false
|boolean delete()|删除File对象对应的文件或目录，若删除成功则返回true否则返回false
|boolean createNewFile()|当File对象对应的文件不存在时，该方法将新建一个文件若创建成功则返回true，否则返回false
|String getName()|返回File对象表示的文件或文件夹的名称
|String getPath()|返回File对象对应的路径
|String getAbsolutePath()|返回File对象对应的绝对路径(在Unix/Linux等系统上，如果路径是以正斜线/开始，则这个路径是绝对路径;在Windows等系统上，如果路径是从盘符开始，则这个路径是绝对路径)
|String getParentFile()|返回File对象对应目录的父目录(即返回的目录不包含最后一级子目录)
|boolean canRead()|判断File对象对应的文件或目录是否可读，若可读返回true，反之返回false
|boolean canWrite()|判断File对象对应的文件或目录是否可写，若可写返回true，反之返回false
|boolean isFile()|判断File对象对应的是否是文件(不是目录)，若是文件则返回true，反之返回false
|boolean isDirectory()|判断File对象对应的是否是目录(不是文件)，若是目录则返回true，反之返回false
|boolean isAbsolute()|判断File对象对应的文件或目录是否是绝对路径
|long lastModified()|返回1970年1月1日0时0分0秒到文件最后修改时间的毫秒值
|long length()|返回文件内容的长度（单位是字节)
|String[] list()|递归列出指定目录的全部内容（包括子目录与文件），只是列出名称
|File[] listFiles()|返回一个包含了File对象所有子文件和子目录的File数组

在一些特定情况下，程序需要读写一些临时文件，为此，File类提供了createTempFile()方法和deleteOnExit()方法，用于操作临时文件。createTempFile()方法用于创建一个临时文件，deleteOnExit()方法在JVM退出时自动删除临时文件。

## 遍历文件

- 遍历指定目录下的所有文件

```java
File file = new File("D:\\file");
if(file.isDirectory()) {
	String names = file.list();
	for(String name : names) {
		System.out.println(name);
	}
}
```

- 遍历指定目录下指定扩展名的文件

```java
File file = new File("D:\\file");
// 创建过滤器对象
FilenameFilter filter = new FilenameFilter(){
	// 实现accept方法
	public boolean accept(File dir, String name) {
		File curFile = new File(dir, name);
		// 如果文件以.java结尾返回true
		if(curFile.isFile() && name.endsWith(".java")) {
			return true;
		} else {
			return false;	
		}
	}
};

if(file.exists()) {
	String[] names = file.list(filter);
	for(String name : names) {
		System.out.println(name);
	}
}

```

- 遍历包括子目录文件的所有文件

```java
public static void main(String[] args) {
	File file = new File("D:\\file");
	FileDir(file);
}

public static void FileDir(File dir) {
	// 获取表示目录下所有文件的数组
	File[] files = dir.listFiles();
	for(File file : files) {
		if(file.isDirectory()) {
			FileDir(file);
		}
		System.out.println(file);
	}
}
```

## 删除文件及目录

File类的delete()方法只能删除一个指定的文件，如果File对象代表目录，并且目录下包含子目录或文件，则File类的delete()方法不允许直接删除这个目录。在这种情况下，需要通过递归的方式将整个目录以及目录中的文件全部删除。

```java
public static void deleteDir(File dir) {
	if(dir.exists()) {
		File[] files = dir.listFiles();
		for(File file : files) {
			if(file.isDirectory()) {
				// 如果是目录，递归调用deleteDir()
				deleteDir(file);
			} else {
				// 如果是文件，直接删除
				file.delete();
			}
		}
	}
}
```

