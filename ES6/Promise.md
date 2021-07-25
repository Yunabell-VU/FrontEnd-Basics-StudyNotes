## Promise



### Promise 基本认识

```html
<!DOCTYPE html>
<html lang="en">
 <head>
    <meta charset="UTF-8" />
  <title>Promise 是什么</title>
    <style>
   \* {
 		padding: 0;
		margin: 0;
   	}

	\#box {
		width: 300px;
		height: 300px;
		background-color: red;
		transition: all 0.5s;
		}
  </style>
     
 </head>
 <body>
    <div id="box"></div>
     
    <script>
	...
  </script>
     
 </body>
</html>
```

 

1. 认识 Promise

```javascript
// Promise 是异步操作的一种解决方案

// 回调函数

//回调函数举例
document.addEventListener(
	'click',() => {
		console.log('这里是异步的');
	},false
);

console.log('这里是同步的');

//console会直接输出 “这里是同步的”
//只有点击文档时，才会输出“这里是异步的”
```



2. 什么时候使用 Promise

```javascript
// Promise 一般用来解决层层嵌套的回调函数（回调地狱 callback hell）的问题

//回调地域举例，html和css参考上面

// 运动

const move = (el, { x = 0, y = 0 } = {}, end = () => {}) => {
	el.style.transform = `translate3d(${x}px, ${y}px, 0)`; //改变x和y轴
    
	el.addEventListener(
 		'transitionend',() => {
    		console.log('end');
    		end();
		},false
	);
};

const boxEl = document.getElementById('box');

document.addEventListener(
	'click',() => {
 		//一步一步的运动，如果运动过多，就会层层嵌套
        move(boxEl, { x: 150 }, () => {
            move(boxEl, { x: 150, y: 150 }, () => {
                move(boxEl, { y: 150 }, () => {
                    move(boxEl, { x: 0, y: 0 });
                });
            });
        });
    },false
);
```



#### 基本用法

1. 实例化构造函数生成实例对象

```JavaScript
// Promise 解决的不是回调函数，而是回调地狱
const p = new Promise(() => {}); //生成Promise需要有回调函数
```



2. Promise 的状态

```JavaScript
const p = new Promise((resolve, reject) => {
	// Promise 有 3 种状态，一开始是 pending（未完成），执行 resolve，变成 fulfilled(resolved)，已成功
	// 执行 reject，变成 rejected，已失败
	// Promise 的状态一旦变化，就不会再改变了
	
    // 状态变化 pending->fulfilled
	resolve();
	
    // 如果没执行 resolve，状态 rejected，如果已经执行resolve，则状态还是fulfilled
	reject();  
});

//注意：只能从pending 变为 fulfilled或rejected，没有从fulfilled变为rejected或rejected变为fulfilled的情况
```



3. then 方法

```JavaScript
p.then(
    () => { //状态变为fulfilled时执行
	console.log('success');
	},	
    () => {//状态变为rejected时执行
		console.log('error');
	}
);
```

   

4. resolve 和 reject 函数的参数

```JavaScript
const p = new Promise((resolve, reject) => {
	//resolve('succ');
	resolve({ username: 'alex' });

	//reject('reason');
	reject(new Error('reason'));
});

p.then(
    data => {
        console.log('success', data); //success {username: 'alex'}
    },
    err => {
        console.log('error', err); //error Error
    }
);

console.log(p);
```



### Promise实例方法

#### then()

```html
<head>
    <meta charset="UTF-8" />
  <title>then()</title>
    <style>
   * {
       padding: 0;
       margin: 0;
   }

   #box {
       width: 300px;
       height: 300px;
       background-color: red;
       transition: all 0.5s;
   }
  </style>
 </head>

 <body>
    <div id="box"></div>
	
     <script>
     ...
     </script>
</body>
```

1. 什么时候执行

- pending->fulfilled 时，执行 then 的第一个回调函数

- pending->rejected 时，执行 then 的第二个回调函数



2. 执行后的返回值

```javascript
// then 方法执行后返回一个新的 Promise 对象

const p = new Promise((resolve, reject) => { //注意这里的（resolve,reject)是回调函数的参数，不是Promise()的第一个参数
    resolve(); //决定 p 的状态
    reject(); //决定 p 的状态
});

const p2 = p.then(
    () => {},
    () => {}
)
console.log(p, p2);
//Promise {<fulfilled>: undefined}
//Promise {<pending>}

console.log(p === p2); // false

//p2 是单独的Promise，所以可以有自己的 then
const p2 = p.then(
    () => {},
    () => {}
).then();

//.then()会返回一个Promise方法，所以可以继续then

const p2 = p.then(
    () => {},
    () => {}
).then().then();

```

  

