## Set和Map

### Set

1. 什么是 Set


```JavaScript
// 集合
// 数组是一系列有序（对应index）的数据集合
// Set 是一系列无序、没有重复值的数据集合
```

   

2. 理解 Set

```JavaScript
//两种创建新数组的形式
console.log([1, 2, 1]);
console.log(new Array(1, 2, 1));

//Set需要使用new来创建
const s = new Set();
//添加成员(add方法一次只能添加一个)
s.add(1);
s.add(2);
console.log(s); // Set(2) {1,2}

// Set 中不能有重复的成员
s.add(1);
console.log(s); // Set(2) {1,2}

// Set 没有下标去标示每一个值，所以 Set 是无序的，也不能像数组那样通过下标去访问 Set 的成员
```



#### Set实例的方法和属性

1. 方法

```JavaScript
// add
const s = new Set();
//add可以连续后缀
s.add(1).add(2).add(2);
console.log(s); // Set(2) {1,2 }


// has 判断Set中是否有某元素
console.log(s.has(1)); //true
console.log(s.has(3)); //false


// delete
s.delete(1);

// 使用 delete 删除不存在的成员，什么都不会发生，也不会报错
s.delete(3);
console.log(s); //Set(1) {2}


// clear 全部删除
s.clear();
console.log(s); // Set(0) {}


// forEach 遍历
s.forEach(function (value, key, set) { //value是Set中每个成员，在Set中key和value相同，set就是set本身
	// Set 中 value = key
	console.log(value, key, set === s); //1 1 true //2 2 true
	console.log(this); //#document
}, document);

s.forEach(function (value, key, set) {
	console.log(this); //Window
});

s.forEach(function (value, key, set) => {
	console.log(this); //Window (回调函数是箭头函数时，this指向不看第二项参数)
}, document);

// 按照成员添加进集合的顺序遍历
```



2. 属性

```JavaScript
// size
console.log(s.size); // 2
console.log(s); //Set(2) {1, 2}
```



#### Set构造函数的参数

```html
<!DOCTYPE html>
<html lang="en">
 <head>
    <meta charset="UTF-8" />
  <title>Set 构造函数的参数</title>
 </head>
 <body>
    <p>1</p>
    <p>2</p>
    <p>3</p>
    <script>
	... 
  </script>
 </body>
</html>
```

1. 数组

```javascript
const s = new Set([1, 2, 1]);
console.log(s);
```



2. 字符串、arguments、NodeList、Set 等

```javascript
console.log(new Set('hi')); //Set(2) {"h", "i"}

function func() {
	console.log(new Set(arguments));
}

func(1, 2, 1); // Set(2) {1, 2}

console.log(new Set(document.querySelectorAll('p'))); //Set(3) {p, p, p}

const s = new Set([1, 2, 1]);
console.log(new Set(s) === s); //false 复制了一个Set，但不是同一个
console.log(s);
```



#### Set的注意事项

1. 判断重复的方式

```javascript
const s = new Set([1, 2, 1]);
console.log(1 === 1); //true
console.log(NaN === NaN); //false

// Set 对重复值的判断基本遵循严格相等（===）
// 但是对于 NaN 的判断与 === 不同，Set 中 NaN 等于 NaN
const s = new Set([NaN, 2, NaN]);
console.log(s); //Set(2) {NaN, 2}

//对于对象成员的判断
const s = new Set();
s.add({})
console.log(s); //Set(1) {{...}}

const s = new Set();
s.add({}).add({});
console.log(s); //Set(2) {{...}, {...}}

console.log({} === {}); //false
```



2. 什么时候使用 Set

   ① 数组或字符串去重时

   ② 不需要通过下标访问，只需要遍历时

   ③ 为了使用 Set 提供的方法和属性时（add delete clear has forEach size 等）



#### Set的应用

```html
<body>
    <p>1</p>
    <p>2</p>
    <p>3</p>
    <script>
   ...
  </script>
 </body>
```

1. 数组去重

```javascript
// [1, 2, 1];

const s = new Set([1, 2, 1]);
console.log(s);

//可以通过forEach把Set转成数组
s.forEach

//也可通过展开运算符将Set转成数组
console.log(...s); // 1 2
console.log([...s]); //[1,2]
console.log([...new Set([1, 2, 1])]); // [1, 2] 完成去重
```



2. 字符串去重

```javascript
// 'abbacbd';

const s = new Set('abbacbd');
//使用数组转字符串
console.log([...s].join('')); //abcd

console.log([...new Set('abbacbd')].join(''));
```



3. 存放 DOM 元素

```javascript
console.log(document.querySelectorAll('p')); //NodeList(3) [p,p,p]

const s = new Set(document.querySelectorAll('p'));
console.log(s); //Set(3) {p,p,p}

//通过set的forEach遍历元素并做修改
s.forEach(function (elem) {
	console.log(elem); //<p>1</p> <p>2</p> <p>3</p>
	elem.style.color = 'red';
	elem.style.backgroundColor = 'yellow';
});
```



### Map

1. 认识 Map

