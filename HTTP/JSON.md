## JSON

#### 基本认识

##### JSON 是什么

- Ajax 发送和接收数据的一种格式

- XML

- username=alex&age=18

- JSON
- JSON 全称是 JavaScript Object Notation



```javascript
const url = 'https://www.imooc.com/api/http/search/suggest?words=js';

const xhr = new XMLHttpRequest();

xhr.onreadystatechange = () => {
    if (xhr.readyState != 4) return;
    if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
        console.log(xhr.responseText);
    }
};

xhr.open('GET', url, true);
xhr.send(null);

// {"code":200,"data":[{"word":"jsp"},{"word":"js"},{"word":"json"},{"word":"js \u5165\u95e8"},{"word":"jstl"}]}
// 替代HTML/XML，因为更容易解析成object，而且没有无效信息
```



##### 为什么需要 JSON

- JSON 有 3 种形式，每种形式的写法都和 JS 中的数据类型很像，可以很轻松的和 JS 中的数据类型互相转换

- JS->JSON->PHP/Java

- PHP/Java->JSON->JS



#### JSON的三种形式

##### 简单值形式

**.json** （以.json结尾的文件）

- JSON 的简单值形式就对应着 JS 中的基础数据类型

- 数字、字符串、布尔值、null



`plain.json`

```json
"str"
```



```javascript
const xhr = new XMLHttpRequest();

xhr.onreadystatechange = () => {
    if (xhr.readyState != 4) return;
    if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
        console.log(xhr.responseText);
        console.log(typeof xhr.responseText);
    }
};

xhr.open('GET', './plain.json', true);
xhr.send(null);

//结果
"str"
string
```



**注意事项**

① JSON 中没有 undefined 值

② JSON 中的字符串必须使用双引号

③ JSON 中是不能注释的



##### 对象形式

 JSON 的对象形式就对应着 JS 中的对象

`obj.json`

```json
{
  "name": "张三",
  "age": 18,
  "hobby": ["足球", "乒乓球"],
  "family": {
    "father": "张老大",
    "mother": "李四"
  }
}
```



```javascript
const xhr = new XMLHttpRequest();

xhr.onreadystatechange = () => {
    if (xhr.readyState != 4) return;
    if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
        console.log(xhr.responseText);
        console.log(typeof xhr.responseText);
    }
};

xhr.open('GET', './obj.json', true);

xhr.send(null);

//结果
{
  "name": "张三",
  "age": 18,
  "hobby": ["足球", "乒乓球"],
  "family": {
    "father": "张老大",
    "mother": "李四"
  }
}

string
```



**注意事项**

- JSON 中对象的属性名必须用双引号，属性值如果是字符串也必须用双引号

- JSON 中只要涉及到字符串，就必须使用双引号

- 不支持 undefined



##### 数组形式

 JSON 的数组形式就对应着 JS 中的数组

```json
 [1, "hi", null]
```

`arr.json`

```json
[
  {
    "id": 1,
    "username": "张三",
    "comment": "666"
  },
  {
    "id": 2,
    "username": "李四",
    "comment": "999 6翻了"
  }
]
```



```javascript
const xhr = new XMLHttpRequest();

xhr.onreadystatechange = () => {
    if (xhr.readyState != 4) return;
    if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
        console.log(xhr.responseText);
        console.log(typeof xhr.responseText);
    }
};

xhr.open('GET', './arr.json', true);

xhr.send(null);

//结果
[
  {
    "id": 1,
    "username": "张三",
    "comment": "666"
  },
  {
    "id": 2,
    "username": "李四",
    "comment": "999 6翻了"
  }
]
string
```

**注意事项**

- 数组中的字符串必须用双引号

- JSON 中只要涉及到字符串，就必须使用双引号

- 不支持 undefined



#### JSON的常用方法

##### JSON.parse()

```javascript
// JSON.parse() 可以将 JSON 格式的字符串解析成 JS 中的对应值
// 一定要是合法的 JSON 字符串，否则会报错

const xhr = new XMLHttpRequest();

xhr.onreadystatechange = () => {
    if (xhr.readyState != 4) return;
    if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
        console.log(xhr.responseText);
        console.log(typeof xhr.responseText);
        console.log(JSON.parse(xhr.responseText));
        console.log(JSON.parse(xhr.responseText).data);
    }
};

xhr.open(
    'GET',
    'https://www.imooc.com/api/http/search/suggest?words=js',
    true
);

xhr.send(null);
```

![image-20210812202655987](http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/image-20210812202655987.png)



##### JSON.stringify()

```javascript
// JSON.stringify() 可以将 JS 的基本数据类型、对象或者数组转换成 JSON 格式的字符串

console.log(
    JSON.stringify({
        username: 'alex',
        age: 18
    })
);

//实际应用
const xhr = new XMLHttpRequest();

xhr.open(
    'POST',
    'https://www.imooc.com/api/http/search/suggest?words=js',
    true
);

xhr.send(
    JSON.stringify({
        username: 'alex',
        age: 18
    })
);
```



##### 使用 JSON.parse() 和 JSON.stringify() 封装 localStorage

`storage.js`

```javascript
const storage = window.localStorage;

// 设置
const set = (key, value) => {
  // {
  //   username: 'alex'
  // }
  storage.setItem(key, JSON.stringify(value));
};

// 获取
const get = key => {
  // 'alex'
  // {
  //   "username": "alex"
  // }
  return JSON.parse(storage.getItem(key));
};

// 删除
const remove = key => {
  storage.removeItem(key);
};

// 清空
const clear = () => {
  storage.clear();
};

export { set, get, remove, clear };

```



```javascript
import { get, set, remove, clear } from './storage.js';

set('username', 'alex');

console.log(get('username'));

set('zs', {
    name: '张三',
    age: 18
});

console.log(get('zs'));

remove('username');

clear();
```