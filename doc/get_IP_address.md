如果要通过远程连接树莓派，需要获取树莓派的IP地址。

可以通过查看路由器里面的信息知道树莓派的IP地址。

还有一个方法，在我们的PC上发送udp广播给局域网内的设备。树莓派上运行服务，收到广播之后在发送数据给PC。这样PC就可以知道树莓派的IP地址了。

树莓派作为广播接收者，在收到数据之后，再给发送者发送数据。

listener.py

```
# coding=utf-8

import socket

BUFSIZE = 1024 
PORT = 1060

# 设置协议为UDP 
s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
# 绑定端口号
s.bind(('', PORT))
print('Listening for broadcast at ', s.getsockname())

while True:
	# 接受数据,这里会阻塞, 可以获得数据和IP 地址
    data, address = s.recvfrom(BUFSIZE)
	# 打印地址
    print('client received from {}:{}'.format(address, data.decode('utf-8')))
	# 给发送者发送数据
    s.sendto('Client send message!'.encode('utf-8'), address)
```

PC作为广播发送者，主动发送广播，等待接受者的消息回复。

broadcast.py

```
# coding=utf-8

import socket

BUFSIZE = 1024
PORT = 1060
HOST = '<broadcast>'

ADDR = (HOST, PORT)

# 设置UDP 
s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
# 设置为广播
s.setsockopt(socket.SOL_SOCKET, socket.SO_BROADCAST, 1)

# 发送广播
network = '<broadcast>'
s.sendto('server broadcast message!'.encode('utf-8'), ADDR)

# 接收广播数据
data, address = s.recvfrom(BUFSIZE)
print('receive message from {}:{}'.format(address, data.decode('utf-8')))

s.close()
```