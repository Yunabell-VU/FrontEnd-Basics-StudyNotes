## Flex布局

### 基本概念

**什么是 flex 容器（flex container）？**

采用 flex 布局的元素，称为 flex 容器

```css
 .box {
    display: flex | inline-flex;
}
```

**什么是 flex 项目（flex item）？**

flex 容器的所有**子元素**自动成为容器成员(子元素的子元素不会成为容器成员)，称为 flex 项目

![3.3-1](http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/3.3-1.png)

- 默认水平方向是主轴（main axis），垂直方向是交叉轴（cross axis）
- 主轴有 main start 和 main end， 交叉轴亦有 cross start 和 cross end
-  容器内子元素和主轴平行方向的宽度是主轴空间 main size，垂直方向是交叉空间 cross size
- 项目默认沿主轴排列 （水平排列）



****



### 容器的属性

写在容器上的属性：

- display 
- flex-direction
- flex-wrap
- flex-flow
- justify-content
- align-items
- align-content



#### display 属性

- display 属性决定是否使用flex布局

```css
 .box {
    display: flex
}
```
**flex**: 将对象作为弹性伸缩盒显示，父容器有自己的宽度

![image-20210828200539148](http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/image-20210828200539148.png)

```css
 .box {
    display: inline-flex;
}
```

**inline-flex**：将对象设置为内联元素，但是又有块元素的特性；父容器在没有指定宽度时，宽度由子元素撑开

![image-20210828200549185](http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/image-20210828200549185.png)



#### flex-direction 属性

- 决定主轴的方向

```css
.box {
    flex-direction: row；
}
```

**row（默认值）**：主轴为水平方向，起点在左端

![image-20210828201125789](http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/image-20210828201125789.png)

```css
.box {
    flex-direction: row-reverse;
}
```

**row-reverse**: 主轴为水平方向，起点在右端

![image-20210828201149010](http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/image-20210828201149010.png)

```css
.box {
    flex-direction: column;
}
```

**column**: 主轴为垂直方向，起点在上沿

![image-20210828201354339](http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/image-20210828201354339.png)

```css
.box {
    flex-direction: column-reverse;
}
```

**column-reverse**: 主轴为垂直方向，起点在下沿

![image-20210828201412001](http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/image-20210828201412001.png)



#### flex-wrap 属性

- 默认情况下，项目都排在一条线（又称"轴线"）上
- flex-wrap 属性定义，如果一条轴线排不下，如何换行

```css
.box{ 
    flex-wrap: nowrap; 
}
```

**nowrap（默认值）**: 不换行 (即使规定的每个子元素的宽度，仍会被自动压缩宽度)

![image-20210828201544277](http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/image-20210828201544277.png)

```css
.box{ 
    flex-wrap: wrap; 
}
```

**wrap**: 换行，第一行在上方

![image-20210828201704272](http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/image-20210828201704272.png)

```css
.box{ 
    flex-wrap: wrap-reverse; 
}
```

**wrap-reverse**:换行，第一行在下方

![image-20210828201713655](http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/image-20210828201713655.png)



#### flex-flow 属性

- flex-flow 属性是 flex-direction 属性和 flex-wrap 属性的简写形式，默认值为 row nowrap

```css
.box { 
    flex-flow: <flex-direction> || <flex-wrap>; 
}
```

![image-20210828201833182](http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/image-20210828201833182.png)



#### justify-content 属性

- justify-content 属性定义了项目在主轴上的对齐方式

```css
.box { 
    justify-content: flex-start; 
}
```

**flex-start（默认值）**: 左对齐

![image-20210828202230076](http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/image-20210828202230076.png)

```css
.box { 
    justify-content: flex-end; 
}
```

**flex-end**: 右对齐

![image-20210828202326519](http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/image-20210828202326519.png)

```css
.box { 
    justify-content: center; 
}
```

**center**:  居中

![image-20210828202417146](http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/image-20210828202417146.png)

```css
.box { 
    justify-content: space-between; 
}
```

**space-between**: 两端对齐，项目之间的间隔都相等

![image-20210828202455254](http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/image-20210828202455254.png)

```css
.box { 
    justify-content: space-around; 
}
```

**space-around**: 每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍

![image-20210828202533313](http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/image-20210828202533313.png)



#### align-items 属性

- align-items 属性定义项目在交叉轴上如何对齐

```css
.box { 
    align-items: flex-start; 
}
```

**flex-start**: 交叉轴的起点对齐

![image-20210828202725199](http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/image-20210828202725199.png)

