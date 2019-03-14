### BFC的形成条件和特性分析
> + 原文链接：https://www.jianshu.com/p/8858828987be
> + 原文: https://www.w3cplus.com/css/understanding-bfc-and-margin-collapse.html © w3cplus.com

+ BFC（Block formatting contexts），翻译过来就是块级格式化上下文，指的是一种上下文环境。
+ 如何形成BFC？根据W3C的定义，以下这些元素就会为他们的内容创建一个BFC：
    + 浮动元素
    + 绝对定位元素
    + 非块级盒子的块级容器（例如inline-blocks，table-cells，and table-captions）
    + overflow属性值不是“visible”（visible是overflow的默认值）的块级盒子（视口除外）
+ BFC的通俗理解：
    + 可以理解为一个箱子（实际上是看不见摸不着的），箱子里面物品的摆放是不受外界的影响的。转换为BFC的理解则是：BFC中的元素的布局是不受外界的影响（我们往往利用这个特性来消除浮动元素对其非浮动的兄弟元素和其子元素带来的影响。）
+ BFC的特点：
    + 在一个BFC中，垂直方向上，盒子是从包含块顶部开始一个挨着一个布局的，在一个BFC中的两个相邻的块级盒子的垂直外边距会产生折叠。
    + 在一个BFC中，水平方向上，==每个盒子的左边缘都会接触包含块的左边缘==（从右向左的格式则相反）。
    + 创建了新的BFC的元素与它的子元素的外边距不会折叠，即解决父子元素间的外边距塌陷问题。
##### 外边距塌陷与BFC
+ 产生外边距塌陷的必备条件：**margin必须是邻接的！**
    + 必须是处于==常规文档流==（非float和绝对定位）的==块级盒子==（inline-block不算）,块级盒子的display属性必须是以下三种之一：'block'， 'list-item'， 和 'table'。
    + 没有线盒，没有空隙（clearance，设置clear属性），没有padding和border将他们分隔开
    +  都属于垂直方向上相邻的外边距，可以是下面任意一种情况、
        + 元素的margin-top与其第一个常规文档流的子元素的margin-top
        + 元素的margin-bottom与其下一个常规文档流的兄弟元素的margin-top
        + height为auto的元素的margin-bottom与其最后一个常规文档流的子元素的margin-bottom
        + 高度为0并且最小高度也为0，不包含常规文档流的子元素，并且自身没有建立新的BFC的元素的margin-top和margin-bottom
##### BFC的应用：
+ 1.清除元素之间的影响:eg我们都知道文本会围绕着浮动元素布局，如果我们不想让文本环绕，只想让文本位于右侧，只需要在文本外套一层元素，并且把这个元素变成BFC。如下例子，只需给inner设置`overflow:auto;`即可。
```
<div class="out">
    <div class="f"></div><div class="inner">我是文本我是文本我是文本我是文本我是文本我是文本我是文本
我是文本我是文本我是文本我是文本我是文本我是文本我是文本我是文本我是文本</div></div>
```
+ 2.清除内部浮动元素对父级元素的影响：以下html代码会让浮动的子元素撑出父元素，父元素只会显示成一条直线，要想让父元素包含子元素，只需给父元素设置`overflow:auto;`
```
<div class="out">
    <div class="f"></div>
</div>
```
+ 3.创建自适应布局：如果我们想创建一个两列布局，其中左侧宽度不定，右侧宽度自适应占用剩下的空间，有一种方法就是利用浮动元素和BFC元素的相互作用实现的。
```
HTML
<div class="out">
    <div class="f">浮动元素</div>
    <div class="r"></div>
</div>



CSS
<style type="text/css">
    html,body{
            width: 100%;
            height: 100%;
    }
        .out{
            background: blue;
            width: 100%;
            height: 100%;
        }
        .f{
            float: left;
            margin-right: 20px;
            height: 100%;
            background: red;
        }
        .r{
            overflow: auto;
            height: 100%;
            background: yellow;
        }
    </style>
```
---
### display属性
+ 可能的值：
    + inline:默认。此元素会被显示为内联元素，元素前后没有换行符。
    + block:此元素将显示为块级元素，此元素前后会带有换行符。
    + inline-block:行内块元素
    + none
    + list-item:	此元素会作为列表显示。
    + 




#### overflow属性
+ overflow 属性规定当内容溢出元素框时发生的事情。可能的值：
    + visible:默认值。内容不会被修剪，会呈现在元素框之外。
    + hidden:内容会被修剪，并且其余内容是不可见的。
    + scroll:内容会被修剪，但是浏览器会显示滚动条以便查看其余的内容。
    + auto:	如果内容被修剪，则浏览器会显示滚动条以便查看其余的内容。
    + inherit: 规定应该从父元素继承 overflow 属性的值。

#### background属性
+ 语法：`background:bg-color bg-image position/bg-size bg-repeat bg-origin bg-clip bg-attachment initial|inherit;`
+ background:颜色 图片 起始位置/尺寸 重复性 背景图片的起点从哪个盒子开始 从哪条盒子开始剪切 
    + background-color:默认值为transparent透明的，值可以时rgb。
    + background-position属性设置背景图像的起始位置。注意当这个属性工作在Firefox和Opera，background-attachment必须设置为 "fixed（固定）"。该属性的可能值为`left top`、`left center`、`left bottom`、`right top`、`right center`、`center bottom`、`center top`、`center center`、`center bottom`，如果仅指定一个关键字，其他值将会是"center"。也可以设置成两个百分数。
    + background-attachment设置背景图像是否固定或者随着页面的其余部分滚动。可能值是sroll、fixed和inherit。默认scroll。
    + 最后的inherit表示继承父元素的背景图片。initial 关键字用于设置 CSS 属性为它的默认值。 关键字可用于任何 HTML 元素上的任何 CSS 属性。

#### list-style属性
+ list-style 简写属性在一个声明中设置所有的列表属性。可以设置的属性（按顺序）： list-style-type, list-style-position, list-style-image.
+ list-style-type 属性设置列表项标记的类型。可能值为none、默认disc(实心圆)、circle(空心圆)、square(实心方块)、decimal(数字)、lower-roman(小写罗马数字)、upper-roman、lower-alpha(小写英文字)等等。
+ list-style-position属性表示列表标记的位置。可能的值：
    + inside:列表项目标记放置在文本以内，即在表格里面，且环绕文本根据标记对齐。
    + outside:默认值。保持标记位于文本的左侧。列表项目标记放置在文本以外，即在表格外面，且环绕文本不根据标记对齐。
    + inherit
+ list-style-image 属性使用图像来替换列表项的标记。注意: 请始终规定一个 "list-style-type" 属性以防图像不可用。可能的值：
    + none:默认值
    + url
    + inherit
```
ul
{
list-style-image:url('sqpurple.gif');
}
```

#### text-decoration属性
+ text-decoration 属性规定添加到文本的修饰。注意： 修饰的颜色由 "color" 属性设置。可能的值为：
    + none:默认值
    + underline:	定义文本下的一条线。
    + overline:定义文本上的一条线。
    + line-through:定义穿过文本下的一条线。
    + blink:定义闪烁的文本。
    + inherit

#### CSS3 opacity属性
+ 设置 div 元素的不透明级别，值为从 0.0 （完全透明）到 1.0（完全不透明），或者继承inherit。

##### filter:alpha()属性
+ alpha设置透明度,

