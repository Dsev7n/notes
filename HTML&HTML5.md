#### HTML语义化
+ SEO（Search Engine Optimization）:汉译为搜索引擎优化。是一种方式:利用搜索引擎的规则提高网站在有关搜索引擎内的自然排名。
+ 为什么要语义化？
    + 参考简书https://www.jianshu.com/p/6bc1fc059b51（如何理解HTML结构的语义化？）
    + a. 为了在没有CSS的情况下，页面也能呈现出很好地内容结构、代码结构:为了裸奔时好看；
    + b. 用户体验：例如title、alt用于解释名词或解释图片信息的标签尽量填写有含义的词语、label标签的活用；
    + c. 有利于SEO：和搜索引擎建立良好沟通，有助于爬虫抓取更多的有效信息：爬虫依赖于标签来确定上下文和各个关键字的权重；
    + d. 方便其他设备解析（如屏幕阅读器、盲人阅读器、移动设备）以有意义的方式来渲染网页；
    + e. 便于团队开发和维护，语义化更具可读性，遵循W3C标准的团队都遵循这个标准，可以减少差异化。
+ 写语义化的HTML代码时，应注意什么?
    + 1.尽可能少的使用无语义的标签div和span
    + 2.  在语义不明显时，既可以使用div或者p时，尽量用p, 因为p在默认情况下有上下间距，对兼容特殊终端有利；
    + 3.  不要使用纯样式标签，如：b、font、u等，改用css设置。
    + 4.  需要强调的文本，可以包含在strong或em标签中，strong默认样式是加粗（不要用b），em是斜体（不要用i）；
    + 5.  使用表格时，标题要用caption，表头用thead，主体部分用tbody包围，尾部用tfoot包围。表头和一般单元格要区分开，表头用th，单元格用td；
    + 6.表单域要用fieldset标签包起来，并用legend标签说明表单的用途；
    + 7.每个input标签对应的说明文本都需要使用label标签，并且通过为input设置id属性，在lable标签中设置for=someld来让说明文本和相对应的input关联起来。

+ HTML5新增了哪些语义化标签？
    + 1、header元素: 通常包含H1~H6元素或者hgroup元素。作为整个页面或者内容块的标题，也可以包裹一节的目录部分，一个搜索框，一个nav，或者任何相关logo。整个页面没有限制header元素的个数，可以拥有多个，可以为每个内容块增加一个header元素
    + 2、footer元素
    + 3、hgroup元素：如果只需要一个h1-h6标签就不用hgroup，如果有连续多个h1-h6标签就用hgroup
    + 4、nav元素:代表页面的导航链接区域。用于定义页面的主要导航部分。
    + 5、aside元素:aside在article内表示主要内容的附属信息，在article之外则可做侧边栏，没有article与之对应，最好不用。
    + 6、article元素:article代表一个在文档，页面或者网站中自成一体的内容，其目的是为了让开发者独立开发或重用。譬如论坛的帖子，博客上的文章，一篇用户的评论，一个互动的widget小工具。（特殊的section）.自身独立的情况下：用article,是相关内容：用section,没有语义的：用div。
 
---
+ 块级元素

  1.总是从新的一行开始

  2.高度、宽度都可以设置

  3.宽度没有设置时，默认为100%

  4.块级元素中可以包含块级元素和行内元素

+ 行内元素（内联元素）

  1.和其他元素都在一行

  2.高度、宽度以及内边距都是不可控的

  3.行内元素只能包含行内元素，不能包含块级元
  4.但行标签img,textarea,select,input是可以设置宽和高并且有效的。

+ 元素除了可以分为块级元素和行内元素，也可以分为替换元素和非替换元素。像==img,br,hr这样的都是替换元素==，浏览器遇到替换元素的时候会将它换成其他东西(一张图片，一个换行符等等)。
+ 行内元素只有左右的padding和margin可以设置。
+ 转换：
    + 如果要将行内元素转换为块级元素，只需要在该标签内设置样式`display:block;`

