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

