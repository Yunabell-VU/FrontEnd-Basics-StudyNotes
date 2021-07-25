## ES6新增方法



### 字符串的新增方法



#### includes()

判断字符串中是否含有某些字符



1. 基本用法

```JavaScript
console.log('abc'.includes('a')); // true

console.log('abc'.includes('ab')); // true

console.log('abc'.includes('bc')); // true

console.log('abc'.includes('ac')); // false (需要连续)
```



2. 第二个参数

```JavaScript
// 表示开始搜索的位置，默认是 0

console.log('abc'.includes('a')); // true

console.log('abc'.includes('a', 0)); // true

console.log('abc'.includes('a', 1)); // false （从b开始搜索）
```



3. 应用

```javascript
//通过点击目标，给url链接增加指定字符
// https://www.imooc.com/course/list
// https://www.imooc.com/course/list?c=fe&sort=pop&name=value

let url = 'https://www.imooc.com/course/list?';

const addURLParam = (url, name, value) => {
	url += url.includes('?') ? '&' : '?'; //如果url中包含？，增加 '&'，如果没有则添加'?'
	url += `${name}=${value}`; //模板字符串
	return url;
};

url = addURLParam(url, 'c', 'fe');
console.log(url); //https://www.imooc.com/course/list?c=fe

url = addURLParam(url, 'sort', 'pop');
console.log(url); //https://www.imooc.com/course/list?c=fe&sort=pop
```



#### padStart() 和padEnd()

补全字符串长度



1. 基本用法

```javascript
console.log('x'.padStart(5, 'ab'));//ababx (字符串总共5位，用ab来补)

console.log('x'.padEnd(5, 'ab')); //xabab

console.log('x'.padEnd(4, 'ab')); //xaba
```



2. 注意事项

```javascript
// 原字符串的长度，等于或大于最大长度，不会消减原字符串，字符串补全不生效，返回原字符串

console.log('xxx'.padStart(2, 'ab')); //xxx

console.log('xxx'.padEnd(2, 'ab')); //xxx


// 用来补全的字符串与原字符串长度之和超过了最大长度，截去超出位数的补全字符串，原字符串不动

console.log('abc'.padStart(10, '0123456789')); //0123456abc

console.log('abc'.padEnd(10, '0123456789')); //abc0123456


// 如果省略第二个参数，默认使用空格补全长度

console.log('x'.padStart(4)); //    x (空格填充)

console.log('x'.padEnd(4)); //x    (空格填充)
```



3. 应用

```javascript
// 显示日期格式

// 2020
// 10
// 10


// 2020-10-10
// 2020-01-01

console.log('10'.padStart(2, 0)); //10
console.log('1'.padStart(2, 0)); //01
```



#### trimStart() 和 trimEnd()

清除字符串的首或尾空格，中间的空格不会清除



1. 基本用法

```javascript
const s = ' a b c ';
console.log(s); //·a·b·c· （·代表空格）

console.log(s.trimStart()); //a·b·c· 
console.log(s.trimLeft()); //a·b·c·

console.log(s.trimEnd()); //·a·b·c
console.log(s.trimRight()); //·a·b·c

console.log(s.trim()); //a·b·c
```



2. 应用

```html
<body>
    <input type="text" id="username" />
    <input type="submit" value="提交" id="btn" />
    <script>
    ...
    </script>
</body>
```



```javascript
//应用在表单提交
const usernameInput = document.getElementById('username');
const btn = document.getElementById('btn');


btn.addEventListener(
	'click',
	() => {
	console.log(usernameInput.value);

	// 验证
	console.log(usernameInput.value.trim()); //过滤 纯空格 的内容
	if (usernameInput.value.trim() !== '') { //判断是否为空
        // 可以提交
		console.log('可以提交');
	} else {
		// 不能提交
		console.log('不能提交');
	}

	// 手动提交 (之后利用AJAX提交)
	},
	false
);
```



#### replaceAll 方法

1. 为什么引入replaceAll方法？

   由于字符串的实例方法 **replace()** 方法只能替换第一个匹配的内容，如：

   ```JavaScript
   'aabbf'.replace('b', '_'); 
   ```

   输出的结果为：`aa_bf`。只替换了第一个b。

   

   若要替换所有内容，就得使用正则表达式的g修饰符。如：

   ```JavaScript
   'aabbcc'.replace(/b/g,'_');
   ```

   输出结果为：`aa__cc`.

   由于正则表达式并不直观、方便。所以，引入replaceAll()方法，可以一次性替换所有匹配的项目。如：

   ```javascript
   'aaddf'.replaceAll('d','3');
   ```

   输出结果为：`aa33f`.



2. replaceAll() 语法规范   