它的基本属性是filter：alpha（opacity，finishopacity，style，startX，startY，finishX，finishY）
+ opacity代表透明度数,选值0-100,0是完全透明,100是不透明.
+ style指渐变类型,0是无变化,1是线行渐变,2是放射渐变,3是X型渐变.


---
#### transtion属性
+ 作用：设置四个过渡属性
+ 语法：`transiton: property duration timing-function delay;`默认值`all 0 ease 0`。请始终设置 transition-duration 属性，否则时长为 0，就不会产生过渡效果。
    + `transition-property`:	规定设置过渡效果的 CSS 属性的名称。
    + `transition-duration`:	规定完成过渡效果需要多少秒或毫秒。
    + `transition-timing-function`:规定速度效果的速度曲线。
    + `transition-delay`:	定义过渡效果何时开始。
```
//把鼠标指针放到 div 元素上，其宽度会从 100px 逐渐变为 300px：
div
{
width:100px;
transition: width 2s;
}
```
---
#### px em rem
+ px像素，相对长度单位，相对于显示器屏幕分辨率而言的。
+ em相对长度单位，相对于当前对象内文本的字体尺寸，如当前行内文本的字体尺寸未被人为设置，则相对于浏览器的默认字体尺寸。==em的值并不是固定的，em会继承父级元素的字体大小。==
+ rem(root em根em)，rem为元素设定字体大小时，仍然是相对大小，但相对的只是HTML根元素。

#### css选择器优先级
+ 当CSS选择器权重相同，则最后的声明的CSS选择器覆盖靠前的 CSS。
+ 相同CSS表达式，在DOM结构中的距离是不会对元素优先级计算产生影响的。
+ 网页编写者设置的CSS 样式的优先权高于浏览器所设置的样式。
+ 继承的CSS 样式不如后来指定的CSS 样式
+ 在同一组属性设置中标有“!important”规则的优先级最大。
+ 优先级从小到大：
1. 通用选择器
2. 标签选择器
3. 类class选择器
4. 属性选择器
5. 伪类
6. ID选择器
7. 内联样式
+ 多重样式：优先级为：（外部样式）External style sheet <（内部样式）Internal style sheet <（内联样式）Inline style
+ 有个例外的情况，就是如果外部样式放在内部样式的后面，则外部样式将覆盖内部样式。？？？
+ 选择器的优先权：
1. 内联样式表的权值最高 1000。
2. ID 选择器的权值为 100。
3. Class 类选择器、属性选择器或伪类的权值为 10。
4. HTML 标签选择器的权值为 1
5. 通配选择器对特殊性没有贡献，为0
+ 属性选择器：http://www.w3school.com.cn/css/css_syntax_attribute_selector.asp

#### 浏览器兼容
+ －o－：以Presto为渲染引擎的浏览Opera的私有属性、　
+ －moz－：以Gecko为渲染引擎的浏览器Mozilla　Firefox的私有属性　
+ －webkit－：以Webkit为渲染引擎的浏览器Safari、Google　Chrome的私有属性
+ －ms－：IE8的私有属性的transition：CSS3的过度效果
---
# 慕课网《十天精通CSS3》
+ CSS3把很多以前需要使用图片和脚本来实现的效果、甚至动画效果，只需要短短几行代码就能搞定。比如圆角，图片边框，文字阴影和盒阴影，过渡、动画等。

## 第2章 边框
#### 圆角效果 border-radius
+ 圆角效果border-radius: 5px 4px 3px 2px; /* 四个半径值分别是左上角、右上角、右下角和左下角，顺时针 */ 
+ border-radius的值也可以是百分比或者em，但是目前兼容性不太好
+ 实心上半圆：把高度(height)设为宽度（width）的一半，并且只设置左上角和右上角的半径与元素的高度一致（大于也是可以的）。
```
div{
    height:50px;/*是width的一半*/
    width:100px;
    background:#9da;
    border-radius:50px 50px 0 0;/*半径至少设置为height的值*/
    }
```
+ 实心圆：把宽度（width）与高度(height)值设置为一致（也就是正方形），并且四个圆角值都设置为它们值的一半。如下代码：
```
div{
    height:100px;/*与width设置一致*/
    width:100px;
    background:#9da;
    border-radius:50px;/*四个圆角值都设置为宽度或高度值的一半*/
    }
```
#### 阴影 box-shadow
+ box-shadow: X轴偏移量 Y轴偏移量 [阴影模糊半径] [阴影扩展半径] [阴影颜色] [投影方式];（当只有三个距离量时，第三个参数认为是模糊半径）
    + X轴偏移量：必需。水平阴影的位置。==允许负值==。为负值时反方向，效果大有不同。
    + Y轴偏移量：必需。垂直阴影的位置。允许负值。
    + 阴影模糊半径:可选。其值只能是为正值，如果其值为0时，表示阴影不具有模糊效果，其值越大阴影的边缘就越模糊；
    + 阴影扩展半径：可选，其值可以是正负值，如果值为正，则整个阴影都延展扩大，反之值为负值时，则缩小；
    + 阴影颜色：可选。默认为黑色。
    + 投影方式：可选。默认为外阴影方式。设置inset时为内部阴影方式。inset 可以写在参数的第一个或最后一个，其它位置是无效的。
+ 如果添加多个阴影，只需用逗号隔开即可。
```
.box_shadow{
    box-shadow:4px 2px 6px #f00, -4px -2px 6px #000, 0px 0px 12px 5px #33CC00 inset;
}
```
#### 为边框应用图片 border-image（最好参看这一节的教程，看图片比较好理解）
+ border-image:url(border.png) 70 70 70 70 repeat;
    + 三个参数，第一个为边框图片的地址。
    + 第二个为切割图片的宽度，单位为像素，但不用写Px.也可以用百分比，遵循顺时针的规律来分别设置，四个相等时可以只写一个。
    + 第三个参数为图片延伸方式。三个可选参数分别为：round(平铺)，repeat(重复)，stretch(拉伸)。

## 第三章 CSS3颜色
#### 颜色之RGBA
+ RGBA是在RGB的基础上增加了控制alpha透明度的参数。
+ 语法：`color：rgba(R,G,B,A);` 别漏了中间的逗号。
    + R、G、B三个参数，正整数值的取值范围为：0 - 255。
    + A为透明度参数，取值在0~1之间，不可为负值。

#### 渐变色彩
+ CSS3 Gradient 分为线性渐变(linear)和径向渐变(radial)。
    + 第一个参数:指定渐变方向，可以用“角度”的关键词或“英文”来表示,默认为“180deg”，等同于“to bottom”。
        + to top = 0deg
        + to right = 90deg
        + to bottom = 180deg
        + to left = 270deg
        + to top left:右下角到左上角
        + to top right:左下角到右上角
    + 第二个和第三个参数，表示颜色的起始点和结束点，可以有多个颜色值。
```
background-image:linear-gradient(to left, red, orange,yellow,green,blue,indigo,violet);
```
## 第四章 CSS3文字与字体
#### text-overflow与word-wrap
+ text-overflow用来设置是否使用一个省略标记（...）标示对象内文本的溢出。
    + clip表示剪切
    + ellipsis表示显示省略标志