3. then 方法返回的 Promise 对象的状态改变

```javascript
const p = new Promise((resolve, reject) => {
    reject();
});

p.then(
    () => {
        console.log('success');
    },
    () => {
        console.log('err');
    }
).then( //p.then()的状态决定执行以下哪个函数
    ()=> {
        console.log('success2');
    },
    ()=> {
        console.log('err2');
    }
)
//这里会执行 err success2


const p = new Promise((resolve, reject) => {
    reject();
});

p.then(
    () => {
        console.log('success');
    },
    () => {
        console.log('err');
        // 在 then 的回调函数中，return 后面的东西，会用 Promise 包装一下，
        // 默认返回的永远都是成功状态的 Promise 对象
        return undefined;
        
        // 等价于
        return new Promise(resolve => {
            resolve(undefined);
        });
        
        //如果return 123，这下面会执行 err success2 123
    }
).then(
   	data => { //接受return值
        console.log('success2', data);
    },
    ()=> {
        console.log('err2');
    }
)
//这里会执行 err success2 undefined


//如果要then().then()接受失败参数，则需要自己写一个返回Promise
const p = new Promise((resolve, reject) => {
    reject();
});

p.then(
    () => {
        console.log('success');
    },
    () => {
        console.log('err'); 

        return new Promise((resolve, reject) => {
            reject('reason');
        });
    }).then(
    	data => {
        	console.log('success2', data);
        	return undefined;
    	},
    	err => {
        	console.log('err2', err);
    	}
	).then(
    	data => {
        	console.log('success3', data);
    	},
    	err => {
        	console.log('err3', err);
    	}
	);
//执行结果 err err2 reason success3 undefined
```



4. 使用 Promise 解决回调地狱

```javascript
// 运动

const move = (el, { x = 0, y = 0 } = {}, end = () => {}) => {
    el.style.transform = `translate3d(${x}px, ${y}px, 0)`;
    el.addEventListener(
        'transitionend',
        () => {
            end();
        },false
    );
};

const boxEl = document.getElementById('box');

//原始方法
document.addEventListener(
    'click',() => {
        move(boxEl, { x: 150 }, () => {
            move(boxEl, { x: 150, y: 150 }, () => {
                move(boxEl, { y: 150 }, () => {
                    move(boxEl, { x: 0, y: 0 });
                });
            });
        });
    },false
);

//利用Promise改造
const movePromise = (el, point) => {//point表示xy坐标
    return new Promise(resolve => {
        move(el, point, () => {
            resolve(); //每运动一次，会resolve执行then方法
        });
    });
};

document.addEventListener(
    'click',() => {
        movePromise(boxEl, { x: 150 })
       	.then(
            () => {
                return movePromise(boxEl, { x: 0, y: 0 });
            })
        .then(
            () => {
                return movePromise(boxEl, { x: 150, y: 150 });
            })
        .then(() => {
            return movePromise(boxEl, { y: 150 });
        });
    },false
);

```



#### catch()

1. 有什么用

```javascript
//标准的then方法有两个回调
then(
    data => {},
    err => {}
);

//但一般我们只需要用到成功的回调，失败的回调不写
then(data => {});

// 此时就会使用 catch 专门用来处理 rejected 状态
// catch 本质上是 then 的特例

then(null, err => {});
```



2. 基本用法

```javascript
new Promise((resolve, reject) => {
    //resolve(123);
    reject('reason');
})

.then(
    data => {
    	console.log(data);
	}
)
.catch(err => {
    console.log(err);
    return undefined;
    throw new Error('reason');
}) //执行结果为 reason

//相当于
.then(null, err => {
    console.log(err);
});


new Promise((resolve, reject) => {
    //resolve(123);
    reject('reason');
})
.then(
    data => {
    	console.log(data);
	}
)
.catch(err => {
    console.log(err);
    //return undefined; （默认返回undefined，之后的then会执行成功）
    throw new Error('reason');
})
.then(data => {
    console.log(data);
})
.catch(err => {
    console.log(err);
});


// catch() 可以捕获它前面的错误

// 一般总是建议，Promise 对象后面要跟 catch 方法，这样可以处理 Promise 内部发生的错误
```



#### finally()

