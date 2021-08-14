## XHR对象

### XHR属性

##### responseType 和 response 属性

```javascript
const url = 'https://www.imooc.com/api/http/search/suggest?words=js';
const xhr = new XMLHttpRequest();


xhr.onreadystatechange = () => {
    if (xhr.readyState != 4) return;
    if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {

	// 文本形式的响应内容
	// responseText 只能在没有设置 responseType 或者 responseType = '' 或 'text' 的时候才能使用
	console.log('responseText:', xhr.responseText);
	console.log(JSON.parse(xhr.responseText));
        
	 // 可以用来替代 responseText
	console.log('response:', xhr.response); 
        //如果responseType设置的是'text'，这里获取的数据和通过responseText获取的是一模一样的
        //如果responseType设置的是'json'，则直接获得解析好的数据，不需要再调用JSON.parse
    }
};

xhr.open('GET', url, true);

//设置响应数据类型，默认是'text'，为空则表示text类型
xhr.responseType = '';

//'text'：text类型
xhr.responseType = 'text';

//'json': json类型
xhr.responseType = 'json';


xhr.send(null);

// IE6~9 不支持，IE10 开始支持
```

 

##### timeout 属性

- 设置请求的超时时间（单位 ms）
- 如果一定时间内没有完成，会取消该次请求，之后可以触发事件给用户反馈

```javascript
const url = 'https://www.imooc.com/api/http/search/suggest?words=js';

const xhr = new XMLHttpRequest();


xhr.onreadystatechange = () => {
    if (xhr.readyState != 4) return;
    if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
        console.log(xhr.response);
    }
};


xhr.open('GET', url, true);

xhr.timeout = 10000; //一般设置为10秒左右，根据具体项目来决定

xhr.send(null);


// IE6~7 不支持，IE8 开始支持
```



##### withCredentials 属性

- 指定使用 Ajax 发送请求时是否携带 Cookie

```javascript
// 使用 Ajax 发送请求，默认情况下，同域时，会携带 Cookie；跨域时，不会携带
// 想要跨域携带Cookie，需要以下属性设置为true
xhr.withCredentials = true;

// 但是最终能否成功跨域携带 Cookie，还要看服务器同不同意（后端支持，需要指定域名，而不能使用通配符*）


const url = 'https://www.imooc.com/api/http/search/suggest?words=js';

// const url = './index.html';


const xhr = new XMLHttpRequest();

xhr.onreadystatechange = () => {
    if (xhr.readyState != 4) return;
    if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
        console.log(xhr.response);
    }
};


xhr.open('GET', url, true);

xhr.withCredentials = true;

xhr.send(null);

// IE6~9 不支持，IE10 开始支持

```



### XHR的方法

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

##### abort()

- 终止当前请求

- 一般配合 abort 事件一起使用

```javascript
const url = 'https://www.imooc.com/api/http/search/suggest?words=js';

const xhr = new XMLHttpRequest();

xhr.onreadystatechange = () => {
    if (xhr.readyState != 4) return;
    if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
        console.log(xhr.response);
    }
};

xhr.open('GET', url, true);
xhr.send(null);

xhr.abort(); //在.send之后调用
```



##### setRequestHeader()

- 可以设置请求头信息

  ```javascript
  // xhr.setRequestHeader(头部字段的名称, 头部字段的值);
  
  ////以下接口可以处理x-www-form-urlencoded请求头数据
  const url = 'https://www.imooc.com/api/http/search/suggest?words=js';
  
  //以下接口可以处理json请求头数据
  const url = 'https://www.imooc.com/api/http/json/search/suggest?words=js';
  
  const xhr = new XMLHttpRequest();
  
  xhr.onreadystatechange = () => {
      if (xhr.readyState != 4) return;
      if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
          console.log(xhr.response);
      }
  };
  
  xhr.open('POST', url, true); //GET不会有内容在请求头里，所以Content-Type只需要在POST类使用
  
  // 请求头中的 Content-Type 字段用来告诉服务器，浏览器发送的数据是什么格式的
  
  xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
  //或者
  xhr.setRequestHeader('Content-Type', 'application/json');
  
  //Content-Type对应以下send()内的格式：
  
  //'application/x-www-form-urlencoded'
  xhr.send('username=alex&age=18'); //此类格式和html表单form里的请求格式一致
  
  //'application/json'
  xhr.send(
      JSON.stringify({
          username: 'alex'
      })
  );
  ```



### XHR的事件

##### load 事件

- 响应数据可用时触发

```javascript
const url = 'https://www.imooc.com/api/http/search/suggest?words=js';

const xhr = new XMLHttpRequest();

//代替onreadystatechange，不需要再手动判断readyState
xhr.onload = () => {
    if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
        console.log(xhr.response);
    }
};

//相当于,一般用addEventListener更好：
xhr.addEventListener(
    'load',
    () => {
        if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
            console.log(xhr.response);
        }
    },
    false
);

xhr.open('GET', url, true);
xhr.send(null);

// IE6~8 不支持 load 事件
```



##### error 事件

- 请求发生错误时触发

```javascript
//const url = 'https://www.imooc.com/api/http/search/suggest?words=js';

//错误的url
const url = 'https://www.iimooc.com/api/http/search/suggest?words=js';

const xhr = new XMLHttpRequest();


xhr.addEventListener(
    'load',
    () => {
        if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
            console.log(xhr.response);
        } 
    },
    false
);

//注意，error是请求时就发生错误，而不是响应阶段的错误
xhr.addEventListener(
    'error',
    () => {
        console.log('error');
    },
    false
);

xhr.open('GET', url, true);
xhr.send(null);

// IE10 开始支持
```



##### abort 事件

- 调用 abort() 终止请求时触发

```javascript
const url = 'https://www.imooc.com/api/http/search/suggest?words=js';

const xhr = new XMLHttpRequest();

xhr.addEventListener(
    'load',
    () => {
        if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
            console.log(xhr.response);
        }
    },
    false
);

xhr.addEventListener(
    'abort',
    () => {
        console.log('abort');
    },
    false
);

xhr.open('GET', url, true);
xhr.send(null);

xhr.abort();

//IE10 开始支持
```



##### timeout 事件

- 请求超时后触发

```javascript
const url = 'https://www.imooc.com/api/http/search/suggest?words=js';
const xhr = new XMLHttpRequest();

xhr.addEventListener(
    'load',
    () => {
        if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
            console.log(xhr.response);
        }
    },
    false
);

xhr.addEventListener(
    'timeout',
    () => {
        console.log('timeout');
    },
    false
);

xhr.open('GET', url, true);

xhr.timeout = 10;

xhr.send(null);

// IE8 开始支持
```