+ 但是text-overflow只是用来说明文字溢出时用什么方式显示，要实现溢出时产生省略号的效果，还须定义强制文本在一行内显示（white-space:nowrap）及溢出内容为隐藏（overflow:hidden），只有这样才能实现溢出文本显示省略号的效果，代码如下：
```
text-overflow:ellipsis; 
overflow:hidden; 
white-space:nowrap;
```
#### 嵌入字体@font-face
+ @font-face能够加载服务器端的字体文件，让浏览器端可以显示用户电脑里没有安装的字体。
```
@font-face {
    font-family : 字体名称;
    src : 字体文件在服务器上的相对或绝对路径;
}


p {
    font-size :12px;
    font-family : "My Font";
    /*必须项，设置@font-face中font-family同样的值*/
}
```
#### 文本阴影 text-shadow
+ `text-shadow: 0 1px 1px #fff;`
    + X-Offset：表示阴影的水平偏移距离，其值为正值时阴影向右偏移，反之向左偏移； 
    + Y-Offset：是指阴影的垂直偏移距离，如果其值是正值时，阴影向下偏移，反之向上偏移；
    + Blur：是指阴影的模糊程度，其值不能是负值，如果值越大，阴影越模糊，反之阴影越清晰，如果不需要阴影模糊可以将Blur值设置为0；
    + Color：是指阴影的颜色，其可以使用rgba色。

## 第五章 CSS3背景
#### background-origin
```
background-origin ： border-box | padding-box | content-box;
```
+ 设置元素背景图片的原始起始位置。参数分别表示背景图片是从边框最外边，还是内边距（默认值），或者是内容区域开始显示。
+ 需要注意的是，如果背景不是no-repeat，这个属性无效，它会从边框最外边开始显示

#### background-clip
```
background-clip ： border-box | padding-box | content-box | no-clip
```
+ 参数分别表示从边框、或内填充，或者内容区域向外裁剪背景。no-clip表示不裁切，和参数border-box显示同样的效果。backgroud-clip默认值为border-box。

#### background-size
```
background-size: auto | <长度值> | <百分比> | cover | contain
```
+ auto：默认值，不改变背景图片的原始高度和宽度；
+ <长度值>：成对出现如200px 50px，将背景图片宽高依次设置为前面两个值，当设置一个值时，将其作为图片宽度值来等比缩放；
+ <百分比>：0％~100％之间的任何值，将背景图片宽高依次设置为所在元素宽高乘以前面百分比得出的数值，当设置一个值时同上；
+ cover：顾名思义为覆盖，即将背景图片等比缩放以填满整个容器；
+ contain：容纳，即将背景图片等比缩放至某一边紧贴容器边缘为止。

#### mutiple backgrounds
+ 多张背景图片的缩写，缩写时为用逗号隔开的每组值；用分解写法时，如果有多个背景图片，而其他属性只有一个（例如background-repeat只有一个），表明所有背景图片应用该属性值。
```
background ： [background-color]  [background-image]  [background-position][/background-size]  [background-repeat]  [background-attachment]  [background-clip]  [background-origin],设置下一张紧挨着的背景图
```
+ 分解的写法：
```
background-image:url1,url2,...,urlN;
background-repeat : repeat1,repeat2,...,repeatN;
backround-position : position1,position2,...,positionN;
background-size : size1,size2,...,sizeN;
background-attachment : attachment1,attachment2,...,attachmentN;
background-clip : clip1,clip2,...,clipN;
background-origin : origin1,origin2,...,originN;
background-color : color;
```
+ 注意：
1. 如果有 size 值，需要紧跟 position 并且用 "/" 隔开；
2. background-color 只能设置一个。

## 第六章 CSS3选择器
#### 属性选择器
+ E[att^="val"]：拥有以val开头的某属性
+ E[att$="val"]:以val结尾
+ E[att*="val"]:包含val的属性。例如
```
a[title*=more]{
  background: blue;
  color: #fff;
}
```
#### 结构性伪类选择器：root
+ `:root`选择器，根选择器，在HTML文档中，根元素始终是<html>。“:root”选择器等同于<html>元素。建议使用:root方法。
```
:root{background:orange}

等同于

html {background:orange;}
```
#### 结构性伪类选择器-not
+ `:not`选择器称为否定选择器，和jQuery中的:not选择器一模一样，可以选择除某个元素之外的所有元素。就拿form元素来说，比如说你想给表单中除submit按钮之外的input元素添加红色边框，CSS代码可以写成：
```
form {
  width: 200px;
  margin: 20px auto;
}
div {
  margin-bottom: 20px;
}
input:not([type="submit"]){
  border:1px solid red;
}
```
#### 结构性伪类选择器--empty
+ :empty选择器表示的就是空。用来选择没有任何内容的元素，这里没有内容指的是一点内容都没有，哪怕是一个空格.如
```
p:empty {
  display: none;
}
```
#### 结构性伪类选择器--target
+ 触发元素的URL中的标志符通常会包含一个#号，后面带有一个==标志符名称==，上面代码中是：#brand。：target就是用来匹配id为“brand”的元素（id="brand"的元素）,上面代码中是那个div元素。
+ 也就是说，触发元素和被触发元素的标志符应该是相同的。
+ 当只有一个url一个target时，:target前面可以省略标识符，当有多个url和多个target时，在前面加上#标识符即可对不同的target对象分别设置不的样式。
```
#brand:target {
  background: orange;
  color: #fff;
}
#jake:target {
  background: blue;
  color: #fff;
}
```
#### 结构性伪类选择器--first-child、last-child
+ “:first-child”选择器表示的是选择父元素的第一个子元素的元素E。简单点理解就是选择元素中的第一个==子元素==，记住是子元素，而不是后代元素。
```
ol > li:first-child{
  color: red;
}
```
#### 结构性伪类选择器--nth-child(n)、nth-last-child(n)
+ “:nth-child(n)”选择器用来定位某个父元素的一个或多个特定的子元素。其中“n”是其参数，而且可以是整数值(1,2,3,4)，也可以是表达式(2n+1、-n+5)和关键词(odd、even)，但参数n的起始值始终是1，而不是0。也就是说，参数n的值为0时，选择器将选择不到任何匹配的元素。
+ 当“:nth-child(n)”选择器中的n为一个表达式时，其中n是从0开始计算，当表达式的值为0或小于0的时候，不选择任何匹配的元素。
```
ol > li:nth-child(2n){
  background: orange;
}
```
+ `:nth-last-child(n)`表示倒数第几项。

#### 结构性伪类选择器：first-of-type
+ 用来定位一个父元素下的某个类型的第一个子元素。
+ 如果某元素并不是第一个子元素，这时候用first-child是没有效果的，只能用first-of-type
```
.wrapper > p:first-of-type {//p不一定是.wrapper的第一个子元素
  background: orange;
}
```
#### 结构性伪类选择器：nth-of-type(n)
+ 当某个元素中的子元素不单单是同一种类型的子元素时，使用“:nth-of-type(n)”选择器来定位于父元素中某种类型的子元素
+ 在“:nth-of-type(n)”选择器中的“n”和“:nth-child(n)”选择器中的“n”参数也一样，可以是具体的整数，也可以是表达式，还可以是关键词。
```
.wrapper > p:nth-of-type(2n){
  background: orange;
}
```
#### last-of-type选择器
+ 选择父元素下的某个类型的最后一个子元素。

#### nth-last-of-type(n)
+ 选择父元素中指定的某种子元素类型，倒数。

#### only-child选择器
+ `only-child`匹配的元素的父元素中仅有一个子元素，而且是一个唯一的子元素。

#### only-of-type选择器
+ 表示一个元素他有很多个子元素，而其中只有一种类型的子元素是唯一的，使用“:only-of-type”选择器就可以选中这个元素中的唯一一个类型子元素。