1. 什么时候执行 （实际开放中并不常用）

```javascript
// 当 Promise 状态发生变化时，不论如何变化都会执行，不变化不执行

new Promise((resolve, reject) => {
    //resolve(123);
    reject('reason');
})

.finally(data => {
    console.log(data);
})
.catch(err => {});
//执行结果 undefined （这里只执行了finally，但data为空，因为没有捕获到任何传参）

//finally的使用是在Promise执行后进行收尾操作，比如关闭文档，关闭数据库
```

   

2. 本质

```javascript
// finally() 本质上是 then() 的特例

new Promise((resolve, reject) => {
    resolve(123);
    reject('reason');
})

.finally(data => {
    console.log(data);
})

.catch(err => {});

// 等同于

new Promise((resolve, reject) => {
    resolve(123);
    reject('reason');
})

.then(
    result => {
        return result;
    },
    err => {
        return new Promise((resolve, reject) => {
            reject(err);
        });
    }
)

.then(data => {
    console.log(data);
})

.catch(err => {
    console.log(err);
});
```



### Promise的构造函数方法



#### Promise.resolve() 和 Promise.reject()

1. Promise.resolve()

```JavaScript
// 是成功状态 Promise 的一种简写形式

new Promise(resolve => resolve('foo'));

// 简写

Promise.resolve('foo');
```



```JavaScript
// 参数

// 一般参数

Promise.resolve('foo').then(data => {
	console.log(data);
});

```

  

```JavaScript
// Promise当做参数

// 当 Promise.resolve() 接收的是 Promise 对象时，直接返回这个 Promise 对象，什么都不做

const p1 = new Promise(resolve => {
	setTimeout(resolve, 1000, '我执行了');
	
    //相当于
	setTimeout(() => {
		resolve('我执行了');
	}, 1000);

});

Promise.resolve(p1).then(data => {
	console.log(data);
});
//执行结果为 一秒后出现 我执行了

// 等价于 什么都没做 直接返回p1

p1.then(data => {
	console.log(data);
});

console.log(Promise.resolve(p1) === p1); //true


// 当 resolve 函数接收的是 Promise 对象时，后面的 then 会根据传递的 Promise 对象的状态变化决定执行哪一个回调

new Promise(resolve => resolve(p1)).then(data => { //then属于p1 而不管new Promise的状态 
	console.log(data);
});//执行结果为 1秒后出现 我执行了

```

   

```JavaScript
// 具有 then 方法的对象

const thenable = {
	then() {
		console.log('then');
	}
};

// 含有then的方法会被当成是一个Promise对象来对待

//then方法中的then如果没有具体状态操作，Promise.resovle(then方法)会返回一个Promise对象， 状态是pending

//thenable传进之后会直接调用thenable里的then方法， 但这里的.then()不会被执行
Promise.resolve(thenable).then( 
	data => console.log(data),
	err => console.log(err)
);// 执行结果为 then

console.log(Promise.resolve(thenable)); //状态是pending

//如果有具体操作 .then会被执行
const thenable = {
	then(resolve, reject) {
		console.log('then');
		resolve('data');
		//reject('reason');
	}
};

Promise.resolve(thenable).then(
	data => console.log(data),
	err => console.log(err)
);//执行结果为 then data


//有关以上内容的理解， 相当于一个function的参数是object，在调用function时，传进去实际参数
function func(obj) {
	obj.then(1, 2);
}

func({
	then(resolve, reject) {
		console.log(resolve, reject);
	}
});

```



2. Promise.reject()

```JavaScript
// 失败状态 Promise 的一种简写形式

new Promise((resolve, reject) => {
	reject('reason');
});

// 等价于

Promise.reject('reason');

```

   

```JavaScript
// 参数

// 不管什么参数，都会原封不动地向后传递，作为后续方法的参数

const p1 = new Promise(resolve => {
	setTimeout(resolve, 1000, '我执行了');
});

Promise.reject(p1).catch(err => console.log(err)); //(立刻执行) Promise {<pending>}

//Example 成功状态用Promise.resovle()来return意义不大
new Promise((resolve, rejcet) => {
	resolve(123);
})
.then(data => {
	return data;
    //相当于
	//return Promise.resolve(data); (成功状态的Promise)
})
.then(data => {
	console.log(data);
}) //123

//Example 失败状态变的更容易传递 不需要再new一个Promise
new Promise((resolve, rejcet) => {
	resolve(123);
})
.then(data => {
	return Promise.reject('reason');
})
.then(data => {
	console.log(data);
})
.catch(err => console.log(err)); // reason
```



