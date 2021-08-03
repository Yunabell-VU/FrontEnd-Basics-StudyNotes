## Ajax&Fetch 与跨域请求

## Ajax 基础

#### 基础认识

 // 1.Ajax 是什么

   // Ajax 是 Asynchronous JavaScript and XML（异步 JavaScript 和 XML）的简写



   // Ajax 中的异步：可以异步地向服务器发送请求，在等待响应的过程中，不会阻塞当前页面，浏览器可以做自己的事情。直到成功获取响应后，浏览器才开始处理响应数据



   // XML（可扩展标记语言）是前后端数据通信时传输数据的一种格式



   // XML 现在已经不怎么用了，现在比较常用的是 JSON



   // Ajax 其实就是浏览器与服务器之间的一种异步通信方式



   // 使用 Ajax 可以在不重新加载整个页面的情况下，对页面的某部分进行更新



   // ① 慕课网注册检测

   // ② 慕课网搜索提示



   // 2.搭建 Ajax 开发环境

   // Ajax 需要服务器环境，非服务器环境下，很多浏览器无法正常使用 Ajax



   // Live Server



   // windows phpStudy

   // Mac MAMP



#### 基本用法

 // 1.XMLHttpRequest

   // console.log(Ajax);



   // Ajax 想要实现浏览器与服务器之间的异步通信，需要依靠 XMLHttpRequest，它是一个构造函数



   // 不论是 XMLHttpRequest，还是 Ajax，都没有和具体的某种数据格式绑定



   // 2.Ajax 的使用步骤

   // 2.1.创建 xhr 对象

   // const xhr = new XMLHttpRequest();



   // 2.2.监听事件，处理响应

   // 当获取到响应后，会触发 xhr 对象的 readystatechange 事件，可以在该事件中对响应进行处理



   // xhr.addEventListener('readystatechange', () => {}, fasle);



   // xhr.onreadystatechange = () => {

   //  if (xhr.readyState !== 4) return;



   //  // HTTP CODE

   //  // 获取到响应后，响应的内容会自动填充 xhr 对象的属性

   //  // xhr.status：HTTP 200 404

   //  // xhr.statusText：HTTP 状态说明 OK Not Found

   //  if ((xhr.status >= 200) & (xhr.status < 300) || xhr.status === 304) {

   //   // console.log('正常使用响应数据');

   //   console.log(xhr.responseText);

   //  }

   // };



   // readystatechange 事件也可以配合 addEventListener 使用，不过要注意，IE6~8 不支持 addEventListener

   // 为了兼容性，readystatechange 中不使用 this，而是直接使用 xhr

   // 由于兼容性的原因，最好放在 open 之前



   // readystatechange 事件监听 readyState 这个状态的变化

   // 它的值从 0 ~ 4，一共 5 个状态

   // 0：未初始化。尚未调用 open()

   // 1：启动。已经调用 open()，但尚未调用 send()

   // 2：发送。已经调用 send()，但尚未接收到响应

   // 3：接收。已经接收到部分响应数据

   // 4：完成。已经接收到全部响应数据，而且已经可以在浏览器中使用了



   // 2.3.准备发送请求

   // xhr.open(

   //  'HTTP 方法 GET、POST、PUT、DELETE',

   //  '地址 URL https://www.imooc.com/api/http/search/suggest?words=js ./index.html ./index.xml ./index.txt',

   //  true

   // );



   // 调用 open 并不会真正发送请求，而只是做好发送请求前的准备工作



   // 2.3.发送请求

   // 调用 send() 正式发送请求



   // send() 的参数是通过请求体携带的数据

   // xhr.send(null);



   // 3.使用 Ajax 完成前后端通信

   const url = 'https://www.imooc.com/api/http/search/suggest?words=js';



   const xhr = new XMLHttpRequest();

   xhr.onreadystatechange = () => {

​    if (xhr.readyState !== 4) return;



​    if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {

​     console.log(xhr.responseText);

​     console.log(typeof xhr.responseText);

​    }

   };

   xhr.open('GET', url, true);

   xhr.send(null);



#### GET请求

```html
<body>
    <form action="https://www.imooc.com/api/http/search/suggest" method="get">
      <input type="text" name="username" />
      <input type="text" name="words" />
      <input type="password" name="password" />
      <input type="submit" value="提交" />
    </form>
</body>
```



 // 1.携带数据

   // GET 请求不能通过请求体携带数据，但可以通过请求头携带

   // const url =

   //  'https://www.imooc.com/api/http/search/suggest?words=js&username=alex&age=18';

   // const xhr = new XMLHttpRequest();



   // xhr.onreadystatechange = () => {

   //  if (xhr.readyState != 4) return;



   //  if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {

   //   console.log(xhr.responseText);

   //  }

   // };



   // xhr.open('GET', url, true);



   // xhr.send(null);



   // 不会报错，但不会发送数据

   // xhr.send('sex=male');



   // 2.数据编码

   // 如果携带的数据是非英文字母的话，比如说汉字，就需要编码之后再发送给后端，不然会造成乱码问题

   // 可以使用 encodeURIComponent() 编码

   const url = `https://www.imooc.com/api/http/search/suggest?words=${encodeURIComponent(

​    '前端'

   )}`;

   const xhr = new XMLHttpRequest();



   xhr.onreadystatechange = () => {

​    if (xhr.readyState != 4) return;



​    if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {

​     console.log(xhr.responseText);

​    }

   };



   xhr.open('GET', url, true);



   xhr.send(null);



#### POST请求

```html
<body>
    <form
      action="https://www.imooc.com/api/http/search/suggest?words=js"
      method="post"
    >
      <input type="text" name="username" />
      <input type="password" name="password" />
      <input type="submit" value="提交" />
    </form>
</body>
```

 // 1.携带数据

   // POST 请求主要通过请求体携带数据，同时也可以通过请求头携带

   // const url = 'https://www.imooc.com/api/http/search/suggest?words=js';

   // const xhr = new XMLHttpRequest();



   // xhr.onreadystatechange = () => {

   //  if (xhr.readyState != 4) return;



   //  if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {

   //   console.log(xhr.responseText);

   //  }

   // };



   // xhr.open('POST', url, true);



   // 如果想发送数据，直接写在 send() 的参数位置，一般是字符串

   // xhr.send('username=alex&age=18');



   // 不能直接传递对象，需要先将对象转换成字符串的形式

   // xhr.send({

   //  username: 'alex',

   //  age: 18

   // });

   // [object Object]



   // 2.数据编码

   // xhr.send(`username=${encodeURIComponent('张三')}&age=18`);
