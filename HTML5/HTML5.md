# HTML 5
## 常用快捷键

复制当前行 `ctrl+shift+D`
注释 `ctrl+/`
缩进 `ctrl+[` `ctrl+]`
自动换行 `alt+z`
上下移动 `ctrl+shift+↑` `ctrl+shift+↓` 
批量修改 `按住鼠标滚轮`
自动补全标签和class内容 `div.header`->`<div class="header"></div>`

----------


## 互联网基本原理

在本地开发，在服务器共享
.html, .css, .js 上传至服务器

网址是文件位置的表达。

HTTP协议：
:   Hypertext Transfer Protocol 超文本传输协议
输入网址-》HTTP请求， 服务器识别HTTP请求，分析出用户想看哪个文件夹中的哪个文件
服务器上Java、PHP等程序运行，执行数据库（后端）
将.html发回给用户-》响应HTTP
浏览器中HTML、CSS和JS程序将运行，执行页面结构渲染、美化、交互效果


----------


## 创建网页

1. 创建一个空文件夹，在VScode编辑器中打开文件夹。
2. 按ctrl+N快捷键新建文件，保存格式必须要手动填写".html"后缀
3. 或者在文件夹中直接点击鼠标右键“新建文本文件”，将“txt”改成“html”后缀。

### HTML骨架的生成

输入 ！，按tab键， 即可自动生成HTML5的骨架
`ctrl+[`和`ctrl+]`可实现缩进

如果骨架没有生成，就说明你没有将网页保存，或者网页保存格式不是.html后缀

```html
<!DOCTYPE html> <!-- 文档类型声明DTD -->
<html lang="en"> <!-- zh表示中文 -->
<head> <!-- 网页配置 -->
    <meta charset="UTF-8"> <!-- 字符集 -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0"> <!-- 视口设置 -->
    <title>Document</title> <!-- 网页标题，搜索引擎搜索此项 -->
</head>
<body>
    
</body>
</html>
```
W3C ：
:   万维网联合会


----------


### UTF-8和GB2312的选择
|字符集|覆盖字符|1个汉字字节数|适用场景|
|:-:|:-:|:-:|:-:|
|UTF-8|涵盖全球所有国家、民族的文字和大量图行字符|3|制作有非汉字文字的网页|
|gb2312(gbk)|收录所有汉字字符（包含简体、繁体）和英语、少量韩文、日语和少量图形字符|2|制作只有汉语和英语的网页、由于1个汉字仅占2字节，网页文件尺寸明显减少|

**中文网页使用国标可以有效加快网页加载速度**
无论使用哪种字符集，一定要在VScode编辑器中将文件也设置为相同字符集，否则会出现乱码，然后更改meta标签：

```
<meta charset="UTF-8">
<meta charset="gb2312">
<meta charset="gbk">
```
`Live Server 插件不支持gb2312字符集，只支持UTF-8字符集`

![image_1eq7t04lo3im1lrnqc01llk1n6bm.png-229.5kB][1]

#### 网页关键词和页面描述

```
<meta name="Keywords" content = "牛奶，新鲜，好喝"> <!--注意K大写-->
<meta name ="Description" content="XX牛奶XXXXXXX">
```


----------


### 浏览网页的方法

1. 双击网页文件图标，即可查看网页
2. Chrome浏览器适合开发。
3. 安装Live Server插件，按ctrl+shift+p 选择 open with live server
4. 注意，网页必须存放在文件夹中，且VScode已经打开这个文件夹


----------


## 开发流程

##原型图/设计图软件
设计师使用Axure或者Sketch等软件，可以直接给我们“直观标注”的原型图/设计图

### 项目起步

- 创建文件夹结构，主要文件夹如下

|文件夹名|意义|
|:-:|:-:|
|images|存放图片|
|css|存放样式表|
|js|存放js文件|


- 网站首页`index.html`
绝大多数服务器默认的网站首页名为`index.html`

- html搭建框架
用`div`标签将页面划分


----------


## HTML特性

### 空白折叠现象

- 文字和文字之间的多个空格、换行会被折叠成一个空格
- 标签“内壁”和文字之间的空格会被忽略