#### Promise.all()

1. 有什么用

```JavaScript
// Promise.all() 关注多个 Promise 对象的状态变化

// 传入多个 Promise 实例，包装成一个新的 Promise 实例返回
```

 

2. 基本用法

```JavaScript
const delay = ms => {
	return new Promise(resolve => {
		setTimeout(resolve, ms); //ms毫秒之后，状态变为成功
	});
};

const p1 = delay(1000).then(() => {
	console.log('p1 完成了');
	return 'p1';
    //return Promise.reject('reason');
});

const p2 = delay(2000).then(() => {
	console.log('p2 完成了');
	return 'p2';
	//return Promise.reject('reason');
});
//执行结果 p1 完成了 p2 完成了
```

   

```JavaScript
// Promise.all() 的状态变化与所有传入的 Promise 实例对象状态有关

// 所有状态都变成 resolved，最终的状态才会变成 resolved

// 只要有一个变成 rejected，最终的状态就变成 rejected


const p = Promise.all([p1, p2]); //参数如果是空数组，p的状态为成功

p.then( //返回的是数组
	data => {
		console.log(data); 
	},
	err => {
		console.log(err);
	}
);

//执行结果 
//p1 完成了
//p2 完成了
//["p1", "p2"]

//在一个参数失败后，Promise.all的then会立刻执行失败回调
const p1 = delay(1000).then(() => {
	console.log('p1 完成了');
    return Promise.reject('reason');
});

const p2 = delay(2000).then(() => {
	console.log('p2 完成了');
	return 'p2';

});
const p = Promise.all([p1, p2]);

p.then(
	data => {
		console.log(data); 
	},
	err => {
		console.log(err);
	}
);
//执行结果 
//p1 完成了
//reason
//p2 完成了

```



#### Promise.race() 和Promise.allSettled()

```JavaScript
const delay = ms => {
	
    return new Promise(resolve => {
		setTimeout(resolve, ms);
	});
};

const p1 = delay(1000).then(() => {
	console.log('p1 完成了');
	return 'p1';
	//return Promise.reject('reason');
});

const p2 = delay(2000).then(() => {
	console.log('p2 完成了');
	//return 'p2';
	return Promise.reject('reason');
});

```



1. Promise.race()

```JavaScript
// Promise.race() 的状态取决于第一个完成的 Promise 实例对象，如果第一个完成的成功了，那最终的就成功；如果第一个完成的失败了，那最终的就失败

const racePromise = Promise.race([p1, p2]);

racePromise.then(
	data => {
		console.log(data);
	},
	err => {
        console.log(err);
    }
);
//执行结果
//p1 完成了
//p1
//p2 完成了
```



2. Promise.allSettled()

```JavaScript
// Promise.allSettled() 的状态与传入的Promise 状态无关

// 永远都是成功的 永远不会执行第二个回调

// 它只会忠实的记录下各个 Promise 的表现 （status）

const allSettledPromise = Promise.allSettled([p1, p2]);

allSettledPromise.then(data => {
    console.log('succ', data);
});

```



#### Promise.any()

1. 语法

   ```JavaScript
   Promise.any(iterable);
   ```

2. 参数
   - iterable：表示一个可迭代的对象，例如：数组

3. 常见返回值

   传入的参数是一组Promise实例。只有当所有Promise实例都编程rejected状态时，返回的Promise才会成为rejected状态，参数中只要有一个Promise是成功状态，则返回的Promise状态是成功。

   

   示例：

   ```JavaScript
   // 失败
   const p1 = new Promise((resolve, reject) => {
       reject()
   });
   // 失败
   const p2 = new Promise((resolve, reject) => {
       reject()
   });
   // 成功
   const p3 = new Promise(resolve => {
       resolve()
   });
   const res = Promise.any([p1, p3, p2])
   console.log(res) // 返回成功状态的Promise
   
   //传入的一组Promise实例参数中，虽然p1、p2这两个是失败状态，但其中的p3是成功状态，所以Promise.any()最终返回结果是成功状态的Promise
   ```

   示例2：

   ```JavaScript
   // 失败
   const p1 = new Promise((resolve, reject) => {
       reject()
   });
   // 失败
   const p2 = new Promise((resolve, reject) => {
       reject()
   });
   // 失败
   const p3 = new Promise((resolve, reject) => {
       reject()
   });
   const res = Promise.any([p1, p3, p2])
   console.log(res) // 返回失败状态的Promise
   
   //由于参数中的p1、p2、p3这三个Promise实例都是失败状态，所以Promise.any()返回一个失败状态的Promise实例
   ```

   

