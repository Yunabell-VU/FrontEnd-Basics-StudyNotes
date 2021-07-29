## HTTP协议

### 基本认识

1. HTTP 是什么

   - **HyperText Transfer Protocol**

   - 超文本传输协议

   - HTML：超文本标记语言



**超文本：**原先一个个单一的文本，通过超链接将其联系起来。由原先的单一的文本变成了可无限延伸、扩展的超级文本、立体文本

- HTML、JS、CSS、图片、字体、音频、视频等等文件，都是通过 HTTP（超文本传输协议） 在服务器和浏览器之间传输
- 每一次前后端通信，前端需要主动向后端发出请求，后端接收到前端的请求后，可以给出响应
- HTTP 是一个请求-响应协议



2. HTTP 请求响应的过程

   - 浏览器在请求某一个地址时，会先在缓存中查找是否访问过此地址。如果访问过，则不会像服务器发送HTTP请求了。

   

   ![2.HTTPResponsProcess](http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/2.HTTPResponsProcess.png)


#### HTTP报文

```html
<body>
    <!--GET请求-->
    <form action="https://www.imooc.com" method="get">
      <input type="text" name="username" placeholder="用户名" />
      <input type="password" name="password" placeholder="密码" />
      <input type="submit" value="注册" />
    </form>

    <!--POST请求-->
    <form action="https://www.imooc.com" method="post">
      <input type="text" name="username" placeholder="用户名" />
      <input type="password" name="password" placeholder="密码" />
      <input type="submit" value="注册" />
    </form>
</body> 
```



1. HTTP 报文是什么
   - 浏览器向服务器发送请求时，请求本身就是信息，叫请求报文
   - 服务器向浏览器发送响应时传输的信息，叫响应报文



2. HTTP 报文格式

   ![3.httpReport](http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/3.httpReport.png)

   **请求：**

   - 请求头：起始行+首部
   - 请求体
   - GET 请求，没有请求体，数据通过请求头携带
   - POST 请求，有请求体，数据通过请求体携带

   **响应：**

   - 响应头：起始行+首部
   - 响应体



### HTTP方法

1. 常用的 HTTP 方法
   - 浏览器**发送请求时**采用的方法，和响应无关
   - GET、POST、PUT、DELETE
   - 用来定义对于资源采取什么样的操作的，有各自的语义



2. HTTP 方法的语义



**GET 获取数据**

- 获取资源（文件）



**POST 创建数据**

- 注册



**PUT 更新数据**

- 修改个人信息，修改密码



**DELETE 删除数据**

- 删除一条评论



注意：增删改查，这些方法虽然有各自的语义，但是并不是强制性的



3. RESTful 接口设计

   一种接口设计风格，充分利用 HTTP 方法的语义



**注意以下例子都不是RESTful风格**

 通过用户 ID 获取个人信息，使用 GET 方法

-  如：https://www.imooc.com/api/http/getUser?id=1

注册新用户，使用 POST 方法

-  如：https://www.imooc.com/api/http/addUser

修改一个用户，使用 POST 方法

- 如：https://www.imooc.com/api/http/modifyUser

删除一个用户，使用 POST 方法

- 如：https://www.imooc.com/api/http/deleteUser



**以下是RESTful风格：**

同一个接口，不同的HTTP方法就可以区分目的



**GET**

- 如：https://www.imooc.com/api/http/user?id=1

**POST**

- 如：https://www.imooc.com/api/http/user

**PUT**

- 如：https://www.imooc.com/api/http/user

**DELETE**

- 如：https://www.imooc.com/api/http/user



### GET和POST的对比

1. 语义

   - GET：获取数据

   - POST：创建数据



2. 发送数据

   **GET** 通过地址在请求头中携带数据

   - 能携带的数据量和地址的长度有关系，一般最多就几K



   **POST** 既可以通过地址在请求头中携带数据，也可以通过请求体携带数据

   - 能携带的数据量理论上是无限的

   - 携带少量数据，可以使用 GET 请求，大量的数据可以使用 POST 请求

   - send方法发送的数据，是通过请求体携带的，POST请求可以通过“xhr.send(`username=${encodeURIComponent('张三')} `);”的形式发送数据

     


3. 缓存

   GET 可以被缓存，POST 不会被缓存



4. 安全性

   ?username=alex

   GET 和 POST 都不安全

   发送密码或其他敏感信息时不要使用 GET，主要是避免直接被他人窥屏或通过历史记录找到你的密码



### HTTP状态码

1. HTTP 状态码是什么

   **定义服务器对请求的处理结果，是服务器返回的**



2. HTTP 状态码的语义

   

   100~199 消息：代表请求已被接受，需要继续处理
   - websocket (ws)



   200~299 成功
   - 200 (最常见)



   300~399 重定向
   - 301 Moved Permanently （重定向位置会被浏览器保存，除非缓存清空）
   - 302 Move Temporarily （不会保存到缓存，每次请求都要从服务器获取新地址）
   - 304 Not Modified （文件没有被修改，可以继续使用。）



   400~499 请求错误
   - 404 Not Found

     


   500~599 服务器错误
   - 500 Internal Server Error