----------


### 转义字符

常见转义字符：
:   如表格
|转义字符|意义|
|:-:|:-:|
|& lt ;|小于号|
|& gt ;|大于号|
|& nbsp ;|空格（不会被折叠）|
|& copy ;|版权符号&copy;|

用大于小于号代替>和<，让网页上可以显示出标签，如&lt;p&gt; 
```html
&lt;p>&lt;/p>标签对的功能是段落 
```
效果：
:    &lt;p>&lt;/p>标签对的功能是段落  
    
----------

## 标签对

不是所有标签都是成对的，也有起始标签，称为单标签
如：
`<meta charset="UTF-8">`
HTML4代，单标签必须写一个结尾的反斜杠，HTML5不用写
`<meta charset="UTF-8" />`


----------


### 标题标签

搜索引擎非常看重`h1`，应该将重点内容放到`h1`中，比如网页logo，但**一个页面应该只用一个`h1`**,很多网页`title`和`h1`使用同样内容

h系列标签表示“标题”语义，h是headline的意思
|标签|语义|
|:-:|:-:|
|h1|一级标题|
|h2|二级标题|
|h3|三级标题|
|h4|四级标题|
|h5|五级标题|
|h6|六级标题|

**没有h7及以后**


----------

### 段落标签

&lt;p>&lt;/p>标签：
:   表示段落标签，p是英语paragraph的意思
任何段落都要放到&lt;p>&lt;/p>标签中，因为HTML中即使代码换行了，页面显示效果也不会换行，必须写到&lt;p>&lt;/p>中
&lt;p>&lt;/p>标签中**不能嵌套h系列标签和其他p标签**
`alt+z`可以切换自动换行（仅VScode中代码显示效果生效，网页效果不换行）


----------

### div标签

&lt;div>&lt;/div>标签：
:   div是英语division，意为“分隔”
&lt;div>&lt;/div>标签对**用来将相关的内容组合到一起，以和其它内容分隔**，使文档结构更清晰。
比如，网页的头部要放到一个&lt;div>&lt;/div>标签对中，轮播图也要放到一个&lt;div>&lt;/div>标签对中，文章内容也要放到一个&lt;div>&lt;/div>标签对中
&lt;div>&lt;/div>是最常见的HTML标签，因为它可以结合CSS使用，实现网页的布局，这种布局形式叫做“DIV+CSS”
&lt;div>&lt;/div>像是一个容器，什么都可以容纳，因此工程师也习惯称呼&lt;div>&lt;/div>为`“盒子”`
&lt;div>&lt;/div>可以添加class属性表示“类名”，类名服务于CSS

|区域|类名|
|:-:|:-:|
|页头|header|
|logo|logo|
|导航条|nav|
|横幅|banner|
|内容区域|content|
|页脚|footer|

**HTML5中，部分区域有自己的独立标签对，不需要再使用div+类名的形式，如header、nav等。后文对此类标签有详细描述。**


----------


### 列表标签

|标签|语义|
|:-:|:-:|
|&lt;ul>&lt;/ul>|无序列表|
|&lt;ol>&lt;/ol>|有序列表|
|&lt;dl>&lt;/dl>|定义列表|

#### 无序列表

无序列表：
:   没有刻意顺序的列表
`ul` -> unordered list ->父标签
`li` -> list item ->子标签
**无序列表是一个父子组合标签，不能单独出现**
**`li`标签可以出现在`ul`和`ol`标签中，`ul`标签里只能有`li`标签**

```html
<ul>
        <li>面包</li>
        <li>牛奶</li>
        <li>鸡蛋</li>
        <li>水果</li>
    </ul>
```

**列表的标题如果使用h系列，则应该放在ul外面，而不能放在ul里面与li并列**

```html
<!-- 错误范例 -->

    <ul>
        <h1>购物清单</h1>
        <li>面包</li>
        <li>牛奶</li>
        <li>鸡蛋</li>
        <li>水果</li>
    </ul>
```
```html
<!-- 正确示范 -->

    <h1>购物清单</h1>
    <ul>
        <li>面包</li>
        <li>牛奶</li>
        <li>鸡蛋</li>
        <li>水果</li>
    </ul>
```

