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



### Webpack

### 基本认识

Vue，React之类的框架底层也是基于Webpack。

 https://webpackjs.com 



1. 认识 Webpack

- webpack 是**静态模块打包器**，当 webpack 处理应用程序时，会将所有这些模块打包成一个或多个文件

```JavaScript
import './module.js'
require('./module.js')
```

 

2. 什么是 Webpack

- 模块
  - webpack 可以处理 js/css/图片、图标字体等单位



- 静态

  - 开发过程中存在于本地的 js/css/图片/图标字体等文件，就是静态的

    ```html
    <!--静态-->
    <img src="./img/logo.png" alt="" />
    
    <!--动态-->
    <img src="https://www.imooc.com/static/img/index/logo.png" alt="" />
    ```

  - 动态的内容，webpack没办法处理，只能处理静态的

------



### 使用流程



1. 初始化流程

```shell
$ npm init
package name: <name> #必须是英文名称
version: <(1.0.0)> #默认值即可
... #一路回车

Is this OK? (yes)
#生成package.json
```



2. 安装webpack需要的包

   ```shell
   $ npm install --save-dev webpack-cli@3.3.12 webpack@4.44.1
   ```

   

3. 配置 webpack

   ```
   ├─- src
   	├── index.js
   	└── module.js
   ├─- index.html 
   ├─- webpack.config.js
   ├─- package.json
   └─- package-lock.json
   ```

   

   `webpack.config.js`

   ```json
   //使用的是Node.js的模块方法，而不是ES6
   
   const path = require('path');
   
   //需要导出的内容写在module.exports中
   //以下是官网中提供的例子
   module.exports = {
     mode: 'development', //指定开发模式的话，编译出来的代码不会被压缩。这条不写，编译出的代码是挤在一起的
     entry: './src/index.js',
     output: {
       path: path.resolve(__dirname, 'dist'),
       filename: 'bundle.js'
     }
   };
   ```



4. 编译并测试

   `index.js`

   ```javascript
   import age from './module.js';
   console.log('index.js', age);
   ```

   `module.js`

   ```javascript
   export default 18;
   console.log('module.js');
   ```

   `package.json`

   ```json
   {
     "name": "webpack2",
     "version": "1.0.0",
     "description": "",
     "main": "index.js",
     "scripts": {
       "webpack": "webpack --config webpack.config.js" //自行创立的Babel命令，"<命名>"可以自定义
       //"webpack": "webpack" //默认webpack.config.js命名情况下以上两种写法效果一致
     },
     "author": "",
     "license": "ISC",
     "devDependencies": {
       "webpack": "^4.44.1",
       "webpack-cli": "^3.3.12"
     }
   }
   ```

   `执行webpack编译`

   ```shell
   $ npm run webpack
   ```

   目录中多出`dist`文件夹

   ```
   ├─- src
   	├── index.js
   	└── module.js
   ├─- dist
   	└── bundle.js	
   ├─- index.html 
   ├─- webpack.config.js
   ├─- package.json
   └─- package-lock.json
   ```

   ------

   

### 核心概念

#### entry和output

1. entry

- 指定入口文件

- 单入口

  ```json
  const path = require('path');
  
  module.exports = {
    mode: 'development',
    entry: './src/index.js',
    output: {
      path: path.resolve(__dirname, 'dist'),
      filename: 'bundle.js'
    }
  };
  ```

  

- 多入口

  ```json
  module.exports = {
    mode: 'development',
    entry: {
        main: './src/index.js',
        search: './src/search.js' //"search"这样的命名是自定义的，根据需求来起名
    },
    output: {
      path: path.resolve(__dirname, 'dist'),
      filename: '[name].js'
    }
  };
  ```

  

2. output

- 出口配置

  ```json
  const path = require('path');
  
  module.exports = {
    mode: 'development',
    entry: {
        main: './src/index.js',
        search: './src/search.js'
    },
    output: {
      path: path.resolve(__dirname, 'dist'),
      filename: '[name].js'  //多入口的情况下，需要这样来书写，如果指定一个名字，会使多个入口的出口有冲突
    }
  };
  ```

  

#### loader

1. 什么是 loader

-  webpack js/css/图片
- loader 让 webpack 能够去处理那些非 JS 文件的模块
-  babel



2. babel-loader

   ```shell
   npm install --save-dev babel-loader@8.1.0
   ```

   

3. 安装 Babel

   ```shell
   npm install --save-dev @babel/core@7.11.0 @babel/preset-env@7.11.0
   ```

   

4. 配置 babel-loader

​    https://www.webpackjs.com/loaders/

​	`webpack.config.js`

