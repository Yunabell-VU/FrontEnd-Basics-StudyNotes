## Class 类



### Class基本认识



1. 认识 Class

- 具体的人：实例、对象
- 类可以看做是对象的模板，用一个类可以创建出许多不同的对象



2. Class 的基本用法

```JavaScript
// 类名一般大写

class Person {} √

class Person() {} ×

class Person {}; ×

function func() {}
```



```JavaScript
class Person {

   // 实例化时执行构造方法，所以必须有构造方法，但可以不写出来

	constructor(name, age) {

		console.log('实例化时执行构造方法');
        //this 代表实例对象，上面定义的是实例属性/方法
        
        this.name = name;
        this.age = age;
        
        // 一般在构造方法中定义属性，方法不在构造方法中定义
        //this.speak = () => {};
    }
	
    speak:function(){}

   
// 各实例共享的方法

   speak() {
       console.log('speak');
   }

}

// Person(); （报错，需要通过new来实例化）

const zs = new Person('ZS', 18);

const ls = new Person('LS', 28);

console.log(zs.name);

console.log(zs.age);

console.log(zs.speak);

zs.speak();

console.log(ls.name);

console.log(ls.age);

console.log(ls.speak);

console.log(zs.speak === ls.speak); //在constructor中创立方法，这里就是false，构造方法外声明的方法，这里则是true
```



3. Class 与构造函数

```JavaScript
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
        // this.speak = () => {};
	}

	speak() {
    	console.log('speak');
	}
    // run(){}
}

//ES6里 Class的本质还是function，在原型上添加方法和在Class内添加是一样的，但是实际应用中，没有人会这么写
Person.prototype.run = function () {};
console.log(typeof Person);
console.log(Person.prototype.speak);

//构造函数(有Class之后，构造函数基本就用不上了)
function Person(name, age) {
    this.name = name;
    this.age = age;
   //this.speak = () => {};
}

Person.prototype.speak = function () {};
```



#### Class的两种定义形式

1. 声明形式

```JavaScript
class Person {
    constructor() {}
    speak() {}
}
```



2. 表达式形式

```JavaScript
//构造函数的表达式形式
function Person(){}
const Person = function () {};

//class的表达式形式
const Person = class {
    constructor() {
        console.log('constructor');
    }
    speak() {}
};

new Person(); //constructor

//立即执行的匿名函数
(function () {
    console.log('func');
})();


// 立即执行的匿名类
new (class {
    constructor() {
        console.log('constructor');
    }
})(); //constructor

```



### Class 属性和方法

#### 实例属性、静态方法和静态属性

1. 实例属性

```JavaScript
// 方法就是值为函数的特殊属性

class Person {
    age = 0;
	sex = 'male';
	getSex = function () {
        return this.sex;
    };

	constructor(name, sex) {
        this.name = name;
        this.age = 18;
        this.sex = sex;
    }
	speak() {
        this.age = 18;
    }
}

const p = new Person('Alex');

console.log(p.name);

console.log(p.age);
```



2. 静态方法

```javascript
// 类的方法

class Person {
    constructor(name) {
        this.name = name;
	}

	speak() {
    	console.log('speak');
    	console.log(this); //指向实例化对象
	}

	static speak() {
    	console.log('人类可以说话');
    	console.log(this); // this 指向类
	}
}

//static 函数相当于
Person.speak = function() {
    console.log('人类可以说话');
    console.log(this); // this 指向类
}

Person.speak = function () {
    console.log('人类可以说话');
    console.log(this);
};

const p = new Person('Alex');

//实例方法
p.speak();  //speak

//类的方法
Person.speak();  //人类可以说话

```



3. 静态属性

```JavaScript
// 类的属性

class Person {
    constructor(name) {
        this.name = name;
    }

// 不要这么写，目前只是提案，有兼容性问题
// static version = '1.0';

	static getVersion() {
    	return '1.0';
	}
}

// Person.version = '1.0';

const p = new Person('Alex');
console.log(p.name);

//console.log(Person.version);
console.log(Person.getVersion());
```



