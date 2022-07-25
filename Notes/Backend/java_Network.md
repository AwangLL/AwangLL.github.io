# Java Network

## 网络通信

### IP

方法被封装在`InetAddress`类中，例如`InetAddress.getByName(String)`可以获取ip地址。

|常用对象方法|解释|
|---|---|
|getCanoniclalHostName()|获取规范的名字|
|getHostAddress()|获取IP|
|getHostName()|获取域名|

### 端口

端口表示计算机上的一个程序的进程
- 不同的进程有不同的端口号，用来区分软件
- 0-65535
- TCP，UDP：65535 * 2
- 端口分类
	- 公有端口 0 - 1023
		- HTTP：80
		- HTTPS：443
		- FTP：21
		- TELENT：23
	- 程序注册端口 1024 - 49151 分配给用户或程序
		- Tomcat：8080
		- MySql：3306
		- Oracle：1521
	- 动态、私有 49152 - 65535

```bash
netstat -ano # 查看所有的端口
netstat -ano|findstr "5900" # 查看指定端口
tasklist|findstr "5900" # 查看指定端口的进程
```

利用`InetSocketAddress`类

```java
InetSocketAddress address = new InetSocketAddress("127.0.0.1". 8080);

// 获取地址
address.getAddress();
// 获取主机名
address.getHostName();
// 获取端口
address.getPort();
```

### 通信协议

> TCP/IP协议簇：实际上是一组协议

- TCP：用户传输协议
- UDP：用户数据报协议
- IP：网络互连协议

> TCP、UDP对比

TCP：
- 连接、稳定
- 三次握手、四次挥手
- 客户端、服务端
- 传输成功，释放连接，效率低
UDP：
- 不连接、不稳定
- 客户端和服务的没有明确的界限
- 不管有没有准备好，都可以发

## TCP

### TCP实现聊天

> 服务器端

```java
public class TcpServer {  
    public static void main(String[] args) {  
        ServerSocket serverSocket = null;  
        Socket socket = null;  
        InputStream is = null;  
        ByteArrayOutputStream baos = null;  
        try {  
            // 端口  
            serverSocket = new ServerSocket(9999);  
            // 等待客户端连接  
            socket = serverSocket.accept();  
            // 读取客户端消息  
            is = socket.getInputStream();  
            baos = new ByteArrayOutputStream();  
  
            byte[] buffer = new byte[1024];  
            int len;  
            while ((len = is.read(buffer)) != -1) {  
                baos.write(buffer, 0, len);  
            }  
            System.out.println(baos.toString());  
        } catch (Exception e) {  
            e.printStackTrace();  
        } finally {  
            try {  
                if (baos != null) baos.close();  
                if (is != null) is.close();  
                if (socket != null) socket.close();  
                if (serverSocket != null) serverSocket.close();  
            } catch (Exception e) {  
                e.printStackTrace();  
            }  
        }  
    }  
}
```

> 客户端

```java
public class TcpClient {  
    public static void main(String[] args) {  
        Socket socket = null;  
        OutputStream os = null;  
        try {  
            // 服务器地址、端口号  
            InetAddress address = InetAddress.getByName("127.0.0.1");  
            int port = 9999;  
            // 创建Socket连接  
            socket = new Socket(address, port);  
            // 发送消息  
            os = socket.getOutputStream();  
            os.write("Hello".getBytes());  
        } catch (Exception e) {  
            e.printStackTrace();  
        } finally {  
            try {  
                if(os != null) os.close();  
                if(socket != null) socket.close();  
            } catch (Exception e) {  
                e.printStackTrace();  
            }  
        }  
    }  
}
```