## FormData

##### 使用 Ajax 提交表单

- 不通过AJAX的话，submit会自动跳转

```html
<body>
    <form
      id="login"
      action="https://www.imooc.com/api/http/search/suggest?words=js"
      method="POST"
      enctype="multipart/form-data"
    >
      <input type="text" name="username" placeholder="用户名" />
      <input type="password" name="password" placeholder="密码" />
      <input id="submit" type="submit" value="登录" />
    </form>
</body>
```

- 不使用FormData的情况：


```javascript
const login = document.getElementById('login');

//通过解构赋值，获取username和password
const { username, password } = login;

//相当于
// console.log(login.username);
// console.log(login.password);

const btn = document.getElementById('submit');
const url = 'https://www.imooc.com/api/http/search/suggest?words=js';

btn.addEventListener(
    'click',
    e => {
        // 阻止表单默认的自动提交
        e.preventDefault();
        
        // 表单数据验证，这里省略
        
        // 发送 Ajax 请求
        const xhr = new XMLHttpRequest();
        
        xhr.addEventListener(
            'load',() => {
                if (
                    (xhr.status >= 200 && xhr.status < 300) ||xhr.status === 304) {
                    console.log(xhr.response);
                }
            },
            false
        );
        xhr.open('POST', url, true);
        
        // 组装数据
        const data = `username=${username.value}&password=${password.value}`;
        
        xhr.setRequestHeader(
            'Content-Type',
            'application/x-www-form-urlencoded'
        );
        
        xhr.send(data);
        
    },false
);
```

以上组装数据的方法在表单数据简单的情况下还可以使用，但如果表单提交内容复杂，这种方法就不适用了。需要使用FormData。



##### FormData 的基本用法

- 通过 HTML 表单元素创建 FormData 对象

  ```javascript
  const fd = new FormData(表单元素);
  xhr.send(fd);
  ```

- 通过 append() 方法添加数据

  ```javascript
  const fd = new FormData(表单元素);
  
  //实际运用中一般不需要手动添加
  
  fd.append('age', 18);
  fd.append('sex', 'male');
  
  //遍历查看表单内容
  for (const item of data) {
              console.log(item);
          }
  
  xhr.send(fd);
  
  // IE10 及以上可以支持
  ```
  
- 实际应用：

```javascript
const login = document.getElementById('login');

const { username, password } = login;

const btn = document.getElementById('submit');
const url = 'https://www.imooc.com/api/http/search/suggest?words=js';

btn.addEventListener(
    'click',
    e => {
        e.preventDefault();
        
        const xhr = new XMLHttpRequest();
        
        xhr.addEventListener(
            'load',() => {
                if (
                    (xhr.status >= 200 && xhr.status < 300) ||xhr.status === 304) {
                    console.log(xhr.response);
                }
            },
            false
        );
        xhr.open('POST', url, true);
        
        // 组装数据
        const data = new FormData(login);
        
        //注意不需要设置Content-Type
        
        xhr.send(data);
        
    },false
);
```

------



## Ajax封装

```shell
|- ajax
	|- ajax.js
	|- constants.js
	|- defaults.js
	|- index.js
	|_ utils.js
|_ index.html
```



##### `ajax.js`

