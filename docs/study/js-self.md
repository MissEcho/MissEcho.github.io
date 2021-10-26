## html、css

### 1.如何理解css盒子模型

```
css的盒子模型，margin，padding。所有元素都是有对应的宽高。

// 标准的盒子模型：宽度=content+border+padding
低版本IE盒子模型：宽度=内容宽度（content+border+padding）
```

### 2.BFC

```
BFC，block formatting context。格式化上下文。
// 是web页面中盒模型布局的css渲染模式，指一个独立的渲染区域或者说是一个隔离容器。
形成条件是：
1.浮动元素，float除none之外的值。
2.定位元素，position，不会影响其他元素。
3.display为line-block，table-cell，table-caption
4.overflow除了visible（显示）之外的值
特性：
1.内部的box会在垂直方向上，一个接一个的放置。
2.垂直方向上，margin也算上。
3.BFC不会和float元素区域重叠
4.计算bfc的时候，浮动元素也算上
5.页面上独立的容器，而容器内部的元素，不会影响外面的元素。
```

### 3.标签语义化

```
必要性 & 好处：
1.方便dom结构分区。如果通篇都是div，这样的话，在非视觉阅览的情况下，很难准确描述文档内容。
2.减少不必要的css。一般语义标签都有自带的css样式，比如strong，h3等，可以减少一部分css的编写
3.方便维护。对页面的结构描述精准。//代码结构更加清晰
4.见名知意，没有基础的人也知道是干嘛的。
5.方便团队开发，代码可读性更强
6.有利于SEO，爬虫依赖于标签确定上下文关系
```

### 4.css和javascript引入设置

```
css可以通过link文件引入，也可以使用style标签在本地编写。
区别是，本地的会覆盖文件引入的。

javascript的引入，可以通过script src 引入，也可以在本地通过script编写。一般使用cdn加速。
//一般放在文档最后引入，1.因为如果js资源过大，容易造成加载时间过长页面白屏。渲染进程和js进行时互斥的，脚本会阻塞页面的渲染，但是脚本之间是同步进行的，按引入顺序执行。但也有属性控制js的加载和执行。比如 defer，async。2.如果js报错导致页面加载不出来。放文档后问题不大。3.
```

### 5.HTML的块级元素、行内元素、行内块元素的区别是什么

```
块级元素有：div，section，p，h1-6，ul，ol，dl，li，hr，dd，form，table等
行内元素：span，em，i,strong,a
行内块元素，img，input表单元素，一般使用display:block实现。
块级元素，独占一行，宽高生效，默认宽和父级一致，内容撑开高度，margin，padding生效；
行内元素，在当前行内，不会跨行；宽度根据内容来。左右margin生效上下不生效，
行内块元素，也是在当前行内，但是宽度根据width设置的值来确定，在一行排列。margin和爬到顶生效
```

### 6.CSS3的新特性

```
css3的新特性：
border-radius；border-image；border-width；box-shadow；text-shadow；linear-gradient;background;@meia媒体查询，用来解决移动端适配，根据屏幕大小使相应的css生效。

因为css发展到现在，基本上规范都是根据模块来划分的了。比如css grid，css builde等。后面是根据这些模块的版本来命名。后面也不会有css4这类名称了。
css3的新特性，有calc，计算属性。有类似于sass的变量命名；
```

### 7.实现元素隐藏

```
1.display:none;
2.width:0;height:0
3.透明度为0，opacity:0
4.float or position移动到页面边缘
5.z-index隐藏到其他元素下面
6.visibility:hidden,
```

### 8.如何实现元素水平居中

```
1.margin:0 auto;(对块级元素有用)
2.position+left;(已知本身的宽度)
3.text-align:center;(父级元素是block，子元素是行内or 行内块级元素)
4.padding(已知父级和子元素宽度)
5.flex 
6.grid
```

### 9.如何实现元素垂直居中

```
1.flex
2.position+top(已知父子高度)
3.grid
4.line-heigt:height;
5.verticle-align:middle;
```

###  10.position

```
默认是static，
relative，相对定位。不脱离文档流，相对于自身位置进行偏离，不影响本身特性，z-index提高层级。
absolute，脱离文档流，而在相对于父级进行定位。
fixed的时候，是根据视窗进行定位
```

### 11.解释下浮动和原理，如何清除浮动

```
为什么有浮动:因为元素之间的
浮动元素父元素高度自适应（父元素不写高度的时候，子元素写了浮动，父元素会发生高度塌陷。）
因为本来父元素的高度是靠子元素的内容高度来撑起来的，但你写了浮动，半脱离了文档流（占据了该位置，但是对父级没有高度内容，也会影响旁边元素的布局。浮动原本被设计出来的作用是实现文字环绕效果的），就相当于这部分高度没了。所以要清除浮动。
{
	clear:both;
	height:0;
	overflow:hidden;
}

清除浮动：
1.给浮动元素的父级增加高度（防止高度塌陷）
2.父级同时浮动
3.父级设置inline-block，行内块级元素，或者增加overflow:hidden;清除浮动
4.:after清除
```

### 12.css的选择器，优先级是哪些

```
1. 标签
2. #id
3. .class
4. 标签[title=]属性
5. 伪类
6. 通配符
```

### 13.各种布局的优缺点

```
1.grid布局（和flex相似）
2.flex布局（不支持IE8及以下）
3.position布局（有效性和可使用性比较差）
4.float布局（需要手动清除浮动，容易造成高度塌陷）
5.table布局（容易联动更改）
```

### 14.html5的新特性，移除了哪些元素。如何处理html5元素的兼容问题，如何区别html和html5

```
html5的新特性：增加了section标签，

移除元素：
1.纯表现元素：big，center，font，s，strike
2.对可用性产生负面影响的元素：frame，frameset，noframes；

兼容问题：使用优雅降级
比如通过document.createElement方法产生标签，利用这个特性让浏览器支持html5新标签。当然最好是直接使用成熟的框架，使用最多的是html5shim框架。

区别在于头部的DOCTYPE声明、新增的结构元素、功能元素等。
```

### 15.解释盒模型宽高值的计算方式，边界塌陷，负值作用。box-sizing的概念。

```
盒模型的宽高：宽=元素的宽+padding；高=元素的高度+padding
边界塌陷：因为子元素撑不起父级的高度，导致对其他元素的影响。
负值：使用负值让元素往相反方向运动。比如margin:-5px 0 0 0;则表示往上5px挪动
box-sizing:宽=元素的宽+padding+margin;
```

### 16.如何实现浏览器内，多个标签的通信？

```
1.同一个域名下，使用cookie通信；
2.不同域名下，使用ajax通信
```

### 