```JavaScript
String.prototype.replaceAll(searchValue, replacement);
```

- searchValue： 表示搜索模式， 可以是一个字符串， 也可以是一个全局的正则表达式（ 带有g修饰符）。        

- replacement： 表示替换的文本， 是一个字符串示例：    

```html
<script>        
	const fruits ='苹果+草莓+';        
	const fruitsWithBanana = fruits.replace(/\+/g,'香蕉');
    console.log(fruits); //苹果+草莓+
	console.log(fruitsWithBanana); //苹果香蕉草莓香蕉   
	
    //replaceAll()会将所有匹配的内容 + 替换为香蕉， 返回一个新的字符串，不会改变原字符串
</script>
```



注意事项：

- 如果searchValue是一个不带g修饰符的正则表达式，replaceAll()会报错，但是replace()不会报错。

  ```JavaScript
  let str = "aabbcc";
  
  // /b/不带有g修饰符，会导致replaceAll()报错
  str.replace(/b/, '_') //不报错
  str.relaceAll(/b/, '_') //报错
  ```

  

3. repalceAll() 应用场景

   去除字符串多余的文字。

   ```JavaScript
   const str1 ='广东省，福建省，浙江省，湖南省，河北省，河南省，……';
   
   //使用replaceAll将“省”字全部替换为空白字符
   const str = str1.replaceAll("省", "");
   ```

   

replaceAll()的主要目的是给开发者提供一个简单直接的操作方式，可以一次性直接替换所有匹配的内容。



### 数组的新增方法

### includes()

1. 基本用法

```javascript
// 判断数组中是否含有某个成员

console.log([1, 2, 3].includes('2')); //false

console.log([1, 2, 3].includes(2)); //true


// 第二个参数表示搜索的起始位置，默认值是 0

console.log([1, 2, 3].includes(2, 2)); //false


// 基本遵循严格相等（===）,但是对于 NaN 的判断与 === 不同，includes 认为 NaN === NaN

console.log(NaN === NaN); // false

console.log([1, 2, NaN].includes(NaN)); //true
```



2. 应用

```javascript
// 去重

// [1, 2, 1];

const arr = [];

for (const item of [1, 2, 1]) {
	if (!arr.includes(item)) {
		arr.push(item);
	}
}

console.log(arr); //[1, 
2]
```



### Array.from()

**将其他数据类型转换成数组**



1. 基本用法

```javascript
console.log(Array.from('str')); //["s", "t", "r"]
```

   

2. 哪些可以通过 Array.from() 转换成数组

   

   2.1. 所有可遍历的

```javascript
// 数组、字符串、Set、Map、NodeList、arguments

console.log(Array.from(new Set([1, 2, 1]))); //[1, 2]

//相当于展开，一般更常用此方法
console.log([...new Set([1, 2, 1])]); // [1, 2]
```



​	2.2. 拥有 length 属性的任意对象

```javascript
const obj = {
	length: 1
};
console.log([...obj]); // × 报错 （不可遍历则不能展开）
console.log(Array.from(obj)); //[undefined]


const obj = {
	'0': 'a',
	'1': 'b',
	name: 'Alex',
	length: 3
};

console.log(Array.from(obj)); // ["a", "b", undefined] 键名不是数字的，不会被转换
```

  

3. 第二个参数

```javascript
// 作用类似于数组的 map 方法，用来对每个元素进行处理，将处理后的值放入返回的数组

console.log(
	[1, 2].map(value => {
		return value * 2;
	})
); // [2, 4]

console.log(Array.from('12', value => value * 2)); //[2, 4]

console.log(Array.from('12').map(value => value * 2)); //[2, 4]
```



4. 第三个参数

```javascript
Array.from('12',value => {
	console.log(this); //Window Window (箭头函数)
},document);

Array.from(
	'12',
	
    function () {
		console.log(this); //#document #document
	},
	document
);
```



### find() 和 findIndex()



- find()：找到满足条件的一个**立即返回值**

- findIndex()：找到满足条件的一个，**立即返回其索引**



1. 基本用法

```javascript
console.log(
	[1, 5, 10, 15].find((value, index, arr) => {
		console.log(value, index, arr); 
        //1  0 Array(4) 
        //5 1 Array(4) 
        //10 2 Array(4) 
        //15 3 Array(4)
        
		console.log(this); //Window
		return value > 9;
      }, document)
); // 10

[1, 5, 10, 15].find(function (value, index, arr) {
	console.log(this); //#document

	return value > 9;
}, document); //2

console.log(
	[1, 5, 10, 15].findIndex((value, index, arr) => {
		return value > 9;
	}, document)
);
```

  

2. 应用