**小练习**

以下代码中，有几处会报错？

```JavaScript
class Person {
    run(){
        console.log('run')
    }
    static eat() {
        console.log('eat')
    }
    static age = 18
}
console.log(Person.age)
const p = new Person()
console.log(p.age)
p.eat()
Person.run()

//本题主要考查class的实例属性和静态属性。

//实例属性只能通过实例对象访问，静态属性只能通过类本身访问。

//本题代码中run是实例方法，而Person是类，类不可以访问实例方法，会出现报错，所以Person.run()是一处错误。

//eat方法是静态方法，而p是实例对象，实例对象无法访问静态方法，会出现报错，所以p.eat()是一处错误。

//age是静态属性，可以在实例化对象之前通过类本身访问，所以console.log(Person.age)这代码可以正常输出结果。

//
 
    实例对象不可以访问静态属性，但是p.age相当于访问对象上一个不存在的属性，默认返回undefined，并不会报错，不能算作一个错误。
```



#### 私有属性和方法

1. 为什么需要私有属性和方法

```javascript
// 一般情况下，类的属性和方法都是公开的

// 公有的属性和方法可以被外界修改，造成意想不到的错误

class Person {
    constructor(name) {
        this.name = name;
}

	speak() {
    	console.log('speak');
	}

	getName() {
    	return this.name;
	}
}

const p = new Person('Alex');
console.log(p.name); //Alex
p.speak();


// ....

p.name = 'zs'; //属性可以被任意修改
console.log(p.name); //zs
```



2. 模拟私有属性和方法

     2.1. **_** 开头表示私有 （不具备约束力，是约定俗成的风格）

```JavaScript
class Person {
    constructor(name) {
        this._name = name; //私有
    }
	speak() {
        console.log('speak');
    }
	
    getName() {
        return this._name;
    }
}

const p = new Person('Alex');

console.log(p.name); //undefined
console.log(p.getName());// Alex

p.name = 'zd';
```



2.2. 将私有属性和方法移出类

```JavaScript
//杜绝外部访问的可能性 一般不太会这么用 除非一定要私有
(function () {
    let name = '';
    class Person {
        constructor(username) {
            // this.name = name;
            name = username;
        }
        speak() {
            console.log('speak');
        }
        getName() {
            return name;
        }
    }
    window.Person = Person; //class添加到全局中，就可以访问到class
})();

(function () {
    const p = new Person('Alex');
    console.log(p.name); //undefined
    console.log(p.getName()); //Alex
})();
```



### Class的继承

#### extends

1. 子类继承父类

```JavaScript
class Person {
    constructor(name, sex) {
        this.name = name;
        this.sex = sex;
        this.say = function () {
            console.log('say');
        };
    }
    speak() {
        console.log('speak');
    }
    
    static speak() {
        console.log('static speak');
    }
}

Person.version = '1.0';

class Programmer extends Person {
    constructor(name, sex) {
        super(name, sex); //如果super里没有参数，这name和sex都不会被记录，默认undefined
    }
}

const zs = new Programmer('zs', '男');
console.log(zs.name); // zs
console.log(zs.sex); // 男

zs.say(); //say
zs.speak();//speak

Programmer.speak(); //static speak

console.log(Programmer.version); //1.0
```



2. 改写继承的属性或方法

```JavaScript
class Programmer extends Person {
    constructor(name, sex, feature) {
        // this.feature = feature; ×
        // this 操作不能放在 super 前面
        super(name, sex);
        this.feature = feature;
    }
    
    hi() {
        console.log('hi');
    }
    
    // 同名覆盖 以子类为准
    
    speak() {
        console.log('Programmer speak');
    }
    
    static speak() {
        console.log('Programmer static speak');
    }
}

Programmer.version = '2.0';

const zs = new Programmer('zs', '男', '秃头');
console.log(zs.name); //zs
console.log(zs.sex); //男
console.log(zs.feature); //秃头

zs.say(); //say
zs.speak(); //Programmer static speak
zs.hi(); //hi
Programmer.speak(); //Programmer static speak

console.log(Programmer.version); //2.0
```



