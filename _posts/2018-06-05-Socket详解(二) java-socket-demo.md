---
layout: post
title: Socket详解(二):Java Socket Demo
date: 2018-06-05
tag: Java
---

[<-返回Java Socket简介](http://pnoker.top/2018/06/Socket%E8%AF%A6%E8%A7%A3(%E4%B8%80)-Java-Socket%E7%AE%80%E4%BB%8B/)

### Server
```java
public class SocketServer {
    boolean started = false;
    ServerSocket serverSocket = null;

    public static void main(String[] args) {
        new SocketServer().start();
    }

    public void start() {
        try {
            serverSocket = new ServerSocket(8888);
            started = true;
            log.info("[socket server] test sokcet server start success");
        } catch (BindException e) {
            log.info("[socket server] port 8888 occupied");
        } catch (IOException e) {
            log.info(e.getMessage());
        }
        try {
            while (started) {
                Socket socket = serverSocket.accept();
                Client client = new Client(socket);
                log.info("[socket server] socket connected");
                new Thread(client).start();
            }
        } catch (IOException e) {
            log.info(e.getMessage());
        } finally {
            try {
                serverSocket.close();
            } catch (Exception e) {
                log.info(e.getMessage());
            }
        }
    }

    class Client implements Runnable {
        private Socket socket;
        private DataInputStream dataInputStream = null;
        private DataOutputStream dataOutputStream = null;
        private boolean bConnected = false;

        public Client(Socket socket) {
            this.socket = socket;
            try {
                dataInputStream = new DataInputStream(socket.getInputStream());
                dataOutputStream = new DataOutputStream(socket.getOutputStream());
                bConnected = true;
            } catch (IOException e) {
                log.info(e.getMessage());
            }
        }

        @Override
        public void run() {
            try {
                while (bConnected) {
                    String str = dataInputStream.readUTF();
                    log.info("[socket server] revice cmd :" + str);
                    dataOutputStream.writeUTF(str);
                }
            } catch (EOFException e) {
                log.info("[socket server] socket closed!");
            } catch (IOException e) {
                log.info(e.getMessage());
            } finally {
                try {
                    if (dataInputStream != null) {
                        dataInputStream.close();
                    }
                    if (dataOutputStream != null) {
                        dataOutputStream.close();
                    }
                    if (socket != null) {
                        socket.close();
                    }
                } catch (IOException e) {
                    log.info(e.getMessage());
                }
            }
        }
    }
}
```
### Client

```java
public class Client {  
    public static void main(String args[])  
        throws Exception {  
        String host = "127.0.0.1"; // 本地地址  
        int port = 8888;  
        // 与服务端建立连接  
        Socket client = new Socket(host, port);  
        Scanner sc = new Scanner(System.in);  
        String str = sc.nextLine();  
        System.out.println("输入的内容为：" + str);  
        // 建立连接后就可以往服务端写数据了  
        Writer writer = new OutputStreamWriter(client.getOutputStream(), "UTF-8");  
        writer.write(str);  
        writer.write("eof\n");  
        writer.flush();  
        // 写完以后进行读操作  
        BufferedReader br = new BufferedReader(new InputStreamReader(client.getInputStream()));  
        StringBuffer sb = new StringBuffer();  
        String temp;  
        int index;  
        while ((temp = br.readLine()) != null) {  
            if ((index = temp.indexOf("eof")) != -1) {  
                sb.append(temp.substring(0, index));  
                break;  
            }  
            sb.append(temp);  
        }  
        System.out.println("from server: " + sb);  
        writer.close();  
        br.close();  
        client.close();  
    }  
}  
```