```javascript
// 映射
// Map 和对象都是键值对的集合
// 键->值，key->value

const person = {
	name: 'alex',
	age: 18
};

const m = new Map();
m.set('name', 'alex');
m.set('age', 18);
console.log(m); // Map(2) {"name" => "alex", "age" => 18}
```



2. Map 和对象的区别

```javascript
// 对象一般用字符串当作键

const obj = {
	name: 'alex', //虽然name这样的键名没有打引号，但这里是合法省略，其实就是字符串
	true: 'true',
	[{}]: 'object'
};

console.log(obj); {name:"alex", true:"true", [object Object]:"object"}
console.log({}.toString()); //object Object

// 基本数据类型：数字、字符串、布尔值、undefined、null
// 引用数据类型：对象（[]、{}、函数、Set、Map 等）
// 以上都可以作为 Map 的键

const m = new Map();
m.set('name', 'alex');
m.set(true, 'true'); //这里的键是布尔值 true 而不是字符串
m.set({}, 'object'); //键是 {...}
m.set(new Set([1, 2]), 'set');
m.set(undefined, 'undefined');

console.log(m);
```



#### Map实例的属性和方法

1. 方法

```javascript
// set （可看作是Set中add方法）
const m = new Map();

// Map使用 set 添加的新成员，键如果已经存在，后添加的键值对覆盖已有的

m.set('age', 18).set(true, 'true').set('age', 20);
console.log(m); //Map(2) {"age" => 20, true =>"true"}

// Set中没有get方法，但Map中可以
// get
console.log(m); // Map(2) ...
console.log(m.get('age')); // 20

// get 获取不存在的成员，返回 undefined
console.log(m.get('true')); //undefined
console.log(m.get(true)); // true


// has
console.log(m.has('age')); //true
console.log(m.has('true')); //false

// delete
m.delete('age');
m.delete('name');


// 使用 delete 删除不存在的成员，什么都不会发生，也不会报错
console.log(m);

// clear 清空
m.clear();
console.log(m); // Map(0)

// forEach
m.forEach(function (value, key, map) {
	console.log(value, key, map === m); //Map(2) {...} true
	console.log(this); //#document
}, document);
```



2. 属性

```javascript
// size
// 对象没有类似的属性

console.log(m.size);
```



#### Map构造函数的参数

1. 数组

```javascript
console.log(new Map(['name', 'alex', 'age', 18])); //报错 没有键值对

// 只能传二维数组，而且必须体现出键和值
console.log(
	new Map([
		['name', 'alex'],
		['age', 18]
	])
);
```



2. Set、Map 等

```javascript
// Set
// Set 中也必须体现出键和值

const s = new Set([
	['name', 'alex'],
	['age', 18]
]);

console.log(new Map(s));
console.log(s);

// Map
// 复制了一个新的 Map

const m1 = new Map([
	['name', 'alex'],
	['age', 18]
]);

console.log(m1);
const m2 = new Map(m1); //复制但不相同
console.log(m2, m2 === m1); //false
```



#### Map的注意事项

1. 判断键名是否相同的方式

```javascript
// 基本遵循严格相等（===）
// 例外就是 NaN，Map 中 NaN 也是等于 NaN

console.log(NaN === NaN); //false

const m = new Map();
m.set(NaN, 1).set(NaN, 2);

console.log(m); //Map(1) {NaN => 2}
```



2. 什么时候使用 Map

```javascript
// 如果只是需要 key -> value 的结构，或者需要字符串以外的值做键，使用 Map 更合适
// forEach for in
// size

// 只有模拟现实世界的实体时，才使用对象
const person = {};
```



#### Map的应用

```html
<!DOCTYPE html>
<html lang="en">
 <head>
    <meta charset="UTF-8" />
  <title>Map 的应用</title>
 </head>
 <body>
    <p>1</p>
    <p>2</p>
    <p>3</p>
    <script>
   	const [p1, p2, p3] = document.querySelectorAll('p');
	const m = new Map();
    m.set(p1, 'red');
    m.set(p2, 'green');
    m.set(p3, 'blue');
    console.log(m); //Map(3) { p => "red", p => "green", p => "blue"}
    
    //或者
    const m = new Map([
        [p1, 'red'],
        [p2, 'green'],
        [p3, 'blue']
    ]);
        
    //将key（elem）的颜色变成value（color）值
    m.forEach((color, elem) => {
   		elem.style.color = color;
	});

	//对象作为值
   const m = new Map([
	[
		p1,
		{
			color: 'red',
			backgroundColor: 'yellow',
			fontSize: '40px'
		}
	],
	[
		p2,
		{
			color: 'green',
			backgroundColor: 'pink',
			fontSize: '40px'
		}
	],
	[
		p3,
		{
            color: 'blue',
			backgroundColor: 'orange',
			fontSize: '40px'
		}
	]
	]);

   m.forEach((propObj, elem) => {
       //遍历对象属性
		for (const p in propObj) {
			elem.style[p] = propObj[p];
		}
	});
   
  </script>
 </body>
</html>
```