#### enabled选择器、disabled
+ 在Web的表单中，有些表单元素有可用（“:enabled”）和不可用（“:disabled”）状态，比如输入框，密码框，复选框等。在默认情况之下，这些表单元素都处在可用状态。那么我们可以通过伪选择器“:enabled”对这些表单元素设置样式。
```
<input type="text" name="name" id="name" placeholder="可用输入框"  />
<input type="text" name="name" id="name" placeholder="禁用输入框"  disabled="disabled" />

input[type="text"]:enabled {
  background: #ccc;
  border: 2px solid red;
}
input[type="text"]:disabled {
  background: rgba(0,0,0,.15);
  border: 1px solid rgba(0,0,0,.15);
  color: rgba(0,0,0,.15);
}
```
#### :checked选择器
+ 在表单元素中，单选按钮和复选按钮都具有选中和未选中状态。“:checked”表示的是选中状态。
```
<input type="checkbox" checked="checked" id="usename" /><span>√</span>
<input type="checkbox"  id="usepwd" /><span>√</span>

input[type="checkbox"]:checked + span {
  opacity: 1;
}
input[type="checkbox"] + span {
  opacity: 0;
}
```
#### ::selection选择器
+ 浏览器默认情况下，用鼠标选择网页文本是以“深蓝的背景，白色的字体”显示的，有的时候设计要求,不使用上图那种浏览器默认的突出文本效果，此时“::selection”伪元素就非常的实用。不过在Firefox浏览器还需要添加前缀。、
+ IE9+、Opera、Google Chrome 以及 Safari 中支持 ::selection 选择器。Firefox 支持替代的 ::-moz-selection。
```
::-moz-selection {
  background: red;
  color: green;
}
::selection {
  background: red;
  color: green;
}
```
#### :read-only选择器
+ :read-only用来指定处于只读状态元素的样式，该元素设置了`readonly="readonly"`
```
<input type="text" name="address" id="address" placeholder="中国上海" readonly="readonly" />

input[type="text"]:-moz-read-only{
  border-color: #ccc;
}
input[type="text"]:read-only{
  border-color: #ccc;
}
```
#### read-white选择器
+ :read-write选择器刚好与“:read-only”选择器相反，主要用来指定当元素处于非只读状态时的样式。

#### ::before和::after
+ ::before和::after这两个主要用来给元素的前面或后面插入内容，这两个常和"content"配合使用，使用的场景最多的就是清除浮动。
```
.clearfix::before,
.clearfix::after {
    content: ".";
    display: block;
    height: 0;
    visibility: hidden;
}
.clearfix:after {clear: both;}
.clearfix {zoom: 1;}
```
+ 想看这个知识点深入讲解的小伙伴请观看《css3实现图片阴影效果》中的第1-6小节。
+ 一个冒号是CSS2的写法，两个冒号是CSS3的写法，建议用两个冒号。

#### 插播一则清除浮动的知识点
+ 为什么要清除浮动？如果没有给外层的div设置高度，那么如果他里面的元素不浮动的话，这个外层的高会被自动撑开。当内层元素设置了浮动之后，就出现了一些影响:
1. 背景不能显示
2. 高度不能被撑开
3. margin设置值不能正确显示
+ 清除浮动有三种常见的方法：
    + 添加一个空的内层div元素，并令其 clear:both;
    + 给外层div设置：overflow:auto;（因为是应用了.clearfix和.menu的菜单极有可能是多级的，所以overflow: hidden或overflow: auto也不满足需求（会把下拉的菜单隐藏掉或者出滚动条））
    + 为外层div设置:after方法
+ 终极版一：
```
.clearfix:after { 
    content:"\200B"; 
    display:block; 
    height:0; 
    clear:both; 
} 
.clearfix {*zoom:1;}/*IE/7/6*/
```
content:"\200B";这个参数，Unicode字符里有一个“零宽度空格”，即 U+200B，代替原来的“.”，可以缩减代码量。而且不再使用visibility:hidden。
+ 终极版二：
```
.clearfix:before,.clearfix:after{ 
    content:""; 
    display:table; 
} 
.clearfix:after{clear:both;} 
.clearfix{ 
    *zoom:1;/*IE/7/6*/
}
```
## 第八章 CSS3变形与动画
+ rotate()、skew()、scale()、translate()、matrix()前面要用transform作为属性，而transition本身就是一个属性，不用再加transform。

#### rotate
```
.wrapper div {
  width: 200px;
  height: 200px;
  background: orange;
  -webkit-transform: rotate(45deg);
  transform: rotate(45deg);
}
```
+ 如果角度值为正值，元素相对中心顺时针旋转；如果角度值为负值，元素相对原点中心逆时针旋转。这个中心原点一般是对称中心。div旋转之后，它包含的span子元素会一起旋转。

#### 扭曲skew()
+ 扭曲skew()函数能够让元素倾斜显示。它可以将一个对象以其中心位置围绕着X轴和Y轴按照一定的角度倾斜。rotate()函数只是旋转，而不会改变元素的形状。skew()函数不会旋转，而只会改变元素的形状。
+ skew(x,y)使元素在水平和垂直方向同时扭曲（X轴和Y轴同时按一定的角度值进行扭曲变形）；第一个参数对应X轴，第二个参数对应Y轴。如果第二个参数未提供，则值为0，也就是Y轴方向上无斜切。
+ skewX(x)仅使元素在水平方向扭曲变形（X轴扭曲变形）；skewY(y)仅使元素在垂直方向扭曲变形（Y轴扭曲变形）
```
-webkit-transform: skew(45deg);
  -moz-transform:skew(45deg) 
  transform:skew(45deg);
```
#### 缩小放大scale()
+ scale()函数 让元素根据中心原点对对象进行缩放。scale(X,Y)使元素水平方向和垂直方向同时缩放（也就是X轴和Y轴同时缩放）。Y是一个可选参数，如果没有设置Y值，则表示X，Y两个方向的缩放倍数是一样的。
+ scaleX(x)元素仅水平方向缩放（X轴缩放），scaleY(y)元素仅垂直方向缩放（Y轴缩放）。
+ scale()的取值默认的值为1，当值设置为0.01到0.99之间的任何值，作用使一个元素缩小；而任何大于或等于1.01的值，作用是让元素放大。
```
div:hover {
  -webkit-transform: scale(1.5,0.5);
  -moz-transform:scale(1.5,0.5)
  transform: scale(1.5,0.5);
}
```
#### 位移translate()
+ 使用translate()函数，可以把元素从原来的位置移动，而不影响在X、Y轴上的任何Web组件。
    + translate(x,y)水平方向和垂直方向同时移动（也就是X轴和Y轴同时移动）
    + translateX(x)仅水平方向移动（X轴移动）
    + translateY(Y)仅垂直方向移动（Y轴移动）