#### super

1. 作为函数调用

```JavaScript
// 代表父类的构造方法，只能用在子类的构造方法中，用在其他地方就会报错

// super 虽然代表了父类的构造方法，但是内部的 this 指向子类的实例

class Person {
	constructor(name) {
		this.name = name;
        console.log(this);
    }
}

class Programmer extends Person {
    constructor(name, sex) {
        super(name, sex);
    }
    
    hi() {
        //super(); // ×
    }
}

new Person();

new Programmer();

```

   

2. 作为对象使用

     2.1.在构造方法中使用或一般方法中使用

   ```javascript
   // super 代表父类的原型对象 Person.prototype
   // 所以定义在父类实例上的方法或属性，是无法通过 super 调用的
   
   // 通过 super 调用父类的方法时，方法内部的 this 指向当前的子类实例
   
   class Person {
       constructor(name) {
           this.name = name;
           console.log(this);
       }
       
       speak() {
           console.log('speak');
           console.log(this);
       }
       
       static speak() {
           console.log('Person speak');
           console.log(this);
       }
   }
   
   class Programmer extends Person {
       constructor(name, sex) {
           super(name, sex);
           console.log(super.name);
           super.speak();
       }
       hi() {
           //super(); // ×
       }
       speak() {
           super.speak();
           console.log('Programmer speak');
       }
       
        //2.2.在静态方法中使用
       
       // 指向父类，而不是父类的原型对象
   	// 通过 super 调用父类的方法时，方法内部的 this 指向当前的子类，而不是子类的实例
   
   	static speak() {
       	super.speak();//在这里调用的话，constructor里面就不要调用super.speak了
       	console.log('Programmer speak');
   	}
   }
   
   new Person();
   new Programmer();
   Programmer.speak();
   ```

   

3. 注意事项

```javascript
// 使用 super 的时候，必须显式指定是作为函数还是作为对象使用，否则会报错

class Person {
    constructor(name) {
        this.name = name;
    }
    speak() {
        console.log('speak');
    }
}

class Programmer extends Person {
    constructor(name, sex) {
        super(name, sex);
        console.log(super); //报错
        console.log(super()); //
        console.log(super.speak); //
    }
}
```

- super作为函数调用时，代表父类的构造函数
- super中的this指向的是子类的this
- 非静态方法中，指向父类的原型对象
- 在静态方法中，指向父类



**小练习**

```JavaScript
//super关键字作为对象使用时，在子类实例方法中，指向父类的原型对象；在子类静态方法中，指向父类。

class Parent {
    color = "red"
	constructor(username) {
        this.username = username
    }
	static pSay() {
        console.log(this.username)
    }
	pShow() {
        console.log(this.color)
    }
}
class Child extends Parent {
    constructor(username) {
        super(username)
    }
    static cSay() {
        super.pSay()
    }
    cShow() {
        super.pShow()
        console.log(super.color)
    }
}
const c1 = new Child('c1')
Child.cSay() //undefined
c1.cShow() //red undefined


//本题代码中Child类继承了Parent类，具体的解析参考如下：

//1、子类静态方法cSay中，super作为对象时，指向父类Parent。
//调用父类的方法pSay时，该方法内部的this指向子类Child, 无法访问到实例属性username，结果为undefined。

//2、子类实例方法cShow中super作为对象使用，指向父类原型,即：Parent.prototype，
//调用父类的方法pShow时，该方法内部的this指向子类实例对象，可以访问实例对象上的color属性，结果为red。

//3、super.color这句代码相当于访问父类原型上的color属性，不可以访问实例属性color，由于父类原型上并没有color属性，所以结果为undefined。

//代码最终的输出结果为undefined red undefined

```