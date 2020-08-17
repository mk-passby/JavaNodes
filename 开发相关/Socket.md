---
title: socket连接
date: 2019-7-17 20:01:00
tags: socket
---


# TCP/IP

TCP/IP（Transmission Control Protocol/Internet Protocol）是一种可靠的网络数据传输控制协议。定义了主机如何连入因特网以及数据如何在他们之间传输的标准

五层/七层模型：应用(表示，会话)，传输，网络，链路，物理传输

![001](socket/001.png)

- 三次握手

  （1）第一次握手：Client将标志位SYN置为1，随机产生一个值seq=J，并将该数据包发送给Server，Client进入SYN_SENT状态，等待Server确认。

  （2）第二次握手：Server收到数据包后由标志位SYN=1知道Client请求建立连接，Server将标志位SYN和ACK都置为1，ack=J+1，随机产生一个值seq=K，并将该数据包发送给Client以确认连接请求，Server进入SYN_RCVD状态。

  （3）第三次握手：Client收到确认后，检查ack是否为J+1，ACK是否为1，如果正确则将标志位ACK置为1，ack=K+1，并将该数据包发送给Server，Server检查ack是否为K+1，ACK是否为1，如果正确则连接建立成功，Client和Server进入ESTABLISHED状态，完成三次握手，随后Client与Server之间可以开始传输数据了。

- 四次挥手协议

  （1）第一次挥手：Client发送一个FIN，用来关闭Client到Server的数据传送，Client进入FIN_WAIT_1状态。

  （2）第二次挥手：Server收到FIN后，发送一个ACK给Client，确认序号为收到序号+1（与SYN相同，一个FIN占用一个序号），Server进入CLOSE_WAIT状态。

  （3）第三次挥手：Server发送一个FIN，用来关闭Server到Client的数据传送，Server进入LAST_ACK状态。

  （4）第四次挥手：Client收到FIN后，Client进入TIME_WAIT状态，接着发送一个ACK给Server，确认序号为收到序号+1，Server进入CLOSED状态，完成四次挥手。

  

  # Socket链接

  

  ## 1v1链接
  - 客户端

```java
public class SocketClient {

  public static void main(String[] args) {
    try {
      Socket socket = new Socket("localhost", 8888);
      PrintWriter printWriter = new PrintWriter(socket.getOutputStream(), true);
      printWriter.println("hello server");
      BufferedReader bufferedReader = new BufferedReader(
          new InputStreamReader(socket.getInputStream()));
      while (true) {
        String serverData = bufferedReader.readLine();
        if (serverData == null) {
          break;
        }
        System.out.println("客户端消息：" + serverData);
      }
      printWriter.close();
      socket.close();
    } catch (IOException e) {
      e.printStackTrace();
    }
  }
}

```

  - 服务端

```java

public class SocketServer {

  public static void main(String[] args) throws IOException {
    ServerSocket serverSocket = null;
    try {
      serverSocket = new ServerSocket(8888);
      while (true) {
        Socket socket = serverSocket.accept();
        new Thread(() -> {
          try {
            BufferedReader reader = new BufferedReader(
                new InputStreamReader(socket.getInputStream()));
            PrintWriter printWriter = new PrintWriter(
                new OutputStreamWriter(socket.getOutputStream()));
            while (true) {
              String clientData = reader.readLine();
              if (clientData == null) {
                break;
              }
              System.out.println("服务端消息：" + clientData);
              printWriter.println("hello.mike");
              printWriter.flush();
            }

          } catch (IOException e) {
            e.printStackTrace();
          }
        }).start();
      }
    } catch (IOException e) {
    } finally {
      if (serverSocket != null) {
        serverSocket.close();
      }
    }
  }

}
```

## 组播

```java
public class MulticastServer {

  public static void main(String[] args) {
    //地址段：224.0.0.0-239.255.255.255
    try {
      InetAddress group=InetAddress.getByName("224.1.2.3");
      MulticastSocket multicastSocket=new MulticastSocket();
      for (int i = 0; i <10 ; i++) {
        String data="hello,index"+i;
        byte[] bytes=data.getBytes();
        multicastSocket.send(new DatagramPacket(bytes,bytes.length,group,8888));
        TimeUnit.SECONDS.sleep(2);
      }

    } catch (UnknownHostException e) {
      e.printStackTrace();
    } catch (IOException e) {
      e.printStackTrace();
    } catch (InterruptedException e) {
      e.printStackTrace();
    }
  }


}


```





```java
public class MulticastClient {

  public static void main(String[] args) {

    try {
      InetAddress group=InetAddress.getByName("224.1.2.3");
      MulticastSocket multicastSocket=new MulticastSocket(8888);
      multicastSocket.joinGroup(group);
      byte[] bytes=new byte[1024];
      while (true){
        DatagramPacket datagramPacket=new DatagramPacket(bytes,bytes.length);
        multicastSocket.receive(datagramPacket);
        String msg=new String(datagramPacket.getData());
        System.out.println("接受到msg："+msg);
      }
    } catch (UnknownHostException e) {
      e.printStackTrace();
    } catch (IOException e) {
      e.printStackTrace();
    }
  }
}


```