>**&lt;li>中可以放任何标签**

&lt;li>标签是容器，内部可以放任何其他标签
```html
<ul>
        <li>面包</li>
        <li>
            牛奶
            <p>要脱脂或低脂的</p>
        </li>
        <li>鸡蛋</li>
        <li>水果</li>
    </ul>
```

>**列表的嵌套**

```html
<ul>
        <li>
            <h3>江苏省</h3>
            <ul>
                <li>南京</li>
                <li>苏州</li>
                <li>无锡</li>
            </ul>
        </li>
        <li>
            <h3>湖北省</h3>
            <ul>
                <li>武汉</li>
                <li>黄石</li>
            </ul>
        </li>
    </ul>
```

>无序列表的type属性

无序列表有type属性，可以定义前导符号的样式，但是在HTML5中已经被废弃，建议使用CSS替代

|值|描述|
|:-:|:-:|
|disc|默认值，实心圆|
|circle|空心圆|
|square|实心方块|

```html
<h1>要购买的东西</h1>
    <ul type="disc">
        <li>牛奶</li>
        <li>面包</li>
    </ul>
```


----------


#### 有序列表

有序列表：
:   可以有顺序的列表
有序列表使用&lt;ol>&lt;/ol>标签，每个列表项都是&lt;li>&lt;/li>标签
ol->ordered list
li->list item

>**&lt;ol>标签的type属性**

ol标签可以设置type属性，用来设置编号的类型
|type属性值|意义|
|:-:|:-:|
|a|表示小写英文字母编号|
|A|表示大写英文字母编号|
|i|表示小写罗马数字编号|
|I|表示大写罗马数字编号|
|1|表示数字编号（默认）|

>**&lt;ol>标签的start属性**

start属性值必须是一个整数，指定了列表编号的起始值
此属性的值应为**阿拉伯数字**，尽管列表条目的编号类型type属性可能指定为罗马数字编号等其他类型的编号。

```html
<h1>考试成绩排名</h1>
    <ol type="A" start="5">
        <li>小慕</li>
        <li>小明</li>
        <li>小红</li>
        <li>小强</li>
    </ol>
```

>**&lt;ol>标签的reversed属性**

reversed属性指定列表中的条目是否是倒序排列的
reversed属性不需要值，只需要写reversed单词即可，这是HTML5的全新特性
```html
<ol reversed>
</ol>
```


----------

#### 定义列表

定义列表：
:   需要逐条给出定义描述的列表，就是定义列表
dl -> definition list
dt -> data term
dd -> data definition

**dt和dd可以不交替出现，而是分别处于不同的定义列表中**
```html
    <dl>
        <dt>北京</dt>
        <dd>我国政治中心、文化中心、国际交往中心、科技创新中心</dd>
    </dl>
    <dl>
        <dt>上海</dt>
        <dd>我国国际经济、金融、贸易、航运、科技创新中心</dd>
    </dl>
    <dl>
        <dt>深圳</dt>
        <dd>中国经济特区、全国性经济中心城市和国际化城市</dd>
    </dl>   
```

>**哪里应该使用定义列表**

![image_1eqh6vr1hhmgpot15iu1cpfhvu9.png-218.9kB][2]


----------


### 多媒体与语义标签

#### 图片标签

&lt;img>标签用来在网页中插入图片
```html
<img src="images/gugong.jpg">
```
- img是image（图片）的缩写
- src是source（来源）的缩写
- "images/gugong.jpg"图片的存储目录和完整文件名

**注意，图片必须要存储到项目文件夹中，一般将图片保存到项目文件夹中的images子文件夹中**

<br/>

>**&lt;img>标签的alt属性**

alt属性是alternate“替代品”的缩写，它是对图像的文本描述，不是强制性的
```html
<img src="images/gugong.jpg" alt="故宫角楼">
```
- 如果由于某种原因无法加载图像，浏览器会在页面上显示alt属性中的备用文本。
- 供实力不方便的人士使用的网页朗读器，也会朗读alt中的文本