#### 标签闭不闭合
+ 非内容标签（也就是自闭合标签)的斜杠是可选的，也就是<br>和<br/>没有实质区别。
+ 比较常见的无内容标签有：
```
<br>、<hr>、<img>、<input>、<link>、<meta>
```
---
###  meta标签
> 原文链接：https://www.cnblogs.com/wangyang108/p/5995379.html

+ <meta> 标签提供了HTML文档的==元数据==。元数据不会显示在客户端，但是会被浏览器解析。META元素通常用于指定网页的描述，关键词，文件的最后修改时间，作者及其他元数据。元数据通常以 名称/值 对出现。
+ 元数据可以被使用浏览器（如何显示内容或重新加载页面），搜索引擎（关键词），或其他 Web 服务调用。
+ **<meta> 标签永远位于 head 元素内部**。在 HTML 中 <meta> 标签没有结束标签。
+ meta标签共有两个属性，分别是http-equiv属性和name属性。
##### 1.name属性
+ name属性主要用于描述网页，比如网页的关键词，叙述等。与之对应的属性值为content，content中的内容是对name填入类型的具体描述，便于搜索引擎抓取。meta标签中name属性语法格式是：
```
<meta name="参数" content="具体的描述">。
```
+ 其中name属性共有以下几种参数。
    1. keywords(关键字)
    2. description(网站内容的描述)
    3. viewport(移动端的窗口):这个属性常用于设计移动端网页。
    4. robots(定义搜索引擎爬虫的索引方式):robots用来告诉爬虫哪些页面需要索引，哪些页面不需要索引。content的参数有all,none,index,noindex,follow,nofollow。默认是all。
        1. none : 搜索引擎将忽略此网页，等价于noindex，nofollow。
        2. noindex : 搜索引擎不索引此网页。
        3. nofollow: 搜索引擎不继续通过此网页的链接索引搜索其它的网页。
        4.all : 搜索引擎将索引此网页与继续通过此网页的链接索引，等价于index，follow。
        5. index : 搜索引擎索引此网页。
        6. follow : 搜索引擎继续通过此网页的链接索引搜索其它的网页。
        7. author(作者)
        8.  copyright(版权)
        9.  revisit-after(搜索引擎爬虫重访时间): 如果页面不是经常更新，为了减轻搜索引擎爬虫对服务器带来的压力，可以设置一个爬虫的重访时间。如果重访时间过短，爬虫将按它们定义的默认时间来访问。举例：


```
<meta name="viewport" content="width=device-width, initial-scale=1">
<meta name="robots" content="none">
<meta name="copyright" content="Lxxyx"> //代表该网站为Lxxyx个人版权所有。
<meta name="revisit-after" content="7 days" >
```
##### 2.  http-equiv属性
+ 使用带有 http-equiv 属性的 <meta> 标签时，服务器将把名称/值对添加到发送给浏览器的内容头部。meta标签中http-equiv属性语法格式是：
```
<meta http-equiv="参数" content="具体的描述">
```
+ 其中http-equiv属性主要有以下几种参数：
    1.  content-Type(设定网页字符集)(推荐使用HTML5的方式) : 用于设定网页字符集，便于浏览器解析与渲染页面
    2. X-UA-Compatible(浏览器采取何种版本渲染当前页面): 用于告知浏览器以何种版本来渲染页面。（一般都设置为最新模式)
    3. cache-control(指定请求和响应遵循的缓存机制)
    4. expires(网页到期时间)
    5. refresh(自动刷新并指向某页面): 网页将在设定的时间内，自动刷新并调向设定的网址。
    6.  Set-Cookie
```
<meta http-equiv="content-Type" content="text/html;charset=utf-8">  //旧的HTML，不推荐

<meta charset="utf-8"> //HTML5设定网页字符集的方式，推荐使用UTF-8

<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1"/> //指定IE和Chrome使用最新版本渲染当前页面

<meta http-equiv="cache-control" content="no-cache">

<meta http-equiv="expires" content="Sunday 26 October 2016 01:00 GMT" />


<meta http-equiv="refresh" content="2；URL=http://www.lxxyx.win/"> //意思是2秒后跳转向我的博客

<meta http-equiv="Set-Cookie" content="User=Lxxyx; path=/; expires=Sunday, 10-Jan-16 10:00:00 GMT"> //具体范例
```
