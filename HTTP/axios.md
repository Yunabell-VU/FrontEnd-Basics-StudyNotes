## Axios

#### axios 是什么

- axios 是一个基于 Promise 的 HTTP 库，可以用在浏览器和 node.js 中

- 第三方 Ajax 库

- Axios中文官方文档： http://www.axios-js.com/zh-cn/docs/



##### axios 的基本用法

- 引入 axios

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>axios</title>
  </head>
  <body>
      <!--链接从官网文档获取-->
    <!-- <script src="https://unpkg.com/axios/dist/axios.min.js"></script> -->
    <script src="https://unpkg.com/axios@0.19.2/dist/axios.min.js"></script>
```

- 发送请求

```javascript
//检查有无成功引入axios
console.log(axios);

const url = 'https://www.imooc.com/api/http/search/suggest?words=js';

axios(url, {

   method: 'post',

	// 请求时的头信息
	headers: {
        //如果data用对象的形式，这里就要使用json格式
        //'Content-Type': 'application/x-www-form-urlencoded'
        'Content-Type': 'application/json'
    },

	// 通过请求头携带的数据
    params: {
        username: 'alex'
    },

	// 通过请求体携带的数据
	// application/json

	data: {
        age: 18,
        sex: 'male'
    }

	// 如果是 application/x-www-form-urlencoded 格式，data就要是以下格式
	//data: 'age=18&sex=male'

	timeout: 10

	withCredentials: true

 })
    .then(response => {
    	console.log(response);
    	console.log(response.data.data);
	})
    .catch(err => {
    	console.log(err);
	});

```

- 直接用get请求

```javascript
axios.get(url, {
    params: {
        username: 'alex'
    }
})
.then(response => {
    console.log(response);
});
```

- 直接用post请求

```javascript
//post的第二个参数是直接用来传输数据的，不用再写data:{}

axios.post(url, 'username=alex&age=18')
    .then(response => {
    	console.log(response);
	})
    .catch(err => {
    	console.log(err);
	});

//如果要发送json格式，第二个参数直接传对象格式{}, 但是需要服务器端允许（服务器Content-Type是jason）
axios.post('https://www.imooc.com/api/http/json/search/suggest?words=js', {
    username: 'alex'
})
    .then(response => {
    	console.log(response);
	})
    .catch(err => {
    	console.log(err);
	});

//还有一些其他方法
axios.put()
axios.delete()
```





****



## Fetch

##### Fetch 是什么

- Fetch 也是前后端通信的一种方式

- Fetch 是 Ajax（XMLHttpRequest）的一种替代方案，它是基于 Promise 的

- Ajax 的兼容性比 Fetch 好

- 原生没有abort 和 timeout 等方式，如果需要，得自己实现



##### Fetch 的基本用法

```javascript
//fetch是本身就有的函数
console.log(fetch);


// fetch() 调用后返回 Promise 对象

const url = 'https://www.imooc.com/api/http/search/suggest?words=js';

//fetch返回的内容：

//body: (...)
//bodyUsed: false
//ok: true
//status: 200
//statusText: "OK"
//type: "cors"
//url: "https://www.im

// body/bodyUsed
//获得的数据在body里，采用ReadbleStream的形式，要用Fetch提供的方式来读取
// body 只能读一次，读过之后就不让再读了（bodyused变为true）

// ok
// 如果为 true，表示可以读取数据，不用再去判断 HTTP 状态码了

const fd = new FormData();
fd.append('username', 'alex');


// 第二个参数是对象，用来配置 fetch，不写第二个参数则使用默认值
fetch(url, {
    method: 'post',
    
    
    //fetch是没有param的
    
    //body上的内容就是请求体信息
    // body: null
    // body: 'username=alex&age=18',
    // body: JSON.stringify({ username: 'alex' })
    
    body: fd,
    
    //FormData不需要设置header，会自动设置
    
    //headers: {
    // 'Content-Type': 'application/x-www-form-urlencoded'
    // 'Content-Type': 'application/json'
    // }
    
    //跨域资源共享是默认的设定（cors）
    mode: 'cors'
    // credentials:'include'
})
.then(response => {
    console.log(response);
	if (response.ok) {
         return response.json(); //返回的是一个Promise对象
            
          // return response.text();
    } else {
        throw new Error(`HTTP CODE 异常 ${response.status}`);
    }
})
.then(data => {
    console.log(data);
})
.catch(err => {
   console.log(err);
});
```