```html
<body>
    <h1>北京景点</h1>
    <p>
        <img src="images/gugong.jpg" alt="故宫角楼">
    </p>
    <p>
        <img src="images/niaochao.jpg" alt="鸟巢-国家体育场">
    </p>
</body>
```

<br/>

>**&lt;img>标签的width、height属性**

- width、height属性分别设置宽度和高度、单位是**像素**，但是不需要写单位。

```html
<img src="images/gugong.jpg" alt="故宫角楼" width="300">
```

- 如果省略期中一个属性，则表示按原始比例缩放图片

<br/>

>**网页上支持的图片格式**

|格式|说明|
|:-:|:-:|
|.bmp|windows画图软件默认保存的格式，位图|
|.gif|支持动画（比如表情包）|
|.jpeg(.jpg)|有损压缩图片，用于照片|
|.png|便携式网络图像，用于logo、背景图形等，支持透明和半透明|
|.svg|矢量图片|
|.webp|最新的压缩算法非常优秀的图片格式|

<br/>

>**相对路径和绝对路径**

相对路径：
:   描述从网页触发、如何找到图片。比如“前面路口左转，直走100米后右转”

```html
<img src="images/gugong.jpg">
```

- 随着网页和图片的位置关系不同，插入图片的代码随之改变。
- 如果需要回退层级，使用"`../`"这样的写法

绝对路径：
:   描述图片精准地之。比如“北京市海淀区西三环北路甲2号院中关村国防科技园2号楼”

- 不管网页在哪里，绝对路径不需要改变。

```html
<img src="https://www.imooc.com/static/img/index/logo-recommended.png">
```


----------


#### 超级链接

超级链接：
:   超级链接是将网页和网页连结到一起的方法，是互联网成“网”的原因

>**&lt;a>标签**

- 使用&lt;a>标签制作超级链接

```html
<a href="2.html">去第二个网页</a>
```

- a是anchor(锚)的首字母
- href->hypertext reference 超文本引用

可以使用p标签包裹a标签：
```html
<body>
    <p>
        <a href="lvyou.html">去看我的旅游分享</a>
    </p>
</body>
```

>**href属性支持相对路径和绝对路径**

```html
    <a href="web/2.html">去第二个网页</a>
    <a href="../web/2.html">去第二个网页</a>
    <a href="http://www.immoc.com">去慕课</a>
```

>**&lt;a>标签的title属性**

&lt;a>标签的title属性用于设置鼠标的悬停文本。
```html
    <a href="web/2.html" title="很好看哦">去第二个网页</a>
```
![image_1eqh8is701fp115129pgpefotqm.png-43.9kB][3]

>**在新窗口中打开网页**

将&lt;a>标签的target属性设置为blank， 即可在**新标签页中打开网页**
```html
    <a href="web/2.html" target="blank">去第二个网页</a>
```
在HTML4代，blank之前有一个下划线
```html
    <a href="web/2.html" target="_blank">去第二个网页</a>
```

>**给图片设置超级链接**

图片也可以设置超级链接，只需要用&lt;a>标签包裹&lt;img>标签即可。
```html
    <a href="web/2.html" target="_blank">
        <img src="images/goblin.png">
    </a>
```

>**页面内锚点**

其他页面的超级链接，可以链接到指定锚点。
```html
    <a href="lvyou.html#wuxin">看无锡美景</a>
    
    <h2 id="wuxi">无锡旅游照片</h2>
    <p>
        <a href="#top">返回顶部</a>
    </p>
```

>**下载链接**

指向exe、zip、rar等文件格式的链接，将自动称为下载链接

```html
<a href="1.zip">下载</a>
```

>**邮件链接、电话链接**

有`mailto:`前缀的链接是邮件链接，系统将自动打开Email相关软件。

```html
<a href="mailto:me@test.com">给小编发邮件</a>
```

有`tel:`前缀的的链接是电话链接，系统将自动打开拨号盘。
```html
<a href="tel:12306">打电话买火车票</a>
```


----------


#### 音频和视频

曾几何时，在网页中插入音频和视频需要借助Flash，今天，Flash技术已经快要被淘汰，HTML5可以轻松在网页中插入音频和视频。

