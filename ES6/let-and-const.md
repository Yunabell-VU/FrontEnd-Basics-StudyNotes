## let和const

1. let 和 const 是什么

​    声明变量或声明常量

​    var 声明变量

​    **let** 代替 var，声明**变量**

​    **const** 声明**常量** constant



2. let 和 const 的用法

​    var 一样

```javascript
var username = 'Alex';
let age = 18;
const sex = 'male';
console.log(username, age, sex);
```



3. 什么是变量，什么是常量

```javascript
username = 'ZS';
age = 28;
console.log(username, age);

// 报错
sex = 'female';
```

 var、let声明的就是变量，变量一旦初始化之后，还可以重新赋值

 const 声明的就是常量，常量一旦初始化，就不能重新赋值了，否则就会报错



### const

1. 为什么需要 const

```javascript
let sex = 'male';
 //  ...
 sex = 'female';
 console.log(sex);

const sex = 'male';
// ...
sex = 'female'; //报错
console.log(sex); 
```

const 就是为了那些一旦初始化就不希望重新赋值的情况设计的



2. const 的注意事项

   2.1.使用 const 声明常量，一旦声明，就必须立即初始化，不能留到以后赋值

   ```javascript
   const sex;
   sex='male' //报错
   
   //应该这样初始化
   const sex = 'male';
   ```

   

   2.2.const 声明的常量，允许在不重新赋值的情况下修改它的值

   基本数据类型（不可以重新赋值）

   ```JavaScript
   //报错
   const sex = 'male';
   sex = 'female';
   ```

   引用数据类型

   ```JavaScript
   const person = { username: 'Alex' };
   person = {};//报错
   person.username = 'ZhangSan';//不报错
   console.log(person);//person被修改
   ```

   

3. 什么时候用 const，什么时候用 let

   ```JavaScript
   // for循环这种需要改变的用let
   for (let i = 0; i < 3; i++) {}
   
   //不知道用什么时，先用const，如后面需要修改再改回let
   const username = 'Alex';
   // ...
   username = 'ZhangSan';
   ```

   

### let, const 和 var 的区别

1. 重复声明

   已经存在的变量或常量，又声明了一遍

   var 允许重复声明，let、const 不允许

```javascript
var a = 1;
// ...
var a = 2;//不报错

let a = 1;
// ...
let a = 2;//报错
console.log(a);
```



```javascript
function func(a) {
 let a = 1; //报错。 函数参数形式存在的变量也不可以被再次声明
}

func();
```



2. 变量提升

  var 会提升变量的声明到当前作用域的顶部

```javascript
console.log(a); //报错
```

   

```javascript
console.log(a); //不报错，会打印 undefined
var a = 1;
```

  相当于

```javascript
var a;
console.log(a);
a = 1;
console.log(a);
```



**let、const 不存在变量提升**

```javascript
console.log(a); //报错
let a = 1;
```

养成良好的编程习惯，对于所有的变量或常量，做到先声明，后使用



3. 暂时性死区

只要作用域内存在 let、const，它们所声明的变量或常量就**自动“绑定”这个区域**，不再受到外部作用域的影响

let、const 存在暂时性死区

```javascript
let a = 2;
let b = 1;

//注意，函数只有在被调用时才会形成函数作用域，只是声明并不会存在作用域
function func() {
  console.log(b);//不报错，打印出 1
  console.log(a);//报错，并不会打印外部的 a, 因为下面有let已经绑定了a
  let a = 1;
}
func();
```

 养成良好的编程习惯，对于所有的变量或常量，做到先声明，后使用



4. window 对象的属性和方法

全局作用域中，var 声明的变量，通过 function 声明的函数，会自动变成 window 对象的属性或方法

let、const 不会

```javascript
// var/function
var age = 18;

function add() {}
console.log(window.age); //18
console.log(window.add === add); //true

// let/const
let age = 18;
const add = function () {};
console.log(window.age); //undefined
console.log(window.add === add); //false
```

   

### 块级作用域

1. 什么是块级作用域

 var 没有块级作用域

```javascript
for (var i = 0; i < 3; i++) {
  console.log(i);
}
console.log(i); //3
```



let/const 有块级作用域

```javascript
for (let i = 0; i < 3; i++) {
   console.log(i);
}
console.log(i); // 报错， i is not defined
```

 

2. 作用域链

```javascript
function func() {
   for (let i = 0; i < 3; i++) {
   console.log(i);
   }
}
func();//函数调用时形成函数作用域，函数调用结束，作用域销毁
console.log(i);
```

<img src="http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/image-20210627181210676.png" alt="image-20210627180317315" style="zoom:80%;" /> 

作用域链：内层作用域->外层作用域->...->全局作用域



3. 有哪些块级作用域

```javascript
 // {}
{
  let age = 18;
  console.log(age);
}
console.log(age); //报错

```

  

```javascript
for(){}
while(){}
do{}while()
if(){}
switch(){}
```



```javascript
function(){}

const person = { //对象的{}不构成作用域
  getAge: function () {} //对象内的方法构成函数作用域
};
```



### let和const的应用



<img src="http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/image-20210628152931953.png" alt="image-20210627181210676" style="zoom:50%;" />

三个按钮，点击按钮打印对应数值

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>let 和 const 的应用</title>
    <style>
      body {
        padding: 50px 0 0 150px;
      }

      .btn {
        width: 100px;
        height: 100px;
        margin-right: 20px;
        font-size: 80px;
        cursor: pointer;
      }
    </style>
  </head>
  <body>
    <button class="btn">0</button>
    <button class="btn">1</button>
    <button class="btn">2</button>

    <script>
      //....
    </script>
  </body>
</html>

```

1. 使用 var 来实现

```JavaScript
// var
var btns = document.querySelectorAll('.btn');
for (var i = 0; i < btns.length; i++) {
  btns[i].addEventListener(
      'click',
      function () {
        console.log(i); //点击任何按钮只会打印出 3
      },
   false //是否捕获
   );
}
```



2. 闭包方式解决上面var的问题

```javascript
var btns = document.querySelectorAll('.btn');
for (var i = 0; i < btns.length; i++) {
  //采用立即执行匿名函数的方式
  (function (index) {
     btns[index].addEventListener(
       'click',
       function () {
         console.log(index); //可以打印出对应的按钮数值
       },
       false
     );
   })(i);
}
```



3. let和const实现

```javascript
 // 3.let/const
let btns = document.querySelectorAll('.btn');
for (let i = 0; i < btns.length; i++) {
   btns[i].addEventListener(
      'click',
      function () {
         console.log(i);
      },
      false
   );
}
```