4. 注意事项

   Promise.any() 不会因为某个Promise实例变为失败状态而结束，这个方法用于返回第一个成功的Promise。只要有一个Promise成功此方法就会终止，它不会等待其他的Promise全部完成

```JavaScript
const p1 = new Promise((resolve, reject) => {
    reject("失败");
});

const p2 = new Promise((resolve, reject) => {
    setTimeout(resolve, 500, "最后完成");
});

const p3 = new Promise((resolve, reject) => {
    setTimeout(resolve, 100, "第一个完成");
});

const res = Promise.any([p1, p2, p3])
res.then((value) => {
    console.log(value);
})
//返回结果
//第一个完成

//上面这段代码中，p1是失败状态，Promise.any()不会在这里结束，当p3成功后，Promise.any终止，并不会执行p2的最后完成
```



5. 与Promise.any() 和 Promise.race()的区别

   - Promise.any() 会返回一组完成值，而Promise.any()只能得到最多一个成功值。当我们只需要一个promise成功，而不关心是哪一个成功时，此方法快捷有效 

   - Promise.race() 总是返回第一个结果，而Promise.any() 返回第一个成功的结果。any会忽略掉所有失败的promise，直到遇到第一个成功

     

6. 实际应用场景

   实际开发中，可能会有这样的需求： 一次性加载多张图片，哪一张先加载出来就显示哪一张。那么此时就可以使用Promise.any()方法实现效果

   

### Promise的注意事项和应用

#### 注意事项

1. resolve 或 reject 函数执行后的代码

```javascript
// 推荐在调用 resolve 或 reject 函数的时候加上 return，不再执行它们后面的代码

new Promise((resolve, reject) => {
	// return resolve(123);
	return reject('reason');
	console.log('hi'); //没有return 就会被执行，这里有return，不会被执行
});

```

 

2. Promise.all/race/allSettled 的参数问题

```javascript
// 参数如果不是 Promise 数组，会将不是 Promise 的数组元素转变成 Promise 对象

Promise.all([1, 2, 3]).then(datas => {
	console.log(datas);
});

// 等价于

Promise.all([
	Promise.resolve(1),
	Promise.resolve(2),
	Promise.resolve(3)
]).then(datas => {
	console.log(datas);
}); //执行结果 [1, 2, 3]


// 不只是数组，任何可遍历的都可以作为参数
// 数组、字符串、Set、Map、NodeList、arguments

Promise.all(new Set([1, 2, 3])).then(datas => {
	console.log(datas);
});
//执行结果 [1, 2, 3]
```

 

3. Promise.all/race/allSettled 的错误处理

```javascript
const delay = ms => {
	return new Promise(resolve => {
		setTimeout(resolve, ms);
	});
};

//单独处理rejected
const p1 = delay(1000).then(() => {
	console.log('p1 完成了');
	return Promise.reject('reason');
});
.catch(err => {
	console.log('p1', err);
});

const p2 = delay(2000).then(() => {
	console.log('p2 完成了');
	return 'p2';
});
.catch(err => {
	console.log('p2', err);
});


const allPromise = Promise.all([p1, p2]);

allPromise.then(datas => {
	console.log(datas);
})
//执行结果
//p1 完成了
//p1 reason
//p2 完成了
//(2)[undefined, "p2"]


// 错误既可以单独处理，也可以统一处理
// 一旦被处理，就不会在其他地方再处理一遍

//集中处理rejected
const p1 = delay(1000).then(() => {
	console.log('p1 完成了');
	return Promise.reject('reason');
});

const p2 = delay(2000).then(() => {
	console.log('p2 完成了');
	return 'p2';
});

const allPromise = Promise.all([p1, p2]);

allPromise.then(datas => {
	console.log(datas);
})
.catch(err => console.log(err));
```



