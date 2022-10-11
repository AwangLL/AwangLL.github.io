# UDP通信

## DatagramPacket类

DatagramPacket类用于封装UDP通信中发送或者接收的数据，DatagramPacket类对象也称为数据报对象。利用UDP通信时，发送端使用DatagramPacket类将数据打包，即用DatagramPacket类创建一个数据报对象，这个数据报对象包含有需要传输的数据、数据报的长度、IP地址和端口号等信息。

|构造方法|功能描述|
|---|---|
|DatagramPacket(byte[] buf,int length)|用于创建一个接收端的数据报对象，buf数组用于接收发送端发送过来的数据报中的数据，接收长度Iength。没有指定IP地址和端口号。这样的对象只能用于接收端，不能用于发送端。因为发送端一定要明确指出数据的目的地(IP地址和端口号)，而接收端不需要明确知道数据的来源，只需要接收到数据即可。
|DatagramPacket(byte[] buf,int length,InetAddress addr,int port)|创建一个用于发送给远程系统的数据报对象，并将数组buf中长度为length的数据发送到地址为address、端口号为port的主机上。创建的数据报对象通常用于发送端。
|DatagramPacket(byte[] buf,int offset,int length)|该构造方法与第一个构造方法类似，同样用于接收端，只不过在第一个构造方法的基础上，增加了一个offset参数，该参数用于指定接收到的数据在放入buf缓冲数组时是从offset索引处开始的。
|DatagramPacket(byte[] buf,int offset,int length,InetAddress addr,int port)|该构造方法与第二个构造方法类似，同样用于发送端，只不过在第二个构造方法的基础上，增加了一个offset参数，该参数用于指定一个从数组的offset索引处开始发送数据。

|常用方法|功能描述|
|---|---|
|InetAddress getAddress()|该方法用于返回发送端或者接收端的IP地址，如果是发送端的DatagramPacket对象，就返回接收端的IP地址，反之，就返回发送端的IP地址
|int getPort()|该方法用于返回发送端或者接收端的端口号，如果是发送端的DatagramPacket对象，就返回接收端的端口号;反之，就返回发送端的端口号
|bytel] getData()|该方法用于返回将要接收或者将要发送的数据，如果是发送端的DatagramPacket对象，就返回将要发送的数据;反之，就返回接收到的数据
|int getLength()|该方法用于返回接收或者将要发送数据的长度，如果是发送端的DatagramPacket对象，就返回将要发送的数据长度;反之，就返回接收到数据的长度

## DatagramSocket类

DatagramSocket类用于在发送主机中建立数据报通信方式，提出发送请求，实现数据报的发送与接收。在创建发送端和接收端的DatagramSocket对象时，使用的构造方法也有所不同。

|构造方法|功能描述|
|---|---|
|DatagramSocket()|该构造方法用于创建发送端的DatagramSocket对象，在创建DatagramSocket对象时，并没有指定端口号，系统会分配一个没有被其他网络程序所使用的端口号
|DatagramSocket(int port)|该构造方法既可用于创建接收端的DatagramSocket对象，又可以创建发送端的DatagramSocket对象，在创建接收端的DatagramSocket对象时，必须要指定一个端口号，这样就可以监听指定的端口
|DatagramSocket(int port,InetAddress addr)|该构造方法用于在有多个IP地址的当前主机上，创建一个以addr为指定P地址、以port为指定端口的数据报连接

|常用方法|功能描述|
|---|---|
|void receive(DatagramPacket p)|该方法用于接收数据，并将接收到的数据保存到DatagramPacket数据报中，在接收到数据之前，receive()方法会一直处于阻塞状态，只有当接收到数据报时，该方法才会返回
|void send(DatagramPacket p)|该方法用于发送DatagramPacket数据报，将数据报中包含的报文发送到所指定的IP地址主机
|void setSoTimeout(int timeout)|设置传输数据时超时时间为timeout
|void close()|关闭数据报连接

