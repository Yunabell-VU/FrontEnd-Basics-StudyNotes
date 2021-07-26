## Babel和Webpack



### Babel

#### 什么是Babel

1. 认识 Babel

- 官网：https://babeljs.io/

- 在线编译：https://babeljs.io/repl



   Babel 是 JavaScript 的编译器，用来将 ES6 的代码，转换成 ES6 之前的代码



2. 使用 Babel

```JavaScript
//在Babel官网的 Try it out 中，可以简单的编译JS代码

// ES6版本

let name = 'Alex';
const age = 18;

const add = (x, y) => x + y;


// Set Map

new Promise((resolve, reject) => {
    resolve('成功');
}).then(value => {
    console.log(value);
});

Array.from([1, 2]);

class Person {
    constructor(name, age) {
        Object.assign(this, { name, age });
    }
}

new Person('Alex', 18);

import './index.js';
```



```JavaScript

// 使用 Babel 编译后 (勾选es2015)

('use strict'); //模块中默认strict模式

require('./index.js'); //import的编译，这里需要结合Webpack才能实现import功能

function _instanceof(left, right) {
    if (
        right != null &&
        typeof Symbol !== 'undefined' &&
        right[Symbol.hasInstance]
    ) {
        return !!right[Symbol.hasInstance](left);
    } else {
        return left instanceof right;
    }
}

function _classCallCheck(instance, Constructor) {
    if (!_instanceof(instance, Constructor)) {
        throw new TypeError('Cannot call a class as a function');
    }
}

var name = 'Alex';
var age = 18;

var add = function add(x, y) {
    return x + y;
};

new Promise(function (resolve, reject) {
    resolve('成功');
}).then(function (value) {
    console.log(value);
});

Array.from([1, 2]);

var Person = function Person(name, age) {
    _classCallCheck(this, Person);
    Object.assign(this, {
        name: name,
        age: age
    });
};

new Person('Alex', 18);
```



3. 解释编译结果

- Babel 本身可以编译 ES6 的大部分语法，比如 let、const、箭头函数、类

- 但是对于 ES6 新增的 API，比如 Set、Map、Promise 等全局对象，以及一些定义在全局对象上的方法（比如 Object.assign/Array.from）都不能直接编译，需要借助其它的模块

- Babel 一般需要配合 Webpack 来编译模块语法



#### 使用Babel前的准备工作



官网Setup里可以查看使用方法，常用是CLI（命令行工具）是Webpack



文档：

```
├─- package-lock.json
└─- package.json
```

1. 什么是 Node.js 和 npm

- Node.js 是个平台或者工具，对应浏览器

- 相当于后端的 JavaScript = ECMAScript + IO + File + ...等服务器端的操作



-  `npm：node` 包管理工具

  ```shell
  $ npm install
  ```

  

2. 安装 Node.js

   ```shell
   $ node -v
   $ npm -v
   ```

   

3. 初始化项目

   ```shell
   $ npm init 
   
   # 生成package.json，记录安装过的npm文件
   ```

   

4. 安装 Babel 需要的包

   ```shell
   $ npm install --save-dev @babel/core @babel/cli
   #开发依赖，上线后不需要
   
   $ npm install --save-dev @babel/core@7.11.0 @babel/cli@7.10.5
   #可以选择安装指定版本号
   
   $ npm install
   #在package.json复制过来的情况下，直接通过以上命令可以安装package.json中的包
   ```

   

#### 使用Babel编译ES6代码

```
├─- dist
|   └── babel.js
├─- src
|   └── babel.js
├─- package-lock.json
└─- package.json
```

1. 首先安装 包

   ```shell
   $ npm install
   ```

   

2. 在官网 **Setup-->CLI--> Usage** 中复制（"scripts: {}"）以下代码到package.json中

   ```json
   +   "scripts": {
   +     "build": "babel src -d lib"
   +   },
   
   //package.json文件示例
     {
       "name": "my-project",
       "version": "1.0.0",
   +   "scripts": {
   +     "build": "babel src -d lib" 
         //-d 是 --out-dir 的缩写，代表输出目录，前面的src是源目录，后面的lib是可修改的输出文件夹名
   +   },
       "devDependencies": {
         "@babel/cli": "^7.0.0"
       }
     }
   
   ```

   

3. 编辑配置文件

   ```shell
   $ npm install @babel/preset-env --save-dev 
   #最新版
   
   $ npm install @babel/preset-env@7.11.0 --save-dev
   #指定版本号
   ```

   `.babelrc` file:

   ```json
   {
       "presets": ["@babel/preset-env"]
   }
   ```

   



4. 执行build命令

```shell
$ npm run build

#之前src文件夹中的所有js文件会被编译，编译好的文件在lib中
```