```
-webkit-transform: translate(50px,100px);
  -moz-transform:translate(50px,100px);
  transform: translate(50px,100px);
```
+ translate可以用来让==不知道宽度和高度的元素实现水平、垂直居中(相对于父元素）==。
```
.wrapper {
  padding: 20px;
  background:orange;
  color:#fff;
  position:absolute;  //设为绝对定位
  top:50%; 
  left:50%;//这时子DIV的边框左上角会移动到父级元素（一般为body)的中央，而不是DIV的中心与父级元素的中心重合
  border-radius: 5px;
  -webkit-transform:translate(-50%,-50%);//translate(x,y)括号里填百分比数据的话，会以本身的长宽做参考
  -moz-transform:translate(-50%,-50%);
  transform:translate(-50%,-50%);
}
```
#### 矩阵matrix()
+ matrix() 是一个含六个值的(a,b,c,d,e,f)变换矩阵，用来指定一个2D变换，相当于直接应用一个[a b c d e f]变换矩阵。就是基于水平方向（X轴）和垂直方向（Y轴）重新定位元素,如果需要深入了解，需要对数学矩阵有一定的知识。
+ 通过matrix()函数来模拟transform中translate()位移的效果。
```
-webkit-transform: matrix(1,0,0,1,50,50);
  -moz-transform:matrix(1,0,0,1,50,50);
  transform: matrix(1,0,0,1,50,50);
```
#### transform-style
+ transform--style属性指定嵌套元素是怎样在三维空间中呈现。使用此属性必须先使用 transform 属性.
+ 语法：`transform-style: flat|preserve-3d;`
    + flat:表示所有子元素在2D平面呈现。
    + preserve-3d:表示所有子元素在3D空间中呈现。


#### 原点 transform-origin
+ 在没有重置transform-origin改变元素原点位置的情况下，CSS变形进行的旋转、位移、缩放，扭曲等操作都是以元素自己中心位置进行变形。但很多时候，我们可以通过transform-origin来对元素进行原点位置改变，使元素原点不在元素的中心位置，以达到需要的原点位置。
+ transform-origin取值和元素设置背景中的background-position取值类似，如下表所示：
    + top = top center = center top = 50% 0
    + right = right center = center right = 100% 或 （100% 50%）
    + bottom = bottom center = center bottom = 50% 100%
    + left = left center = center left = 0 或 （0 50%）
    + center = center center = 50% 或 （50% 50%)
    + top left = left top = 0 0(00指的是左上角，其他的百分数就好记了)
    + top right = right top = 100% 0
    + bottom right = right bottom = 100% 100%
    + bottom left = left bottom = 0 100%
```
div {
  -webkit-transform-origin: left top;
  transform-origin: left top;
}
```
#### 过渡transition
+ 通过鼠标的单击、获得焦点，被点击或对元素任何改变中触发，并平滑地以动画效果改变CSS的属性值。
+ 在CSS中创建简单的过渡效果可以从以下几个步骤来实现：
    + 第一，在默认样式中声明元素的初始状态样式；
    + 第二，声明过渡元素最终状态样式，比如悬浮状态；
    + 第三，在默认样式中通过添加过渡函数，添加一些不同的样式。
+ transition属性是一个复合属性，主要包括以下几个子属性：
    + transition-property:指定过渡或动态模拟的CSS属性
    + transition-duration:指定完成过渡所需的时间
    + transition-timing-function:指定过渡函数
    + transition-delay:指定动画开始执行的时间
+ transition-property用来指定过渡动画的CSS属性名称，而这个过渡属性只有具备一个中点值的属性（需要产生动画的属性）才能具备过渡效果，其对应具有过渡的CSS属性主要有：跟长度、形状、位置、颜色、透明度、阴影缩进、可见性等有关的属性。
+ 当“transition-property”属性设置为all时：假设你的初始状态设置了样式“width”,“height”,“background”,当你在终始状态都改变了这三个属性，那么all代表的就是“width”、“height”和“background”。如果你的初始状态只改变了“width”和“height”时，那么all代表的就是“width”和“height”。
```
//起始状态
div {
  width: 200px;
  height: 200px;
  background: red;
  margin: 20px auto;
  -webkit-transition: width,background;
  transition: width,background;
  -webkit-transition-duration:.5s;
  transition-duration:.5s;
  -webkit-transition-timing-function: ease-in;
  transition-timing-function: ease-in;
  -webkit-transition-delay: .18s;
  	transition-delay:.18s;
}
//终止状态
div:hover {
  width: 400px;
  background:blue;
}
```
+ 缩写的形式：
```
div {
  width: 200px;
  height: 200px;
  background-color:red;
  margin: 20px auto;
  -webkit-transition: background-color .5s ease .1s;
  transition: background-color .5s ease .1s;
}
div:hover {
  background-color: orange;
}
```
+ transition-timing-function和animation-timing-function属性指的是过渡的“缓动函数”。主要用来指定浏览器的过渡速度，以及过渡期间的操作进展情况。包括五种函数：
    + ease:默认值。速度由快到慢，逐渐变慢。
    + linear:恒速。
    + ease-in:速度越来越快，呈一种加速状态。常称渐显效果
    + ease-out:减速状态。常称渐隐效果
    + ease-in-out:先加速再减速。常称渐显渐隐效果。