```json	
const path = require('path');

module.exports = {
  mode: 'development',
  entry: {
    index: './src/index.js'
  },
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: '[name].js'
  },
  module: {
    rules: [
      {
        test: /\.js$/, //匹配在哪些文件中使用babel文件，此示例中，匹配所有.js文件
        exclude: /node_modules/, //符合上述条件中排除哪个文件夹
        loader: 'babel-loader' //连通babel和webpack，先babel编译再webpack打包
      }
    ]
  }
};
```

到这一步，虽然有一些编译，但是很多ES6特有的语法并没有改变，所以需要接下来的后续工作。



5. 引入 core-js

      编译新增 API

   http://babeljs.io/docs/en/babel-polyfill

   ```shell
   npm install --save-dev core-js@3.6.5
   ```

   

   ```JavaScript
    import "core-js/stable"; //在index.js中
   ```

   

6. 打包并测试

   ```shell
   npm run webpack
   ```

   

#### plugins

1. 什么是 plugins

- 插件

- loader 被用于帮助 webpack 处理各种模块，而插件则可以用于执行范围更广的任务



   https://www.webpackjs.com/plugins/



2. html-webpack-plugin （以此插件为例，该插件可以自动在html中导入js文件）

   ```shell
    npm install --save-dev html-webpack-plugin@4.3.0
   ```

   

3. 配置 html-webpack-plugin 插件

```json
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin'); //引入插件

module.exports = {
  mode: 'development',
  entry: {
    index: './src/index.js',
    search: './src/search.js'
  },
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: '[name].js'
  },
  plugins: [
    // 单入口
    // new HtmlWebpackPlugin({
    //   template: './index.html'
    // })

    // 多入口
    new HtmlWebpackPlugin({
      template: './index.html',
      filename: 'index.html', //必须指定命名，否则后面的模板会把前面的覆盖到一个文件
      chunks: ['index'], //引入的文件名（import部分，如果不写，会全部js文件都引入）
      minify: {
        // 删除 index.html 中的注释
        removeComments: true,
        // 删除 index.html 中的空格
        collapseWhitespace: true,
        // 删除各种 html 标签属性值的双引号
        removeAttributeQuotes: true
      }
    }),
    new HtmlWebpackPlugin({
      template: './search.html',
      filename: 'search.html',
      chunks: ['search']
    })
  ]
};
```

------



### 应用

#### 处理CSS文件

```
├─- src
	├── index.js
	└── index.css
├─- index.html 
├─- webpack.config.js
├─- package.json
└─- package-lock.json
```

**在js文件中导入css文件**

```javascript
import './index.css';
```

`package.json`配置

```json
{
  "name": "webpack-css",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "webpack": "webpack"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "css-loader": "^4.1.1",
    "html-webpack-plugin": "^4.3.0",
    "mini-css-extract-plugin": "^0.9.0",
    "style-loader": "^1.2.1",
    "webpack": "^4.44.1",
    "webpack-cli": "^3.3.12"
  }
}
```



**添加css loader**

webpack原生不能识别css文件，需要通过loader来连接

```shell
$ npm install --save-dev css-loader@4.1.1
```

**添加style loader**

光有css-loader只能帮助识别css文件，但是并不会正常引入，所以需要style-loader来引入

```shell
$ npm install --save-dev style-loader@1.2.1
```

**通过link标签引入**

除了style标签引入，也可以通过link标签引入，而且这种方式更常用。这里需要用到`miniCssExtract`插件

```shell
$ npm install --save-dev mini-css-extract-plugin@0.9.0
```



`webpack.config.js`配置

```javascript
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

module.exports = {
  mode: 'development',
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: '[name].js'
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        // loader: 'css-loader' //单个loader可以这样写，多个loader需要使用use
        // use: ['style-loader', 'css-loader'] //通过style标签引入，注意顺序
        use: [MiniCssExtractPlugin.loader, 'css-loader'] //通过link标签引入
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './index.html',
      filename: 'index.html'
    }),
    new MiniCssExtractPlugin({
      filename: 'css/[name].css' //整合css输出位置
    })
  ]
};
```



**打包webpack**

```shell
$ npm run webpack
```



#### 使用file-loader处理CSS中的图片

远程图片（外部资源）是不需要考虑webpack的，只有本地的图片才需要被webpack处理。

```
├─- src
	├── img
	    └── logo.png
	├── index.js
	└── index.css
├─- index.html 
├─- webpack.config.js
├─- package.json
└─- package-lock.json
```

本地图片不经过file-loader处理，webpack无法识别。

注意：file-loader并不是单独用来处理图片的，任何文件都可以

