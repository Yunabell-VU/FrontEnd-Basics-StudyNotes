## Module 模块

### 基本认识

   // 1.什么是模块

   // 模块：一个一个的局部作用域的代码块



   // 2.什么是模块系统

   // 模块系统需要解决的主要问题

   // ① 模块化的问题

   // ② 消除全局变量

   // ③ 管理加载顺序



   // RequireJS seaJS （第三方模块库，已经是过去式）



   // ES Module （ES6的官方模块）



#### 基本用法

1. 使用 Module 模块化之前的例子



2. 使用 script 标签加载模块

   // console.log(Slider);



   // 一个文件就是一个模块

   // **只要你会用到 import 或 export，在使用 script 标签加载的时候，就要加上`type="module"`**



3. 分析 Module 解决的问题

   ① 模块化的问题

   ② 消除全局变量

   ③ 管理加载顺序



### Module的导入和导出

1. 认识导出和导入

   - 导出的东西可以被导入（import），并访问到
   - 一个模块没有导出，也可以将其导入
   -  **被导入的代码都会执行一遍，也仅会执行一遍**

```JavaScript
import './module.js';
import './module.js';
import './module.js';
//多次导入也只会执行一次
```

2. 基本用法

   ```JavaScript
   //export default用法
   //**一个模块只能有一个 export default**
   
   //module.js文件中
   const age = 18
   export default age;
   
   //html文件中
   //可以随便起名
   
   //import aaa from './module.js';
   import age from './module.js';
   console.log(age);
   
   //export default 可以导出多种类型
   export default age;
   export default 20;
   export default {};
   
   const fn = () =>{};
   export default fn;
   export default function(){};
   export default class{};
   //无需给函数等起名是因为外面导入时可以自己命名，用不到这里的命名
   ```



#### export 和 对应的 import

1. 基本用法

```JavaScript
// export 声明或语句
// export const age = 18; 但一般不这样export，而是使用下面的语法

const age = 18;
export { age };


// 不能随意命名
import { age } from './module.js';
console.log(age);

//export可导出内容

function fn(){}
export fn; //报错

export function fn(){}
export function (){} //报错 匿名不行

class className {}
export className; //可以

export class className {}
export class {} //报错 匿名不行
```



2. 多个导出导入

```javascript
//可以导出多个内容
export {fn, className, age};

//可以分开导入，也可以统一导入
import { fn } from './module.js';
console.log(fn);

import { className } from './module.js';
console.log(className);


import { fn, age, className } from './module.js';
console.log(fn, age, className);
```



3. 导出导入时起别名

```javascript
export { fn as func, className, age };

import { func, age, className as Person } from './module.js';

console.log(Person);
```



4. 整体导入

```javascript
// 会导入所有输出，包括通过 export default 导出的

import { func, age, className } from './module.js';
console.log(func, age, className);


import obj from './module.js';
import * as obj from './module.js';

console.log(obj);
```



5. 同时导入

```JavaScript
// export default 可以和export同时使用

import { func, age, className } from './module.js';
import age2 from './module.js';


// 一定是 export default 的在前

import age2, { func, age, className } from './module.js';
import { func, age, className },age2 from './module.js'; ×
```



### 注意事项和应用

#### 注意事项

```html
 <body>

  <!-- <script src="./module.js"></script> -->
  <!-- <script src="./module.js" type="module"></script> -->

    <script type="module">
        ...
  </script>
 </body>
```



1. 模块顶层的 this 指向

   ```JavaScript
   //module.js
   console.log(this);
   
   //非模块加载方式，this指向Window
   // 模块中，顶层的 this 指向 undefined
   import './module.js';
   
   //执行结果 undefined
   ```
   
   

2. import 和 import()

```JavaScript
// import 命令具有提升效果，会提升到整个模块的头部，率先执行

console.log('沙发');
console.log('第二');

import './module.js';
//执行结果
//undefined
//沙发
//第二


// import 执行的时候，代码还没执行
// import 和 export 命令只能在模块的顶层，不能在代码块中执行

if (PC) {
    import 'pc.js';
} else if (Mobile) {
    import 'mobile.js';
}
//以上pseudo code 是错误写法，无法执行

// 想要完成以上需求，使用import()
//import() 可以按条件导入，返回promise对象

if (PC) {
    import('pc.js').then().catch();
} 
else
    import('mobile.js').then().catch();
}

//注意：import()现在只是提案，还不是标准，有不兼容的风险
```



3. 导入导出的复合写法

```JavaScript
// 复合写法导出的，无法在当前模块中使用
//导入导出的复合写法，在导入的同时将其导出出去，相当于中转站，在当前模块是无法访问导入的变量

export { age } from './module.js';
console.log(age); //报错

// 等价于，但是下面的写法，是可以访问到导入的内容的

//导入文件
import { age } from './module.js';

//模块文件
export { age } from './module.js';
console.log(age); // 18
```