`音频`

**在浏览器中插入音频需要使用&lt;audio>标签， 兼容到IE9**

```html
<audio controls src="音频地址">
    抱歉，您的浏览器不支持audio标签，请升级浏览器
</audio>
```
- controls-> 显示播放控件
- src的值是音频来源地址
- audio包裹的文字内容是对不兼容audio标签的浏览器显示的文字
- 支持MP3和ogg格式

>**autoplay属性**

- 声明autoplay属性，音频会自动播放。
- 常用浏览器为了不打扰用户，可能会**不允许自动播放音乐**，必须让用户手动点击之后才能播放。

>**loop属性**

- 声明loop属性，将循环播放音频


----------
`视频`

**在浏览器中插入音频需要使用&lt;video>标签， 兼容到IE9**

```html
<video controls src="视频地址">
    抱歉，您的浏览器不支持video标签，请升级浏览器
</video>
```
- 常用视频格式是mp4、ogv、webm等格式。
```html
<video src="video/flower.mp4"  controls loop autoplay width="300"></video>
```


----------

## 语义化标签

### 区块标签

**div标签实现文档区块分隔：**

- 曾几何时，div标签是文档区块分隔唯一手段，为了区分每个div功能，程序员会借助div标签的class属性。

```html
<body>
    <div class="header">
        <div class="logo"></div>
        <div class="nav"></div>
    </div>
    <div class="banner"></div>
    <div class="content">
        <div class="aside"></div>
        <div class="main"></div>
    </div>
    <div class="footer"></div>
</body>
```

**HTML5区块标签：**

- HTML5推出了众多新的区块标签

|区块标签|说明|
|:-:|:-:|
|`<section>`|文档的区域，语义比div大|
|`<article>`|文档的核心文章内容，会被搜索引擎主要抓取|
|`<aside>`|文档的非必要相关内容，比如广告等|
|`<nav>`|导航条|
|`<header>`|页头|
|`<main>`|网页核心部分|
|`<footer>`|页脚|


----------

### span标签

&lt;span>标签:
:   &lt;span>标签是文本中的“区块”标签，本身没有任何特殊的显示效果，可以结合CSS来丰富样式

```html
<p>
    <span>北京</span>的区号是<span>010</span>
</p>
<p>
    <span>上海</span>的区号是<span>021</span>
</p>
```

----------

### b，u，i标签

&lt;b>,&lt;u>,&lt;i>标签：
:   &lt;b>,&lt;u>,&lt;i>标签充满浓浓的“样式”意味，已经被CSS替代，但是在网页中可以表示需要强调的文本。

|标签|说明|
|:-:|:-:|
|`<b>`|被加粗的文字，CSS已经替代了它的功能|
|`<u>`|加下划线的文字|
|`<i>`|倾斜的文字|


----------

### strong，em，mark标签

&lt;strong>,&lt;em>,&lt;mark>标签：
:   &lt;strong>,&lt;em>,&lt;mark>标签均表示强调语义

|标签|说明|
|:-:|:-:|
|`<strong>`|代表特别重要的文字|
|`<em>`|代表强调文字|
|`<mark>`|代表需要被高亮的文字|


----------

### figure、figcaption标签

- 被放在figure里的图片可有可无，不影响整个网页内容
`<figure>`元素代表一段独立的内容，与说明`<figcaption>`配合使用，它是一个独立的引用单元，比如建议读者拓展视野的图片等，当这部分转移到附录中或者其他页面时不会影响到主体。

```html
<p>
        <figure>
            <img src="images/beijing/1.jpg" alt="">
            <figcaption>北京鸟巢</figcaption>  
        </figure>
        <figure>
            <img src="imges/beijing/2.jpg" alt="">
            <figcaption>颐和园十七孔桥</figcaption>
        </figure>
    </p>
```

----------

## 表单标签

- 所有HTML表单都以一个&lt;form&gt;元素开始

```html
<form action="save.php" method="post">
</form>
```
- action属性表示表单要提交到的后台程序的网址
- method属性表示表单提交的方式，有get或post

##单行文本框

