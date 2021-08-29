

## 移动Web开发入门

**前端开发分类**

- PC端开发
- 移动端开发
  - 移动Web开发
  - 混合式开发（Hybrid App）
  - 微信小程序/公众号开发



### 移动端开发的基础概念

#### 像素

- 分辨率

- 物理像素 (physical pixel 或 dp: device pixel)

- CSS像素 （logical pixel 或 dip ：device independent pixel）: 

  实际开发中并不以物理像素为准，而是CSS像素

- 设备像素比（dpr：device pixel ratio）：

  设备像素/CSS像素（缩放比是1的情况下）

  dpr=2表示1个css像素用2x2个设备像素来绘制

- 标清屏和高清屏：

  标清：dpr = 1

  高清：dpr > 1

  浏览器会自动推算一个CSS像素用几个设备像素来表现

  ![image-20210826155124222](http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/image-20210826155124222.png)

- 缩放 ：缩放改变的是CSS像素的大小

  放大：同设备，如1css像素 = 1物理像素，放大两倍后：1css像素 = 2x2 物理像素

  缩小： 同设备，如1css像素 = 1物理像素，缩小两倍后：2x2css 像素= 1 物理像素

- PPI/DPI

  PPI（pixels per inch）

  DPI （dots per inch）

  两者相同，只是不同的称呼，表示像素密度。

  ![image-20210826160033550](http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/image-20210826160033550.png)



****



### 视口 -- viewport

viewport是移动端引入的概念

下图所示视口宽度为980px，屏幕宽度为375px，两者并不匹配。

一般的做法是把视口缩放到可视区范围内，避免横向滚动条。

![2.2-2](http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/2.2-2.png)



#### 设定视窗比例

**指定视口宽度**：

```html
<meta name="viewport" content="width=375">
```

这里是指定的视口宽度和设备宽度一致，但用户设备的宽度是不能提前预知的，所以这里就需要：

**自动调整视口宽度=设备宽度：**

```html
<meta name="viewport" content="width=device-width">

<!--也可以让高度自动适配，但是基本没有用到的时候-->
<!-- <meta name="viewport" content="height"> -->
```

**也可以按缩放比自动适配：**

```html
<!--scale是1时，相当于content="width=device-width"-->
<meta name="viewport" content="initial-scale=1">

<!--scale > 1时，放大视口-->
<meta name="viewport" content="initial-scale=2">

<meta name="viewport" content="initial-scale=0.5">
```

考虑到浏览器兼容问题，一般会把两种写法都写上，保证能生效：

```html
<meta name="viewport" content="width=device-width, initial-scale=1">
```

一般在移动端，不希望用户进行缩放：

```html
<meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=no">

<!-- 有些浏览器不允许设定user-scalable为no，此时可以限定scale，同样可以限制缩放 -->
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, minimum-scale=1">
```

**因此通用的视窗设定可以设定为**：

```html
<meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=no, maximum-scale=1, minimum-scale=1">
```



#### 获取设备屏幕宽度

```javascript
//以下三种方法都可以获得用户当前设备宽度
console.log(window.innerWidth);
console.log(document.documentElement.clientWidth);
console.log(document.documentElement.getBoundingClientRect().width);
```

考虑到兼容性问题：

```javascript
var viewWidth = document.documentElement.clientWidth || window.innerWidth;
```

注意不要使用：

```javascript
console.log(screen.width);
//screen.width有时获取视口，有时获取物理分辨率，容易出错
```

还可以获取到dpr (设备像素比)：

```javascript
console.log(window.devicePixelRatio);
```



****

#### box-sizing

移动端一般不需要考虑box-sizing的兼容性问题，可以大胆使用。

pc端可能更多的是使用`content-box`，在移动端会更多的使用`border-box`.

- content-box (是默认值，不特别标注的话，就是这种类型)

  盒模型宽/高 = width/height + padding + border

- border-box （总占位不变，有padding和border时，会把内容往里压缩）

  盒模型宽/高 = width/height

  注意：margin是另外计算的，不包括是border-box里面

border-box在使用float时非常好用，避免添加边框后把盒子挤下去。



****



### 媒体查询

##### 什么是媒体查询 media query

媒体查询是指查询屏幕尺寸

```html
<style>
    /*在设备屏幕大于等于900px时，执行里面的语句*/
	@media screen and (min-width: 900px) {
		body {
			background-color: red;
		}
	}
</style>
```



##### 为什么需要媒体查询

一套样式不可能适应各种大小的屏幕，针对不同的屏幕大小写样式，让我们的页面在不同大小的屏幕上都能正常显示



##### 媒体类型

- all (default) ：默认值，所有都可以

  ```html
  <style>
  	@media (min-width: 900px) {
  		body {
  			background-color: red;
  		}
  	}
  </style>
  ```

  

- screen : 屏幕设备

- print ：打印预览

- speech ：屏幕阅读器



##### 媒体查询中的逻辑

- 与( and )

  ```html
  <style>
      /* 900<= 设备屏幕 <= 1024 时执行*/
  	@media screen and (min-width: 900px) and (max-width: 1024px) {
          body {
              background-color: red;
          }
  	}
  </style>
  ```

- 或( , )

  ```html
  <style>
      /*设备屏幕 >= 1024 或 all <= 900 时执行*/
      /*注意这里 screen 后面的 and 只包含 screen和min-width，逗号之后的max-width不被包含在内*/
  	@media screen and (min-width: 1024px), (max-width: 900px) {
          body {
              background-color: red;
          }
  	}
  </style>
  ```

  ```html
  <style>
      /*设备屏幕 >= 1024 或 设备屏幕 <= 900 时执行*/
  	@media screen and (min-width: 1024px), screen and (max-width: 900px) {
          body {
              background-color: red;
          }
  	}
  </style>
  ```