由于UDP连接是不可靠的通信方式，所以调用receive()方法时不一定能接收到数据，为了防止线程死掉，应该调用setSoTimeout()方法设置超时参数timeout()。另外，receive()方法和send()方法都可能产生输入、输出异常，因此都可能抛出IOException异常。

## UDP通信中数据包的发送过程

1. 创建一个用于发送数据报的DatagramPacket对象，使其包含如下信息。
	- 要发送的数据;
	- 数据报分组的长度;
	- 发送目的地的主机IP地址和目的端口号。
2. 在指定的或可用的本机端口创建DatagramSocket对象。
3. 调用DatagramSocket对象的send()方法，以DatagramPacket对象为参数发送数据报。

## UDP通信中数据包的接收过程

1. 创建一个用于接收数据报的DatagramSocket对象，其中包含空白数据缓冲区和指定数据报分组的长度。
2. 在指定的或可用的本机端口创建DatagramSocket对象。
3. 调用DatagramSocket对象的receive()方法，以DatagramPacket对象为参数接收数据报，接收到的信息有：
	- 收到的数据报分组。
	- 发送端主机的IP地址。
	- 发送端主机的发送端口号。

## 简单的UDP通信

### 接收端

```java
public class Receiver {  
    public static void main(String[] args) throws Exception{  
        // 创建一个字符数组，用于接收数据  
        byte[] buf = new byte[1024];  
        // 定义一个DatagramSocket对象，端口号为8954  
        DatagramSocket ds = new DatagramSocket(8954);  
        // 定义一个DatagramPacket对象，用于接收数据  
        DatagramPacket dp = new DatagramPacket(buf, buf.length);  
        System.out.println("等待接收数据");  
        ds.receive(dp); // 接收数据  
        /*调用DatagramPacket的方法获得接收到的信息包括数据  
        的内容、长度、发送的IP地址和端口号*/  
        String str = new String(dp.getData(), 0, dp.getLength()) + "from" +  
                dp.getAddress().getHostAddress() + ":" + dp.getPort();  
        System.out.println(str);  
        ds.close();  
    }
}
```

### 发送端

```java
public class Sender {  
    public static void main(String[] args) throws Exception{  
        // 创建一个DatagramSocket对象  
        DatagramSocket ds = new DatagramSocket(3000);  
        // 要发送的数据  
        String str = "hello world";  
        // 将定义的字符串转为字节数组  
        byte[] arr = str.getBytes();  
        /*创建一个要发送的数据报，数据报包括发送的数据，  
        数据的长度，接收端的IP地址以及端口号*/  
        DatagramPacket dp = new DatagramPacket(arr, arr.length,  
                InetAddress.getByName("localhost"), 8954);  
        System.out.println("发送信息");  
        ds.send(dp);  
        ds.close();  
    }
}
```

## 多线程的UDP网络程序

创建接收端程序Receive类继承Thread类

```java
class Receive extends Thread {  
    public void run() {  
        try {  
            //创建socket相当于创建码头  
            DatagramSocket socket = new DatagramSocket(6666);
            //创建packet相当于创建集装箱  
            DatagramPacket packet = new DatagramPacket(new byte[1024], 1024);  
            while (true) {  
                socket.receive(packet);//接收货物  
                byte[] arr = packet.getData();  
                int len = packet.getLength();  
                String ip = packet.getAddress().getHostAddress();  
                System.out.println(ip + ":" + new String(arr, 0, len));  
            }        
		} catch(IOException e){  
            e.printStackTrace();  
        }   
    }
}
```

创建发送端程序Send类继承Thread类

```java
class Send extends Thread {  
    public void run() {  
        try {  
            //创建socket相当于创建码头  
            DatagramSocket socket = new DatagramSocket();  
            Scanner sc = new Scanner(System.in);  
            while (true) {  
                String str = sc.nextLine();  
                if (" quit".equals(str))  
                    break;  
                DatagramPacket packet = new DatagramPacket(str.getBytes(),  
                    str.getBytes().length, InetAddress.getByName("127.0.0.1"), 6666);  
                socket.send(packet);//发货  
                socket.close();  
            }        
		} catch(IOException e){  
            e.printStackTrace();  
        }    
    }
}
```