- 使用type属性值被设置为`text`的`<input>`元素可以创建但行文本框，它是一个单标签。
```html
<input type="text">
```
- value属性表示已经填写好的值
```html
<input type="text" value="123">
```
- placeholder属性表示提示文本，将以浅色文字写在文本框中，并不是文本框中的值。
```html
<input type="text" placeholder="请输入姓名">
```
- disabled属性表示用户不能与元素交互，即“锁死”
```html
<input type="text" value="女" disabled>
```

```html
<form action="">
        <p>
            请输入你的姓名：<input type="text">
        </p>
        <p>
            报考院校：<input type="text" value="清华大学">
        </p>
        <p>
            毕业学校：<input type="text" placeholder="请输入真实的毕业学校哦">
        </p>
    </form>
```
实际网页效果：
![image_1eqbs7lrs1pkmpjm12h3jt8k1gil.png-44.3kB][4]


----------


### 单选按钮

使用type属性值被设置为radio的`<input>`元素可以创建单选按钮
```html
<input type="radio">
```

- 互斥的单选按钮应该设置它们的name为相同值。
- 单选按钮要有value属性值，向服务器提交的就是value值。
- 单选按钮如果加上了checked属性，表示默认被选中。
```html
    <p>
        性别：
        <input type="radio" name="sex" value="male">男
        <input type="radio" name="sex" value="female">女
    </p>
```
----------

### label标签

label标签用来将文字的单选按钮进行绑定，用户单击文字的时候也视为点击了单选按钮
```html
    <label>
        <input type="radio">男
    </label>
    <label>
        <input type="radio">女
    </label>
```
- 在HTML4时代，label标签是通过for属性和单选按钮的id属性绑定的
```html
    <input type="radio" id="male">
    <label for="male">男</label>
```
```html
    <input type="radio" name="bloodtype" checked id="A"><label for="A">A</label>
```


----------

### 复选框

使用type属性值被设置为checkbox的`<input>`元素可以创建复选框

```html
<input type="checkbox">
```
- 同组复选框应该设置它们的name为相同值
- 复选框要有value属性值，向服务器提交的就是value值


----------

### 密码框

使用type属性值被设置为password的`<input>`元素可以创建密码框

```html
<p>
    请输入密码：
    <input type="password">
</p>
```

----------

### 下拉菜单

`<select>`标签表示下拉菜单，`<option>`是它内部的选项。

```html
    <select>
        <option value="alipay">支付宝</option>
        <option value="wx">微信</option>
        <option value="bank">网银</option>
    </select>
```
```html
    <p>
        请选择你最喜欢的编程语言：
        <select>
            <option value="JavaScript">JavaScript</option>
            <option value="Java">Java</option>
            <option value="PHP">PHP</option>
            <option value="C++">C++</option>
        </select>
    </p>
```


----------

### 多行文本框

`<textarea></textarea>`表示多行文本框

- rows和cols属性，用于定义多行文本框的行数和列数

```html
<p>
        留言：
        <textarea name="message" id="message" cols="30" rows="10"></textarea>
    </p>
```

----------

### 按钮

|按钮|功能|
|:-:|:-:|
|button|普通按钮|
|submit|提交表单|
|reset|重置表单|

```html
<!--以下两种形式都可以-->
    <p>
        <button>我是一个普通按钮</button>
    </p>
    <p>
        <input type="button" value="我是一个普通按钮">
    </p>
```

----------

### HTML5新增表单控件（兼容IE9）

#### 更丰富的input种类

|type属性值|控件|
|:-:|:-:|
|color|颜色选择控件|
|date、time|日期、时间选择控件|
|email|电子邮件输入控件|
|file|文件选择控件|
|number|数字输入控件|
|range|拖拽条|
|search|搜索框|
|url|网址输入控件|

```html
    <form action="">
        <p>
            时间选择：
            <input type="time">
        </p>
        <p>
            电子邮件：
            <input type="email">
        </p>
        <p>
            必填：
            <input type="text" required>
        </p>
        <p>
            <input type="submit" value="提交表单">
        </p>
    </form>
```
- **使用input的控件，HTML5会自带纠错。比如email如果格式不对，则表单无法提交。**