```shell
$ npm install --save-dev file-loader@6.0.0
```

`webpack.config.js`配置

```javascript
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

module.exports = {
  mode: 'development',
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: '[name].js'
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          {
            loader: MiniCssExtractPlugin.loader,
            options: {
              publicPath: '../' //加了这个之后，图片等引用路径会加上../
            }
          },
          'css-loader'
        ]
      },
      {
        test: /\.(jpg|png|gif)$/,
        use: {
          loader: 'file-loader',
          options: {
            name: 'img/[name].[ext]' //指定命名和输出路径
          }
        }
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './index.html',
      filename: 'index.html'
    }),
    new MiniCssExtractPlugin({
      filename: 'css/[name].css'
    })
  ]
};
```



#### 使用html-withimg-loader处理HTML中的图片

html标签中引入的本地图片如果不经过loader处理，则无法复制图片文件到打包文件中，也不会被替换正确路径。

```shell
npm install --save-dev html-withimg-loader@0.1.16
```

`webpack.config.js`

**注意file-loader的配置**

```javascript
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

module.exports = {
  mode: 'development',
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: '[name].js'
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          {
            loader: MiniCssExtractPlugin.loader,
            options: {
              publicPath: '../'
            }
          },
          'css-loader'
        ]
      },
      {
        test: /\.(jpg|png|gif)$/,
        use: {
          loader: 'file-loader',
          options: {
            name: 'img/[name].[ext]',
            esModule: false //这里不写的话，html文件中的img会被替换成对象形式
          }
        }
      },
      {
        test: /\.(htm|html)$/,
        loader: 'html-withimg-loader'
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './index.html',
      filename: 'index.html'
    }),
    new MiniCssExtractPlugin({
      filename: 'css/[name].css'
    })
  ]
};
```



#### 使用file-loader处理JS中的图片

在js文件中同样可以引入图片

```javascript
import './index.css';
import img from './img/logo.png';

console.log(img);

const imgEl = document.createElement('img');
imgEl.src = img;

document.body.appendChild(imgEl);
```



#### 使用url-loader处理图片

使用url-loader的话，就不用再配置file-loader了，但是url-loader的底层涉及file-loader，所以还是需要安装file-loader

```shell
$ npm install --save-dev url-loader@4.1.0
```

`webpack.config.js`

```javascript
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

module.exports = {
  mode: 'development',
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: '[name].js'
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          {
            loader: MiniCssExtractPlugin.loader,
            options: {
              publicPath: '../'
            }
          },
          'css-loader'
        ]
      },
      {
        test: /\.(jpg|png|gif)$/,
        use: {
          loader: 'url-loader',
          options: {
            name: 'img/[name].[ext]',
            esModule: false,
            limit: 10000 //小于10kb的文件会被做base64处理，文件不会再被复制过去
          }
        }
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './index.html',
      filename: 'index.html'
    }),
    new MiniCssExtractPlugin({
      filename: 'css/[name].css'
    })
  ]
};
```

**注意：小图标等比较小的图片可以做base64处理，稍大的图片不要这么处理，会让js文件变的很大**



#### 使用webpack-dev-server搭建开发环境

使用webpack-dev-server可以帮助每次保存之后自动打包，免除每次手动删除输出文件再次打包的重复劳动。

```shell
$ npm install --save-dev webpack-dev-server@3.11.0
```

`package.json`

```json
{
  "name": "webpack-url-loader",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "webpack": "webpack",
    "dev": "webpack-dev-server --open chrome" //使用一次即可
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "css-loader": "^4.1.1",
    "html-webpack-plugin": "^4.3.0",
    "mini-css-extract-plugin": "^0.9.0",
    "webpack": "^4.44.1",
    "webpack-cli": "^3.3.12",
    "webpack-dev-server": "^3.11.0"
  }
}
```

```shell
$ npm run dev
```

每次项目保存之后，浏览器中可以实时看到变化。输出的内容在内存中，不在项目目录中。



`webpack.config.js` 

使用默认配置可以不用再webpack.config.js文件中做任何修改。

如果需要改变配置，可以参考 https://www.webpackjs.com/configuration/dev-server/

```javascript
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

module.exports = {
  mode: 'development',
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: '[name].js'
  },
  // devServer:{},//默认配置，不需要写
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          {
            loader: MiniCssExtractPlugin.loader,
            options: {
              publicPath: '../'
            }
          },
          'css-loader'
        ]
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './index.html',
      filename: 'index.html'
    }),
    new MiniCssExtractPlugin({
      filename: 'css/[name].css'
    })
  ]
};
```