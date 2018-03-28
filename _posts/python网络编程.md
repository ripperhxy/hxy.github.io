---
layout: post
title: python网络编程
date: 2018-02-15
tags: Python
---


# python网络编程

## 客户端编写

### 引入第三方库 socket

1.创建一个socket

参数1：指定协议 AF_INET 或 AF_INET6

参数2：SOCK_STREAM执行使用面向流的TCP协议

```python
sk =socket.socket(socket.AF_INET, socket.SOCK_STREAM)
```

2.建立连接

参数：是一个元组，第一个元素为要连接的服务器IP地址，第二个参数为端口

```python
sk.connect(("www.sina.com",80))
sk.send(b'GET / HTTP/1.1\r\nHost:
        www.sina.cn\r\nConnection: close\r\n\r\n')
```

等待接收数据

```python
data = []
while True:
    #每次接收1k的数据
    tempData = sk.recv(1024)
    if tempData:
        data.append(tempData)
    else:
        break

 datStr = (b''.join(data)).decode("utf-8")
```

断开连接

```python
sk.close()
#print(dataStr)

headers, HTML = dataStr.split('\r\n\r\n',1)
print(headers)
print(HTML)
```

## 客户端与服务器间的数据交互

### server端

1.创建一个socket

```python
server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
```

2.绑定ip和端口

```python
server.bind(('localhost',8080))
#监听
server.listen(5)
print("服务器启动成功.....")
#等待连接
clientSocket,clientAddress = server.accept()
print("%s -- %s 连接成功" % (str(clientSocket),clientAddress))
while True:
	data = clientSocket.recv(1024)
    print("收到" + str(clientSocket) + "的数据")
    data.decode("UTF-8")


```

### client端

```python
client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client.connect(("localhost",8081))

count = 0
while True:
    count += 1
    data = input("请输入给服务器发送的数据")
    client.send(data.encode("utf-8"))
    info = client.recv(1024)
    print("server say", info)
```
