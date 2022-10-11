# TCP通信

## ServerSocket类

|构造方法|功能描述|
|---|---|
|ServerSocket()|通过该方法创建的ServerSocket对象不与任何端口绑定，这样的ServerSocket对象创建的服务器端没有监听任何端口，不能直接使用，还需要继续调用bind(SocketAddress endpoint)方法将其绑定到指定的端口号上，才可以正常使用。
|ServerSocket(int port)|该方法的作用是以端口port创建ServerSocket对象，并等待客户端的连接请求。最常用的构造方法。
|ServerSocket(int port, int backlog)|该构造方法在第二个构造方法的基础上，增加了一个backlog参数。该参数用于指定最大连接数，即可以同时连接的客户端数
|ServerSocket(int port, int backlog, InetAddress bindAddr)|该构造方法在第三个构造方法的基础上，增加了一个bindAddr参数，该参数用于指定相关的IP地址。

|常用方法|功能描述|
|---|---|
|Socket accept()|该方法用于等待客户端的连接，在客户端连接之前会一直处于阻塞状态，如果有客户端连接，就会返回一个与之对应的Socket对象。
|InetAddress getInetAddress()|该方法用于返回一个InetAddress对象，该对象中封装了ServerSocket绑定的IP地址。
|boolean isClosed()|该方法用于判断ServerSocket对象是否为关闭状态，如果是关闭状态则返回true，反之则返回false。
|void bind(SocketAddress endpoint)|该方法用于将ServerSocket对象绑定到指定的IP地址和端口号，其中参数endpoint封装了IP地址和端口号。

## Socket类

|构造方法|功能描述|
|---|---|
|Socket()|使用该构造方法在创建Socket对象时，并没有指定IP地址和端口号，也就意味着只创建了客户端对象，并没有去连接任何服务器。最常用的构造方法。
|Socket(String host, int port)|该构造方法用于在客户端以指定的服务器地址host和端口号port创建一个Socket对象，并向服务器端发出连接请求。
|Socket(InetAddress address, int port)|创建一个流套接字，并将其连接到指定IP地址的指定端口
|Socket(InetAddress address, int port,boolean stream)|该构造方法在使用上与第二个构造方法类似，但IP地址由host指定。

|方法名称|功能描述|
|---|---|
|int getPort()|该方法用于获取Socket对象与服务器端连接的端口号。
|InetAddress getLocalAddress()|该方法用于获取Socket对象绑定的本地IP地址，并将IP地址封装成InetAddress类型的对象返回。
|InetAddress getInetAddress()|该方法用于获取创建Socket对象时指定的服务器的IP地址。
|void close()|该方法用于关闭Socket连接，结束本次通信。在关闭Socket之前，应将与Socket相关的所有的输入输出流全部关闭，这是因为一个良好的程序应该在执行完毕时释放所有的资源。
|InputStream getInputStream()|该方法返回一个InputStream类型的输入流对象，如果该输入流对象是由服务器端的Socket返回，就用于读取客户端发送的数据，反之，用于读取服务器端发送的数据。
|OutputStream getOutputStream()|该方法返回一个OutputStream类型的输出流对象，如果该输出流对象是由服务器端的Socket返回，就用于向客户端发送数据，反之，用于向服务器端发送数据。

## 简单的TCP通信

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

## 多线程的TCP网络程序

### 服务端

编写服务端程序

```java
public class TCPServer {  
    public static void main(String[] args) throws Exception {  
        // 创建Socket对象，监听指定的端口  
        ServerSocket serverSocket = new ServerSocket(7788);  
        // 使用while循环不停地接收客户端发送的数据  
        while(true) {  
            // 调用ServerSocket的accept()方法等待客户端的连接  
            final Socket client = serverSocket.accept();  
            int port = client.getPort();  
            System.out.println("connect to port" + port);  
            /* 创建一个线程并开启，处理客户端发送的数据 */
            new Thread() {  
                public void run() {  
                    OutputStream os = null; // 定义一个输出流对象  
                    try {  
                        // 获取客户端的输出流
                        os = client.getOutputStream();   
                        System.out.println("开始与客户端交互数据");  
                        os.write("hello".getBytes());  
                        Thread.sleep(5000);  
                        System.out.println("结束与客户端交互数据");  
                        os.close();  
                        client.close();  
                    } catch (Exception e) {  
                        e.printStackTrace();  
                    }                
                }            
            }.start();  
        }    
    }
}
```