- 非( not )

  ```html
  <style>
      /*设备屏幕 >= 1024 或 设备屏幕 <= 900 时执行*/
      /*not 包含所有and的内容 */
  	@media not screen and (min-width: 900px) and (max-width: 1024px) {
          body {
              background-color: red;
          }
  	}
  </style>
  ```

  ```html
  <style>
      /*设备屏幕 < 1024 或 设备屏幕 <= 900 时执行*/
      /*not 只作用在第一个and上，逗号之后的部分不被not涵盖 */
  	@media not screen and (min-width: 1024px), screen and (max-width: 900px) {
          body {
              background-color: red;
          }
  	}
  </style>
  ```

  

##### 媒体特征表达式

- width/max-width/min-width : 单独的width只能规定一个特定的值，很少使用

- -webkit-device-pixel-ratio/-webkit-max-device-pixel-ratio/-webkit-min-pixel-ratio

  ```html
  <style>
      /*DPR <= 2 时生效 */
  	@media (-webkit-max-device-pixel-ratio: 2) {
          body {
              background-color: red;
          }
  	}
  </style>
  ```

- orientation
  - landscape/portrait: 横屏/竖屏

  ```css
  <style>
      /*DPR <= 2 并且横屏时 （宽 > 高）生效 */
  	@media (-webkit-max-device-pixel-ratio: 2) and (orientation: landscape) {
          body {
              background-color: red;
          }
  	}
  </style>
  ```

- height：根据屏幕高度生效，基本不会用到
- device-width/device-height : 和JS中的screen.width/screen.height问题类似，获取的有时是物理像素有时是CSS像素，不好用
- aspect-ratio ：视口的宽高比，基本不会用到



#### 媒体查询策略

**断点： 发生变化的点**

- xs （超小屏，一般的手机）: < 576px
- sm （小屏，大屏手机）: 576px ~ 768px
- md （中屏，平板）: 768px ~ 992px
- lg （大屏，显示器）: 992px ~ 1200px
- xl （超大屏）: > 1200px

实际开发中，只要页面显示不正常时，就应该设置断点了，不一定非要按照以上的尺寸来划分。



下面的代码是一个不太聪明的写法，虽然一目了然，但是很啰嗦：

```css
<style>
/*超小屏*/
@media (max-width: 576px) {
    .col {
        width: 100%;
    }
}
/*小屏*/
@media (min-width: 577px) and (max-width: 768px) {
    .col {
        width: 50%;
    }
}
/*中屏*/
@media (min-width: 769px) and (max-width: 992px) {
    .col {
        width: 25%;
    }
}
/*大屏*/
@media (min-width: 993px) and (max-width: 1200px) {
    .col {
        width: 16.6666666667%;
    }
}
/*超大屏*/
@media (min-width: 1201px) {
    .col {
        width: 8.333333333%;
    }
}
</style>
```

**比较好的写法是：**

注意：下面的写法同上面的代码不同，顺序是不能更改的

```html
<style>
/*优先PC端*/
    .col {
            width: 8.333333333%;
        }
        @media (max-width: 1200px) {
            .col {
                width: 16.6666666667%;
            }
        }
        @media (max-width: 992px) {
            .col {
                width: 25%;
            }
        }
        @media (max-width: 768px) {
            .col {
                width: 50%;
            }
        }
        @media (max-width: 576px) {
            .col {
                width: 100%;
            }
        }
</style>
```

```html
<style>
    /*或者优先手机端*/
    .col {
            width: 100%;
        }
        @media (min-width: 576px) {
            .col {
                width: 50%;
            }
        }
        @media (min-width: 768px) {
            .col {
                width: 25%;
            }
        }
        @media (min-width: 992px) {
            .col {
                width: 16.6666666667%;
            }
        }
        @media (min-width: 1200px) {
            .col {
                width: 8.33333333%;
            }
        }
</style>
```



****



### 移动端常用单位

- **px** ：CSS像素

- **em** ：根据字体大小变化，1em 等于**元素自身**一个字体size所代表的的px的大小。

  如，font-size是100px，则1em=100px， 2em = 200px

  注意，在pc端最小生效的字体是12px

  如果元素自身字体大小就是使用em作为单位，则em继承父元素的字体大小

  因为em是基于自身元素字体大小的，所以实际开发很少用em（除了有些情况，如首行缩进2字符）

- **rem**：根据**根元素**的字体大小变化，即根据`html`的`font-size`，实际开发一般使用rem作为单位

- **vw**：视口宽度的百分比，如 50vw 为视口宽度的50%

- **vh**：视口高度的百分比，如 100vh 为视口高度的100%

  因为兼容性问题，vw和vh用的不是很多，rem还是主流



根据视口宽度计算合适的rem对应的font-size值（等比例缩放）：

```html
<script>
    setRemUnit();
    
    window.onresize = setRemUnit;
    
    function setRemUnit() {
        var docEl = document.documentElement;
        var viewWidth = docEl.clientWidth;
        
        docEl.style.fontSize = viewWidth / 375 * 20 + 'px';
        
        // docEl.style.fontSize = viewWidth / 750 * 40 + 'px';
        // 1rem = 20px
    }
</script>
```