```html
<p>
    搜索：
    <input type="search">
</p>
```

- **搜索框比普通文本框多了取消功能**


----------

#### datalist 控件

`<datalist>`控件可以为输入框提供一些备选项，当用户输入的内容与备选项文字相同时，将会显示智能感应。

```html
    <input type="text" list="province-list">
    <datalist id="province-list">
        <option value="山东">
        <option value="山西">
        <option value="广东">
        <option value="广西">
        <option value="河南">
        <option value="河北">
        <option value="湖南">
        <option value="湖北">
    </datalist>
```

- **list属性与id要完全相同才能产生绑定关系。**


----------

## 表格标签

### table, tr, td标签

- `<table>` ->表格
- `<tr>` ->table row 表格行 
- `<td>` ->table data 表格数据

![image_1eqc06q3m7nq1f9u14sn1q7hr4lt5.png-46.6kB][5]

```html
    <table>
        <tr>
            <td>A</td>
            <td>B</td>
            <td>C</td>
            <td>D</td>
        </tr>
        <tr>
            <td>E</td>
            <td>F</td>
            <td>G</td>
            <td>H</td>
        </tr>
        <tr>
            <td>I</td>
            <td>J</td>
            <td>K</td>
            <td>L</td>
        </tr>
    </table>
```

>**table的border属性**

为了让表格能够显示边框，`<table>`标签通常有border属性

```html
    <table border="1">
        ...
    </table>
```

<br/>

>**table的caption属性**

`<caption>`是表格的标题，它常常作为`<table>`的第一个子元素出现。
```html
    <table border="1">
        <caption>我是表格的标题</caption>
        <tr>
            <td>A</td>
            <td>B</td>
            <td>C</td>
            <td>D</td>
        </tr>
    </table>
```
- caption默认最宽和表格一样宽


----------

### th标签

`<th>`标签是“标题小格”，可以代替`<td>`的作用，表示标题小格

```html
<table border="1" width="600">
        <caption>我是表格的标题</caption>
        <tr>
            <th>第一季度</th>
            <th>第二季度</th>
            <th>第三季度</th>
            <th>第四季度</th>
        </tr>
        <tr>
            ...
        </tr>
        ...
    </table>
```
![image_1eqc0e0en1etdlv20tuc51nee100.png-56.8kB][6]


----------

### 单元格的合并

>**colspan属性**

- colspan属性用来设置td或者th的列跨度
![image_1eqhpe4l41dkop86fmg1u9a1ou01g.png-20.6kB][7]

```html
    <table border="1">
        <tr>
            <td colspan="2">A</td>
            <td>B</td>
            <td>C</td>
        </tr>
        <tr>
            <td>D</td>
            <td colspan="3">E</td>
        </tr>
        <tr>
            <td>F</td>
            <td>G</td>
            <td>H</td>
            <td>I</td>
        </tr>
    </table>
```

<br/>

>**rowspan属性**

- rowspan属性用来设置td或者th的行跨度

![image_1eqhpiol616no19i5ui7q5e1c371t.png-100.2kB][8]

```html
    <table border="1">
        <tr>
            <td>A</td>
            <td>B</td>
            <td>C</td>
            <td>D</td>
        </tr>
        <tr>
            <td>E</td>
            <td rowspan="2">F</td>
            <td>G</td>
            <td rowspan="2">H</td>
        </tr>
        <tr>
            <td>I</td>
            <td>J</td>
        </tr>
        <tr>
            <td>K</td>
            <td>L</td>
            <td>M</td>
        </tr>
    </table>
```

>**同时有rowspan、colspan属性**

![image_1eqhps1u47u23ar19un1r0ds1f2a.png-80.3kB][9]
```html
<h2>同时有行跨度和列跨度</h2>
    <table border="1" width="300">
        <tr>
            <td>A</td>
            <td>B</td>
            <td>C</td>
            <td>D</td>
        </tr>
        <tr>
            <td>E</td>
            <td rowspan="2">F</td>
            <td rowspan="3" colspan="2">G</td>
        </tr>
        <tr>
            <td>H</td>
        </tr>
        <tr>
            <td>I</td>
            <td>J</td>
        </tr>
    </table>
```


