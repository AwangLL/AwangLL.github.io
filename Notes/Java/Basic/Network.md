## Java网络编程

java.net包中提供了两种常见的网络协议的支持：

- TCP(Transmission Control Protocol)是一种面向连接的、可靠的、基于字节流的传输层通信协议，TCP层是位于IP层之上、应用层之下的中间层。TCP保障了两个应用程序之间的可靠性。通常用于互联网协议，称为TCP/IP。
- UDP(User Datafram Protocol)，位于OSI模型的传输层。一个无连接的协议。提供了应用程序之间要发送数据的数据包。由于UDP缺乏可靠性且属于无连接协议，所以应用程序通常必须容许一些丢失、错误或重复的数据包。

### Socket编程

Socket利用TCP提供了两台计算机之间的通信机制。

1. 服务器实例化一个 ServerSocket 对象，表示通过服务器上的端口通信。
2. 服务器调用 ServerSocket 类的 accept() 方法，该方法将一直等待，直到客户端连接到服务器上给定的端口。
3. 服务器正在等待时，一个客户端实例化一个 Socket 对象，指定服务器名称和端口号来请求连接。
4. Socket 类的构造函数试图将客户端连接到指定的服务器和端口号。如果通信被建立，则在客户端创建一个 Socket 对象能够与服务器进行通信。
5. 在服务器端，accept() 方法返回服务器上一个新的 socket 引用，该 socket 连接到客户端的 socket。

连接建立后，通过使用 I/O 流在进行通信，每一个socket都有一个输出流和一个输入流，客户端的输出流连接到服务器端的输入流，而客户端的输入流连接到服务器端的输出流。

### ServerSocket类

|方法|描述|
|---|----|
|getLocalPort()|返回此Socket在其上侦听的端口|
|accept()|侦听并接受到此Socket的连接|
|setSoTimeout(int)|指定超时值启用/禁用SO_TIMEOUT|
|bind(SocketAddress,int)|将ServerSocket绑定到特定地址(IP地址和端口号)|

### Sockt类

|方法|描述|
|---|---|
|connect(SocketAddress, int)|将此Socket连接到服务器，并指定一个超时值|
|getInetAddress()|返回Socket连接的地址|
|getPort()|返回Socket连接到的远程端口|
|getLocalPort()|返回Socket连接到的本地端口|
|getRemoteSocketAddress()|返回Socket连接的端点的地址|
|getInputStream()|返回Socket的输入流|
|getOutputStream()|返回Sockct的输出流|
|close()|关闭Socket|

### InetAddress类

|方法|描述|
|---|---|
|getByAddress(byte[] addr)|在给定原始IP地址的情况下，返回InetAddress对象|
|getByAddress(String host, bytep[] addr|根据提供的主机名和IP地址创建InetAddress|

### Socket客户端

```java
public class SocketServer {  
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

### Socket服务端

```java
public class SocketClient {  
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

