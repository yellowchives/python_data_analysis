# python自带的静态web服务器

1. 进入html文件存放的目录
2. 运行`python -m http.server 9999 `。`-m`表示运行包里面的模块，默认端口是8000，这里设置了9999为端口。

# 手写多任务静态服务器

```python
import socket
import threading
import sys


# 定义web服务器类
class HttpWebServer(object):
    def __init__(self, port):
        # 创建tcp服务端套接字
        tcp_server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        # 设置端口号复用, 程序退出端口立即释放
        tcp_server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, True)
        # 绑定端口号
        tcp_server_socket.bind(("", port))
        # 设置监听
        tcp_server_socket.listen(128)
        # 保存创建成功的服务器套接字
        self.tcp_server_socket = tcp_server_socket

    # 处理客户端的请求
    @staticmethod
    def handle_client_request(new_socket):
        # 代码执行到此，说明连接建立成功
        recv_client_data = new_socket.recv(4096)
        if len(recv_client_data) == 0:
            print("关闭浏览器了")
            new_socket.close()
            return

        # 对二进制数据进行解码
        recv_client_content = recv_client_data.decode("utf-8")
        print(recv_client_content)
        # 根据指定字符串进行分割， 最大分割次数指定2
        request_list = recv_client_content.split(" ", maxsplit=2)

        # 获取请求资源路径
        request_path = request_list[1]
        print(request_path)

        # 判断请求的是否是根目录，如果条件成立，指定首页数据返回
        if request_path == "/":
            request_path = "/index.html"

        try:
            # 动态打开指定文件
            with open("static" + request_path, "rb") as file:
                # 读取文件数据
                file_data = file.read()
        except Exception as e:
            # 请求资源不存在，返回404数据
            # 响应行
            response_line = "HTTP/1.1 404 Not Found\r\n"
            # 响应头
            response_header = "Server: PWS1.0\r\n"
            with open("static/error.html", "rb") as file:
                file_data = file.read()
            # 响应体
            response_body = file_data

            # 拼接响应报文
            response_data = (response_line + response_header + "\r\n").encode("utf-8") + response_body
            # 发送数据
            new_socket.send(response_data)
        else:
            # 响应行
            response_line = "HTTP/1.1 200 OK\r\n"
            # 响应头
            response_header = "Server: PWS1.0\r\n"

            # 响应体
            response_body = file_data

            # 拼接响应报文
            response_data = (response_line + response_header + "\r\n").encode("utf-8") + response_body
            # 发送数据
            new_socket.send(response_data)
        finally:
            # 关闭服务与客户端的套接字
            new_socket.close()

    # 启动web服务器进行工作
    def start(self):
        while True:
            # 等待接受客户端的连接请求
            new_socket, ip_port = self.tcp_server_socket.accept()
            # 当客户端和服务器建立连接程，创建子线程
            sub_thread = threading.Thread(target=self.handle_client_request, args=(new_socket,))
            # 设置守护主线程
            sub_thread.setDaemon(True)
            # 启动子线程执行对应的任务
            sub_thread.start()


# 程序入口函数
def main():

    print(sys.argv)
    # 判断命令行参数长度是否等于2,
    if len(sys.argv) != 2:
        print("执行命令如下: python3 xxx.py 8000")
        return

    # 判断字符串是否都是数字组成
    if not sys.argv[1].isdigit():
        print("执行命令如下: python3 xxx.py 8000")
        return

    # 获取终端命令行参数
    port = int(sys.argv[1])
    # 创建web服务器对象
    web_server = HttpWebServer(port)
    # 启动web服务器进行工作
    web_server.start()


if __name__ == '__main__':
    main()
```

创建服务端socket：
1. 编写一个TCP服务端程序

   ```py
   tcp_server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
   # 循环接受客户端的连接请求
   while True:
       conn_socket, ip_port = tcp_server_socket.accept()
   ```

2. 获取浏览器发送的http请求报文数据

   ```py
   client_request_data = conn_socket.recv(4096)
   ```

1. 获取用户请求资源的路径

   ```py
    request_list = client_request_conent.split(” ”,  maxsplit=2)
    request_path = request_list[1]
   ```

2. 根据请求资源的路径，读取请求指定文件的数据

   ```py
    with open("static" + request_path, "rb") as file:
    file_data = file.read()
   ```
   
4. HTTP响应报文数据发送完成以后，关闭服务于客户端的套接字。

   ```py
   conn_socket.close()
   ```

多线程改进：
1. 当客户端和服务端建立连接成功，创建子线程，使用子线程专门处理客户端的请求，防止主线程阻塞。

   ```py
    while True:
        conn_socket, ip_port = tcp_server_socket.accept()
        # 开辟子线程并执行对应的任务
        sub_thread = threading.Thread(target=handle_client_request, args=(conn_socket,))
   ```

2. 把创建的子线程设置成为守护主线程，防止主线程无法退出。

   ```py
    # 开辟子线程并执行对应的任务
    sub_thread = threading.Thread(target=handle_client_request, args=(conn_socket,))
    sub_thread.setDaemon(True) # 设置守护主线程
    sub_thread.start()
   ```

服务器封装到类中：
1. 把提供服务的Web服务器抽象成一个类(HTTPWebServer)

   ```py
    class HttpWebServer(object):
   ```

2. 提供Web服务器的初始化方法，在初始化方法里面创建socket对象

   ```py
    def __init__(self):
    # 初始化服务端套接字，设置监听，代码省略..
   ```

3. 提供一个开启Web服务器的方法，让Web服务器处理客户端请求操作。

   ```py
    def start(self):
    while True:
        service_client_socket, ip_port = self.tcp_server_socket.accept()
        # 连接建立成功，开辟子线程处理客户端的请求
        sub_thread = threading.Thread(target=self.handle_client_request, args=(service_client_socket,))
        sub_thread.start()
   ```

获取命令行参数：

启动命令是这样的：`C:\Python310\python.exe C:/code/python-workspace/09_web_server.py 9999 `

命令行参数被封装成一个列表，`sys.argv = ['C:/code/python-workspace/09_web_server.py', '9999']`
1. 获取执行python程序的终端命令行参数

   ```py
    sys.argv
   ```

2. 判断参数的类型，设置端口号必须是整型

   ```py
    if not sys.argv[1].isdigit():
        print("启动命令如下: python3 xxx.py 9090")
        return
    port = int(sys.argv[1])
   ```

3. 给Web服务器类的初始化方法添加一个端口号参数，用于绑定端口号

   ```py
    def __init__(self, port):
        self.tcp_server_socket.bind(('', port))
   ```