----------

### 表格的其他特性

`<thead>`、`<tbody>`、`<tfoot>`标签：
:   `<thead>`标签定义表头
`<tbody>`标签定义表核心内容
`<tfoot>`标签定义表脚，通常是汇总行
```html
    <table border="1" width="700">
        <thead>
            <tr>
                <th colspan="2">第一季度</th>
                <th colspan="2">第二季度</th>
                <th colspan="2">第三季度</th>
                <th colspan="2">第四季度</th>
            </tr>
            <tr>
                <th>国内</th>
                <th>国外</th>
                <th>国内</th>
                <th>国外</th>
                <th>国内</th>
                <th>国外</th>
                <th>国内</th>
                <th>国外</th>
            </tr>
        </thead>
    </table>
```

![image_1eqcume631def1jhk12sg221rvs142.png-114.7kB][14]

- **[手机]上方两个空的格用th或者td留空**


----------

### cellspacing、cellpadding属性

- cellpadding属性定义了表格单元的内容和边框之间的空间，已经废弃，使用CSS替代它。
- cellspacing属性（使用百分比或像素）定义了两个单元格之间空间的大小，已经废弃，被CSS替代。
- 但在实际开发中，如有需要使用的场景，仍可使用。

  [1]: http://static.zybuluo.com/Yunabell/9plojlza81xl83wulkb2podf/image_1eq7t04lo3im1lrnqc01llk1n6bm.png
  [2]: http://static.zybuluo.com/Yunabell/y9v8g78z5r7zq25e1qg22cgg/image_1eqh6vr1hhmgpot15iu1cpfhvu9.png
  [3]: http://static.zybuluo.com/Yunabell/cv8x4a0n3rrstdaligqutanr/image_1eqh8is701fp115129pgpefotqm.png
  [4]: http://static.zybuluo.com/Yunabell/2iklqxpplpeiq5jowxpcjis6/image_1eqbs7lrs1pkmpjm12h3jt8k1gil.png
  [5]: http://static.zybuluo.com/Yunabell/6zmji6ta2i6qk3piy0n1qeum/image_1eqc06q3m7nq1f9u14sn1q7hr4lt5.png
  [6]: http://static.zybuluo.com/Yunabell/7ocwyijp0w8x754gss5m95hs/image_1eqc0e0en1etdlv20tuc51nee100.png
  [7]: http://static.zybuluo.com/Yunabell/8pjtwtpjgu2okzv8u28co2tp/image_1eqhpe4l41dkop86fmg1u9a1ou01g.png
  [8]: http://static.zybuluo.com/Yunabell/icwwbpvu6au86m7ehlqc3oz4/image_1eqhpiol616no19i5ui7q5e1c371t.png
  [9]: http://static.zybuluo.com/Yunabell/txdtauxbdj9jdgpwfwdtikt5/image_1eqhps1u47u23ar19un1r0ds1f2a.png
  [10]: http://static.zybuluo.com/Yunabell/b1flqy7ikv34wlj1czlaapr3/image_1eqcuevkc1rdr1fm619781hkp17s912e.png
  [11]: http://static.zybuluo.com/Yunabell/3ufwn9q85vt7sqy8bzuf9a2o/image_1eqcugj281jpr1c9t3ppka11d0t12r.png
  [12]: http://static.zybuluo.com/Yunabell/10tdux8ms0beoew0ir9ljtr9/image_1eqcujp3q1emm1vqi16ofhot1rcc138.png
  [13]: http://static.zybuluo.com/Yunabell/adxxlqaebjyylc3965miducp/image_1eqcul4ld19gton71lnktjq1hfs13l.png
  [14]: http://static.zybuluo.com/Yunabell/imevza9cmpnfffhq0w56jhp3/image_1eqcume631def1jhk12sg221rvs142.png
  [15]: http://static.zybuluo.com/Yunabell/42gytxplhlnyoox3sga6v96t/image_1eqcungsv62uc3fc361f971vgh14f.png