#### 应用

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Promise 的应用</title>
    <style>
      #img {
        width: 80%;
        padding: 10%;
      }
    </style>
  </head>
  <body>
    <img
      src="https://img.mukewang.com/5e6af63d00011da318720764.jpg"
      alt=""
      id="img"
    />

    <script>
      // 异步加载图片
      const loadImgAsync = url => {
        return new Promise((resolve, reject) => {
          const img = new Image(); //Image构造函数

          img.onload = () => { //image加载成功
            resolve(img);
          };

          img.onerror = () => { //iamge加载失败
            reject(new Error(`Could not load image at ${url}`));
          };

          img.src = url; //img.src赋值之后，会立刻从url开始请求
        });
      };

      const imgDOM = document.getElementById('img');
      loadImgAsync('https://2img.mukewang.com/5f057a6a0001f4f918720764.jpg')
        .then(img => {
          console.log(img); //<img scr="http..."> 类似dom元素，但没放到dom时是不会显示出来的 这里的img是Promise里的 const img
          console.log(img.src); 
          setTimeout(() => {
            imgDOM.src = img.src; //切换图片
          }, 1000);
        })
        .catch(err => {
          console.log(err);
        });
    </script>
  </body>
</html>
```



### async / await



1. 什么是async / await

   - async / await 是基于Promise实现的
   - async / await 使得异步代码开起来像同步代码
   - 以前的方法有`回调函数`和`Promise`， async / await 是写异步代码的新方式

   

2. async / await语法

   async函数返回一个Promise对象，可以使用then方法添加回调函数。当函数执行的时候，一旦遇到await就会先返回，等到触发的异步操作完成，再接着执行函数体内后面的语句。

   示例：

   ```JavaScript
   const timeout= time => {
       return new Promise(function (resolve, reject) {
           setTimeout(function () {
               resolve();
           }, time);
       })
   };
   const start = async () => {
       // 在这里使用起来就像同步代码那样直观
       console.log('start');
       await timeout(3000);
       console.log('end');
       return "imooc"
   };
   start().then(()=> {
       console.log('test....')
   });
   
   //测试结果
   //start
   //(3s later)
   //end
   //test....
   ```

   

3. 注意事项

   (1) await命令只能用在async函数之中，如果用在普通函数，就会报错。

   ```JavaScript
   const timeout = time => {
       return new Promise(function (resolve, reject) {
           setTimeout(function () {
               resolve();
           }, time);
       })
   };
   function fn() {
       await timeout(300)
   }
   fn()
   //报错：Uncaught SyntaxError
   ```

   将fn改为async函数则可正常执行

   ```JavaScript
   const timeout = time => {
       return new Promise(function (resolve, reject) {
           setTimeout(function () {
               resolve();
           }, time);
       })
   };
   async function fn() {
       await timeout(300)
   }
   fn()
   ```

   

   （2）若await后面跟着是一个Promise对象，会等待Promise返回结果了，再继续执行后面的代码。

   ```JavaScript
   const timeout = time => {
       return new Promise(function (resolve, reject) {
           setTimeout(function () {
               console.log('imooc')
               resolve();
   
           }, time);
       })
   };
   const start = async () => {
       await timeout(3000);
       console.log('end');
   };
   start()
   
   //结果
   //(3s later)
   //imooc
   //end
   ```

   

   (3) 若await后面跟着的是一个数值或者字符串等数据类型的值，则直接返回该值

   ```JavaScript
   const num = 1
   const str = "imooc"
   const arr = [1,2]
   const start = async () => {
       console.log(await num)
        console.log(await str)
        console.log(await arr)
   };
   start()
   
   //结果
   //1
   //imooc
   //(2) [1, 2]
   ```

   

   (4) 若await后面跟着的是定时器，不会等待定时器里面的代码执行完，而是直接执行后面的代码，然后再执行定时器中的代码

   ```JavaScript
   const start = async () => {
       console.log(1)
       await setTimeout(() => {
           console.log(2)
       }, 1000);
       console.log(3);
   };
   start()
   
   //结果
   //1
   //3
   //(1s later)
   //2
   ```

   

4. 错误捕获

   可以直接用标准的try catch语法捕捉错误

   ```JavaScript
   const timeout = time => {
       return new Promise(function (resolve, reject) {
           setTimeout(function () {
              // 模拟出错了，返回 ‘error’
               reject('error');
           }, time);
       })
   };
   const start = async () =>{
      try {
           console.log('start');
           await timeout(3000); // 这里得到了一个返回错误
   
           // 所以以下代码不会被执行了
           console.log('end');
       } catch (err) {
           console.log(err); // 这里捕捉到错误 `error`
       }
   };
   start()
   
   //start
   //await等待调用timeout方法返回的Promise对象返回了一个错误，后面的代码不会再执行，直接进入catch，捕捉到错误
   //error
   ```