```javascript
const students = [
	{
		name: '张三',
		sex: '男',
		age: 16
	},

	{
		name: '李四',
		sex: '女',
		age: 22
	},

	{
		name: '王二麻子',
		sex: '男',
		age: 32
	}
];

console.log(students.find(value => value.sex === '女')); //{name:"李四", sex:"女", age:22}
console.log(students.findIndex(value => value.sex === '女')); //1
```





### 对象的新增方法



### Object.assign()

**用来合并对象**



1. 基本用法

```javascript
// Object.assign(目标对象, 源对象1,源对象2,...): 目标对象

const apple = {
	color: '红色',
	shape: '圆形',
	taste: '甜'
};

const pen = {
	color: '黑色',
	shape: '圆柱形',
	use: '写字'
};

// Object.assign 直接合并到了第一个参数中，返回的就是合并后的对象
console.log(Object.assign(apple, pen)); //{color: "黑色", shape:"圆柱形", taste:"甜", use:"写字"}

console.log(apple); //{color: "黑色", shape:"圆柱形", taste:"甜", use:"写字"}
console.log(Object.assign(apple, pen) === apple); // true

//看起来与以下展开合并结果一致，但展开之后返回的是新对象
console.log({ ...apple, ...pen }); //{color: "黑色", shape:"圆柱形", taste:"甜", use:"写字"}
console.log({ ...apple, ...pen }===apple); //false



// 如果不想合并到第一个，只要传一个空对象在第一个参数就可以

console.log(Object.assign({}, apple, pen));
console.log(apple); //{color: "红色", shape:"圆形", taste:"甜"}

```



2. 注意事项

     2.1. 基本数据类型作为源对象(希望被合并的对象)

```javascript
// 与对象的展开类似，先转换成对象，再合并
//Object.assign(目标对象，源对象1，源对象2，……);

console.log(Object.assign({}, undefined)); // {}

console.log(Object.assign({}, null)); // {}

console.log(Object.assign({}, 1)); // {}

console.log(Object.assign({}, true)); // {}

console.log(Object.assign({}, 'str')); // {0: "s", 1: "t", 2: "r"}
```

  

​	2.2. 同名属性的替换

```javascript
// 后面的直接覆盖前面的同名属性，不管值的数据类型

const apple = {
	color: ['红色', '黄色'],
	shape: '圆形',
	taste: '甜'
};

const pen = {
	color: ['黑色', '银色'],
	shape: '圆柱形',
	use: '写字'
};

console.log(Object.assign({}, apple, pen)); //{color: ["黑色","银色"], shape:"圆柱形", taste:"甜", use:"写字"}
```



3. 应用

```javascript
// 合并默认参数和用户参数

const logUser = userOptions => {
	const DEFAULTS = {
		username: 'ZhangSan',
		age: 0,
		sex: 'male'
	};

	const options = Object.assign({}, DEFAULTS, userOptions);
	
    //传空对象或者空参数相当于传入undefined
	//const options = Object.assign({}, DEFAULTS, undefined);

	console.log(options);
};

logUser(); //{username:"Zhangsan", age: 0, sex: "male"}

logUser({}); //{username:"Zhangsan", age: 0, sex: "male"}

logUser({ username: 'Alex' }); //{username:"Zhangsan", age: 0, sex: "male"}
```



### Object.keys(), Object.values(), Object.entries()

1. 基本用法

```javascript
const person = {
	name: 'Alex',
	age: 18
};

console.log(Object.keys(person)); //["name", "age"]
console.log(Object.values(person)); // ["Alex",18]
console.log(Object.entries(person)); // [Array(2), Array(2)] // [["name", "Alex"],["age",18]]
```



2. 与数组类似方法的区别

```javascript
// 数组的 keys()、values()、entries() 等方法是实例方法，返回的都是 Iterator
// 对象的 Object.keys()、Object.values()、Object.entries() 等方法是构造函数方法，返回的是数组

console.log([1, 2].keys()); //Array Iterator{}

console.log([1, 2].values()); //Array Iterator{}

console.log([1, 2].entries()); //Array Iterator{}
```



3. 使用 for...of 循环遍历对象

```javascript
const person = {
	name: 'Alex',
	age: 18
};

for (const key of Object.keys(person)) {
	console.log(key);
    //name
    //age
}

for (const value of Object.values(person)) {
	console.log(value);
    //Alex
    //18
}

for (const entries of Object.entries(person)) {
	console.log(entries);
    //["name"," Alex"]
    //["age", 18]
}

//利用解构
for (const [key, value] of Object.entries(person)) {
	console.log(key, value);
    //name Alex
    //age 18
}

// Object.keys()/values()/entires() 并不能保证顺序一定是你看到的样子，这一点和 for in 是一样的
```