```javascript
// ajax.js

// 常量
import {
  HTTP_GET,
  CONTENT_TYPE_FORM_URLENCODED,
  CONTENT_TYPE_JSON
} from './constants.js';

// 工具函数
import { serialize, addURLData, serializeJSON } from './utils.js';

// 默认参数
import DEFAULTS from './defaults.js';

// Ajax 类
class Ajax {
  constructor(url, options) {
    this.url = url; //保存在this之后，其他function才可以调用url
    this.options = Object.assign({}, DEFAULTS, options); //优先使用client参数，如果没有就使用DEFUALTS

    // 初始化
    this.init();
  }

  // 初始化
  init() {
    const xhr = new XMLHttpRequest();

    this.xhr = xhr;

    // 绑定响应事件处理程序
    this.bindEvents();

    xhr.open(this.options.method, this.url + this.addParam(), true);

    // 设置 responseType
    this.setResponseType();

    // 设置跨域是否携带 cookie
    this.setCookie();

    // 设置超时
    this.setTimeout();

    // 发送请求
    this.sendData();
  }

  // 绑定响应事件处理程序
  bindEvents() {
    const xhr = this.xhr;

    const { success, httpCodeError, error, abort, timeout } = this.options; //解构赋值

    // load
    xhr.addEventListener(
      'load',
      () => {
        if (this.ok()) {
          success(xhr.response, xhr);
        } else {
          httpCodeError(xhr.status, xhr);
        }
      },
      false
    );

    // error
    // 当请求遇到错误时，将触发 error 事件
    xhr.addEventListener(
      'error',
      () => {
        error(xhr);
      },
      false
    );

    // abort
    xhr.addEventListener(
      'abort',
      () => {
        abort(xhr);
      },
      false
    );

    // timeout
    xhr.addEventListener(
      'timeout',
      () => {
        timeout(xhr);
      },
      false
    );
  }

  // 检测响应的 HTTP 状态码是否正常
  ok() {
    const xhr = this.xhr;
    return (xhr.status >= 200 && xhr.status < 300) || xhr.status === 304;
  }

  // 在地址上添加数据
  addParam() {
    const { params } = this.options;

    if (!params) return '';

    return addURLData(this.url, serialize(params));
  }

  // 设置 responseType
  setResponseType() {
    this.xhr.responseType = this.options.responseType;
  }

  // 设置跨域是否携带 cookie
  setCookie() {
    if (this.options.withCredentials) {
      this.xhr.withCredentials = true;
    }
  }

  // 设置超时
  setTimeout() {
    const { timeoutTime } = this.options;

    if (timeoutTime > 0) {
      this.xhr.timeout = timeoutTime;
    }
  }

  // 发送请求
  sendData() {
    const xhr = this.xhr;

    if (!this.isSendData()) {
      return xhr.send(null);
    }

    let resultData = null;
    const { data } = this.options;

    // 发送 FormData 格式的数据
    if (this.isFormData()) {
      resultData = data;
    } else if (this.isFormURLEncodedData()) {
      // 发送 application/x-www-form-urlencoded 格式的数据

      this.setContentType(CONTENT_TYPE_FORM_URLENCODED);
      resultData = serialize(data);
    } else if (this.isJSONData()) {
      // 发送 application/json 格式的数据

      this.setContentType(CONTENT_TYPE_JSON);
      resultData = serializeJSON(data);
    } else {
      // 发送其他格式的数据

      this.setContentType();
      resultData = data;
    }

    xhr.send(resultData);
  }

  // 是否需要使用 send 发送数据
  isSendData() {
    const { data, method } = this.options;

    if (!data) return false;

    if (method.toLowerCase() === HTTP_GET.toLowerCase()) return false;

    return true;
  }

  // 是否发送 FormData 格式的数据
  isFormData() {
    return this.options.data instanceof FormData;
  }

  // 是否发送 application/x-www-form-urlencoded 格式的数据
  isFormURLEncodedData() {
    return this.options.contentType
      .toLowerCase()
      .includes(CONTENT_TYPE_FORM_URLENCODED);
  }

  // 是否发送 application/json 格式的数据
  isJSONData() {
    return this.options.contentType.toLowerCase().includes(CONTENT_TYPE_JSON);
  }

  // 设置 Content-Type
  setContentType(contentType = this.options.contentType) {
    if (!contentType) return;

    this.xhr.setRequestHeader('Content-Type', contentType);
  }

  // 获取 XHR 对象
  getXHR() {
    return this.xhr;
  }
}

export default Ajax;

```

##### `defaults.js`

```javascript
//defaults.js

// 常量
import { HTTP_GET, CONTENT_TYPE_FORM_URLENCODED } from './constants.js';

// 默认参数
const DEFAULTS = {
  method: HTTP_GET,
  // 请求头携带的数据
  params: null,
  //如果有参数，会以下面这种对象的形式传递
  // params: {
  //   username: 'alex',
  //   age: 18
  // }
    
  //以上形式在url末端上会体现成
  // username=alex&age=18
    
  // 请求体携带的数据
  data: null,
  // data: {
  //   username: 'alex',
  //   age: 18
  // }
    
  //可以通过上面对象形式或者是：
  // data: FormData 数据

  contentType: CONTENT_TYPE_FORM_URLENCODED, //这里采用这种格式
  responseType: '',
  timeoutTime: 0,
  withCredentials: false,

  // 方法
  success() {},
  httpCodeError() {},
  error() {},
  abort() {},
  timeout() {}
};

export default DEFAULTS;

```