```
 -webkit-transition-timing-function: ease-in-out;
  transition-timing-function: ease-in-out;
```
![image](http://www.w3cplus.com/sites/default/files/transition-timing-function.png)
+ 改变两个或者多个css属性的transition效果时，只要把几个transition的声明串在一起，用逗号（“，”）隔开，然后各自可以有各自不同的延续时间和其时间的速率变换方式。第一个时间的值为transition-duration，第二个为transition-delay。

#### keyframs关键帧
+ 在一个“@keyframes”中的样式规则可以由多个百分比构成的，如在“0%”到“100%”之间创建更多个百分比，分别给每个百分比中给需要有动画效果的元素加上不同的样式，从而达到一种在不断变化的效果。其中0%和100%还可以使用关键词from和to来代表。
+ Chrome 和 Safari 需要前缀 -webkit-；Foxfire 需要前缀 -moz-。
+ transition的第一个参数是transition-property，是一些固定的，如transform等，不能是指定的动画名。
+ 通过“@keyframes”声明一个名叫“wobble”的动画，从“0%”开始到“100%”结束，同时还经历了一个“40%”和“60%”两个过程：
```
@keyframes wobble {
  0% {
    margin-left: 100px;
    background:green;
  }
  40% {
    margin-left:150px;
    background:orange;
  }
  60% {
    margin-left: 75px;
    background: blue;
  }
  100% {
    margin-left: 100px;
    background: red;
  }
}
div {
  width: 100px;
  height: 100px;
  background:red;
  color: #fff;
}
div:hover{
  animation: wobble 5s ease .1s;
}
```
#### CSS3中调用动画
```
-webkit-animation-name:around;
  animation-duration: 10s;
  animation-timing-function: ease;
  animation-delay: 1s;
  animation-iteration-count:infinite;
```
+ animation-name属性主要是用来调用 @keyframes定义好的动画。需要特别注意: animation-name 调用的动画名需要和“@keyframes”定义的动画名称完全一致（区分大小写），如果不一致将不具有任何动画效果。
+ none为默认值，当值为none时，将没有任何动画效果,这可以用于覆盖任何动画。注意：需要在 Chrome和Safari上面的基础上加上-webkit-前缀，Firefox加上-moz-。
+ animation-duration主要用来设置CSS3动画播放时间，也就是完成从0%到100%一次动画所需时间。单位：S秒。其默认值为“0”，也就是没有动画效果（如果值为负值会被视为“0”）。
+ animation-timing-function和transition-timing-function是一模一样的。
+ animation-delay属性用来定义动画开始播放的时间，用来触发动画播放的时间点。和transition-delay属性一样。只在第一帧之前等待，中间不等。
+ animation-iteration-count属性主要用来定义动画的播放次数。其值通常为整数，但也可以使用带有小数的数字，其默认值为1，这意味着动画将从开始到结束只播放一次。如果取值为infinite，动画将会无限次的播放。
+ animation-direction属性主要用来设置动画播放方向，其主要有两个值：normal、alternate
    + normal是默认值，如果设置为normal时，动画的每次循环都是向前播放；
    + 另一个值是alternate，他的作用是，动画播放在第偶数次向前播放，第奇数次向反方向播放。
```
animation-direction:alternate;
```
+ animation-play-state属性主要用来控制元素动画的播放状态。其主要有两个值：running和paused。其中running是其默认值，主要作用就是类似于音乐播放器一样，可以通过paused将正在播放的动画停下来，也可以通过running将暂停的动画重新播放，这里的重新播放不一定是从元素动画的开始播放，而是从暂停的那个位置开始播放。
+ 让停止的动画在hover的时候播放，不是hover状态停止。
```
span {
  animation-play-state:paused;
}

div:hover span {
  animation-play-state:running;
}   
```
#### 设置动画时间外属性animation-fill-mode：
+ animation-fill-mode属性定义在动画开始之前和结束之后发生的操作。主要具有四个属性值：none、forwards、backwords和both。
    + none:默认值。表示动画在完成最后一帧时，会返回到初始帧处，animation-delay用于在原背景和第一帧之间的切换。
    + forwards：表示动画在结束后停在最后的关键帧的位置。
    + backwards:会在==向元素应用动画样式时==迅速应用动画的初始帧，animation用于第一帧到第二帧之间的切换。
    + both:同时具有forwards和backwards效果。

## 第10章 布局样式相关
#### CSS3多列布局columns
+ 多列布局columns属性参数主要就两个属性参数：列宽和列数。
```
columns: 200px 2;
```
+ 当column-width:auto;或没有显式设置时，元素多列的列宽将由其他属性来决定，比如前面的示例就是由列数column-count来决定。
+ column-count的默认值为auto，表示元素只有一列，其主要依靠浏览器计算自动设置。
+ column-gap主要用来设置列与列之间的间距，默认值为normal,即1em（如果你的字号是px，其默认值为你的font-size值）。
```
column-count: 3;
column-gap: 2em;
```
+ column-rule主要是用来定义列与列之间的边框宽度、边框样式和边框颜色。column-rule是不占用任何空间位置的，在列与列之间改变其宽度不会改变任何列的位置。
    + column-rule-width:主要用来定义列边框的宽度，其默认值为“medium”，column-rule-width属性接受任意浮点数，但不接收负值。但也像border-width属性一样，可以使用关键词：medium、thick和thin。
    + column-rule-style:用来定义列边框样式，其默认值为“none”。包括none、hidden、dotted、dashed、solid、double、groove、ridge、inset、outset。
    + column-rule-color:默认值为前景色color的值，如果不希望显示颜色，也可以将其设置为transparent(透明色)
```
column-rule: 2px dotted green;
```
+ 跨列设置column-span:有时需要将一段内容或一个标题不进行分列，也就是横跨所有列。column-span只有两个值：
    + none:此值为column-span的默认值，表示不跨越任何列。
    + all: 表示的是元素跨越所有列，并定位在列的Ｚ轴之上。
```
h2,p:nth-child(2n){
  -webkit-column-span:all;
}
```
#### CSS3盒子模型
+ 在CSS中盒模型被分为两种，第一种是w3c的标准模型，另一种是IE的传统模型，它们相同之处都是对元素计算尺寸的模型，具体说不是对元素的width、height、padding和border以及元素实际尺寸的计算关系，它们不同之处是两者的计算方法不一致，原则上来说盒模型是分得很细的。
+ W3C标准盒模型
    + 外盒尺寸计算（元素空间尺寸）：element空间高度＝内容高度＋内距＋边框＋外距。宽度同理。
    + 内盒尺寸计算（元素大小）：element高度＝内容高度＋内距＋边框（height为内容高度）。宽度同理。
+ IE传统下盒模型（IE6以下，不包含IE6版本或”QuirksMode下IE5.5+”）
    + 外盒尺寸计算（元素空间尺寸）：element空间高度＝内容高度＋外距（height包含了元素内容宽度、边框、内距）
    + 内盒尺寸计算（元素大小）：element高度＝内容高度（height包含了元素内容宽度、边框、内距）
+ 在CSS3中新增加了box-sizing属性，能够事先定义盒模型的尺寸解析方式。其语法规则如下：`box-sizing: content-box | border-box | inherit`
    + content-box:默认值，其让元素维持W3C的标准盒模型.
    + border-box:重新定义CSS2.1中盒模型组成的模式，让元素维持IE传统的盒模型（IE6以下版本和IE6-7怪异模式）。
    + inherit:使元素继承父元素的盒模型模式
![image](http://img.mukewang.com/5365d98000018fa606460416.jpg)
+ 在自适应布局当中，在元素基础上添加内距padding，按照标准盒模型解析，往往会将布局撑破，但使用box-sizing的border-box值，可以让你轻松完成。

#### CSS3伸缩布局(参见W3Cplus《图解CSS3flexbox》的教程)
+ Flexbox布局常用于设计比较复杂的页面，可以轻松的实现屏幕和浏览器窗口大小发生变化时保持元素的相对位置和大小不变，同时减少了依赖于浮动布局实现元素位置的定义以及重置元素的大小。
1. 创建一个flex容器：`display:flex | inline-flex;`。这属性只要设置在父容器上，其所有子元素将自动成为Flex项目。著作权归作者所有。
2. Flex项目显示：`flex-direction:row | column;`。默认值为row横向排列。column为垂直排列方式。
3. Flex项目移动到顶部和左边:垂直排列时设置`align-items:flex-start;`水平排列时设置`justify-content:flex-start;`
4. Flex项目移动到右边：垂直排列时：`align-items:flex-end;`水平排列时：`justify-content:flex-end;`
5. 水平垂直居中，设置justify-content或者align-items为center。`align-items:center; justify-content:center;`单独设置align-items只实现垂直居中，单独设置justity-content只实现水平居中。
6. Flex项目实现自动伸缩。看不懂？？/
```
.bigitem{ -webkit-flex:200; flex:200; }  .smallitem{ -webkit-flex:100; flex:100; }
```
+ flexbox属性分成两个组：flex容器和flex项目。
###### flex容器属性
+ flex-direction:row|row-reverse|column|column-reverse。设为row-reverse时，flex项目在rtl上下文中从右向左排在一行
+ flex-wrap:flex项目在flex容器中默认是只显示一行,即nowrap。\
    + flex-wrap:wrap;Flex项目从左到右和从上到下在flex容器中多行显示。
    + flex-wrap: wrap-reverse;Flex项目从左到右和从下到上在flex容器中多行显示.
+ flex flow是flex-direction和flex-wrap属性的简写。
```
flex-flow: <flex-direction> || <flex-wrap>;
```
+ justify-content:指定flex项目在flex容器沿着主轴在当前行的对齐方式。默认值flex-start。
    + flex-start、flex-end、center上文中有提及
    + space-around:Flex项目之间间距相对，第一个和最后一个flex项目向flex容器的边缘对齐。
+ align-items:这个属性可以设置所有flex项目对齐方式，并且包括匿名元素。默认值为stretch。属性值如下：
    + stretch:Flex项目沿着flex容器侧轴方向填满整个flex容器高度（或宽度）。也就是每个项目的高度都跟容器高度一样？
    + flex-start沿着顶边排列、flex-end沿着底边排列、center
    + baseline:Flex项目按文本基线在flex容器侧轴中排列。也就是文本的结束线与侧轴中重合。
+ align-content:Flex项目在flex容器中多行显示行，其多行在flex容器的侧轴方向对齐方式。默认值为stretch。(看图好理解)属性值如下：
    + stretch:Flex项目行在flex容器侧轴按分布式空间排列，
    + flex-start、flex-end、center多行flex项目中间没有大的空格，只有多行之外的上方或者下方有空白。
    + space-between:Flex项目行与行之间间距相等，并且flex项目行第一行排在flex容器侧轴开始之处，flex项目行最后一行排在flex容器侧轴末尾之处。
    + space-around:Flex项目行上下间距相等，并且flex容器第一行距flex容器侧轴开始处间距是flex项目行与行之间间距一半。同时项目行最后一行距flex容器侧轴末尾处间距是flex项目行与行之间间距一半。
+ 所有column-*属性在flex容器上都不生效。
##### flex项目属性
+ order:order属性是用来控制flex容器中flex项目的排列顺序。默认情况flex项目在flex容器的顺序是flex项目出现的顺序。Flex项目可以使用这个简单的属性重新排序。其值一般为整数,可正可负。默认值为0.
+ flex-grow:这个属性用来指定flex项目的放大比例，如果所有flex项目的flex-grow值相同，那么flex项目在flex容器中具有相同的尺寸。默认值为0，负数无效。数值大的flex项目占更多的空间。
+ flex-shrink:用来指定flex项目缩小比例，默认值为1。默认情况之下，所有flex项目都可以收缩，但如果将它们设置为0时，他们不会缩小会保持原来的大小。
+ flex-basis:这个属性和width和height属性相同，用来指定flex项目的大小。默认值auto。
+ flex:这个属性是flex-grow、flex-shrink和flex-basis属性的简写。其他值可以设置为auto(1 1 auto)和none(0 0 auto)。默认值0 1 auto。W3C鼓励使用简写方式，而不是使用单独的属性。
+ align-self:指定单个flex项目自身的对齐方式，覆盖其默认的对齐方式。默认值为auto。align-self取值为auto值时，flex项目对齐方式会根据其父元素align-items来决定。
+ float，clear和vertical-align属性应用在flex项目上将会无效。

## media queries与responsive设计
#### media queries使用方法
+ 媒体类型（Media Type）常碰到的就是all(全部)、screen(屏幕)、print(页面打印或打印预览模式)，其实媒体类型远不止这三种，W3C总共列出了10种媒体类型。
+ 媒体类型的引用方法也有多种，常见的有：link标签、@import和CSS3新增的@media几种：
1. link方法引入媒体类型其实就是在<link>标签引用样式的时候，通过link标签中的media属性来指定不同的媒体类型。如下所示。
```
<link rel="stylesheet" type="text/css" href="style.css" media="screen" />
<link rel="stylesheet" type="text/css" href="print.css" media="print" />
```
2. @import可以引用样式文件，同样也可以用来引用媒体类型。@import引入媒体类型主要有两种方式，一种是在样式中通过@import调用另一个样式文件；另一种方法是在<head></head>标签中的<style></style>中引入，但这种使用方法在IE6~7都不被支持.
```
@importurl(reset.css) screen;   
@importurl(print.css) print;

<head>
<style type="text/css">
    @importurl(style.css) all;
</style>
</head>
```
3. @media引入媒体类型和@import有点类似也具有两方式。
```
（1）在样式文件中引用媒体类型：
@media screen {
   选择器{/*你的样式代码写在这里…*/}
}

（2）使用@media引入媒体类型的方式是在<head>标签中的<style>中引用。
<head>
<style type="text/css">
    @media screen{
    选择器{/*你的样式代码写在这里…*/}
}
</style>
</head>
```
+ media queries的使用规则：
```
@media 媒体类型and （媒体特性）{你的样式}
```
+ 当屏幕在600px~900px之间时，body的背景色渲染为“#f5f5f5”，如下所示。
```
@media screen and (min-width:600px) and (max-width:900px){
  body {background-color:#f5f5f5;}
}
```
+ “max-device-width”所指的是设备的实际分辨率，也就是指可视面积分辨率。
```
<link rel="stylesheet" media="screen and (max-device-width:480px)" href="iphone.css" />
```
+ not关键词用来排除某种特定的媒体类型。
```
@media not print and (max-width: 1200px){样式代码}
```
+ only关键词可以用来排除不支持媒体查询的浏览器。支持媒体特性的设备，正常调用样式，此时就当only不存在；表示不支持媒体特性但又支持媒体类型的设备，这样就会不读样式，因为其先会读取only而不是screen；另外不支持Media Queries的浏览器，不论是否支持only，样式都不会被采用。
```
<linkrel="stylesheet" media="only screen and (max-device-width:240px)" href="android240.css" />
```
+ 在Media Query中如果没有明确指定Media Type，那么其默认为all。
+ 另外在样式中，还可以使用多条语句来将同一个样式应用于不同的媒体类型和媒体特性中，指定方式如下所示。
```
<linkrel="stylesheet" type="text/css" href="style.css" media="handheld and (max-width:480px), screen and (min-width:960px)" />
```
上面代码中style.css样式被用在宽度小于或等于480px的手持设备上，或者被用于屏幕宽度大于或等于960px的设备上。

#### Responsive设计 响应式设计
+ 流体网格：将每个网格格子使用百分比单位来控制网格大小。这种网格系统最大的好处是让你的网格大小随时根据屏幕尺寸大小做出相对应的比例缩放。
+ 弹性图片：指的是不给图片设置固定尺寸，而是根据流体网格进行缩放，用于适应各种网格的尺寸。而实现方法是比较简单，一句代码就能搞定的事情。
```
img {max-width:100%;}
```
+ 不幸的是，这句代码在IE8浏览器存在一个严重的问题，让你的图片会失踪。
+ 屏幕分辨率：屏幕分辨简单点说就是用户显示器的分辨率，深一点说，屏幕分辨率指的是用户使用的设备浏览您的Web页面时的显示屏幕的分辨率，
+ 主要断点：就是设备宽度的临界值。
+ 布局技巧：
    + 第一， 尽量少用无关紧要的div；
    + 第二，不要使用内联元素（inline）；
    + 第三，尽量少用JS或flash；
    + 第四，丢弃没用的绝对定位和浮动样式；
    + 第五，摒弃任何冗余结构和不使用100%设置。
+ 在响应式设计中如果没有这个meta标签，你就是蹩脚的，响应式设计就是空谈。meta标签被称为可视区域meta标签。
```
<meta name=”viewport” content=”” />
```
+ 在content属性中主要包括以下属性值，用来处理可视区域。
![image](http://img.mukewang.com/53660f2c0001190005270386.jpg)
+ 在实际项目中，为了让Responsive设计在智能设备中能显示正常，也就是浏览Web页面适应屏幕的大小，显示在屏幕上，可以通过这个可视区域的meta标签进行重置，告诉他使用设备的宽度为视图的宽度，也就是说禁止其默认的自适应页面的效果，具体设置如下：
```
<meta name=”viewport” content=”width=device-width,initial-scale=1.0” />
```
#### 不同设备的分辨率设置
+ 1024显屏、800px显屏、640px显屏
```
@media screen and (max-width : 1024px) {                    
/* 样式写在这里 */          
}  
```
+ iPad横板显屏
```
@media screen and (max-device-width: 1024px) and (orientation: landscape) {              
/* 样式写在这 */            
}  
```
+ iPad竖板显屏
```
@media screen and (max-device-width: 768px) and (orientation: portrait) {         
/* 样式写在这 */            
}   
```
+ iPhone 和 Smartphones
```
@media screen and (min-device-width: 320px) and (max-device-width: 480px) {              
/* 样式写在这 */            
}
```
+ portrait：竖屏,landscape：横屏

## 用户界面与其他重要属性
#### 自由缩放属性 resize
+ 它允许用户通过拖动的方式来修改元素的尺寸来改变元素的大小。
    + none: 用户不能拖动元素修改尺寸大小。
    + both: 用户可以拖动元素，同时修改元素的宽度和高度
    + horizontal:用户可以拖动元素，仅可以修改元素的宽度，但不能修改元素的高度。
    + vertical: 用户可以拖动元素，仅可以修改元素的高度，但不能修改元素的宽度。
    + inherit:继承父元素的resize属性值。
```
resize: none | both | horizontal | vertical | inherit
```
#### CSS3外轮廓属性
+ 外轮廓outline在页面中呈现的效果和边框border呈现的效果极其相似，但和元素边框border完全不同，外轮廓线不占用网页布局空间，不一定是矩形。
```
outline: ［outline-color］ || [outline-style] || [outline-width] || [outline-offset] || inherit
```
+ outline和border边框属性的使用方法极其类似。outline-color相当于border-color、outline-style相当于border-style，而outline-width相当于border-width.
    + outline-color:可以将此参数省略，省略时此参数的默认值为黑色。
    + outline-style:可以将此参数省略，省略时此参数的默认值为none，省略后不对该轮廓线进行任何绘制。
    + outline-width:可以将此参数省略，省略时此参数的默认值为medium，表示绘制中等宽度的轮廓线。
    + outline-offset:定义轮廓边框的偏移位置的数值，此值可以取负数值。当此参数的值为正数值，表示轮廓边框向外偏离多少个像素；当此参数的值为负数值，表示轮廓边框向内偏移多少个像素。
+ border 可应用于几乎所有有形的html元素,而outline是针对链接、表单控件和ImageMap等元素设计。从而另一个区别也可以推理出,那就是: outline 的效果将随元素的 focus而自动出现,相应的由blur而自动消失。这些都是浏览器的默认行为,无需JavaScript配合CSS来控制。

#### CSS生成内容
+ ==在Web中插入内容==，在CSS2.1时代依靠的是JavaScript来实现。但进入CSS3进代之后我们可以通过CSS3的伪类“:before”，“:after”和CSS3的伪元素“::before”、“::after”来实现，其关键是依靠CSS3中的“content”属性来实现。不过这个属性对于img和input元素不起作用。
+ 在CSS中有一种清除浮动的方法叫“clearfix”。而这个“clearfix”方法就中就使用了“content”，只不过只是在这里插入了一个空格。
```
.clearfix:before,

.clearfix:after {

       content:””;

       display:table;

}

.clearfix:after {

       clear:both;

       overflow:hidden;

}
```
+ <a href="##" title="我是一个title属性值，我插在你的后面">我是元素</a>
可以通过”:after”和”content:attr(title)”将元素的”title”值插入到元素内容“我是元素”之后：
```
a:after {
  content:attr(title);
  color:#f00;
}
```
## 经典布局
#### 三栏布局：左右浮动+中间静态布局
+ middle必须放在left和right之后
+ 左边栏向左浮动，右边栏向右浮动，middle设置左右margin值。

#### 双飞翼布局
+ middle必须放在left和right之前
+ 中间栏的容器设置向左浮动和宽度width百分百
+ 左边栏向左浮动，margin-left为-100%，右边栏也向左浮动，margin-left为边栏的宽度。
+ 中间栏设置左右margin值即可。

#### 页面页脚+中间两栏布局
+ main主栏要写在aside边栏前面
+ 边栏的样式设置,相对定位，向左浮动，设置width，left值和margin-left值：
```
aside{
  position: relative;
  left:-210px;
  float: left;
  width:200px;
  margin-left: -100%;
}
```
+ 主栏设置向左浮动，width百分百。
```
main{
    float:left;
    width:100%;
```
+ 页脚设置`clear:both;`

#### 页眉页脚+中间三栏布局
###### 要点如下：
+ `body{text-align:center};`
+ 中间三栏内容区的container设置clearfix的class样式
```
.clearfix:after {  /*设置父元素高度自适应*/
    content: "";
    clear: both;
    display: block;
    visibility: hidden;
}
```
+ midContainer为只包含middle的父元素。设置左中右三个容器都左浮动，但是左右边栏的margin-left不一样，中间的最内层元素设置左右margin，为边栏腾出空间。
```
#midContainer {
    width: 100%;  /*自适应窗口大小*/
    float: left;
}
#middle {
    margin: 0 150px; /*为左右栏腾出空间*/
}
#left {
    float: left;
    width: 150px;
    margin-left: -100%;
}
#right {
    float: left;
    width: 150px;
    margin-left: -150px
}
```
+ 页眉页脚和中间三栏的大容器，都设置一个warp的class样式，三个都设置`margin:0 auto;`
---
## rem和em
+ rem是基于html元素的字体（font-size)大小来决定
+ em则根据使用它的元素(font-size)的大小决定（很多人错误以为是根据父类元素，实际上是使用它的元素继承了父类的属性才会产生的错觉）
+ 使用这两个单位的好处：使用 em 和 rem单位可以让我们的设计更加灵活，能够控制元素整体放大缩小，允许浏览器用户调整浏览器大小来达到最佳体验。
+ 常识：默认情况下浏览器通常有字体大小 16px，但这可以被用户更改为从 9px 到 72px的任何值。根 html 元素将继承浏览器中设置的字体大小，除非显式设置固定值去覆盖。所以 html 元素的字体大小虽然是直接确定 rem 值，但字体大小可能首先来自浏览器设置。因此浏览器的字体大小设置可以影响每个使用 rem 单元以及每个通过 em 单位继承的值。
+ 当 em或rem单位设置在 html 元素上时，它将转换为em值乘以浏览器字体大小的设置。
+ 为什么使用rem单位：它给我们的一个途经去获取用户的偏好来影响网站中每一处使用rem的元素大小，确保无论用户如何设置自己的浏览器，我们的布局都能调整到合适大小。
+ 如果您实在需要更改 html 元素的字体大小，那么就使用em，rem单位，这样根元素的值还会是用户浏览器字体大小的乘积。
+ 什么时候使用em？在非默认字体大小的元素上的padding、 margin、 width、 height和line-height等值。使用 em 单位，他们使用的元素的字体大小应设置对rem单位，以保留的可扩展性，但避免继承混淆。通常不使用 em 单位控制字体大小。
+ 什么时候使用rem？
    + 一般使用 rem 作为字体大小单位，em 单位只在特殊的情况下使用。一切可扩展都应该使用 rem 单位，几乎包括你布局的每部分。
    + 始终使用 rem 单位做媒体查询，这将确保，无论用户浏览器的字体大小，您的媒体查询会对它作出反应和调整您的布局。
+ 什么时候不使用em和rem?
    + 多列布局,布局中的列宽通常应该是 %，因此他们可以流畅适应无法预知大小的视区。
    + 当元素应该是严格不可缩放的时候。这真的不常出现。