```css
.box { 
    align-items: flex-end; 
}
```

**flex-end**: 交叉轴的终点对齐

![image-20210828202814392](http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/image-20210828202814392.png)

```css
.box { 
    align-items: center; 
}
```

**center**： 交叉轴的中点对齐

![image-20210828203100359](http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/image-20210828203100359.png)

```css
.box { 
    align-items: baseline; 
}
```

**baseline**: 项目的第一行文字的基线对齐

![image-20210828203152195](http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/image-20210828203152195.png)

```css
.box { 
    align-items: stretch; 
}
```

**stretch（默认值）**:  如果项目未设置高度或设为auto，将占满整个容器的高度

![image-20210828203244138](http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/image-20210828203244138.png)



#### align-content 属性

- align-content 属性定义了多根轴线（多行）在交叉轴上的对齐方式
- 如果项目只有一根轴线（一行），该属性不起作用

```css
.box { 
    align-content: flex-start; 
}
```

**flex-start**：交叉轴的起点对齐

![image-20210828203344966](http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/image-20210828203344966.png)

```css
.box { 
    align-content: flex-end; 
}
```

**flex-end**：与交叉轴的终点对齐

![image-20210828203420483](http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/image-20210828203420483.png)

```css
.box { 
    align-content: center; 
}
```

**center**：与交叉轴的中点对齐

![image-20210828203511035](http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/image-20210828203511035.png)

```css
.box { 
    align-content: space-between; 
}
```

**space-between**：与交叉轴两端对齐，轴线之间的间隔平均分布

![image-20210828203557622](http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/image-20210828203557622.png)

```css
.box { 
    align-content: space-around; 
}
```

**space-around**：每根轴线两侧的间隔都相等, 所以，轴线之间的间隔比轴线与边框的间隔大一倍

![image-20210828203651379](http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/image-20210828203651379.png)

```css
.box { 
    align-content: stretch; 
}
```

**stretch（默认值）**：轴线占满整个交叉轴

![image-20210828203842956](http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/image-20210828203842956.png)



****



### 项目的属性

- order
- flex-grow
- flex-shrink
- flex-basis
- flex
- align-self



#### order 属性

- order 属性定义项目的排列顺序
- 数值越小，排列越靠前，默认为0

```css
.item { 
    order: <integer>; 
}
```

![image-20210829151912432](http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/image-20210829151912432.png)



#### flex-grow 属性

- flex-grow 属性定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大
- 如果所有项目的 flex-grow 属性都为1，则它们将等分剩余空间（如果有的话）
- 如果一个项目的 flex-grow 属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍

```css
.item { 
    flex-grow: <number>; /* default 0 */ 
}
```

![image-20210829152218893](http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/image-20210829152218893.png)

- 如果有的项目有 flex-grow 属性，有的项目有 width 属性，有 flex-grow 属性的项目将等分剩余空间

![image-20210829152250382](http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/image-20210829152250382.png)



#### flex-shrink 属性

- flex-shrink 属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小
- 如果所有项目的 flex-shrink 属性都为1，当空间不足时，都将等比例缩小
- 如果一个项目的 flex-shrink 属性为0，其他项目都为1，则空间不足时，前者不缩小
- 负值对该属性无效。

```css
.item { 
    flex-shrink: <number>; /* default 1 */ 
}
```

![image-20210829152404711](http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/image-20210829152404711.png)



#### flex-basis 属性

- flex-basis 属性定义了在分配多余空间之前，项目占据的主轴空间（main size）
- 浏览器根据这个属性，计算主轴是否有多余空间
- 它的默认值为auto，即项目的本来大小

```css
.item { 
    flex-basis: <length>; | auto; /* default auto */ 
}
```

![image-20210829152453552](http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/image-20210829152453552.png)



#### flex 属性

- flex 属性是 flex-grow, flex-shrink 和 flex-basis 的简写，默认值为`0`,  `1`, ` auto`
- 后两个属性可选
- 该属性有两个快捷值：auto (1 1 auto) 和 none (0 0 auto)

```css
.item { 
    flex: none | [ <flex-grow> <flex-shrink>? || <flex-basis> ] 
}
```

![image-20210829154249237](http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/image-20210829154249237.png)



#### align-self 属性

- align-self 属性允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性
- 默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch

```css
.item { 
    align-self: auto | flex-start | flex-end | center | baseline | stretch; 
}
```

![image-20210829154336805](http://yunabell-image-repository.oss-cn-shanghai.aliyuncs.com/img/image-20210829154336805.png)
