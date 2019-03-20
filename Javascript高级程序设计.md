# 第3章 基本概念
## 3.1 语法
#### 3.1.1 区分大小写
+ ECMAScript中的一切（变量、函数名和操作符）都区分大小写。

#### 3.1.2 标识符
+ 所谓标识符，就是指变量、函数、属性的名字，或者函数的参数。标识符可以是按照下列格式规则组合起来的一或多个字符：
    + 第一个字符必须是一个字母、下划线或者美元符号。
    + 其他字符可以是字母、下划线、美元符号或数字。
+ 按照惯例，ECMAScript标识符采用驼峰大小写格式。

#### 3.1.4 严格模式
+ ES5引入了严格模式的概念，严格模式是为JS定义了一种不同的解析与执行模式。在严格模式下，ES3中的一些不确定的行为将得到处理，而且对某些不安全的操作也会抛出错误。要在整个脚本中启用严格模式，可以在顶部添加如下代码：
```
"use strict";
```
+ 在函数内部的上方包含这条编译指示，也可以指定函数在严格模式下执行：
```
function doSomething() {
    "use strict";
    //函数体
}
```
+ 支持严格模式的浏览器包括IE10+、Firefox4+、Safari5+、Opera12+和Chrome。

## 3.2 关键字和保留字
+ 关键字可用于表示控制语句的开始或结束，或者用于执行特定操作等。按照规则，关键字也是语言保留的，不能用作标识符。一下就是ECMAScript的全部关键字。（带星号表示第五版增加）
```
break  do   instanceof   typeof   case   else  new  var 
catch   finally  return  void  continue  for  switch  while  
debugger*  function  this  with  default  if  throw  delete   in  try 
```
+ 容易忘记的只有几个，其他一般都不会想到拿来做函数名，比如：try  delete  default 。没了。
+ ECMA-262还描述了另外一组不能用作标识符的保留字。尽管保留字在这门语言中还没有任何特定的用途，但他们有可能在将来被用作关键字。以下是ECMA-262第3版定义的全部保留字。
```
abstract  enum  int  short  boolean  export  interface  static  byte
extends long  super  char final  native  synchronized  class  float
packgae  throws  const  goto  private  transient  debugger  implements
protected  volatile  double  import  public
```

## 3.3 变量
+ ES的变量是松散类型的，所谓松散类型就是可以用来保存任何类型的数据。换句话说，每个变量仅仅是一个用于保存值的占位符而已。
+ 未经过初始化的变量会保存一个特殊的值--undefined。
```
var message = "hi";
```
+ 像这样初始化变量并不会把它标记为字符串类型，初始化的过程就是给变量赋一个值这么简单。因此，可以再修改变量值的同时修改值的类型（有效，但不推荐）.
+ 用var操作符定义的变量将成为定义该变量的作用域中的局部变量。也就是说，如果在函数中使用var定义一个变量，那么这个变量在函数退出后就会被销毁。
+ 不过，可以像下面这样省略var操作符，从而创建一个全局变量,这样，只要调用过一次test()函数，这个变量就有了定义，就可以在函数外部的任何地方被访问到：
```
function test () {
    message = "hi";    //全局变量
}
test();   
alert("message");  //"hi"
```
+ 虽然省略var操作符可以定义全局变量，但这也不是我们推荐的做法。因为在局部作用于中定义的全局变量很难维护，而且如果有意地忽略了var操作符，也会由于相应变量不会马上就有定义而导致不必要的混乱。给未定声明的变量赋值在严格模式下会导致抛出ReferenceError错误。
+ 由于ECMAScript是松散类型的，因而使用不同类型初始化变量的操作可以放在一条语句中来完成。

## 3.4 数据类型
+ ES中有5种基本数据类型（也叫简单数据类型）：Undefined、Null、Boolean、Number和String。还有一种复杂数据类型--Object。ECMAScript不支持任何创建自定义类型的机制，而所有值最终都将是6种数据类型之一。由于ES数据类型具有动态性，因此的确没有再定义其他数据类型的必要了。

#### 3.4.1 typeof操作符
+ 鉴于ES是松散类型的，因此需要有一种手段来检测给定变量的数据类型--typeof就是负责提供这方面的操作符。对一个值使用typeof操作符可能返回下列某个字符串：
    + "undefined"--如果这个值未定义。
    + "boolean" 、"string" 、 "number"
    + "object"-- 如果这个值时对象或null；
    + "function" -- 如果这个值是函数。
```
var message = "some string";
alert(typeof message);  //"string"
alert(typeof (message));  //"string"
alert(typeof 95);     //"number"
```
+ 注意，typeof是一个操作符而不是函数，因此例子中的圆括号尽管可以使用，但不是必需的。

#### 3.4.2 Undefined类型
```
var message;  //这个变量声明之后默认取得了undefined值

//下面这个变量并没有声明

alert(message);  //"undefined"
alert(age);      //产生错误
```
+ 然而，令人疑惑的是：对未初始化的变量执行typeof操作符会返回undefined值，而对未声明的变量执行typeof操作符同样也会返回undefined值。
```
var message;

alert(typeof message);  //"undefined"
alert(typeof age);   //"undefined"
```
+ 即便未初始化的变量会自动被赋予undefined值，但显式地初始化变量依然是我们明知的选择。如果能够做到这一点，那么当typeof操作符返回“undefined”值时，我们就知道被检测的变量还没有被声明，而不是尚未初始化。

#### 3.4.3 Null类型


















---
# 第5章  引用类型
## 5.2 Array类型

+ 数组都是有序列表，ES数组的每一项可以保存任何类型的数据。可以用数组的第一个位置来保存字符串，第二位置来保存数值等。
+ 创建数组的基本方式有两种
    + 使用Arry构造函数
    + 使用数组字面量表示法
```
//使用Array构造函数
var colors = new Array();
var colors = new Array(20);
var colors = new Array("red", "blue", "green");
var colors = Array(3);//可以省略new操作符


//数组字面量表示法
var colors = ["red", "blue", "green"];//包含三项
var names = []; //空数组
var values = [1, 2,] //不要在数组字面量的最后一项添加逗号！会导致创建一个包含2或3项的数组。
var options = [,,,,,]  //强烈不建议！包含5或6项
```
+ 如果设置某个值得索引超过了数组现有项数，数组就会自动增加到该索引值加一的长度。空数组的长度为0.
+ 数组的length不是只读的，通过设置这个属性，可以从数组的末尾移除项或新增项，新增的每一项都会取得undefined。
```
var colors = ["red", "blue", "green"];
colors[color.length] = "brown"; //4项
colors[99] = "black";
alert(colors.length); //100
```
#### 5.2.1 检测数组
+ 对于一个网页或者一个全局作用域而言，使用instanseof操作符就能得到满意的结果
```
if(value instanceof Array){
    //对数组进行某些操作
}
```
+ instanceof操作符的问题在于，它假定只有一个全局执行环境，如果网页中包含多个框架，那实际上就存在两个以上不同的全局执行环境，从而存在两个以上不同版本的Array构造函数。如果你从一个框架向另一个框架传入一个数组，那么传入的数组与在第二个框架中原生穿件的数组分别具有各自不同的构造函数。
+ ES5新增了`Array.isArray()`方法，这个方法的目的是最终确定这个值到底是不是数组，而不管它是在全局执行环境中创建的。
```
if(Array.isArray(value)){
    
}
```
#### 5.2.2 转换方法
+ `valueOf()`返回数组本身
+ `toString()`返回由数组中每个值得字符串形式拼接而成的一个以逗号分隔的字符串，为了创建这个字符串会调用数组每一项的`toString()`方法
```
var colors = ["red", "blue", "green"];
alert(colors.toString());  //red,blue,green
alert(colors.valueOf());  //red,blue,green
alert(colors);  //red,blue,green
```
+ alert()要接收字符串参数，所以他会在后台调用toString()方法，由此会得到与直接调用toString()方法相同的结果。
+ `toLocaleString()`方法与前两个方法唯一的不同之处在于，这一次为了取得每一项的值，调用的事每一项的toLocaleString（）方法，而不是toString
```
var person1 = {
    toLocaleString: function(){
        return "Nikolaos";
    },
    
    toString : function() {
        return "Grigotios"
    }
};

var person2 = {
    toLocaleString : function() {
        return "Grigorios";
    },
    
    toString : function() {
        return "Greg";
    }
};

var person1 = [person1, person2];
alert(people);  //Nicholas,Greg
alert(people.toString());  //Nicholas,Greg
alert(people.toLocaleString());  //Nikolaos,Grigorios
```
+ 数组继承的`toLocaleString()`、`toString()`和`valueOf()`方法，在默认情况下都会以逗号分隔的字符串的形式返回数组项。
+ `join()`方法，可以使用不同的分隔项来构建返回的字符串。如果不给join()方法传入任何值，则使用逗号作为分隔符。如果数组中的某一项的值是null或undefined,那么该值在返回的结果中以空字符串表示。
```
var colors = ["red", "blue", "green"];
alert(colors.join("||"));   //red||green||blue
```
#### 5.2.3 栈方法
+ 数组可以表现得像栈一样，ES为数组专门提供了push()和pop()方法
+ `push`:可以接收任意数量的参数，返回修改后的数组的长度
+ `pop()`每次只能移除最后一项，返回移除的项，不接受参数。

#### 5.2.4 队列方法
+ `shift()`:移除数组中的第一个项并返回该项，不接受参数 
+ `unshift()`: 在数组前端添加任意个项并返回新数组的长度。

#### 5.2.5 重排序方法
+ `reverse()`  反转顺序
+ `sort()`默认按==升序==排列
    + 为了实现排序，sort()方法会调用每个数组项的`toString（）`方法。即使数组中的每一项都是数值，sort()方法比较的也是字符串。
    + 在进行字符串比较时，“10”则位于“5”的后面。
    + sort()方法可以接收一个比较函数作为参数。比较函数接收两个参数，如果第一个参数应该位于第二个之前返回一个负数，如果两个参数相等则返回0，如果第一个参数应该位于第二个之后则返回一个正数。
```
function compare(value1,value2) {
    if (value1 < value2) {
        return -1;
    } else if (value1 > value2) {
        return 1;
    } else {
        return 0;
    }
}
values=[];
values.sort(compare);
```
也可以通过比较函数产生降序的结果，只要交换比较函数返回的值即可。
+ 对于==数值类型或者valueOf()方法会返回数值类型的对象类型==，可以使用一个更简单的比较函数。由于比较函数通过返回一个小于零、等于零或大于零的值来影响排序结果，因此减法操作就可以适当地处理所有这些情况。
```
//这样出来是降序排列
function compare(value1, value2){
    return value2 - value1;
}
```
+ sort方法的参数也可以是一个匿名函数，这样写跟上面是一样的：
```
//升序排列

values.sort(function(a,b){
    return a - b;
})
```

#### 5.2.6 操作方法
+ `concat（）`方法会先创建当前数组一个副本，然后将接收到的参数添加到这个副本的末尾，最后返回新构建的数组。
```
var colors = ["red","green","blue"];
var colors2 = colors.concat("yellow",["black","brown"]);

alert(colors);//red,green,blue
alert(colors2);//red,green,blue,yellow,black,brown
```
+ `slice()`方法可以接受一或两个参数，即要返回项的起始和结束位置。
    + 在只有一个参数的情况下，`slice()`方法返回从改参数指定位置开始到当前数组末尾的所有项。
    + 如果有两个参数，该方法放回其实和结束位置之间的项--但不包括结束位置的项。
    + 如果slice()方法的参数中有一个负数，则用数组长度加上概述来确定相应的位置。
    + 如果结束位置小于起始位置，则返回空数组。
+ `splice()`主要用途是向数组的中部插入值
    + 删除：接收两个参数：要删除的第一项的位置和要删除的项数。
    + 插入：接受三个参数：起始位置、0（要删除的项数）和要出入的项。如果要出入多个项，可以再传入第四、第五，以至任意多个项。
    + 替换：接受三个参数：起始位置，要删除的项数和要出入的任意数量的项，插入的项数不必与删除的项数相等。如`splice(2,1,"red","green")`
    + splice()方法始终都会返回一个数组，该数组中包含从原始数组中删除的项（如果没有删除任何项，则返回一个空数组）。
```
var colors = ["red", "green", "blue"];
var removed = colors.splice(0,1);
alert(colors);  //green,blue
alert(removed);  //red,返回的数组中只包含一项
```
#### 5.2.7 位置方法
+ `indexOf()`和`lastIndexOf()`，这两个方法都接收两个参数：要查找的项和（可选的）表示查找起点位置的索引。indexOf()从前往后找，lastIndexOf()从后往前找。
+ 这两个方法都返回要查找的项在数组中的位置，或者在没返回的情况下返回-1。在比较第一个参数与数组中的每一项是，会使用全等操作符（===）。

#### 迭代方法（类似于遍历）
+ ES5为数组定义了5个迭代方法。每个方法都接收两个参数：要在每一项上运行的函数和（可选的）运行该函数的作用域对象--影响this的值。传入这些方法中的函数function会接收三个参数：数组项的值item、该项在数组中的索引index和数组对象本身array。
    + `every()`:对数组中的每一项运行给定函数，如果该函数对每一项都返回true,则返回true。
    + `filter()`:对数组中的每一项运行给定函数，返回该函数会返回符合条件的项组成的数组。
    + `forEach()`:对数组中的每一项运行给定函数，这个方法没有返回值。本质上与使用for循环迭代数组一样。
    + `map()`:对数组中的每一项运行给定函数，返回每次函数调用的结果组成的数组。
    + `some()`:对数组的每一项运行给定函数，如果该函数对任一项返回true(),则返回true。
+ 以上方法都不会修改数组中包含的值。这些方法中，最相似的是every()和some()。它们的区别就只是语义上的区别。如下所示：
```
var numbers = [1,2,3,4,5,4,3,2,1];

var everyResult = numbers.every(function(item, index, array){
    return (item > 2); 
});   //false

var someResult = numbers.some(function(item,index,array){
    return (item > 2);
});   //true

var mapResult = numbers.map(function(item, index, array){
    return item*2;  
});
alert(mapResult);  //[2,4,6,8,10,8,6,4,2]
```
#### 5.2.9 归并方法(最终结果是每一项共同影响的结果)
+ ES5新增了两个归并数组的方法：`reduce()`和`reduceRight()`。这两个方法都会迭代数组的所有项，然后构建一个最终返回的值。reduce()方法从数组的第一项开始，逐个遍历到最后，而reduceRight()则从数组的最后一项开始，向前遍历到第一项。
+ 这两个方法都接收两个参数:一个在每一项上调用的函数和（可选的）作为归并基础的初始值。传给reduce()和reduceRight()的函数接收4个参数：前一个值、当前值、项的索引和数组对象。==这个函数返回的任何值都会作为第一个参数==自动传给下一项。
```
var values = [1,2,3,4,5];
var sum = values.reduce(function(prev, cur, index, array){
    return prev + cur;
});
alerts(sum);  //15
```
+ 使用reduceRight()也是完全一样的写法。

#### 数组方法是否改变原数组总结
+ 改变原数组的方法：队列shift、unshift、栈push、pop、排序方法reverse、sort、增删替换splice
+ 不改变原数组的方法：concat、join、slice、map,filter,forEach,some,every等
***
## 5.3 Date类型
+ Date类型使用自UTC(国际协调时间)1970年1月1日零点开始经过的毫秒数来保存日期。要创建一个日期对象，使用new操作符和Date构造函数即可。
```
var now = new Date();
```
+ 在调用Date构造函数而不传递参数的情况下，新创建的对象自动获得当前日期和时间，如果想根据特定的日期和时间创建日期对象，必须传入表示该日期的毫秒数（即从UTC时间起至该日期止经过的毫秒数）。为了简化这一计算过程，ES提供了两个方法：Date.parse()和Date.UTC()。
+ 其中，Date.parse()方法接收一个表示日期的字符串参数，然后尝试根据这个字符串返回相应日期的毫秒数，这个方法的行为因实现而异，将地区设置为美国的浏览器通常都接收下列日期格式：
    + "月/日/年"，如6/13/2004;
    + "英文月名 日，年"， 如January 12,2004
    + "英文星期几 英文月名 日 年 日：分：秒 时区"，如Tue May 25 2004 00:00:00 GMT-0700
+ 例如，要为2004年5月25日创建一个日期对象
```
var someDate = new Date(Date.parse("May 25, 2004"));

等价于

var someDate = new Date("May 25,2004");
```
如果传入Date.parse()方法的字符串不能表示日期，那么它会返回NaN。实际上，如果直接将表示日期的字符串传递给Date构造函数，也会在后台调用Date.parse()。
+ Date.UTC()方法同样是返回表示日期的毫秒数，但它与Date.parse()在构建值时使用不同的信息。Date.UTC()的参数分别是年份、基于0的月份（一月是0，二月是1，以此类推）、月中的额哪一天（1到31）、小时数（0到23）、分钟、秒及毫秒数。在这些参数中，只有前两个参数（年和月）是必须的。如果没有提供月中的天数，则假设天数为1；如果省略其他参数，则统统假设为0.
```
//GMT时间2000年1月1日午夜零时
var y2k = new Date(Date.UTC(2000,0));

//GMT时间2005年5月5日下午5：55：55
var allFives = new Date(Date.UTC(2005,4,5,17,55,55));
```
+ Date构造函数也会模仿Date.UTC()，但有一点明显不同：日期和时间都是基于本地时区而非GMT来创建。
```
//本地时间2000年1月1日午夜零时
var y2k = new Date(2000,0);

//本地时间2005年5月5日下午5：55：55
var allFives = new Date(2005,4,5,17,55,55);
```
+ ES5添加了Date.now()方法，返回表示调用这个方法时的日期和时间的毫秒数，这个方法简化了使用Date对象分析代码的工作。
```
//取得开始时间
var start = Date.now();

//调用函数
doSomething();

//取得停止时间
var stop = Date.now(),
    result = stop - start;
```
#### 5.3.1 继承的方法
+ 与其他引用类型一样，Date()类型也重写了toLocaleString()、toString()和valueOf()方法，但这些方法返回的值与其他类型中的方法不同。Date类型的toLocaleString（）方法会按照与浏览器设置的地区相适应的格式返回日期和时间。这大致意味着时间格式中会包含AM或PM,但不会包含时区信息(当然，具体的格式会因浏览器而异)。而toString()方法通常返回带有时区信息的日期和时间，其中时间一般以军用时间(即小时的范围是0到23)表示。
+ 事实上，toLocaleString()和toString()的这一差别尽在调试代码时比较有用，而在显示时间和日期时没有什么价值。
+ Date类型的valueOf()方法，返回的是日期的毫秒表示。因此，可以方便使用比较操作符(小于或大于)来比较日期值。
```
var date1 = new Date(2007, 0, 1); //"January 1, 2007"
var date2 = new Date(2007, 1, 1); //"February 1,2007"

alert(date1 < date2); //true
alert(date1 > date2); //false
```
#### 5.3.2 日期格式化方法
+ Date类型还有一些专门用于将日期格式化为字符串的方法
    + toDateString()--以特定于实现的格式显示星期几、月、日和年
    + toTimeString()--以特定于实现的格式显示时、分、秒和时区
    + toLocaleDateString()--以特定于地区的格式显示星期几、月、日和年
    + toLocaleTimeString()--以特定于实现的格式显示时、分、秒
    + toUTCString()--以特定于实现的格式完整的UTC日期
+ 与toLocaleString()和toString()方法一样，以上这些字符串格方法的输出也是因浏览器而异的，因此没有哪一个方法能够用来在用户界面显示一致的日期信息。

#### 5.3.3 日期/事件组件方法
+ getTime()--返回表示日期的毫秒数，与valueOf()方法返回的值相同。
+ setTime()--以毫秒数设置日期，会改变着呢个日期。
+ getFullYear()--取得4位数的年份
+ getUTCFullYear()--返回UTC日期的4位数年份
+ setFullYear()--设置日期的年份。传入的年份值必须是4位数字
+ setUTCFullYear()
+ getDate()--返回日期月份中的天数（1到31）
+ getDay()--返回日期中星期的星期几（其中0表示星期日，6表示星期六）
+ getHours()--返回日期中的小时数（0到23）
---

## 5.4 RegExp类型
+ ECMAScript通过RegExp类型来支持正则表达式。使用下面类似Perl的语法，就可以创建一个正则表达式。
```
var expression = /pattern /flags ;
```
+ 其中的模式（pattern）部分可以时任何简单或复杂的正则表达式，可以包含字符类、限定符、分组、向前查找以及反向引用。每个正则表达式都可带有一或多个标志（flags），用以标明正则表达式的行为。正则表达式的匹配模式支持下列3个标志。
    + g--表示全局（global）模式，即模式将被应用于所有字符串，而非在发现第一个匹配项时立即停止；
    + i--表示不区分大小写（case-insensitive）模式。
    + m--表示多行（multiline）模式，即在到达一行文本末尾还会继续查找下一行中是否存在于模式匹配的项。
```
//字面量形式定义正则表达式
var pattern1 = /at/g;

//匹配第一个bat或cat，不区分大小写
var pattern2 = /[bc]at/i;

//匹配所有以“at"结尾的3个字符的组合，不区分大小写
var pattern3 = /.at/gi;
```
+ 模式中使用的所有元字符都必须转义，即在元字符前加一个“/”。正则表达式的元字符包括：`( [ { \ ^ $ | ）? * + . ] }`。如果想要匹配字符串中包含的这些字符，就必须对他们进行转义。
```
//匹配第一个“[bc]at",不区分大小写
var pattern3 = /.at/gi;

//匹配所有“.at"，不区分大小写
var pattern4 = /\.at/gi;
```
+ 另一种创建正则表达式的方式是使用RegExp构造函数，它接收两个参数：一个时匹配的字符串模式，另一个是可选的标志字符串。
```
var pattern1 = /[bc]at/i;
等价于
var pattern2 = new RegExp（"[bc]at","i");
```
+ 由于RegExp构造函数的模式参数是字符串，所以在某些情况下要对字符进行双重转义，所有元字符都必须双重转义，那些已经转义的字符也是如此，例如\n(字符\在字符串中通常被转义为`\\`，而在正则表达式字符串中共就会变成`\\\\`)。
    + 字面量模式--等价的字符串
    + `/\[bc\]at`--`"\\[bc\\]at"`
    + `/\.at/`--`"\\.at"`
    + `/name\/age/`--`"name\\/age"`
    + `/\d.\d{1,2}`--`"name\\/age"`
    + `/\w\\hello\\123/`--`"\\w\\\\hello\\\\123"`
    + `/\s/` -- `"\\s"`或者`" "` -- 表示匹配空格
+ 在ES3中，正则表达式字面量始终会共享同一个RegExp实例，而使用构造函数创建的每一个新RegExp实例都是一个新实例。
```
var re = null, i;

for(i=0; i < 10; i++){
    re = /cat/g;
    re.test("catastrophe");
}

for (i=o; i < 10; i++){
    re = new RegExp("cat", "g");
    re.test("catastrophe");
}
```
+ 在第一个循环中，及时是循环体中指定的，但实际上值为/cat/创建了一个RegExp实例，由于实例属性不会重置，所以在循环中再次调用test()方法会失败。这是因为第一次调用test()方法找到了"cat"，但第二次调用是从索引为3的字符（上一次匹配的末尾）开始的，所以就找不到它了。由于会测试到字符串末尾，所以下一次再调用test()就又从开头开始了。
+ 第二个循环使用RegExp构造函数再每次循环中创建正则表达式。因为每次跌打的都会创建一个新的RegExp实例，所以每次调用test()都会返回true。
+ ES5明确规定，使用正则表达式字面量必须像直接调用RegExp构造函数一样，每次都创建新的RegExp实例。

#### 5.4.1 RegExp实例属性
#### 5.4.2 RegExp实例方法
+ RegExp对象的主要方法时exec(),该方法是专门为捕获组而设计的,第一个括号内的内容叫第一个捕获组。exec()接收一个参数，即要应用模式的字符串，然后返回包含第一个匹配项信息的数组；或者没有匹配项的情况下返回null。返回的数组虽然是Array的实例，但包含两个额外的属性：index和input。其中，index表示匹配项在字符串中的位置，而input表示应用正则表达式的字符串。在数组中，第一项是与整个模式匹配的字符串，其他项是与模式中的捕获组匹配的字符串(如果模式中没有捕获组，则该数组中只包含一项)。
```
var text = "mom and dad and baby";
var pattern = /mom( and dad( and baby)?)?/gi;

var matches = pattern.exec(text);
alert(matches[0]); //"mom and dad and baby"
alert(matches[1]); //"and dad and baby"
alert(matches[2]);  //"and baby"
```
数组中的第一项是匹配的整个字符串，第二项包含与第一个捕获组匹配的内容，第三项包含与第二个捕获组匹配的内容。
+ 对于exec()方法而言，即使在模式中设置了全局标志（g），它每次也只会返回一个匹配项。在不设置全局标志的情况下，在同一个字符串中上多次调用exec()将始终返回第一个匹配项的信息，而在设置全局标志的情况下，每次调用exec()则都会在字符串中继续查找新匹配项。
```
var text = "cat, bat, sat, fat";
var pattern1 = /.at/;

var matches = pattern1.exec(text);
alert(matches.index);  //0
alert(matches[0]);  //cat
alert(pattern1.lastIndex);  //0

matches = pattern1.exec(text);
alert(matches.index);  //0
alert(matches[0]);  //cat
alert(pattern1.lastIndex);  //0

var pattern2 = /.at/g;

var matches = pattern2.exec(text);
alert(matches.index);  //0
alert(matches[0]);  //cat
alert(pattern2.lastIndex);  //3

matches = patteern2.exec(text);
alert(matches.index);  //5
alert(matches[0]);  //bat
alert(pattern2.lastIndex);  //8
```
+ 在全局匹配模式下，lastIndex的值在每次调用exec()后都会增加，而在非全局模式下则始终保持不变。（IE的JS实现在lastIndex属性上存在偏差，即使在非全局模式下，lastIndex属性每次也会变化）。
+ 正则表达式的第二个方法是test(),它接收一个字符串参数。在模式与该参数匹配的情况下返回true；否则，返回false。在只想知道目标字符串与某个模式是否匹配，但不需要知道其文本内容的情况下，使用这个方法非常方便。
```
var text = "000-00-0000"
var pattern = /\d{3}-\d{2}-\d{4}/;
if(pattern.test(text)){
    alert("The pattern was matche.");
}
```
+ RegExp实例继承的toLocaleString()和toString()方法都会返回正则表达式的字面量，与创建正则表达式的方式无关。
```
var pattern = new RegExp("\\[bc\\]at", "gi");
alert(pattern.toString());  //  /\[bc\]at/gi
alert(pattern.toLocaleString());  //       /\[bc\]at/gi
```
+ 正则表达式的valueOf（）方法返回正则表达式本身。

#### 5.4.3 RegExp构造函数属性
+ 这些属性适用于作用域中的所有正则表达式，并且基于所执行的最近一次次正则表达式操作而变化，关于这些属性的另一个独特之处，就是可以通过两种方式访问他们。换句话说，这些属性分别有一个长属性名和一个短属性名。
    + 长属性名 -- 短属性名 -- 说明
    + input -- $_ -- 最近一次要匹配的字符串。
    + lastMatch -- $& -- 最近一次的匹配项
    + lastParen -- $+ -- 最近一次匹配的捕获组
    + leftContext -- $` -- input字符串中共lastMatch之前的文本
    + multiline -- $* -- 布尔值，表示是否所有表达式都使用多行模式。
    + rightContext -- $' -- Input字符串中lastMatch之后的文本
```
var text = "this has been a short summer";
var pattern = /(.)hort/g;

//注意：Opera不支持input、lastMatch、lastParen和multiline属性
//IE不支持multiline属性
if (pattern.test(text)) {
    alert(RegExp.input);  //this has been a short summer
    alert(RegExp.leftContext);  //this has been a
    alert(RegExp.rightContext); //summer
    alert(RegExp.lastMatch);  //short
    alert(RegExp.lastParen);  //s
    alert(RegExp.multiline);  //false
}
```
例子中的长属性名都可以用响应的端属性名来代替。只不过，由于这些端属性名大豆不是有效的ECMAScript表示符，因此必须通过方括号语法来访问他们。
```
var text = "this has been a short summer";
var pattern = /(.)hort/g;

if (pattern.test(text)) {
    alert(RegExp.$_);  //this has been a short summer
    alert(RegExp["$`"]);  //this has been a
    alert(RegExp["$'"]);  //summer
    alert(RegExp["$&"]);  //short
    alert(RegExp["$+"]);  //s
    alert(RegExp["$*"]);  //false
}
```
+ 除了上面介绍的几个属性之外，还有多大9个用于存储捕获组的构造函数属性。访问这些属性的语法是RegExp.$1、RegExp.$2……RegExp.$9,分别用于存储第一、第二……第九个匹配的捕获组。在调用exec()或test()方法时，这些属性会被自动填充。
```
var text = "this has been a short summer";
var pattern = /(..)or(.)/g;

if (pattern.test(text)) {
    alert(RegExp.$1);  //sh
    alert(RegExp.$2);  //t
}
```
+ 正则表达式中通用字符集的替换
    + 正则 -- 扩充表达式 -- 匹配
    + `\d` -- `[0-9]` -- 数字字符
    + `\D` -- `[^0-9]` -- 非数字字符
    + `\w` -- `[a-zA-Z0-9]` -- 数字或者字母
    + `\W` -- `[^\w]` -- 非数字字母
    + `\s` -- `[_\r\t\n\f]` -- 表格，换行等空白区域
    + `\S` -- `[^\s]` --非空白区域
+ 计数符
    + 正则 -- 匹配
    + `^` -- 匹配一行的开头
    + `$` -- 匹配一行的结尾
    + `.` -- 匹配任意一个字符
    + `*` -- 零或多
    + `+` -- 一或多
    + `?` -- 零或一
    + `{n}` -- 出现n次
    + `{n,m}` -- n到m次
    + `{n,}` -- 至少n次

#### 5.4.4 模式的局限性


***
## 5.5 Function
+ ES的==函数实际上是对象，每个函数都是Function类型的实例==，而且都与其他引用类型一样具有属性和方法。由于函数是对象，因此函数名实际上也是一个指向函数对象的指针，不会与某个函数绑定。函数通常是使用函数声明语法定义的。
```
//1.函数声明语法定义函数
function sum (num1, num2) {
    return num1 + num2;
}


//2.函数表达式定义函数,funciton关键字后没有函数名
var sum = function(num1, num2){
    return num1 + num2;
};  //注意末尾有一个分号


//3.使用Function构造函数
var sum = new Function("num1", "num2", "return num1 + num2");   //不推荐
```
+ 使用函数表达式定义函数时，没有必要使用函数名--通过变量sum即可引用函数。
+ 函数是对象，函数名是指针。一个函数可能会有多个名字。
#### 5.5.1 没有重载（深入理解）
#### 5.5.2 函数声明与函数表达式
+ 除了什么时候可以通过变量访问函数这一点区别之外，函数声明和函数表达式的语法其实是等价的。
#### 5.5.3 作为值的函数
+ 因为ES中的函数名本身就是变量，所以函数可以作为值来使用。也就是说，不仅可以像传递参数一样把一个函数传递给另一个函数，而且可以将一个函数作为另一个函数的结果返回。
```
function.callSomeFunction(someFunction, someArgument){
    return someFunction(someArgument)
}
```
#### 函数内部属性
+ 在函数内部，有两个特殊的对象：argument和this。其中，arguments是一个类数组对象，包含着传入函数中的所有参数。虽然arguments的主要用途是保存函数参数，但这个对象还有一个名叫callee的属性，该属性是一个指针，指向拥有这个arguments对象的函数。
+ 定义阶乘函数一般要用到递归算法，在函数有名字，而且名字以后也不会改变的情况下，这样定义没有问题。为了消除紧密耦合的现象，使得当函数名改变时，也不会影响函数的执行，可以使用arguments.callee.
```
function factorial(num){
    if(num<=1) {
        return 1;
    } else {
        return num*arguments.calllee(num-1);
    }
}
```
+ 函数内部的另一个特殊对象是this.
+ 函数对象的另一个属性：caller,保存着调用当前函数的函数的引用，如果是在全局作用域中调用当前函数，他的值为null.
```
function outer() {
    inner();
}

function inner() {
    alert(arguments.callee.caller);
}

outer();         //警告框中显示outer()函数的源代码
```
+ 在严格模式下运行时，访问arugments.callee会导致错误。ES5还定义了arguments.caller属性，但在严格模式下访问他也会导致错误，而在非严格模式下这个属性始终是undefined。定义arguments.callee属性时为了分清arguments.caller和函数的caller属性，以上变化都是为了加强这门语言的安全性，这样第三方代码就不能再相同的环境里窥视其他代码了。严格模式还有一个限制：不能为函数的caller属性赋值，否则会导致错误。
#### 5.5.5 函数属性和方法
+ 每个函数都包含两个属性：length和prototype.其中，length属性表示函数希望接受的命名参数的个数。
+ 对于ES中的引用类型而言，prototype是保存他们所有实例方法的真正所在，诸如toString()和valueOf()等方法实际上都保存在prototype名下，只不过是通过各自对象的实例访问罢了。在ES5中，prototype属性时不可枚举的，因此使用for-in无法发现。
+ 每个函数都包含两个非继承而来的方法：apply()和call().这两个方法的用途都是==在特定的作用域中调用函数，实际上等于设置函数体内this对象的值==。apply()方法接收两个参数：一个是在其中运行函数的作用域，另一个是参数数组。第二个参数可以是Array()的实例，也可以是arguemnts对象。
+ call()方法与apply()方法的作用相同，他们的区别仅在于接收参数的方式不同，call()方法传递给函数的参数必须逐个列举出来。
```
function sum(num1, num2){
    return num1 + num2;
}

function callSum1(num1, num2){
    return sum.apply(this, arguments);
}

fucntion callSum2(num1, num2){
    return sum.appply(this, [num1, num2]);
}

alert(callSum1(10,10));  //20
alert(callSum2(10,10));  //20

function callSum(num1, num2){
    return sum.call(this,num1,num2);
}
alert(callSum(10,10));  //20
```
+ apply()和call()真正强大的地方是能够扩充（即改变）函数赖以运行的作用域。
+ 扩充作用域的最大好处，就是对象不需要与方法有任何耦合关系。
+ bind()方法会创建一个函数的实例，其this值会被绑定到传给bind()函数的值。
```
window.color = "red";
var o = { color:"blue" };

function sayColor(){
    alert(this.color);
}

sayColor();  //red

sayColor.call(this);  //red
sayColor.call(window); //red
sayColor.call(o);  //blue

var objectSaycolor =sayColor.bind(o);
objectSayColor();//blue
等价于
sayColor.bind(o)();
```
+ call和apply都是对函数的直接调用，而bind方法返回的仍然是一个函数，因此后面还需要()来进行调用才可以。
---

####  闭包
+ 概念：闭包是指有权访问另一函数作用域中的变量的函数。
+ 闭包的特性：它们可以捕捉到局部变量（和参数），并一直保存下来。
+ 创建的常用方式：在一个函数内部创建另一个函数
```
function createComparisonFunction(propertyName){
    return function(object1,object2){
        var value1 = object1[propertyName1];
        var value2 = object2[propertyName2];
        
        if(value1 < value2){
            return -1;
        } else if (value1 > value2){
            return 1;
        } else {
            return 0;
        }
    
    };
}
//这里的内部函数是一个匿名函数。整个是一个闭包。
```
```
var scope = "global scope";
function checkScope() {
    var scope = "local scope";
    function f() {
        return scope;
    }
    return f;
}
checkScope()();  //local scope
```
+ remember词法作用域的基础规则：函数被执行时（executed)使用的作用域链是被定义时的作用域链。
+ 闭包的神奇特性：闭包可以捕获到局部变量和参数的外部函数绑定，即便外部函数的调用已经结束。
+ 缺点：由于闭包会携带包含它的函数的作用域，因此比其他函数占用更多的内存，过度使用闭包可能会导致内存占用过多，建议只在绝对必要时考虑闭包。
+ 闭包与变量：闭包只能取得包含函数中任何变量的最后一个值。
```
function createFunction(){
    var result = new Array();
    
    for (var i=0; i < 10;i++){
        result[i] = function(num){
            return function(){//匿名函数
                return num;
            };
        }(i);
    }
    return result;
}
/* return[i]=function(num){}(i)   这里，num表示函数的定义的参数，i表示实际传入的参数。
没有直接把闭包赋值给数组，而是定义了一个匿名函数，并将立即执行该匿名函数的结果赋给数组。
*/ 
```
+ 关于this对象画重点：==匿名函数的执行环境具有全局性==，因此其this对象通常指向window。
```
var name = "The Window";

var object = {
    name : "My Object",
    
    getNameFunc : function(){
        return function(){
            return this.name;
        };
    }
};

alert(object.getNameFunc()());//"The Window"(在非严格模式下)
```
由于getNameFunc()返回一个函数，因此调用object.getNameFunc()()就会立即调用它返回的函数，结果就是返回一个字符串。

不过，把外部作用域中的this对象保存在一个闭包能够访问到的变量(下面的例子是that)里，就可以让闭包访问该对象了。
```
var name = "The Window";

var object = {
    name : "My Object",
    
    getNameFunc : function(){
        var that = this;
        return function(){
            return that.name;
        };
    }
    getName: function(){//简单地返回的例子
        return this.name;
    }
};

alert(object.getNameFunc()());//"My Object"
object.getName();//"My Object"
```
+ 如果想访问作用域中的==arguments==对象，必须将对该对象的引用保存到另一个闭包能够访问的变量中。

IE中存在的特殊问题：如果闭包的作用域链中保存着一个HTML元素，呢么就意味着该元素无法被销毁。
```
function assignHandler(){
    var element = document.getElementById("someElement");
    var id = element.id;
    element.onclick = function(){
        alert(id);
    }；
    element = null;
}
```
以上代码创建了一个作为element元素事件处理程序的闭包，而这个闭包则又创建了一个循环引用，由于匿名函数保存了一个对assignHandler（）的活动对象的引用，因此将会导致无法减少element的引用数，只要匿名函数存在，element的引用数至少也是1，因此它所占用的内存就永远无法被回收，

必须记住：闭包会引用包含函数的整个活动对象，而其中包含着element，即使闭包不直接引用element，包含函数的活动对象也仍然会保存一个引用，设置为null就能够解除对DOM对象的引用，顺利的减少其引用数，确保正常回收其占用的内存。

每次调用JS函数时，都会为之创建一个新的对象用来保存局部变量，把这个对象添加至作用域链中。当函数返回的时候，就从作用域链中将这个绑定变量的对象删除。如果不存在嵌套的函数，也没有其他引用指向这个绑定对象，他就会被当做垃圾回收掉。如果定义了嵌套的函数，每个嵌套的函数都各自对应一个作用域链，并且这个作用域链指向一个变量绑定对象。但如果这些嵌套的函数对象在外部函数中保存下来，
###### 闭包的使用场景
+ 通过循环给页面上多个DOM节点绑定事件
+ 将一些不希望暴露在全局的变量封装成“私有变量”
+ 延续局部变量的寿命

#### 5.6.3 String类型
+ `var stringObject = new String("hello world")`

###### 1.字符方法
+ `charAt()`和`charCodeAt()`这两个方法都接收一个索引，前者单字符字符串的形式返回给定位置的那个字符，后者返回的是字符编码。
```
var stringValue = "hello world";
alert(stringValue.charAt(1));  //"e"

alert(stringValue.charCodeAt(1));  //输出"101"

alert(stringValue[1]);  //"e"
```
+ ES5可以用==方括号加数字索引==来访问字符串中特定字符

###### 2.字符串操作方法
+ `concat()`用于将一或多个字符串拼接起来，返回拼接得到的新字符串,原字符串保持不变。
```
var stringValue = "hello ";
var result = stringValue.concat("world");
alert(result);   //"hello world"  //中间的空格是我自己加的，不是方法自带的
alert(stringValue);  //"hello"
```
+ `concat()`可以接收任意多个参数,也就是可以拼接任意多个字符串。
```
var stringValue = "hello ";
var result = stringValue.concat("world"， "!");
alert(result);   //"hello world!"
alert(stringValue);  //"hello"
```
+ `concat()`虽然是专门用来拼接字符串的方法，但实践中使用更多的还是加号操作符(+)。而且加号操作符在大多数情况下都比使用concat()方法要简单易行（特别是在拼接多个字符串的情况下）。
+ 三个基于子字符串创建新字符串的方法：`slice()`、`substr()`、`substring()`。这三个方法都会返回被操作字符串的一个子字符串。它们都接受一或两个参数，第一个参数指定子字符串的开始位置，第二个参数（在指定的情况下）表示子字符串到哪里结束。
+ `slice()`和`substring()`的第二个参数指定的是子字符串最后一个字符后面的位置，而`substr()`的第二个参数指定的是返回的字符个数（不包含空格？）。如果没有给这些方法传递第二个参数，则将字符串的末尾作为结束位置。这三个方法对原字符串没有任何影响。
```
var stringValue = "hello world";
alert(stringValue.slice(3));  //"lo world"
alert(stringValue.substring(3));  //"lo world"
alert(stringValue.substr(3));  //"lo world"  
alert(stringValue.slice(3，7));  //"lo w"
alert(stringValue.substring(3,7));  //"lo w"  空格也会算一个位置
alert(stringValue.substr(3, 7));  //"lo worl"
```
+ 在传递给这些方法的参数是负值的情况下，`slice()`方法会将传入的负值与字符串的长度相加，`substr()`方法将负的第一个参数加上字符串的长度，而将负的第二个参数转换为0.`substring()`方法会把所有负值参数都转换为0。
```
var stringValue = "hello world";
alert(stringValue.substring(3,-4));  //"hel"
alert(stringValue.substr(3,-4));  //"" (空字符串)
```
+ `substring()`方法会将较小的数作为开始位置，将较大的数作为结束位置，因此`substring(3，0)`相当于调用了`substring(0,3)`。

###### 3.字符串位置方法
+ `indexOf()`和`lastIndexOf()`都是从一个字符串中搜索给定的子字符串，然后返回子字符串的位置（如果没找到，返回-1）。区别在于，indexOf()方法从字符串的开头向后搜索子字符串，而lastIndexOf()从末尾向前搜索。
```
var stringValuef="Lorem in"
var positions = new Array();
var pos = stringValue.indexOf("e");

while(pos>-1){
    positions.push(pos);
    pos= stringValue.indexOf("e",pos+1);
    
}

```
###### 4.trim()方法
+ 这个方法会创建一个字符串的副本，删除前置和后缀的所有空格，然后返回结果
###### 5.字符串大小写转换方法
+ `toLowerCase()`
+ `toUpperCase()`

###### 6.字符串的模式匹配方法
+ `match()`方法只接受一个参数，要么是一个正则表达式，要么是一个RegExp()对象，返回一个数组。在字符串上调用这个方法，本质上与调用RegExp的exec()方法相同。
```
var text = "cat, bat, sat, fat";
var pattern = /.at/;

//与pattern.exec(text)相同
var matches = text.match(pattern);
alert(matches[0]);  //"cat"
```
本例中的match()方法返回了一个数组，如果是调用RegExp对象的exec()方法并传递本例中的字符串作为参数，那么也会得到与此相同的数组。
+ `search()`的唯一参数与match()方法相同，该方法返回字符串中第一个匹配项的索引；如果没有找到匹配项，则返回-1。
```
var text = "cat, bat, sat, fat";
var pos = text.search(/at/);
alert(pos);  //1???
```
+ `replace()`方法接收两个参数：第一个参数可以是一个RegExp对象或者一个字符串（这个字符串不会被转换成正则表达式）,第二个参数可以是一个字符串或者一个函数。如果第一个参数是字符串，那么只会替换第一个子字符串。要想替换所有子字符串，唯一的方法就是提供一个正则表达式，而且要指定全局（g）标志。
```
var text = "cat, bat, sat, fat";
var result = text.replace("at", "ond");
alert(result);  //"cond,bat,sat,fat"

result = text.replace(/at/g, "ond");
alert(result);  //"cond, bond, sond, fond"
```
+ 如果第二个参数是字符串，那么还可以使用一些特殊的字符序列，将正则表达式操作得到的值插入到结果字符串中。ES提供的特殊的字符序列如下：
    + 字符序列：替换文本
    + $$:$
    + $&:匹配整个模式的子字符串，与RegEXP.lastMatch的值相同。
    + $':匹配的子字符串之前的子字符串。与RegExp.leftContext相同
    + $`:匹配的子字符串之后的子字符串。与RegExp.rightContext相同
    + $n:匹配第n个捕获组的子字符串，其中n等于0~9。例如，$1是匹配第一个捕获组的子字符串，$2是匹配第二个捕获组的子字符串，以此类推。如果正则表达式中没有定义捕获组，则使用空字符串。
    + $nn:匹配第nn个捕获组的子字符串，其中nn等于01~99。例如，$01是匹配第一个捕获组的子字符串，$02是匹配第二个捕获组的子字符串，以此类推。如果正则表达式中没有定义捕获组，则使用空字符串。
```
var text = "cat, bat, sat, fat";
result = text.replace(/(.at)/g,"word($1)");
alert(result);  //word(cat), word(bat), word(sat), word(fat)
```
+ `replace()`方法的第二个参数也可以是一个函数，在只有一个匹配项（即与模式匹配的字符串）的情况下，会向这个函数传递3个参数：模式的匹配项、模式匹配项在字符串中的位置和原始字符串。在正则表达式中定义了多个捕获组的情况下，传递给函数的参数依次是模式的匹配项、第一个捕获组的匹配项、第二个捕获组的匹配项……，但最后两个参数仍然分别是模式的匹配项在字符串中的位置和原始字符串。这个函数应该返回一个字符串，表示应该被替换的匹配项。使用函数作为replace()方法的第二个参数可以实现更加精细的替换操作。
```
function htmlEscape(text){
    return text.replace(/[<>"&]/g, function(match, pos, originalText){
        switch(match) {
            case "<":
                return "&lt;";
            case ">":
                return "&gt;";
            case "&":
                return "&amp;";
            case "\"":
                return "&quot;";
        }
    });
}

alert(htmlEscape("<p class=\"greeting\">Hello world!</p>"));
//&lt;p class=&quot;greeting&quot;&gt;Hello world!&lt;/p&gt;
```
+ `split()`方法可以==基于指定的分隔符将一个字符串分成多个子字符串== ，并将结果放在一个数组中，分隔符可以是字符串，也可以是一个RegExp()对象（这个方法不会将字符串看成正正则表达式）。split()方法可以接受可选的第二个参数，用于指定数组的大小，以便确保返回的数组不会超过既定大小

###### 7.localeCompare()
+ 这个方法比较两个字符串，并返回下列值中的一个
    + 如果字符串在字母表中应该排在字符串参数之前，则返回一个负数
    + 如果字符等于富川参数，则返回0
    + 如果字符串在字母表中应该排在字符串参数之后，则返回一个整数








***

## 5.7 单体内置对象
#### 5.7.2 Math对象
+ ES还为保存数学公式和信息提供了一个公共位置，即Math对象。
###### Math对象的属性
+ Math对象包含的属性大都是数学计算中可能会用到的一些特殊值。
    + `Math.E`、`Math.LN10`、`Math.LN2`、`Math.PI`:常量的值
    + `Math.LOG2E`:以2为底e的对数。`LOG10E`同理。
    + `Math.SQRT1_2`:1/2的平方根（即2的平方根的倒数）
    + `Math.SQRT2`:2的平方根
###### min()和max()方法
+ 这两个方法都可以==接收任意多个数值==参数常用于避免多余的循环
```
var max = Math.max(3, 54, 32, 16);
alert(max);  //54

var min = Math.min();
```
+ 要想直接传入数组名称，需要使用`apply()`方法, 可以将任何数组作为第二个参数。
```
var values = [1, 2, 3, 4, 5, 6];
var max =  Math.max.apply(Math, values);
```
###### 舍入方法
+ 将小数值舍入为整数的几个方法
    + `Math.ceil()` 向上舍入
    + `Math.floor()`向下舍入
    + `Math.round()`四舍五入
```
alert(Math.ceil(25.9)); //26

alert(Math.floor(25.9));   //25

alert(Math.round(25.9));   //26
```
###### random（）方法
+ Math.random()方法返回大于等于0小于1的一个随机数。
+ 从某个==整数范围==内随机选择一个值，套用下面的公式：

==值= Math.floor(Math.random() * 可能值的总数 + 第一个可能的值）==
```
//想要选择一个介于2到10之间的整数值
var num = Math.floor(Math.random() * 9 + 2);
```
+ 可以包装成函数：
```
function selectFrom(lowerValue, upperValue) {
    var choices = upperValue -lowerVaule + 1;
    return Math.floor(Math.random() * choices + lowerValue);
}

var num = selectFrom(2,10);//获得2和10中的一个随机数

//获得数组中某个字符串
var colors = ["red", "green", "blue", "yellow"];
var color = colors[selectFrom(0,color.length-1)];
```
+ 其他方法
    + 绝对值`Math.abs(num)`  
    + 返回Math.E的num次幂：`Math.exp(num)`
    + 求自然对数：`Math.log(num)`
    + 求num的power次幂：`Math.pow(num,power)`
    + 平方根：`Math.sqrt(num)`
    + 求三角函数值：`Math.cos(x)`、`Math.sin(x)`、`Math.tan(x)`
    + 求反三角函数值：`Math.atan(x)`、`Math.asin(x)`、`Math.acos(x)
    + 返回y/x的反正切值：`Math.atan2(y,x)`


# 第6章 面向对象的程序设计
##  6.1 理解对象
+ 创建自定义对象的最简单方式就是1. 创建一个Object的实例
```
var person = new Object();
person.name = "Nicholas";
person.sayName = function(){
    alert(this.name);
};
```
+ 2.对象字面量语法
```
var person = {
    name："Nicholas",
    age:29,
    job: "SoftWare Engineer",
    
    sayName: function(){
        alert(this.name);
    }
};
```
#### 6.1.1 属性类型
+ ES中的对象有两种属性：数据属性和访问器属性。
###### 1. 数据属性
+ 数据属性包含一个数据值的位置，在这个位置可以读取和写入值：
    + [[Configurable]]: 表示能能否通过delete删除属性从而重新定义属性，能否修改属性的特性，或者能否把属性修改为访问器属性。默认值为true。一旦把属性定义为不可配置的，就不能再把它变回可配置了。
    + [[Enumerable]]: 标书能否通过for-in循环返回属性。默认值为true
    + [[Writable]]: 表示能否修改属性的值。默认为true
    + [[Value]]: 包含这个属性的值。读取属性值的时候，从这个位置读取；写入属性值的时候，把新值保存在这个位置。默认值为undefined.
+ 要修改属性==默认的特性==，必须使用ES5的Object.defineProperty()方法，接收三个参数：属性所在的对象、属性的名字和一个描述符对象。描述符对象的属性必须是：configurable、enumerable、writable和value。
```
var person = [];
Object.defineProperty(person, "name", {
    configurable: false,
    writable: false,
    value: "Nicholas"
});

delete person.name;  //无效
```
+ 多数情况下，可能都没有必要利用Object.defineProperty()方法提供的这些高级功能，不过，理解这些概念对理解JS对象非常有用。
###### 2.访问器属性

## 6.2 创建对象
+ Object构造函数或对象字面量都可以用来创建单个对象，明显的==缺点==：使用同一个借口创建很多对象，会产生大量的重复代码
#### 6.2.1 工厂模式
+ 工厂模式抽象了创建具体对象的过程，考虑到==ES中无法创建类==，开发人员就发明了一种函数，用函数来封装以特定借口创建对象的细节
```
function createPerson(name, age, job) {
    var o = new Object();
    o.name = name;
    o.age = age;
    o.job = job;
    o.sayName = function(){
        alert(this.name);
    };
    return o;
}

var person1 = createPerson("Nicholas", 29, "Software Engineer");
```
+ 缺点：没有解决对象识别的问题，即怎样知道一个对象的类型。

#### 6.2.2 构造函数模式
+ ES中的构造函数可用来创建特定类型的对象，像Object和Array这样的==原生构造函数==，在运行时会自动出现在执行环境中，此外，也可以创建自定义的构造函数，从而定义自定义对象类型的属性和方法。
+ 构造函数模式相对于工厂模式的不同之处：
    + 没有显式地创建对象；即没有`new Object`语句
    + 直接将属性和方法赋给了==this==对象
    + 没有==return==语句
    + 构造函数始终都应该以一个==大写字母开头==，而非构造函数则应该以一个小写字母开头。
```
function Person(nam, age, job){
    this.name = name;
    this.age = age;
    this.job = job;
    this.sayName = function(){
        alert(this.name);
    };
}

var person2 = new Person("Nicholas", 29, "Software Engineer");
```
+ 上面的person2对象有一个constructor(构造函数)属性，该属性指向Person。对象的construstor属性最初是用来标识对象类型的，但是，提到检测对象类型，用instance操作符更可靠一些。
```
alert(person2.constructor == Person);//true
alert(person2 instanceof Object);//true
alert(person2 instanceof Person);//true
```
###### 1. 将构造函数当做函数
+ 构造函数与其他函数的唯一区别，就在于调用他们的方式不同，不过，构造函数毕竟也是函数，不存在定义构造函数的特殊语法。==任何函数，只要通过new操作符来调用，那他就可以作为构造函数==；而任何函数，如果不通过new操作福来调用，那他跟普通函数也不会有什么两样。
###### 2. 构造函数的问题
+ 构造函数模式的缺点：每个方法都要在每个实例上重新创建一遍。以这种方式创建函数，会导致不同的作用域和标识符解析，但创建Function新实例的机制仍然是相同的。不同实例上的同名函数是不相等的。
+ 在全局作用域中定义的函数实际上只能被某个对象调用，这让全局作用域有点名不副实，更让人无法接受的是：如果对象需要定义很多方法，那么就要定义很多个全局函数，于是我们这个自定义的引用类型就丝毫没有封装性可言了。
#### 6.2.3原型模式
+ prototype就是通过调用构造函数而创建的那个对象实例的原型对象，使用原型对象的好处是可以让所有对象实例共享它所包含的属性和方法。换句话说，不必在构造函数中定义对象实例的信息，而是可以将这些信息直接添加到原型对象中。
```
function Person(){
    
}
Person.prototype.name = "N"；
Person.prototype.age = 29;
Person.prototype.job = "Software Engineer";
Person.prototype.sayName = function(){
    alert（this.name);
};

var 
```
###### 1.理解原型对象
+ 无论什么时候，只要创建了一个新函数，就会根据一组特定的规则为该函数创建一个prototype属性，这个属性指向函数的原型对象。在默认情况下，所有原型对象都会自动获得一个constructor(构造函数)属性，这个属性是一个指向prototype属性所在函数的指针，
+ 创建了自定义的构造函数之后，其原型对象默认只会取得construstor属性；至于其他方法，则都是从Object继承而来的，当调用构造函数创建一个新实例后，该实例的内部将包含一个指针（内部属性），指向构造函数的原型对象。这个指针叫[[Protoype]].虽然在脚本中没有标准的方式访问[[Protoype]]，但Firefox,Safari和Chorme在每个对象上都支持一个属性_proto_;而在其他实现中，这个属性对脚本则是完全不可见的。不过，要明确的真正重要的一点是，==这个连接存在于实例与构造函数的原型对象之间==，而不是存在于实例与构造函数之间。
+ 虽然在所有实现中都无法访问到[[Prototype]],但可以通过isPrototypeOf()方法来确定对象之间是否存在这种关系，从本质上讲，如果[[Prototype]]指向调用isPrototypeOf()方法的对象
+ ES5增加了一个新方法，叫Object.getPrototypeOf(),这个方法返回[[Prototype]]的值。
```
alert(Object.getPrototypeOf(person1) == Person.prototype);//true
```
+ 每当代码读取某个对象的某个属性时，都会执行一次搜索，目标是具有给定名字的属性，==搜索首先从对象实例本身开始，再继续搜索指针指向的原型对象==。
+ 不能通过对象实例重写原型中的值，若创建了同名属性，则实例中的属性将会屏蔽原型中的那个属性。
+ 使用hasOwnProperty()方法可以检测一个属性时存在于实例中，还是存在于原型中。这个方法时从Object继承来的，只在给定属性存在于==对象实例==中时，才会返回true.
###### 2. 原型与in操作符
+ 有两种方式使用in操作符：单独使用和在for-in循环中，在单独使用时，in操作符会在通过对象能够返回属性时返回true，
###### 3. 更简单的原型语法
+ 用一个包含所有属性和方法的对象字面量来重写整个原型对象
```
function Person(){
    
}
Person.prototype = {
    //如果constructor属性的值真的很重要，可以特意将她设置回适当的值
    //constructor:Person,
    name : "Nicholas",
    age : 29,
    sayName : function(){
        
    }
};
```
+ 这种写法的唯一不同的结果是：construstor属性不再指向Person了。每创建一个函数，就会同时创建它的prototype对象，这个对象也会自动获得construstor属性。而我们在这里使用的语法，==本质上完全重写了默认的prototype对象，因此construstor属性也变成了新对象的construstor(指向Object构造函数)==。
```
var friend = new Person();

alert(friend instanceof Object);//true
alert(friend instanceof Person);//true
alert(friend.constructor == Person); //false
alert(friend.constructor == Object);//true
```
+ 注意，以这种方式重设construtor属性会导致他的[[Enumerable]]特性被设置为true。默认情况下，原声的constructor属性是不可枚举的。
```

```
###### 4.原型的动态性
+ 由于在原型中查找值得过程是一次搜索，因此我们对原型对象所做的任何修改都能立即从实力上反映出来--即使是先创建了实例后再修改原型也是如此。
+ 尽管可以随时为原型添加属性和方法，并且修改能够立即在所有原型实例中反映出来，但如果是重写整个原型对象，那么情况就不一样了。调用构造函数时会为实例添加一个指向最初原型的[[Prototype]]指针，而把原型改为另外一个对象就等于切断了构造函数与最初原型之间的联系。
###### 5. 原生对象的原型
+ 原型模式的重要性不仅体现在创建自定义类型方面，就连所有原生的引用类型（如Object、Array、String，等等），都是采用这种方式创建的。
```
alert(typeof Array.prototype.sort);  //"function
```
+ 通过原生对象的原型，不仅可以取得所有默认方法的引用，而且可以定义新方法，可以像修改自定义对象的原型一样修改原生对象的原型，因此可以随时添加方法。下面的代码就给基本包装类型String添加了一个名为startsWith()方法。
```
String.prototype.startsWith = function(text){
    return this.indexOf(text) == 0;
}

var msg = "Hello World!"
alert(msg.starsWith("Hello")); //true
```
+ 不推荐在产品化的程序中修改原生对象的原型，如果因某个实现中缺少某个方法，就在原生对象的原型中添加这个方法，那么当在另一个支持该方法的实现中运行代码时，就可能会导致命名冲突。而且，这样做也可能会意外地重写原生方法。
###### 6. 原型对象的问题
+ 首先，它省略了为构造函数传递初始化参数这一环节，结果所有实例在默认情况下都将取得相同的属性值。
+ 原型中所有属性是被很多实例共享的，这种共享对于==包含引用类型值（如某个属性的值是数组）的属性==来说，问题比较突出，这样的话如果某个实例改变了这个数组的值，就会在所有实例中表现出来。这个问题是很少有人单独使用原型模式的原因所在。

#### 6.2.4 组合使用构造函数和原型模式
+ 创建自定义类型的最常见方式，就是组合使用构造函数模式与原型模式，构造函数模式用于==定义实例属性（如数组）==，而原型模式用于方法和共享的属性。
+ 好处：
    + 每个实例都会有自己的一份实例属性的副本，但同时又共享着对方法的引用，最大限度地节省了内存。
    + 这种混成模式还支持向构造函数传递参数。
+ 这种构造函数与原型混杂的模式，是目前最广泛、认同度最高的一种创建自定义类型的方法，可以说，这是用来定义引用类型的一种默认模式。
```
//构造函数模式部分
function Person(name,age, job){
    this.name = name;
    this.age = age;
    this.job = job;
    this.friends = ["Shelby", "Court"];
}

//原型模式部分
Person.prototype = {
    consructor: Person,
    sayName : function(){
        alert(this.name);
    }
}
```
#### 6.2.5 动态原型模式
+ 动态原型模式把所有信息都封装在了构造函数中，而通过在构造函数中初始化原型（仅在必要的情况下），又保持了同时构造函数和原型的优点，换句话说，可以通过检查某个应该存在的方法是否有效，来决定是否需要初始化原型。
```
function Person(name, age, job){
    
    //属性
    this.name = name;
    this.age = age;
    this.job = job;
    
    //方法
    if(typeof this.sayName != "function"){
        Person.prototype.sayName = function(){
            alert(this.name);
        };
    }
}
```
这段代码只会在初次调用构造函数时才会执行，此后，原型已经初始化，不需要再做什么修改了。

#### 6.2.6 寄生构造函数模式
+ 在前述的几种模式都不适用的情况下，可以使用寄生构造函数模式，这种模式的基本思想是创建一个函数，该函数的作用仅仅是封装创建对象的代码，然后再返回新创建的对象；但从表面上看，这个函数又很像是典型的构造函数。
+ 除了使用new操作符并把使用的包装函数叫做构造函数之外，这个模式跟工厂模式是一模一样的。构造函数在不返回值的情况下，默认会返回新对象实例。而通过在构造函数的末尾添加一个return语句，可以重写调用构造函数时返回的值。
```
function Person(name, age, job){
    var o = new Objec();
    o.name = name;
    o.age = age;
    o.job = job;
    o.sayName = function(){
        alert(this.name);
    };
    return o;
}

var friend = new Person("Nicholas",29, "Software Engineer");
friend.sayName();
```
+ 这个模式可以在特殊的情况下用来为对象构造函数，假设我们想创建一个具有额外方法的特殊数组，由于不能直接修改Array构造函数，因此可以使用这个模式。
#### 6.2.7 稳妥构造函数模式
+ 稳妥对象，指的是没有公共属性，而且其方法也不引用this的对象。稳妥对象最适合在一些安全的环境中（这些环境会禁止使用this和new），或者在防止数据被其他应用程序改动时使用。
+ 稳妥构造函数遵循与寄生构造函数类似的模式，但有两点不同：一是新创建对象的实例方法不引用this；而是不使用new操作符调用构造函数。
```
function Person(name, job, age){
    
    //创建要返回的对象
    var o = new Object();
    //可以再这里定义私有变量和函数
    
    //添加方法
    o.sayName = fucntion(){
        alert(name);  //寄生这里有个this
    };
    
    //返回对象
    return o;
}

var friend = Person("Nicholas",29, "Software Engineer");
friend.sayName();
```
+ 除了使用sayName（）方法之外，没有其他办法访问name的值。即使有其他代码会给这个对象添加方法或数据成员，但也不可能有别的办法访问传入到构造函数中的原始数据。
## 6.3 继承
+ 接口继承只继承方法签名，而实现继承则继承实际的方法。在ES中无法实现接口继承，只支持实现继承，而其==实现继承主要依靠原型链来实现的==。
#### 6.3.1 原型链
+ 原型链的基本思想是利用原型让一个引用类型继承另一个引用类型的属性和方法。假如我们让原型对象等于另一个类型的实例，此时的原型对象将包含一个指向另一个原型的指针，相应的，另一个原型中也包含着指向另一个构造函数的指针。假如另一个原型又是另一个类型的实例，那么上述关系依然成立，如此层层递进，就构成了实例与原型的链条。
+ 实现原型链的基本模式：
```
function SuperType(){
    this.property = true;
}

SuperType.prototype.getSuperType = function(){
    return this.prototype;
};

fucntion SubType(){
    this.subproperty = false;
}

//继承了SuperType
SubType.prototype = new SuperType();

SuperType.prototype.getSubValue = function(){
    return this.subproperty;
};

var instance = new SubType();
alert(instance.getSuperValue()); //true

```
+ 在上面的代码中，没有使用SubType默认提供的原型，而是给他替换了一个原型；这个新原型就是SuperType的实例。于是，新原型不仅具有作为一个SuperType的实例所拥有的全部属性和方法，而且其内部还有一个指针，指向SuperType的原型。最终的结果是这样子的：instance指向SubType原型，SubType的原型又指向SuperType的原型。getSuperValue()方法仍然还在SuperType.prototype中，但prototype则位于SubType.prototype中。
##### 1.别忘记默认的原型Object
##### 2.确定原型和实例的关系
+ 第一种方法：使用instanceof操作符。用这个操作符来测试实例与原型链中出现过的构造函数，结果就会返回true.
```
alert(instance instanceof Object);  //true
alert(instance instanceof SuperType);  //true
alert(instance instanceof SubType);  //true
```
+ 第二种方法：使用`isPrototypeOf()`方法。只要在原型链中出现过的原型，都可以说是该原型链所派生的实例的原型。
```
alert(Object.prorotype.isPrototypeOf());  //true
alert(SuperType.prorotype.isPrototypeOf());  //true
alert(SubType.prorotype.isPrototypeOf());  //true
```
##### 3. 谨慎地定义方法
+ 子类型有时候需要覆盖超类型中的某个方法(如上例中的getSuperValue()，重写这个方法会屏蔽原来原型链中的那个方法），或者需要添加超类型中不存在的某个方法。但不管怎样，给原型添加方法的diamante一定要放在替换原型的语句之后。
+ 在通过原型链实现继承时，不能使用对象字面量量创建原型方法，因为这样做会重写原型链。
##### 4.原型链的问题
+ 最主要的问题来自包含引用类型值的原型。在通过原型来实现继承时，原型实际上会变成另一个类型的实例，于是，原先的实例属性也就顺理成章地编变成了现在的原型属性了。
+ 第二个问题是：在创建子类型的实例时，不能向超类型的构造函数中传递参数。实际上，应该说是没有办法在不影响所有对象实例的情况下，给超类型的构造函数传递参数。实践中很少会单独使用原型链。

#### 6.3.2 借用构造函数
+ 借用构造函数技术（有时候也叫作伪造对象或经典继承）是为了解决原型中包含引用类型值的问题。这种技术的基本思想相当简单，即在子类型构造函数的内部调用超类型构造函数。别忘了，函数只不过是在特定环境中执行代码的对象，因此通过apply()和call()方法也可以在（将来）新创建的对象上构造函数。
```
function SuperType(){
    this.colors = ["red", "blue", "green"];
}
fucntion SubType(){
    //继承了SuperType
    SuperType.call(this);
}

var instance1 = new SubType();
instance1.colors.push("black");
alert(instance1.colors);  //"red,blue,green,black"

var instance2 = new SubType();
alert(instance2.colors);  //"red,blue,green"
```
##### 1.传递参数
+ 借用构造函数有一个很大的优势，即可以在子类型构造函数中向超类型构造函数传递参数。
```
function SuperType(name){
    this.name = name;
}

function SubType(){
    //继承了SuperType,同时还传递了参数
    SuperType.call(this,"Nichilas");
    
    //实例属性
    this.age = 29;
}

var instance = new SubType();
alert(instance.name);  //"Nicholas";
alert(instance.age);  //29
```
##### 2. 借用构造函数的问题
+ 如果仅仅是借用构造函数，那么也就无法避免构造函数模式存在的问题--方法都在构造函数中定义，因此函数复用就无从谈起了。而且，在在超类型的原型中定义的方法，对子类型而也是不可见的，结果所有类型都只能使用构造函数模式。因此借用构造函数的技术也是很少单独使用的。

#### 6.3.3 组合继承
+ 组合继承，有时也叫伪经典继承，指的是将原型链和构造函数的技术组合到一块，从而发挥二者之长的一种继承模式。其背后的思路是使用原型链实现对原型属性和方法的继承，而通过借用构造函数实现对实例属性的继承。
```
function SuperType(name){
    this.name = name;
    tihis.colors = ["red", "blue", "green"];
}
SuperType.prototype.sayName = function(){
    alert(this.name);
};

function SubType(name, age){
    
    //继承属性
    SuperType.call(this,name);
    
    this.age = age;
}

//继承方法
SubType.prototype = new SuperType();
SubType.prototype.constructor = SubType;
SubType.prototype.sayAge = function(){
    alert(this.age);
};

var instance1 = new SubType("Nicholas", 29);
instance1.colors.push("black");
alert(instance1.colors);  //"red ,blue,green,black"
instance1.sayName();  //"Nicholas"
instance1.sayAge();  //29

var instance2 = new SubType("Greg", 27);
alert(instance2.colors);   //"red, blue, greeen"
instance1.sayName();  //"Greg"
instance2.sayAge();   //27
```
#### 6.3.4 原型式继承
+ 借助原型可以基于已有的对象创建新对象，同时还不必因此创建自定义类型。
```
function object(o){
    function F(){}
    F.prototype = o;
    return new F();
}
```
+ 在object()函数内部，先创建了一个临时性的构造函数，然后将传入的对象作为这个构造函数的原型，最后返回了这个临时类型的一个新实例。从本质上讲，object()对传入其中的对象执行了一次浅复制。
```
var person = {
    name:"Nicholas",
    friends:["Shelby","Court","Van"]
};

var anotherPerson = object(person);
anotherPerson.name ="Greg";
anotherPerson.friends.push("Rob");

var yetAnotherPerson = object(person);
yetAnotherPerson.name = "Linda";
yetAnotherPerson.friends.push("Barbie");

alert(person.friends);"Shelby,Court,Van,Rob,Barbie"
```
























# 第8章 BOM
## 8.1 window对象
#### 8.1.4 窗口大小
#### 8.1.5 导航和打开窗口
+ 使用window.open()方法既可以导航到一个特定的URL，也可以打开一个新的浏览器窗口。这个方法可以接收4个参数：要加载的URL、窗口目标、一个特性字符串以及一个表示新页面是否取代浏览器历史记录中当前加载页面的布尔值。通常只需传递第一个参数，最后一个参数只在不打开新窗口的情况下使用
+ 如果为window.open()传递了第二个参数，表示该参数是已有窗口或框架的名称，那么就会在具有该名称的窗口或框架中加载第一个参数指定的URL。
```
//等同于<a href="http://www.wrox.com" target="topName"></a>
window.open("http://www.wrox.com/","topName");
```
+ 此外，第二个参数俺也可以是：_self、_parent、_top或_blank
##### 1.弹出窗口
+ 如果给window.open()传递的第二个参数并不是一个已经存在的窗口或框架，那么该方法就会根据在第三个位置上传入的字符串创建一个新窗口或新标签页。如果没有传入第三个参数，那么就会打开一个带有全部默认设置（工具栏、、地址栏和状态栏等）的新浏览器窗口（或者打开一个新标签页--根据浏览器设置）.在不打开新窗口的情况下，会忽略第三个参数。
+ 第三个参数是一个逗号分隔的设置字符串，表示在新窗口中都显示哪些特性。这个字符串中的设置选项如下：
    + fullscreen:yes或no，表示浏览器窗口是否最大化，仅限IE
    + height、left、top、width
    + location：yes或no,表示是否在浏览器窗口中显示地址栏，不同浏览器的默认值不同，如果设置为no,地址栏可能会隐藏，也可能会被禁用
    + menubar: yes或no,表示是否在浏览器窗口中显示菜单栏，默认值为no
    + resizable:是否可以通过拖动浏览器窗口的边框改变其大小，默认值为no
    + scrollbars：如果内容在视口中显示不下，是否允许滚动，默认值为no
    + status：是否在浏览器中显示状态栏，默认no
    + toolbar:是否在浏览器窗口显示工具栏，默认no
```
window.open("http://www.wrox.com/","wroxWindow","height=400,width=400,top=10,left=10,resizable=yes");
```
+ close(）方法可以关闭通过window.open()打开的弹出窗口。
+ 新创建的window对象有一个opener属性，指向打开它的原始窗口对象，这个属性只在弹出窗口中的最顶层window对象（top)中有定义。
```
var wroxWin = window.open("http://www.wrox.com/","wroxWindow","height=400,width=400,top=10,left=10,resizable=yes");
alert(wroxWin.opener == window);//true
```
+ 虽然弹出窗口中有一个指针指向打开它的原始窗口，但原始窗口中并没有这样的指针指向弹出窗口。
+ 有些浏览器会在独立的进程中运行每个标签页。当一个标签页打开另一个标签页时，如果两个window对象之间需要彼此通信，那么新标签页就不能运行在独立的进程中。在Chorme中，将新创建的标签页的opener属性设置为null,就是告诉浏览器新创建的标签页不需度再要与打开它的标签页通信，因此可以在独立的进程中运行。
```
var wroxWin = window.open("http://www.wrox.com/","wroxWindow","height=400,width=400,top=10,left=10,resizable=yes");
wroxWin.opener=null;
```
##### 2.安全限制
##### 3.弹出窗口屏蔽程序
### 8.1.6 间歇调用和超时调用
+ Javascript是单线程语言，但它允许通过设置超时值和间歇时间值来调度代码在特定的时刻执行。
+ 超时调用需要使用window对象的setTimeout()方法，它接受两个参数：要执行的代码和以毫秒表示的时间。其中，第一个参数可以时一个包含JS代码的字符串（就和在eval()函数中使用的字符串一样），也可以是一个函数。
```
//不建议传递字符串,因为可能导致性能损失
setTimeout("alert('Hello World!')", 1000);

//推荐的调用方法
setTimeout(function() {

    alert("Hello World!")
}, 1000);
```
+ 超时调用经过设定的时间后指定的代码不一定会执行，JS是一个单线程的解释器，因此一定时间内只能执行一段代码。为了控制要执行的代码，就有一个JS任务队列。setTimeout()的第二个参数是告诉JS再过多长时间把当前任务添加到队列中。如果队列是空的，那么添加的代码会立即执行。
+ 调用setTimeout()方法之后，会返回一个数值ID，可以通过它来取消超时调用。只要在指定的时间尚未过去之前调用clearTimeout()，就可以完全取消超时调用，下面的代码就跟什么都没有发生一样。
```
//设置超时调用
var timeoutId = setTimeout(function(){
    alert("Hello World!");
}, 1000);

//把它取消
clearTimeout(timeoutId);
```
+ 超时调用的代码都是在全局作用域中执行的，因此函数中this的值在非严格模式下指向window对象，在严格模式下是undefined。
+ 间歇调用会执行到被取消或者页面被卸载。setInterval()与setTimeout()接受的参数相同。
+ 取消间歇调用的重要性要远远高于取消超时调用，因为在不加干涉的情况下，间歇调用将会一直执行到页面卸载。以下是一个常见的使用间歇调用的例子。
```
var num = 0;
var max = 10;
var intervalId = null;

function incrementNumber() {
    num++;
    
    //如果执行次数达到了max设定的值，则取消后续尚未执行的调用
    if(num == max) {
        clearInterval(intervalId);
        alert("Done")；
    }
}
//变量num每半秒钟递增一次，当递增到最大值时就会取消先前设定的间歇调用。
intervalId = setInterval(incrementNumber, 500);
```
+ 上面的例子也可以用setTimeout()来实现，注意，第一个参数虽然是函数，但是后面不用加括号。
```
var num = 0;
var max = 10;

function incrementNumber() {
    num++;
    
    //如果执行次数未达到max设定的值，则设置另一次超时调用
    if (min < max) {
        setTimeout(incrementNumber, 500);
    } else {
        alert("Done");
    }
}

setTimeout(incrementNumber, 500);
```
+ 一般认为，使用超时调用来模拟间歇调用是一种最佳模式。在开发环境下，很少使用真正的间歇调用，原因是后一个间歇调用可能会在前一个间歇调用结束之前启动，而像前面示例中那样使用超时调用，则可以完全避免这一点。

## 8.2 location对象
+ location它提供了与当前窗口中加载的文档有关的信息，还提供了一些导航功能。location对象是一个很特别的一个对象，因为它既是window对象的属性，也使document对象的属性,`window.location`和`document.location`引用的是同一个对象。
+ location对象的用处不只表现在他保存着当前文档的信息，还表现在它==将URL解析为独立的片段==，让开发人员可以通过不同的属性访问这些片段.location对象的所有属性（省略了每个属性前面的location前缀）
    + `hash`:返回URL中的hash(#号后跟零或多个字符)，如果URL中不包含散列，则返回空字符串。如"#contents"
    + `host`:返回服务器名称和端口号（如果有）。“www.wrox.com:80"
    + `hostname`：发挥不带端口号的服务器名称。“www.wrox.com"
    + `href`: 返回当前加载页面的完整URL。而location对象的toString()方法也返回这个值。“http:/www.wrox.com"
    + `pathname`:返回URL中的目录和（或）文件名。“/WileyCDA/"
    + `port`:返回URL中指定的端口号。如果URL中不包含端口号，则这个属性返回空字符串。“8080”
    + `protocol`:返回页面使用的协议。通常数`http:`或`https：`
    + `search`:返回URL中的查询字符串。这个字符串以问号开头。“？q=javascript"

#### 8.2.1 查询字符串参数
+ 通过上面的属性可以访问到location对象的大多数信息，但其中访问URL包含的查询字符串的属性并不方便。尽管`location.search`返回从问号到URL末尾的所有内容，但却没有办法逐个访问其中的每个查询字符串参数。为此，可以创建一个函数，用以解析查询字符串，然后返回包含所有参数的一个对象。
```
//根据和号（&）来分割查询字符串，并返回name=value格式的字符串数组。下面的
//for循环会迭代这个数组，然后再根据等于号分割每一项，从而返回第一项为参数
//名，第二项为参数值的数组。再使用decodeComponent()分别解码name和value
//(因为查询字符串应该是被编码过的）。

function getQueryStringArgs(){
    //取得查询字符串并去掉开头的问号
    var qs = (location.search.length > 0 ? location.search.substring(1) : " "),
    
    //保存数据的对象
    args = {},
    
    //取得每一项
    items = qs.length ? qs.split("&") : [],
    item = null,
        name = null,
        value = null,
        
    //在for循环中使用
    i = 0,
    len = items.length;
    
    //逐个将每一项添加到args对象中
    for (i=0; i <len; i++) {
        item = items[i].split("=");
        name = decodeURIComponent(item[0]);
        value = decodeURIComponent(item[1]);
        
        if (name.length) {
            args[name] = value;
        }
    }
    
    return args;
}


```
#### 8.2.2 位置操作
+ 使用location对象可以通过很多方式来改变浏览器的位置，也就是立即打开新URL并在浏览器的历史记录中生成一条记录。可以使用`assign()`方法并为其传递一个URL；也可以将`location.href`或`window.location`设置为一个URL值，也会以该值调用`assign()`方法，这样与显式调用assign()方法的效果完全一样。
```
location.assign("http://www.wrox.com");

window.location = "http://www.wrox.com";
location.href = "http://www.wrox.com";
```
+ 另外，修改location对象的其他属性（如hash、search、hostname、pathname和port等)也可以改变当前加载的页面。每次修改location的属性(hash除外)，页面都会以新URL重新加载。
```
//将初始URL“http://www.wrox.com/WileyCDA改为“http://www.yahoo.com/mydir/"
location.hostname = "www.yahoo.com";
```
+ 用户通过单击“后退”按钮都会导航到前一个页面。要禁用这种行为，可以使用`replace()`方法，这个方法只接受一个参数，即要导航到的URL；结果虽然会导致浏览器位置改变，但不会在历史记录中生成新纪录,此时用户不能回到前一个页面内。
```
location.replace("http://www.wrox.com/");
```
+ `reload（）`方法作用是重新加载当前显示的页面。如果调用`reload()`时不传递任何参数，页面会以最有效的方式重新加载，也就是说，如果页面上次请求以来并没有改变过，页面就会从==浏览器缓存中==重新加载。如果要==强制从服务器==重新加载，
***
# 第10章
## 10.1 节点层次
#### 10.1.1 Node类型
+ DOM级定义了一个Node接口，该接口将有DOM中的所有节点类型实现，这个Node接口在JS中是作为Node类型实现的；JS中的所有节点类型都继承自Node类型，因此所有节点类型都共享着相同的基本属性和方法。
+ 每个节点都有一个nodeType属性，用于指明节点的类型，所有的节点类型如下：
    + Node.ELEMENT_NODE(1)
    + Node.ATTRIBUTE_NODE(2)
    + Node.TEXT_NODE(3)
    + Node.CDATA_SELECTION_NODE(4)
    + 
```
if(someNode.nodeType == Node.ELEMENT_NODE){//在IE中无效
    alert("Node is an element");
}

if(someNode.nodeType == 1){//适用于所有浏览器
    alert("ode is an element");
}
```
+ 并不是所有节点类型都收到Web浏览器的支持，开发人员最常用的就是元素和文本节点。
###### 1.nodeName和nodeValue属性
+ 对于元素节点，nodeName中保存的始终是元素的标签名，而nodeValue的值始终是null.
###### 2.节点关系（只读)
+ 每个节点都有一个childNodes属性，（也有firstChild和lastChild属性），其中保存着一个NodeList对象，
+ NodeList是一种类数组对象，用于保存一组有序的节点，可以通过位置来访问这些。。虽然可以通过方括号语法来访问NodeList的值，而且这个对象也有length属性，但他并不是Array的实例，
+ NodeList对象的独特之处在于，它实际上是基于DOM结构动态执行的结果，因此DOM结构的变化能够自动反映到该对象中。
+ 对arguments对象使用Array.prototype.slice()方法可以将其转换为数组，采用同样的方法，也可以将NodeList对象转换为数组。
```
var arrayOfNodes = Array.prototype.slice.call(someNode.childNodes,0);
```
+ 每个节点都有一个parentNode属性，该属性指向文档树中的父节点。
+ 同胞节点通过previousSiblling和childSibling属性访问。
+ hasChildNodes()是一个非常有用的方法，比查询childNodes列表的length属性更简单。
+ 所有节点还有一个ownerDocument属性，指向整个文档的文档节点
###### 3.操作节点
+ appendChild()用于向childNodes列表的末尾添加一个节点，并返回新的节点。
+ 如果传入到appendChild()中的节点已经是文档的一部分了，那结果就是将该节点从原来的位置转移到新位置，如果在调用appendChild（）时传入了父节点的第一个子节点，那么该节点就会成为父节点的最后一个子节点。

#### 10.1.4 Text类型
+ 文本节点由Text类型表示，包含的是可以照字面解释的纯文本内容，纯文本中可以包含转义后的HTML字符，但不能包含HTML代码。Text节点有以下特征：
    + nodeType的值为3
    + nodeName的值为“#text"
    + nodeValue的值为节点所包含的文本
    + parentNode是一个Element；
    + 没有子节点
+ 可以通过nodeValue属性或data属性访问Text节点中包含的文本，这两个属性中包含的值相同
+ 在默认情况下，每个可以包含内容的元素最多只能有一个文本节点，而且必须确实有内容存在。开始和结束标签之间只要存在内容，就会创建一个文本节点。
```
<!--有空格，因而有一个文本节点-->
<div> </div>

<!--没有内容，也就没有文本节点-->
<div></div>
```
+ 对于DOM元素，children是指DOM Object类型的子对象，不包括tag之间隐形存在的TextNode；childNodes包括==tag之间隐形存在==的TextNode对象。













***
# 第13章
+ JS与HTML之间的交互是通过事件实现的。事件，就是文档或浏览器窗口中发生的一些特定的交互瞬间。可以使用侦听器（或处理程序）来预订事件，以便事件发生时执行相应的代码。这种在传统软件工程中被称为观察员模式的模型，支持页面的行为与页面的外观之间的松散耦合。

## 13.1 事件流
+ 事件流描述的是页面中接收事件的顺序。IE的事件流是事件冒泡流，而Netscape Communicator的事件流是事件捕获流。

#### 13.1.1 事件冒泡
+ IE中的时间流叫做事件冒泡（event bubbling)，即事件开始时由最具体的元素（文档中嵌套层次最深的那个节点）接收，然后逐级向上传播到较为不具体的节点（文档）。
+ 如果你单击了页面中的<div>元素，那么这个click事件会按照如下顺序传播：（1）<div>(2)<body>（3）<html>(4)document
+ 所有现代浏览器都支持事件冒泡，但在具体实现上有一些区别。IE5.5及更早版本中的事件冒泡会跳过<html>元素（从<body>直接跳到document)。IE9、Firefox、Chorme和Safari则将事件一直冒泡到window对象。

#### 13.1.2 事件捕获
+ 事件捕获的思想是不太具体的节点应该更早接收到事件，而最具体的节点应该最后接收到事件。事件捕获的用意在于在事件到达预定目标之前捕获它。
+ 单击<div>元素会以下列顺序触发click事件：（1）document(2)<html>（3）<body>(4)<div>
+ 由于老版本的浏览器不支持，因此很少有人使用事件捕获，所以放心的使用事件冒泡，在有特殊需要时再使用事件捕获。

#### 13.1.3 DOM事件流
+ “DOM2级事件”规定的事件流包括三个阶段：事件捕获阶段、处于目标阶段和事件冒泡阶段。首先发生的是事件捕获，为截获事件提供了机会。然后是实际的目标接收到事件。最后一个阶段是冒泡阶段，可以在这个阶段对事件做出响应。
+ 多数支持DOM事件流的浏览器都实现了一种特定的行为；即使"DOM2级事件”明确
要求捕获阶段不会涉及事件目标，但IE9、Safari、Firefox和Opera9.5级更高版本都会在捕获阶段触发事件对象上的事件。结果，就是有两个机会在目标对象上面操作事件。

## 13.2 事件处理程序
+ 事件就是用户或浏览器自身执行的某种动作。诸如click、load和mouseover，都是事件的名字。而响应某个事件的函数就叫做事件处理程序（或者事件侦听器）。事件处理程序的名字以“on”开头，因此click事件的时间处理程序就是onclick,load事件的事件处理程序就是onload。为事件指定处理程序的方式有好几种。

#### 13.1 HTML事件处理程序
+ 在HTML中指定事件处理程序有两个缺点。首先，存在一个时差问题。因为用户可能在HTML元素一出现在页面上触发相应的事件，但当时的事件处理程序有可能尚不具备执行条件。以前面的例子来说明，假设showMessage()函数是在按钮下方、页面的最底部定义的。如果用户在页面解析showMessage()函数之前就单击了按钮，就会引发错误。为此，很多HTML时间处理程序都会被封装在一个try-watch块中，以便错误不会浮出水面，如下：
```
<input type="button" value="Click Me" onclick="try{showMessage();}catch(ex){}">
```
+ 这样，如果在showMessage()函数有定义之前单击了按钮，用户将不会看到JS错误，因为在浏览器有机会处理错误之前，错误就被捕获了。
+ 另一个缺点是，这样扩展事件处理程序的作用域链在不同浏览器中会导致不同结果。不同JS引擎遵循的标识符解析规则略有差异，很可能会在访问非限定对象成员时出错。
+ 通过HTML指定事件处理程序的最后一个缺点是HTML与JS代码紧密耦合。如果要更换事件处理程序，就要改动两个地方：HTML代码和JS代码。这正是许多开发人员摒弃HTML事件处理程序，转而使用JS指定事件处理程序的原因所在。
+ 第一种指定事件处理程序的方法，就是给HTML元素设置一个HTML特性，这个特性的名字与事件处理程序同名，这个特性的值是JS代码。例如：
```
<input type="button" value="Click Me" onclick="alert('Clicked')"/>
```
+ 由于这个特性的值是JS，因此不能再其中使用未经转义的HTML语法字符，例如和号（&）、双引号（""）、小于号（<）或大于号（>）。为了避免使用HTML实体，这里使用了单引号，如果想要使用双引号，那么代码改写成：
```
<input type="button" value="Click Me" onclick="alert(&quot;Click&quot;)"/>
```
+ 在HTML中定义的时间处理程序可以包含要执行的具体动作，也可以调用在页面其他地方定义的脚本,也可以被包含在一个外部文件中。事件处理程序中的代码在执行时，有权访问全局作用域中的任何代码。
```
<script type="text/javascript">
    function showMessage(){
        alert("Hello World!");
    }
</script>
<input type="button" value="Click Me" onclick="showMessage()"/>
```
+ 这样指定事件处理程序具有一些独到之处。首先，这样会创建一个封装这元素属性值的函数。这个函数中有一个局部变量event，也就是事件对象。其次，在这个函数内部，this值等于事件的目标元素。
```
<!--输出"click"-->
<input type="button" value="Click Me" onclick="alert(event.type)">

<!--输出"Click Me-->
<input type="button" value="Click Me" onclick="alert(this.value)">
```
+ 关于这个动态创建的函数，另一个有意思的地方是它扩展作用域的方式。在这个函数内部，可以像访问局部变量一样访问document及该元素本身的成员。这个函数使用with像下面这样扩展作用域：
```
function(){
    with(document){
        with(this){
            //元素属性值
        }
    }
}
```
#### 13.2.2 DOM0级事件处理程序
+ 通过JS指定事件处理程序的传统方式，就是讲一个函数入职给一个事件处理程序属性。这种为事件处理程序赋值的方法至今仍然为所有现代浏览器所支持。原因一是简单，二是具有跨浏览器的优势。要使用JS指定事件处理程序，首先必须取得一个要操作的对象的引用。
+ 事件处理程序可以认为是元素的一种属性，每个元素（包括window和document）都用于这些属性，这些属性通常全部小写，例如onclick。将这种属性的值设置为一个函数，就可以指定事件处理程序。
```
var btn = document.getElementById("myBtn");
btn.onclick = function(){
    alert("Clicked");
}
``` 
要注意，在这些代码运行以前不会指定事件处理程序，因此如果这些代码在页面中位于按钮后面，就有可能在一段时间内怎么单击都没有反应。
+ 使用DOM0级方法指定的事件处理程序被认为是元素的方法。因此，这时候的事件处理程序是在元素的作用域中运行；换句话说，程序中的this引用当前元素。
+ ==以这种方式添加的事件处理程序会在事件流的冒泡阶段被处理==，也可以删除通过DOM0级方法指定的事件处理程序：
```
btn.onclick = null;
```
#### 13.2.3 DOM2级事件处理程序
+ “DOM2级事件处理程序”定义了两个方法，用于处理制定和删除事件处理程序的操作：addEventListener()和removeEventListener()。所有DOM节点中都包含这两个方法，并且他们都接收3个参数：要处理的事件名、作为事件处理程序的函数和一个布尔值。最后这个布尔值参数如果是true，表示在捕获阶段调用事件处理程序；如果是false,表示在冒泡阶段调用事件处理程序。
```
var btn = document.getElementById("mybtn");
btn.addEventListener("click",function(){
    alert(this.id);
},false);
```
+ 使用DOM2级方法添加事件处理程序的主要好处是可以添加多个事件处理程序。多个事件处理程序会按照添加它们的顺序触发。
```
var btn = document.getElementById("mybtn");
btn.addEventListener("click",function(){
    alert(this.id);
},false);
btn.addEventListener("click",function(){
    alert("Hello world!");
},false);
```
+ 通过addEventListener()添加的事件处理程序只能使用removeEventListener()来移除；移除时传入的参数与添加事件处理程序时使用的参数相同。这也意味着通过addEventListener()添加的匿名函数将无法移除。
```
var btn = document.getElementById("mybtn");
btn.addEventListener("click",function(){
    alert(this.id);
},false);

btn.removeEventListener("click", function(){//没有用
    alert(this.id);
},false);
```
```
var btn = document.getElementById("myBtn");
var handler = function(){
    alert(this.id);
};
btn.addEventListener("click", handler, false);

btn.removeEventListener("click", handler, false); //有效
```
+ 大多数情况下，都是将事件处理程序添加到事件流的冒泡阶段，这样可以最大限度地兼容各种浏览器。最好只在需要在事件到达目标之前截获它的时候将事件处理程序添加到捕获阶段。

#### 13.2.4 IE事件处理程序
+ IE实现了与DOM中类似的两个方法：attchEvent()和detachEvent()。这两个方法接收相同的两个参数：事件处理程序名称与事件处理程序函数。由于IE8及更早版本只支持事件冒泡，所以通过attchEvent()添加的事件处理程序都会被添加到冒泡阶段。
```
var btn = document.getElementById("mybtn");
btn.attachEvent("onclick",function(){
    alert("Clicked");
});
```
+ 注意，attchEvent()的第一个参数是"onclick"，而非DOM的addEventListener()方法中的"click"。
+ 在IE中使用attchEvent()与使用DOM0级方法的主要区别在于事件处理程序的作用域。在使用DOM0级方法的情况下，事件处理程序会在所属元素的作用域内运行；在使用attchEvent()方法的情况下，事件处理程序会在全局作用域中运行，因此this等于window。在编写跨浏览器代码时，牢记这一区别非常重要。
```
var btn = document.getElementById("myBtn");
btn.attachEvent("onclick",function(){
    alert(this === window);  //true
});
```
+ 与addEventListener()类似，attchEvent()方法也可以用来为一个元素添加多个事件处理程序。但是这些事件处理程序不是以添加它们的顺序执行，而是以相反的顺序被触发。使用attchEvent()添加的事件可以通过detachEvent()来移除，条件是必须提供相同的参数。与DOM方法一样，这也意味着添加的匿名函数将不能被移除。不过，只要能够将对相同函数的引用（即赋予函数一个函数名，再引用其名字）传给detachEvent()，就可以移除相应的事件处理程序。

#### 13.2.5 跨浏览器的事件处理程序
+ 为了以跨浏览器的方式处理事件，不少开发人员会使用能够隔离浏览器差异的JS库，还有一些开发人员会自己开发最合适的事件处理的方法，只要恰当地使用能力检测即可。要保证处理事件的代码能在大多数浏览器下一致地运行，只需关注冒泡阶段。
+ 第一个要创建的方法是addHandler()，它的职责是视情况分别使用DOM0级、DOM2级方法或IE方法来添加事件。这个方法属于一个名叫EventUtil的对象，本书将使用这个对象来处理浏览器间的差异。addHandler()方法接收3个参数：要操作的元素、事件名称和事件处理程序函数。
+ 与addHandler()对应的方法是removeHandler()，它也接收相同的参数。这个方法的职责是移除之前添加的事件处理程序--无论该事件处理程序时采取什么方式添加到元素中的，如果其他方法无效，默认采用DOM0级方法。
```
var EventUtil = {
    addHandler: function(element, type, handler) {
        if(element.addEventListener) {
            element.addEventListener(type, handler, false);
        } else if {
            element.attachEvent("on" + type, handler);
        } else{
            element["on" + type] = handler;
        }
    },
    removeHandler: function(element, type, handler) {
        if (element.removeEventListener) {
            element.removeEventListener(type, handler, false);
        } else if(element.detachEvent) {
            element.detachEvent("on"+type, handler);
        } else {
            element["on"+type] = null;
        }
    }
};
```
+ 可以像下面这样使用Event.Util对象：
```
var btn = document.getElementById("myBtn");
var handler = function(){
    alert("Clicked");
}
EventUtil.addHandler(btn, "click", handler);

EventUtil.removeHandler(btn, "click", handler);
```
+ addHandler()和removeHandler()没有考虑到所有的浏览器问题，例如在IE中的作用域问题，不过，使用它们添加和移除事件处理程序还是足够了。此外还要注意，DOM0级对每个事件只支持一个事件处理程序。好在，只支持DOM0级的浏览器已经没有那么多了。

## 13.3 事件对象
+ 在触发DOM上的某个事件时，会产生一个事件对象event，这个对象中包含着所有与事件有关的信息。包括导致事件的元素、事件的类型以及其他与也定事件相关的信息。例如，鼠标操作导致的事件对象中，会包含鼠标位置的信息，而键盘操作导致的事件对象中，会包含与按下的键有关的信息，所有浏览器都支持event对象，但支持方式不同。

#### 13.3.1 DOM中的事件对象
+ 兼容DOM的浏览器会将一个event对象传入到事件处理程序中，无论指定事件处理程序时使用什么方法（DOM0级或DOM2级），都会传入event对象。
```
var btn = document.getElementById("myBtn");
btn.onclick = function(event) {
    alert(event.type);  //"click"
}
btn.addEventListener("click", function(event) {
    alert(event.type);  //"click"
},false);
```
+ event对象包含与创建它的特定事件有关的属性和方法。触发的事件类型不一样，可用的属性和方法不一样。不过，所有事件都会有下表列出的成员：
    + 属性/方法-- 类型 -- 读/写 -- 说明
    + bubbles -- Boolean -- 只读 -- 表明事件是否冒泡
    + cancelable -- Boolean -- 只读 -- 表明是否可以取消事件的默认行为
    + currentTarget -- Element --只读 -- 其事件处理程序当前正在处理事件的那个元素
    + defaultPrevented --Boolean -- 只读 -- 为true表示已经调用了preventDefault()(DOM3级事件中新增)
    + detail -- Integer -- 只读 -- 与事件相关的细节信息
    + eventPhase -- Integer -- 只读 -- 调用事件处理程序的阶段：1表示捕获阶段，2表示“处于目标”，3表示冒泡阶段
    + preventDefault() -- Function -- 只读 -- 取消事件的默认行为。如果cancelable是true，则可以使用这个方法。
    + stopImmediatePropagation() --Function -- 只读 -- 取消事件的进一步捕获或冒泡，同时阻止任何事件处理程序被调用（DOM3级事件中新增）
    + stopPropagation() -- Function -- 只读 -- 取消事件的进一步捕获或冒泡。如果bubbles为true，则可以使用这个方法
    + target -- Element -- 只读 --事件的目标
    + trusted -- Boolean -- 只读 -- 为true表示事件是在浏览器生成的，为false表示事件时由开发人员通过JS创建的（DOM3级新增）
    + type -- String -- 只读 -- 被触发的事件的类型
    + view -- AbstractView -- 只读 -- 与事件关联的抽象视图。等同于发生事件的window事件。
+ 在需要通过一个函数处理多个事件时，可以使用type属性。例如：
```
var btn = document.getElementById("myBtn");
var handler = function(event) {
    switch(event.type){
        case "click":
            alert("Clicked");
            break;
            
        case "mouseover":
            event.target.style.backgroundColor = "red";
            break;
        
        case "mouseout":
            event.target.style.backgroundColor = " ";
            break;
    }
};

btn.onclick = handler;
btn.onmouseover = handler;
btn.onmouseout = handler;
```
+ 要阻止特定事件的默认行为，可以使用preventDefault()方法。例如，链接的默认行为就是在被单击时会导航到其href特性指定的URL。如果你想阻止链接导航这一默认行为，那么通过链接的onclick事件处理程序可以取消它，如下：
```
var link = document.getElementById("myLink");
link.onclick = function(event) {
    event.preventDefault();
}
```
+ 只有cancelable属性设置为true的事件，才可以使用preventDefault()来取消其默认行为。
+ stopPropagation()方法用于立即停止事件在DOM层次中的传播，即取消进一步的事件捕获或冒泡。例如，直接添加到一个按钮的事件处理程序可以调用stopPropagation()，从而避免触发注册在document.body上面的事件处理程序。
```
var btn = document.getElementById("myBtn");
btn.onclick = function(event){
    alert("Clicked");
    event.stopPropagation();
};

document.body.onclick = function(event){
    alert("Body clicked");
};
```
在这个例子中，如果不调用stopPropagation()，就会在单击按钮时出现两个警告框。
+ 事件的eventPhase属性，可以用来确定事件当前正位于事件流的哪个阶段。这里注意的是，尽管“处于目标”发生在冒泡阶段，但eventPhase仍然一直等于2。
```
var btn = document.getElementById("myBtn");
btn.onclick = function(event) {
    alert(event.eventPhase);  //2
}

document.body.addEventListener("click", function(event){
    alert(event.eventPhase);   //1
},true);

document.body.onclick = function(event){
    alert(event.eventPhase);  //3
}
```
当单击这个例子中的按钮时，首先执行的事件处理程序是在捕获阶段触发的添加驾到document.body中的哪一个，结果会弹出一个警告框显示表示eventPhase的1.接着，会触发在按钮上注册的事件处理程序，此时的eventPhase值为2。最后一个被触发的事件处理程序，是在冒泡阶段执行的添加到document.body上的那一个，显示eventPhase的值为3。而在eventPhase等于2时，this、target和currentTarget始终都是相等的。
+ 只有在事件处理程序执行期间，event对象才会存在；一旦事件处理程序执行完成，event对象就会被销毁。

#### 13.3.2 IE中的事件对象
+ 要访问IE中的event对象有几种不同的方式，取决于指定事件处理程序的方法。在使用DOM0级方法添加事件处理程序时，event对象是window对象的一个属性。
```
var btn = document.getElementById("myBtn");
btn.onclick = function(){
    var event = document.event;
    alert(event.type);  //"click"
};
```
+ 如果事件处理程序是使用attachEvent()添加的，那么就会有一个event对象作为参数被传入事件处理程序函数中。
```
var btn = document.getElementById("myBtn");
btn.attachEvent("onclick", function(event){
    alert(event.type);   //"click"
});
```
+ 在像这样使用attachEvent()的情况下，也可以通过window对象来访问event对象，就像使用DOM0级方法时一样。不过为方便起见，同一个对象也会作为参数传递。铜鼓欧HTML特性指定的事件处理程序，可以通过一个名叫event的变量来访问event对象。
```
<input type="button" value="Click Me" onclick="alert(event.type)">
```
+ IE的event对象同样也包含与创建它的事件相关的属性和方法，这些属性和方法也会因为事件类型的不同而不同，所有事件对象都会包含下列的属性和方法：
    + 属性/方法 -- 类型 -- 读/写 -- 说明
    + cancelBubble -- Boolean -- 读/写 -- 默认值为false，但将其设置为true就可以取消事件冒泡（与DOM中的stopPropagation()方法的作用相同）
    + returnValue -- Boolean -- 读/写 -- 默认值为true，但将其设置为false就可以取消事件的默认行为（与DOM中的preventDefault()方法的作用相同）
    + srcElement -- Element --只读 -- 事件的目标
    + type -- String -- 只读 -- 被触发的事件的类型
+ 因为事件处理程序的作用域是根据指定它的方式来确定的，所以不能认为this始终等于事件目标。故而，最好还是使用event.srcElement比较保险。
```
var btn = document.getElementById("myBtn");
btn.onclick = function(){
    alert(window.event.srcElement === this);  //true
}；

btn.attachEvent("onclick", function(event){
    alert(event.srcElement === this);  //false
});
```
+ 只要将returnValue设置为false,就可以阻止默认行为。
```
var link = document.getElementById("myBtn");
link.onclick = function(){
    window.event.returnValue = false;
};
```
+ 与DOM不同的是，在此没有办法确定事件是否能被取消。
+ 由于IE不支持事件捕获，因而只能取消事件冒泡；但stopPropagation()可以同时取消事件捕获和冒泡。
```
var btn = document.getElementById("myBtn");
btn.onclick = function(){
    alert("Clicked")；
    window.event.cancelBubble = true;
};

document.body.onclick = function(){
    alert("Body clicked");
};
```
#### 13.3.3 跨浏览器的事件对象
```
var EventUtil = {
    addHandler:function(element,type, handler){
        //省略的代码
    }，
    
    getEvent:function(event){
        return event ? event : window.event;
    },
    
    getTarget: function(event){
        return event.target || event.srcElement;
    },
    
    preventDefault: function(event){
        if (event.preventDefault) {
            event.preventDefault();
        } else {
            event.returnValue = false;
        }
    },
    
    removeHandler: function(element, type, handler){
        //省略的代码
    },
    
    stopPropagation:function(event) {
        if(event.stopPropagation){
            event.stopPropagation();
        } else {
            event.cancelBubble = true;
        }
    }
};
```
+ 我们为EventUtil添加了4个新方法。第一个是getEvent(),它返回对event对象的引用。在使用这个方法时，必须假设有一个事件对象传入到事件处理程序中，而且要把该变量传给这个方法，如下所示。
```
btn.onclick = function(event) {
    event = EventUtil.getEvent(event);
};
```
+ 在兼容DOM的浏览器中，event变量只是简单的传入和返回。而在IE中，event参数是未定义的（undefined),因此就会返回window.event。将这一行代码添加到事件处理程序的开头，就可以确保随时都能使用event对象，而不必担心用户使用的是什么浏览器。
```
var link = document.getElementById("myLink");
link.onclick = function(event) {
    event = EventUtil.getEvent(event);
    EventUtil.preventDefault(event);
};
```
## 13.4 事件类型
+ “DOM3级事件”规定了以下几类事件：
    + UI（用户界面）事件，当用户与页面上的元素交互时触发。
    + 焦点事件、鼠标事件、滚轮事件、键盘事件
    + 文本事件，当在文档中输入文本时触发
    + 合成事件，当为IME(输入法编辑器)输入字符时触发
    + 变动事件，当底层DOM结构发生变化时触发
+ DOM3级事件模块在DOM2级事件模块基础上重新定义了这些事件，也添加了一些新事件，包括IE9在内的所有主流浏览器都支持DOM2事件。IE9也支持DOM3级事件。

#### 13.4.1 UI事件
+ UI事件指的是那些不一定与用户操作有关的事件。现有的UI事件如下：
    + DOMActivate:表示元素已经被用户操作（通过鼠标或键盘）激活。考虑到不同浏览器实现的差异，不建议使用这个事件。
    + load:当页面完全加载(包括所有图像、js文件、CSS文件等外部资源）后在window上面触发，当所有框架都加载完毕时在框架集上面触发，当图像加载完毕时在<img>元素上面触发，或者当嵌入的内容加载完毕时在<object>元素上面触发。
    + unload:当页面完全卸载后在window上面触发，当所有框架都卸载后在框架集上面触发，或者当嵌入的内容卸载完毕后在<object>元素上面触发。只要用户从一个页面切换到另一个页面，就会发生unload事件。而利用这个事件最多的情况是清除引用，以避免内存泄漏。指定onunload事件处理程序第一种方式是使用JS，第二种是为<body>元素添加一个特性。
    + abort:在用户停止下载过程时，如果嵌入的内容没有加载完，则在<object>元素上面触发。
    + error：当发生js错误时在window上面触发，当无法加载图像时在<img>元素上面触发，当无法加载嵌入内容时在<object>元素上面触发，或者当有一或多个框架无法加载时在框架集上面触发。
    + select：当用户选择文本框（<input>或<textarea>）中的一或多个字符时触发。
    + resize:当浏览器窗口或框架的大小变化时在window或框架上面触发。这个事件在window上面触发，可以通过js或者<body>元素的onresize特性指定事件处理程序。关于何时回触发resize事件，不同浏览器有不同的机制。IE、Safari、Chorme和Opera会在浏览器窗口变化了1像素时就触发resize事件，然后随着变化不断重复触发。Firefox则则会在用户停止调整窗口大小时才会触发resize事件，由于存在这个差别，应该注意不要在这个事件的处理程序中加入大计算量的代码，因为这些代码有可能被频繁执行，从而导致浏览器反应明显变慢。
    + scroll：当用户滚动带滚动条的元素中的内容时，在该元素上面触发。<body>元素中包含所加载页面的滚动条。
+ 多数这些事件斗鱼window对象或表单控件有关。除了DOMActivate之外，其他事件在DOM2级事件中都归为HTML事件。要确定浏览器是否支持DOM2级事件规定的HTML事件，可以用如下代码：
```
var isSupported = document.implementation.hasFeature("HTMLevents", "2.0");
```
+ 要确定浏览器是否支持“DOM3级事件”，可以使用如下代码：
```
var isSupported = document.implementation.hasFeature("UIEvent", "3.0");
```
##### 1.load事件
+ 有两种定义window上面onload事件处理程序的方式。第一种是用JS代码：
```
EventUtil.addHandler(window, "load", function(event){
    alert("Loaded!");
});
```
与添加其他事件一样，这里也给事件处理程序传入了一个event对象，这个event对象中不包含这个事件的任何附加信息，但在兼容DOM的浏览器中，event.target属性的值会被设置为document，而IE并不会为这个事件设置srcElement属性。
+ 第二种指定onload事件处理程序的方式是为<body>元素添加一个onload特性。实际上，这只是为了保证向后兼容的一种权宜之计，但所有浏览器都能很好地支持这种方式。建议尽可能使用JS方法。
+ 在HTML中可以为任何图像指定onload事件处理程序。同样的功能也可以用JS来实现。
```
<img src="smile.gif" onload="alert('Image loaded')">
```
+ 在创建新的<img>元素时，可以为其指定一个事件处理程序，以便图像加载完毕后给出提示。此时，最重要的是要在指定src属性之前先指定事件。
```
EventUtil.addHandler(window, "load", function(){
    var image = document.createElement("img");
    EventUtil.addHandler(image, "load", function(event){
        event = EventUtil.getEvent(event);
        alert(EventUtil.getTarget(event).src);
    });
    document.body.appendChild(image);
    image.src = "smile.gif";
});
```
在这个例子中，我们想在window完全加载完毕后再向DOM中添加一个新图片，等图片加载完毕后再添加到页面中。所以首先为window指定onload事件处理程序，再创建一个新图像元素，并设置了其onload事件处理程序，最后又将这个图像添加到页面中，还设置了他的src属性。这里有一点需要格外注意：新图像元素不一定要添加到文档后才开始下载，只要设置了src属性就会开始下载。
+ 还有一些元素也以非标准的方式支持load事件。在IE9+及更高版本中，<script>元素也会触发load事件，以便开发人员确定动态加载的JS文件是否加载完毕。与图像不同，只有在设置了<script>元素的src属性并将该元素添加到文档后，才会开始下载JS文件。换句话说，对于<script>元素而言，指定src属性和指定事件处理程序的先后顺序就不重要了。以下代码展示了怎样为<script>元素指定事件处理程序。
```
//EventUtil对象为本书中定义的跨浏览器事件对象，定义在13.3.3
//这个addHanler方法时封装了DOM中的addEventListener方法，和IE中的attachEvent方法，
//和原本的用法有些许不一样，原本这两个方法时直接定义在DOM元素中的。
EventUtil.addHandler(window, "load", function() {
    var script = document.createElement("script");
    EventUtil.addHandler(script, "load", function(event){
        alert("loaded");
    });
    script.src = "example.js";
    document.body.appendChild("script");
    
});
```
##### sroll事件
+ 虽然sroll事件时在window对象上发生的，但它实际表示的则是页面中相应元素的变化。在混杂模式下，可以通过<body>元素的scrollLeft和scrollTop来监控到这一变化；而在标准模式下，除Safari之外的浏览器都会通过<html>元素来反映这一变化（Safari仍然基于<body>跟踪滚动位置）
```
EventUtil.addHandler(window, "scroll", function(event){
    if (document.compatMode == "CSS1Compat"){
        alert(document.documentElement.scrollTop);
    } else {
        alert(document.body.scrollTop);
    }
});
```
+ 与resize事件类似，scroll事件也会在文档被滚动期间重复被触发，所以有必要尽量保持事件处理程序的代码简单。

#### 13.4.2 焦点事件
+ 焦点事件会在页面元素获得或失去焦点时触发。利用这些事件并与document.hasFoucus()方法及document.activeElement属性配合，可以知晓用户在页面上的行踪。有以下6个焦点事件：
    + blur:在元素失去焦点时触发。这个事件不会冒泡；所有的浏览器都支持它。
    + DOMFocusIn/DOMFocusOut:在元素获得/失去焦点时触发。这个事件与HTML事件focus/blur等价，但它冒泡。只有Opera支持这个事件。DOM3级事件废弃了DOMFocusIn,选择了focusin/focusout.
    + focus:获得焦点时触发。不会冒泡。所有浏览器都支持。
    + focusin:获得焦点。与HTML事件focus等价，但它冒泡。
    + focusout:失去焦点。是blur的通用版本。有很多浏览器支持。
+ 当焦点从页面上的一个元素移动到另一个元素，会依次触发下列事件：
    + （1）focusout在失去焦点的元素上触发
    + （2）focusin在获得焦点的元素上触发
    + （3）blur在失去焦点的元素上触发
    + （4）DOMFocusOut在失去焦点的元素上触发
    + （5）focus在获得焦点的元素上触发
    + （6）DOMFocusIn在获得焦点的元素上触发
+ 要确定浏览器是否支持这些事件，可以使用如下代码：
```
var isSupported = document.implementation.hasFeature("FocusEvent", "3.0");
```
#### 13.4.3 鼠标与滚轮事件
+ DOM3级事件中定义了9个鼠标事件
    + click:在用户单击主鼠标按钮（一般是左边的按钮）或者按下回车键时触发。这一点对确保易访问性很重要，意味着onclick事件处理程序既可以通过键盘也可以通过鼠标执行。
    + dblclick:在用户双击主鼠标按钮时触发。
    + mousedown：在用户按下了任意鼠标按钮时触发。不能通过键盘触发这个事件。
    + mouseenter:在鼠标光标从元素外部首次移动到元素范围之内时触发。这个事件不冒泡，而且在光标移动到后代元素上不会触发。
    + mouseleave：在位于元素上方的鼠标光标移动到元素范围之外时触发。不冒泡，而且在后代元素上不会触发。
    + mousemove:当鼠标指针在元素内部移动时重复地触发。不能通过键盘触发这个事件。
    + mouseout：在鼠标指针位于一个元素上方，然后用户将其移入另一个元素时触发。又移入的另一个元素可能位于前一个元素的外部，也可能是这个元素的子元素。不能通过键盘触发这个事件。
    + mouseover:在鼠标指针位于一个元素外部，的然后用户将其首次移入另一个元素边界之内时触发。
    + mouseup：在用户释放鼠标按钮时触发。
+ 页面上所有元素都支持鼠标事件。除了mouseenter和mouseleave，所有鼠标事件都会冒泡，也可以被取消，而取消鼠标事件将会影响浏览器的默认行为。
+ 只有在同一个元素上相继触发mousedown和mouseup事件，才会触发click事件。
+ 这四个事件触发的顺序始终如下：（1）mousedown(2)mouseup(3)click(4)mousedown(5)mouseup(6)click(7)dblclick
+ 使用以下代码可以检测浏览器是否支持以上DOM2级事件(dblclick、mouseenter和click是DOM3级):
```
var isSupported = document.implementation.hasFeature("MouseEvents", "2.0")
```
+ 要检测浏览器是否支持上面的所有事件，可以使用以下代码：
```
var isSupported = document.implementation.hasFeature("MouseEvent","3.0")
```
+ 鼠标事件还有一个滚轮事件mousewheel
##### 1.客户区坐标位置
+ 鼠标事件都是在浏览器视口中的特定位置上发生的。这个位置保存在事件对象的clientX和clientY属性中。所有的浏览器都支持这两个属性，它们的值表示事件发生时鼠标指针在视口中的水平和垂直坐标。视口即浏览器中除去浏览器工作栏，真正显示用户浏览的内容的地方,这里不包括页面滚动的距离。
+ 可以使用下列代码取得鼠标事件的客户端坐标信息：
```
var div = document.getElementById("myDiv");
EventUtil.addHandler(div, "click", function(event){
    event = EventUtil.getEvent(event);
    alert("Client coordinates:" + evnent.clientX + "," + event.clientY);
});
```
##### 2.页面坐标位置
+ 页面坐标通过事件对象的pageX和pageY属性，告诉你事件在页面中的什么位置发生的。获取方式和上面一样。
+ IE8及更早版本不支持事件对象上的页面坐标，不过使用客户区坐标和滚动信息可以计算出来。这时候需要用到document.body(混杂模式)或document.documentElement(标准模式)中的scrollLeft和scrollTop属性。计算过程如下：
```
var div = document.getElementById("myDiv");
EventUtil.addHandler(div, "click", function(){
    event = EventUtil.getEvent(event);
    var pageX = event.pageX;
    var pageY = event.pageY;
    
    if(pageX === undefined) {
        pageX = event.clientX + (document.body.scrollLeft || document.documentElement.scrollLeft);
    }
    if(pageY === undefined) {
        pageY = event.clientY + (document.body.scrollTop || document.documentElement.scrollTop);
    }
    alert("Page coordinates:" + pageX + "," + pageY);
    
});
```
##### 3.屏幕坐标位置
+ 通过screenX和screenY属性就可以确定鼠标事件发生时鼠标指针相对于整个屏幕的坐标位置。也是用以上同样的方式取得屏幕坐标。

##### 4.修改键
+ 修改键包括Shift、Ctrl、Alt和Meta(在Windows键盘中是Windows键，在苹果机中是Cmd键)，DOM为此规定了4个属性，这些属性属于event对象：shiftKey、ctrlKey、altKey和metaKey。这些属性中包含的都是布尔值，如果相应的键被按下了，则值为true，否则值为false。

##### 5.相关元素


#### 13.4.7 HTML5事件
##### 3.DOMContentLoaded
+ window的load事件会在页面中的一切都加载完毕后触发，而DOMContentLoaded事件则在形成完整的DOM树之后就会触发，不理会图像、JavaScript文件、CSS文件或其他资源是否已经加载完毕。与load事件不同，DOMContentLoaded支持在页面下载的早期添加事件处理程序，这也意味着用户能够尽早地与页面进行交互。
+ 要处理DOMContentLoaded事件，可以为document或window添加相应的事件处理程序（尽管这个事件会冒泡到window，但它的目标实际上是document）。
```
EventUtil.addHandler(document, "DOMContentLoaded", function(event) {
    alert("Content loaded");
});
```
+ 通常这个事件既可以添加事件处理程序，也可以执行其他DOM操作，这个事件始终都会在load事件之前触发。对于不支持DOMContentLoaded的浏览器，我么建议在页面加载期间设置一个时间为0毫秒的超时调用，为了确保这个方法有效，必须将其作为页面中的第一个超时调用；即便如此，也还是无法保证在所有环境中该超时调用一定会早于load事件被触发，如下：
```
setTimeout(function()) {
    //在此添加事件处理程序
}, 0);
```
##### jquery $(document).ready方法
+ 这个方法与原生的DOMContentLoaded对应，当DOM树加载完毕时发生ready事件，由于该事件在文档就绪后发生，因此把所有其他的jQuery事件和函数都置于该事件中是非常好的做法。
```
$(document).ready(function(){
    $("button").click(function(){
        $("p").slideToggle();
    });
});
```
##### jquery的$(this)
> 参考链接：https://www.cnblogs.com/diantao/p/4514238.html

+ this，表示当前的上下文对象是一个html对象，可以调用html对象所拥有的属性和方法。
+ $(this)是一个JQuery对象,代表的上下文对象是一个jquery的上下文对象，可以调用jquery的方法和属性值。
+ query对象$(this)[0]等同于JS里的元素this，这里的this是一样的，相信你应该看出来了，JS里的元素只要包上$()就是jquery对象了，而jquery的对象只要加上[0]或者.get(0)，就是js元素了。
+ $(this)是jquery对象，this就是简单指当前元素。jquery对象不能直接指定元素的属性（ele.style），需要get（index）或者直接（index）取得对象中元素才行
```
$("#textbox").hover(
     function() {
              this.title = "Test";
     },
    fucntion() {
              this.title = "OK”;
    }
);
```
这里的this其实是一个Html元素(textbox)，textbox有text属性，所以这样写是完全没有什么问题的。但是如果将this换成$(this)就报错了。jQuery对象沒有title 属性，因此这样写是错误的。正确的代码：
```
$("#textbox").hover(
     function() {
          $(this).attr(’title’, ‘Test’);
     },
     function() {
          $(this).attr(’title’, ‘OK’);
     }
);
```
## 13.5 内存和性能
+ 在JS中，添加到页面上的事件处理程序数量将直接关系到页面的整体运行性能。原因：1.每个函数都是对象，都会占用内存；内存中的对象越多，性能就越差。2. 必须事先制定所有处理程而导致的DOM访问次数，会延迟整个页面的交互就绪时间。

#### 13.5.1 解决“事件处理程序过多”问题：事件委托
+ 事件委托利用了事件冒泡，只指定一个事件处理程序，就可以管理某一类型的所有事件。例如，click事件会一直冒泡到documents层次。也就是说，我们可以为整个页面指定一个onclick事件处理事件。
```
<ul id="myLinks">
    <li id= "goSomewhere">Go somewhere</li>
    <li id="doSomething">Do something</li>
    <li id="sayHi">Say hi</li>
</ul>

var list= document.getElementById("myLinks");


EventUtil.addHandler(list, "click", function(event){
    event = EventUil.getEvent(event);
    var target = EventUtil.getTarget（event）;
    
    switch(target.id){
        case "doSomething":
            document.title = "I changed the document's title";
            break;
        case "goSomewhere":
            location.href = "htp://www.wrox.com";
            break;
        case "sayHi":
            alert("hi");
            break;
    }
});
```
+ 由于所有列表项都是这个元素的子节点，而且他们的事件会冒泡，所以单击事件最终会被这个函数处理。
+ 如果可行的话，也可以考虑为document对象添加一个事件处理程序，用以处理页面上发生的某种特定类型的事件，这样做与传统做法的优点：
    + document对象很快就可以访问，而且可以在页面生命周期的任何时点上为他添加事件处理程序（无需等待DOMContenLoaded或load事件）。换句话时候，只要可单击的元素呈现在页面上，就可以立即具备适当的功能。
    + 在页面中设置事件处理程序所需的时间更少，只添加一个事件处理时间所需的DOM引用更少，所花的时间也更少。
    + 整个页面占用的内存空间更少，能够提升整体性能。
+ 最适合采用事件委托技术的事件包括click、mouseup、keydown、keyup和keypress。虽然mouseover和mouseout事件也冒泡，但要适当处理它们并不容易，而且经常需要计算元素的位置。

##### 13.5.2 移除事件处理程序
+ 每当将事件处理程序指定给元素时，运行中的浏览器代码与支持页面交互的JS代码就会建立一个连接。这种连接越多，页面执行起来就越慢。如前所述，可以采用事件委托技术，限制建立的连接数量。另外，在不需要的时候移除事件处理程序，也是解决这种问题的一种方案。内存中留有那些过时不用的“空事件处理程序”（dangling event handler)，也是造成Web应用程序内存与性能问题的主要原因。
+ 在两种情况下，可能会造成内存中留有过时不用的“空事件处理程序”：第一种情况就是从文档中移除带有时间处理程序的元素时，这可能是通过纯粹的DOM操作，例如使用removeChild()和replaceChild()方法，但更多的是发生在==使用innerHTML替换页面中某一部分的时候==。如果带有事件处理程序的元素被innerHTML删除了，那么添加到元素中的事件处理程序极有可能无法被当做垃圾回收。
```
<div id="myDiv">
    <input type="button" value="Click Me" id="myBtn">
</div>
<script type="text/javascript">
    var btn = document.getElementById("myBtn");
    btn.onclick = function(){
        //先执行某些操作
        document.getElementById("myDiv").innerHTML="Processing……"；//麻烦了!
    }；
</script>
```
+ 这里，有一个按钮被包含在<div>元素中。为避免双击，单击这个按钮时就将按钮移除并替换成一条信息；这是网站设计时非常流行的一种做法。但问题在于，当按钮从页面中移除时，它还带着一个事件处理程序。如果你知道某个元素即将被移除，那么最好手工移除事件处理程序。在此，我们在设置<div>的innerHTML属性之前，先移除按钮的事件处理程序。就确保了内存可以被再次利用，而从DOM中移除按钮也做到了干净利索。
```
btn.onclick = null;//移除事件处理程序
```
+ 注意，在事件处理程序中删除按钮也能阻止事件冒泡。目标元素在文档中是事件冒泡的前提。
+ 导致“空事件处理程序”另一种情况，就是卸载页面的时候。每次加载完页面再卸载页面（可能是在两个页面间来回切换，也可以是淡季了“刷新”按钮），内存中滞留的对象数目就会增加，因为事件处理程序占用的内存并没有被释放。
+ 一般来说，最好的做法是在页面卸载之前，先通过onunload事件处理程序移除所有事件处理程序。在此，事件委托技术再次表现出他的优势——需要跟踪的事件处理程序越少，移除他们就越容易。对这种类似撤销的操作，我们可以把它想象成：只要是通过onload事件处理程序添加的东西，最后都要通过onunload事件处理程序将它们移除。




---
# 第21章  Ajax与Comet
+ Ajax技术的==核心==是XMLHttpRequest对象（简称XHR).XHR为向服务器发送请求和解析服务器响应提供了流畅的接口。能够以异步方式从服务器取得更多信息，意味着用户单击后，可以不必刷新页面也能取得新数据。也就是说，可以使用XHR对象取得新数据，然后再通过DOM将新数据插入到页面中。另外，虽然名字中包含XML的成分，但Ajax的通信与数据格式无关；这种技术就是无须刷新页面即可从服务器取得数据，但不一定是XML数据。
## 21.1 XMLHttpRequest对象
+ 要使用MSXML库中的XHR对象，需要像创建XML文档一样，编写一个函数。
```
//适用于IE7之前的版本
function createXHR(){
    if (typeof arguments.calleee.activeXString != "string"){
        var versions = ["MSXML2.XMLHttp.6.0", "MSXML2.XMLHttp.3.0", "MSXML2.XMLHttp"], i, len;
        
        for (i=0, len = version.length; i<len; i++){
            try{
                new ActiveXObject(versions[i]);
                arguments.callee.activeXString = versions[i];
                break;
            } catch(ex){
                //跳过
            }
        }
    }
    
    return new ActiveXObject(arguments.callee.activeXString);
}
```
这个函数会尽力根据IE中可用的MSXML库的情况创建最新版本的XHR对象
+ IE7+、firefox、opera、chrome和safari都支持原生的XHR对象，在这些浏览器中创建XHR对象要使用XMLHttpRequest构造函数。`var xhr = new XMLHttpRequest()`
+ 如果只想支持IE7及更高版本，那么大可丢弃前面定义的那个函数，而只用原生的XHR实现。
#### 21.1.1 XHR的用法
+ 在使用XHR对象时，要调用的第一个方法是open(),它接受3个参数：要发送的请求的类型（“get","post"等）、请求的URL和表示是否异步发送请求的布尔值。
```
xhr.open("get", "example.php", false);
```
+ 有关这行代码，需要说明两点：一是URL是相对于执行代码的当前页面来说的，也可以使用绝对路径；二是调用open()方法并不会真正发送请求，而只是启动一个请求以备发送。
+ 要发送特定的请求，必须像下面这样调用send()方法：
```
xhr.send(null);
```
+ 这里的send()方法接收一个参数，即要作为请求主体发送的数据，如果不需要通过请求主体发送数据，则必须传入null，因为这个参数对有些浏览器来说是必须的。调用send()之后，请求就会被分派到服务器。
+ 由于这次请求是同步的，JS代码会等到服务器响应之后再继续执行，在收到响应后，响应的数据会自动填充到XHR对象的属性，相关的属性简介如下：
    + responseText:作为响应主体被返回的文本
    + responseXML: 如果响应的内容类型是“text/xml"或“application/xml",这个属性中将保存包含着响应数据的XML DOM文档。
    + status:响应的HTTP状态。
    + statusText:HTTP状态的说明。
+ 在收到响应后，第一步是检查status属性，以确定响应已经成功返回，一般来说，可以将HTTP状态代码为200作为成功的标志。此时，responseText属性的内容已经就绪，而且在内容类型正确的情况下，responseXML也应该能够访问了。此外，状态代码304表示请求的资源并没有被修改，可以直接使用浏览器的缓存；当然，也意味着响应是有效的。为确保收到适当的响应，应该像下面这样检查上述这两种状态代码。
```
if((xhr.status >= 200 && xhr.status <300) || xhr.status == 304){
    alert(xhr.responseText);
}else{
    alert("Response was unsuccessful:" + xhr.status);
}
```
+ 建议通过status来决定下一步的操作，不要依赖statusText,因为后者在跨浏览器使用时不太可靠。另外，无论内容类型是什么，响应主体的内容都会保存到responseText属性中；而对于非XML数据而言，responseXML属性的值将为null。
+ 像前面这样发送同步请求当然没有问题，但多数情况下，我们还是要发送异步请求，才能让JS继续执行而不必等待响应。此时，可以检测XHR对象的readyState属性，该属性表示请求/响应过程的当前活动阶段。这个属性可取的值如下：
    + 0：未初始化。尚未调用xhr.open()方法。
    + 1：启动。已经调用open()方法，当尚未调用send()方法。
    + 2：发送。已经调用send（）方法，当尚未接收到响应。
    + 3：接收。已经接受到部分响应数据。
    + 4：完成。已经接受到全部响应数据，而且已经可以在客户端使用了。
+ 只要readyState属性的值由一个值变为另一个值，都会触发一次readystatechange事件。可以利用这个事件来检测每次状态变化后的readyState的值。通常，我们只对reagyState值为4的阶段感兴趣，因为这时所有数据都已经就绪。不过，在调用open()之前指定onreadystatechange事件处理程序才能确保跨浏览器兼容性。
```
var xhr = createXHR();
xhr.onreadystatechange = function(){
    if(xhr.readyState== 4){
        if((xhr.status >= 200 && xhr.status < 300)||xhr.status == 304){
            alert(xhr.responseText);
        } else {
            alert("Request was unsuccessful:" + xhr.status);
        }
    }
};
xhr.open("get", "example.txt", true);
xhr.setRequestHeader("MyHeader", "MyValue");//下一节会讲
xhr.send(null);
```
+ 以上代码利用DOM0级为XHR对象添加了事件处理程序，原因是并非所有浏览器都支持DOM2级方法。与其他事件处理程序不同，这里没有向onreadystatechange事件处理程序中传递event对象；必须通过XHR对象本身来确定下一步该怎么做。
+ 另外，在接收到响应之前还可以调用abort()方法来取消异步请求`xhr.abort()`
+ 调用这个方法后，XHR对象会停止触发事件，而且也不再允许任何与响应有关的对象属性，在终止请求之后，还应该对XHR对象进行解引用操作。由于内存原因，不建议重（chong)用XHR对象。
#### 21.2 HTTP头部信息
+ 每个HTTP请求和响应都会带有相应的头部信息。XHR对象也提供了操作这两种头部（即请求头部和响应头部）信息的方法
    + Accept:浏览器能够处理的内容类型
    + Accept-Charset:浏览器能够显示的字符集
    + Accept-Encoding:浏览器能够处理的压缩编码
    + Connection:浏览器与服务器之间连接的类型
    + Cookie：当前页面设置的任何Cookie.
    + Host:发出请求的页面所在的域
    + Referer:发出请求的页面的URL。注意，HTTP规范将这个头部字段拼写错了，将错就错。
    + User-Agent:浏览器的用户代理字符串。
+ 虽然不同浏览器实际发送的头部信息会有所不同，但以上列出的基本上是所有浏览器都会发送的。使用setRequestHeader()方法可以设置自定义的请求头部信息。这个方法接收两个参数：头部字段的名称和头部字段的值。==要成功发送请求头部信息，必须在调用open()方法之后调用send()方法之前调用setRequestHeader()。==
+ 服务器在接收到这种自定义的头部信息后，可以执行相应的后续操作。建议使用自定义的头部字段名称，不要使用浏览器正常发送的字段名称，否则有可能会影响服务器的响应。
+ 调用XHR对象的getResponseHeader()方法并传入头部字段名称，可以取得相应的响应头部信息。而调用getAllHeaders()方法则可以取得一个包含所有头部信息的长字符串。
```
var myHeader = xhr.getReponseHeader("MyHeader");
var allHeaders = xhr.getAllResponseHeaders();
```
+ 在服务器端，也可以利用头部信息想浏览器发送额外的、结构化的数据。在没有自定义信息的情况下，getAllResponseHeaders()方法通常会返回如下所示的多行文本内容：
```
Date: Sun， 14 Nov 2004 18:04:03 GMT
Server: Apache/1.3.29 (Unix)
Vary:Accept
X-Powered-By: PHP/4.3.8
Connection:close
Content-Type: text/html; charset=iso-8859-1
```
+ 这种格式化的输出可以方便我们检查响应中所有字段的名称，而不必一个一个地检查某个字段是否存在。

## 21.4 跨域资源共享
+ CORS背后的基本思想，就是使用自定义的HTTP头部让浏览器和服务器进行沟通，从而决定请求或响应式是应该成功，还是应该失败。
+ 整个CORS通信过程，都是浏览器自动完成，不需要用户参与。对于开发者来说,CORS通信与同源的AJAX通信没有差别，代码完全一样，
#### 21.4.1 IE对CORS的实现
##### 简单请求
+ 简单请求：请求方法是HEAD、GET、POST三者之一，HTTP的头信息不超出以下几种字段：
    + Accept
    + Accept-Language
    + Content-Language
    + Last-Event-ID
    + Content-Type:只限于三个值applicaion/x-www-form-urlencoded、multipart/form-data、text/plain
+ 对于简单请求，浏览器直接发出CORS请求。具体里说，就是自动在头信息之中，增加一个Origin字段。此字段用来说明，本次请求来自哪个源（协议+域名+端口）。服务器根据这个值，决定是否同意这次请求。
    + 如果Origin指定的源，不在许可范围内，服务器会返回一个正常的HTTP回应，这个回应的头信息没有包含`Access-Control-Allow-Origin`字段。这时浏览器就知道出错了，从而抛出一个错误，被`XMLHttpRequest`的`onerror`回调函数捕获。注意，这种错误无法通过状态码识别，因为HTTP回应的状态码有可能是200.
    + 如果Origin指定的域名在许可范围内，服务器返回的响应，会多出几个头信息字段。三个与CORS请求相关的额字段，都以`Access-Control-`开头。
        + （1)Access-Control-Allow-Origin:该自担是必须的，它的值要么是请求时Origin字段的值，要么是一个*，表示接受任意域名的请求。
        + （2）Access-Control-Allow-Credentials:该字段可选，它的值时一个布尔值，表示是否允许发送cookie。默认情况下，Cookie不包括在CORS请求之中。如果这个选项设为true,即表示服务器明确许可，Cookie可以包含在请求中，一起发给服务器。这个值只能设为true，如果服务器不要浏览器发送Cookie，删除该字段即可。
        + （3）Access-Control-Expose-Headers：该字段可选。CORS请求时，`XMLHttpRequest`对象的`getResponseHeader()`方法只能拿到6个基本字段：`Cache-Control`、`Content-Language`、`Content-Type`、`Expires`、`Last-Modified`、`Pragma`。如果想拿到其他字段，就必须在Access-Control-Expose-Headers里面指定。上面的例子指定，getResponseHeader('FooBar')
        
```
Access-Control-Origin:http://api.bob.com
Access-Control-Allow-Credentials:true
Access-Control-Expose-Headers:FooBar
Control-Type:text/html:charset=utf-8
```
+ CORS请求==默认不发送Cookie和HTTP认证信息==，如果要把Cookie发到服务器，一方面要服务器同意，另一方面，开发者必须在AJAX请求中打开withCredentials属性。
```
var xhr = new XMLHttpRequest();
xhr.withCredentials = true;
```
+ 需要注意的是，如果要发送Cookie，Access-Control-Allow-Origin就不能设为星号，必须指定明确的、与请求网页一致的域名。同时，Cookie依然遵循同源策略，只有用服务器域名设置的Cookie才会上传，其他域名的Cookie不会上传，且（跨源）原网页代码中的`document.cookie`也无法读取服务器于域名下的Cookie。
##### 非简单请求
+ 非简单请求时那种对服务器有特殊要求的请求，比如请求方法时`PUT`或`DELETE`，或者`Content-Type`字段的类型是`application/json`
+ 非简单请求的CORS请求，会在正式通信之前，增加一次HTTP查询请求，称为“预检”请求（preflight)。浏览器发现是一个非简单请求，就会自动发出一个“预检”要求，要求服务器确认可以这样请求。
+ 浏览器先询问服务器，当前网页所在的域名是否在服务器的许可名单之中，以及可以使用哪些HTTP动词和头信息字段。只有得到肯定回复，浏览器才会发出正式的`XMLHttpRequest`请求，否则会报错。
``` 
预检请求的HTTP头信息
OPTIONS /cors HTTP/1.1
Origin: http://api.bob.com
Access-Control-Request-Method:PUT
Access-Control-Request-Headers: X-Custom-Header
Host: api.alice.com
Accept-Language: en-US
Connection: keep-alive
User-Agent: Mozilla/5.0...
```
+ “预检”请求用的请求方式是OPTIONS,表示这个请求时用来查询的，头信息里面，关键字段是Origin,表示请求来自哪个源。还包括两个特殊字段：
    + Access-Control-Request-Method该字段是必须的，用来列出浏览器的CORS请求会用到哪些HTTP方法。
    + Access-Control-Request-Headers：该字段是一个逗号分隔的字符串，指定浏览器CORS请求会额外发送的头信息字段，上例是X-Custom-Header。
+ 如果服务器确认允许跨源请请求，会做出一个HTTP回应，回应中最关键的是`Access-Control-Allow-Orign`字段，表示允许该源请求数据。
+ 如果浏览器否定了“预检”请求，会返回一个正常的HTTP回应，但是没有任何CORS相关的头信息字段，这时，浏览器就会认定，服务器不同意预检请求，因此触发一个错误，被XMLHttpRequest对象的onerror回调函数捕获，控制台会打印出如下的报错信息。
```
XMLHttpRequest cannot load http://api.alice.com.
Origin http://api.bob.com is not allowed by Access-Control-Allow-Origin.
```
+ 服务器否定预检请求时回应的其他CORS相关字段如下：
    + （1）Access-Control-Allow-Methods：该字段必需，它的值是逗号分隔的一个字符串，表明服务器支持的所有跨域请求的方法。注意，返回的是所有支持的方法，而不单是浏览器请求的那个方法。这是为了避免多次"预检"请求。
    + （2）Access-Control-Allow-Headers：如果浏览器请求包括Access-Control-Request-Headers字段，则Access-Control-Allow-Headers字段是必需的。它也是一个逗号分隔的字符串，表明服务器支持的所有头信息字段，不限于浏览器在"预检"中请求的字段。
    + （3）Access-Control-Allow-Credentials：能否发送Cookie
    + (4)该字段可选，用来指定本次预检请求的有效期，单位为秒。上面结果中，有效期是20天（1728000秒），即允许缓存该条回应1728000秒（即20天），在此期间，不用发出另一条预检请求。
+ 一旦服务器通过了预检请求，以后每次浏览器正常的CORS请求，就都跟简单请求一样，会有一个Origin头信息字段。服务器的回应，也都会有一个Access-Control-Allow-Origin头信息字段。
##### 与JSONP的比较
+ CORS与JSONP的使用目的相同，但是比JSONP更强大。==JSONP只支持GET请求，CORS支持所有类型的HTTP请求==。==JSONP的优势==在于支持老式浏览器，以及可以向不支持CORS的网站请求数据。




---
# 第23章 离线应用于客户端存储
## 23.3 数据存储
#### 23.3.1 Cookie
+ HTTP cookie,通常直接叫做cookie，最初是在客户端用于存储会话信息的。该标准==要求服务器对任意HTTP请求发送Set-Cookie HTTP头作为响应的一部分==，其中包括会话信息。
+ 限制：cookie在性质上是绑定在特定的域名下的，**当设定了一个cookie后，再给创建它的域名发送请求时，都会包含这个cookie**，即在浏览器和服务器间来回传递。这个限制确保了存储在cookie中的信息只能然批准的接受者访问，而无法被其他域访问。
+ 由于cookie是存在客户端计算机上的，还加入了一些限制确保cookie不会被恶意使用，同时不会占据太多磁盘空间。每个域的cookie 总数是有限的，不过浏览器之间各有不同。
+ 为了最佳的浏览器，最好将整个cookie长度限制在4095B以内，这个尺寸是所有cookie加在一起的尺寸。
+ cookie的构成:
    + 名称：一个唯一确定的名称，cookie名称是不区分大小写的。必须是经过URL编码的。
    + 值：储存在cookie中的字符串，必须被URL编码。
    + 域：cookie对于哪个域是有效的。所有向该域发送的请求中都会包含这个cookie信息。这个值可以包含子域，也可以不包含，如果没有明确固定，那么这个域会被认作来自设置cookie的那个域。
    + 路径：对于指定域中的那个路径，应该向服务器发送cookie。
    + 失效时间：表示cookie何时应该被删除的时间戳（也就是，何时应该停止向服务器发送这个cookie)。默认情况下，浏览器会话结束时即将所有cookie删除；不过也可以自己设置删除时间。这个值时GMT格式的日期（Wdy,DD-Mon-YYYY HH:MM:SS GMT)。cookie可在浏览器关闭后仍然保存在用户的机器上。
    + 安全标志：指定后，cookie只有在使用SSL连接的时候才发送到服务器。secure标志是cookie中唯一一个非名值对儿的部分，直接包含一个secure单词。
+ 每一段信息作为Set-Cookie头的一部分，使用分号加空格分割每一段
```
HTTP/1.1 200 OK
Content-type: text/html
Set-Cookie:name=value;expires=Mon,22-Jan-06 07:10:24 GMT;domain:.
wrox.com； path=/; secure
// 对于www.wrox.com和wrox.com的任何子域（如p2p.wrox.com)都有效。
```
+ 域、路径、失效时间和secure标志都是服务器给浏览器的指示，以指定何时应该发送cookie。这些参数并不会作为发送到服务器的cookie信息的一部分，只有名值对儿才会被发送。
##### JS中的cookie
+ 当用来获取属性值时，document.cookie返回当前页面可用的（根据cookie的域、路径、失效时间和安全设置）所有cookie的字符串，一系列由分号隔开的名值对儿。如`name1=value1;name2=value12;name3=value3`,然后使用`decodeURIComponent()`来解码。
+ 当用来设置值时，document.cookie属性可以设置为一个新的cookie字符串。这个字符串会被解释并添加到现有的cookie集合中。设置document.cookie并不会覆盖cookie,除非设置的cookie名称已经存在。设置cookie的格式如下，和Set-Cookie头中使用的格式一样。这些参数中，只有cookie的名字和值时必须的。最好每次设置cookie时都是用encodeURIComponent()。
```
name=value;expores=exporation_time;path=domain_path;domain=domain_name;secure

document.cookie = "name=Nicholas";//这里正好名称和值都无需编码
document.cookie = encodeURIComponent("name") + "=" + encodeURIComponent("Nicholas")+";domain=.wrox.com;path=/";
```
+ cookie基本操作有三种：读取、写入和删除，常常要写一些函数来简化cookie的功能。
```
var CookieUtil = {

    get : function (name){//根据cookie的名字获取相应的值
        var cookieName = encodeURIComponent(name) + "=",//在document.cookie中查找cookie名加上等于号的位置。
        cookieStart = document.coolkie.indexOf(cookie),
        cookieValue = null;
        
        if (cookieStart > -1) {
            var cookieEnd = document.cookie.index(";",cookieStart);//查找该位置之后第一个分号的位置
            if (cookieEnd == -1){
                cookieEnd = document.cookie.length;//如果没有找到分号，则表示该cookie是字符串中的额租后一个，则余下的字符串都是cookie的值。
            }
            cookieValue = decodeURIComponent(document.cookie.substring(cookieStart + cookieName.length, cookieEnd));
        }
        return cookieValue;
    },
    
    set: function (name, value, expires, path, domain, secure) {//在页面上设置一个cookie，接收几个参数
        var cookieText = encodeURIComponent(name) + "=" + encodeURIComponent(value);
        
        if(expires instanceof Date) {//检查expires参数，如果是Date对象，那么用toGMTString()方法正确格式化Date对象。
            cookieText += ";expires" + expires.toGMTString();
        }
        
        if(path) {
            cookieText += ";path" + path;
        }
        
        if (domain) {
            cookieText += "; domain" + domain;
        }
        
        if (secure) {
            cookieText += "; secure";
        }
        
        document.cookie = cookieText;
    },
    
    //没有删除cookie的直接方法，需要再次设置cookie，并将失效时间设置为过去的时间(这里初始化为0ms的Date对象的值即1970年1月1日)
    unset: function (name, path, domain, secure){
        this.set(name,"",new Date(0), path, domain, secure);
    }
};

**使用上述方法**
//设置cookie
CookieUtil.set("name", "Nicholas");
//读取cookie的值
alert(CookieUtil.get("name"));//"Nicholas"

//删除cookie
CookieUtil.unset("name");

//设置cookie、包括他的路径、域和失效时间
CookieUtil.set("name", "Nicholas", "/books/projs", "www.wrox.com",new Date("January 1,2010"));

//删除刚刚设置的cookie
CookieUtil.unset("name","/books/projs","www.wrox.com");

//设置安全的cookie
CookieUtil.set("name","Nicholas",null,null,null,true);
```
##### 子cookie
##### 关于cookie的思考
+ 还有一类cookie被称为“HTTP专有cookie"。HTTP专有cookie可以从浏览器或者服务器设置，但是只能从服务器端读取，因为js无法获取HTTP专有cookie的值。
+ 由于所有的cookie都会由浏览器作为请求头发送，所以在cookie中存储大量信息会影响到特定域的请求性能。
+ 一定不要在cookie中存储重要和敏感的数据。cookie数据并非存储在一个安全环境中，其中包含的任何数据都可以被他人访问。所以不要在cookie中存储诸如银行卡号或者个人地址之类的数据。

#### 23.3.3 Web存储机制
+ Web Storage是Web应用规范1.0中描述的，这个规范最终成为了html5的一部分。Web Stotage的目的是克服由cookie带来的一些限制，当数据需要被严格控制在客户端时，==无需持续地将数据发回服务器==。最终目标是
    + 提供一种在cookie之外存储会话数据的途径
    + 提供一种存储大量可以跨会话存在的数据的机制
#### cookie和session的区别
> 参考链接：https://www.cnblogs.com/cencenyue/p/7604651.html
+ ==保持状态==：cookie保存在浏览器端，session保存在服务器端
+ ==使用方式==：
    + （1）cookie机制：如果不在浏览器中设置过期时间，cookie被保存在内存中，生命周期随浏览器的关闭而结束，这种cookie简称会话cookie。如果在浏览器中设置了cookie的过期时间，cookie被保存在硬盘中，关闭浏览器后，cookie数据仍然存在，直到过期时间结束才消失。Cookie是服务器发给客户端的特殊信息，cookie是以文本的方式保存在客户端，每次请求时都带上它。
    + （2）session机制：当服务器收到请求需要创建session对象时，首先会检查客户端请求中是否包含sessionid。如果有sessionid，服务器将根据该id返回对应session对象。如果客户端请求中没有sessionid，服务器会创建新的session对象，并把sessionid在本次响应中返回给客户端。通常使用cookie方式存储sessionid到客户端，在交互中浏览器按照规则将sessionid发送给服务器。如果用户禁用cookie，则要使用URL重写，可以通过response.encodeURL(url) 进行实现；API对encodeURL的实现为，当浏览器支持Cookie时，url不做任何处理；当浏览器不支持Cookie的时候，将会重写URL将SessionID拼接到访问地址后。
+ ==存储内容==：cookie只能保存字符串类型，以文本的方式；session通过类似与Hashtable的数据结构来保存，能支持任何类型的对象(session中可含有多个对象)
+ ==存储的大小==：cookie：单个cookie保存的数据不能超过4kb；session大小没有限制。
+ ==安全性==：针对cookie所存在的攻击：Cookie欺骗，Cookie截获；session的安全性大于cookie。原因如下：
    + （1）sessionID存储在cookie中，若要攻破session首先要攻破cookie；
    + （2）sessionID是要有人登录，或者启动session_start才会有，所以攻破cookie也不一定能得到sessionID；
    + （3）第二次启动session_start后，前一次的sessionID就是失效了，session过期后，sessionID也随之失效。
    + （4）sessionID是加密的
+ ==应用场景==
    + cookie 
        + （1）判断用户是否登陆过网站，以便下次登录时能够实现自动登录（或者记住密码）。如果我们删除cookie，则每次登录必须从新填写登录的相关信息。
        + （2）保存上次登录的时间等信息。
        + （3）保存上次查看的页面
        + （4）浏览计数
    + session：Session用于保存每个用户的专用信息，变量的值保存在服务器端，通过SessionID来区分不同的客户。
        + （1）网上商城中的购物车
        + （2）保存用户登录信息
        + （3）将某些数据放入session中，供同一用户的不同页面使用
        + （4）防止用户非法登录
+ ==缺点==
    + cookie
        + （1）大小受限
        + （2）用户可以操作（禁用）cookie，使功能受限
        + （3）安全性较低
        + （4）有些状态不可能保存在客户端。
        + （5）每次访问都要传送cookie给服务器，浪费带宽。
        + （6）cookie数据有路径（path）的概念，可以限制cookie只属于某个路径下。
    + session
        + （1）Session保存的东西越多，就越占用服务器内存，对于用户在线人数较多的网站，服务器的内存压力会比较大。
        + （2）依赖于cookie（sessionID保存在cookie），如果禁用cookie，则要使用URL重写，不安全
        + （3）创建Session变量有很大的随意性，可随时调用，不需要开发者做精确地处理，所以，过度使用session变量将会导致代码不可读而且不好维护。



#### cookie、localStorage、sessionStorage的区别
> 参考链接：https://www.cnblogs.com/cencenyue/p/7604651.html
+ ==生命周期==：
    + ==localStorage==:localStorage的生命周期是永久的，关闭页面或浏览器之后localStorage中的数据也不会消失。除非主动删除数据，否则数据永远不会消失。
    + ==sessionStorage==的生命周期是在仅在当前会话下有效。sessionStorage引入了一个“浏览器窗口”的概念，sessionStorage是在同源的窗口中始终存在的数据。只要这个浏览器窗口没有关闭，即使刷新页面或者进入同源另一个页面，数据依然存在。但是sessionStorage在关闭了浏览器窗口后就会被销毁。同时独立的打开同一个窗口同一个页面，sessionStorage也是不一样的。
    + ==cookie==:默认情况下,浏览器会话结束时即将所有cookie删除；不过也可以自己设置才删除时间，这个值时个GMT格式的日期。因此，cookie可在浏览器关闭后依然保存在客户的机器上。如果你设置的失效日期是个以前的时间，则cookie会被立刻删除。
+ ==存储大小==：
    + ==localStorage==和==sessionStorage==的存储数据大小一般都是：5MB
    + 为了最佳的浏览器兼容性，最好将整个==cookie==长度限制在4095B以内。
+ ==存储位置==：
    + ==localStorage==和==sessionStorage==都保存在客户端，不与服务器进行交互通信。
    + 浏览器会为每个请求添加==cookie== HTTP头将信息发送回服务器。
+ ==存储内容类型==：
    + ==localStorage==和==sessionStorage==只能存储字符串类型，对于复杂的对象可以使用ECMAScript提供的JSON对象的stringify和parse来处理
    + ==cookie==的名称和值必须是经过URL编码的。
+ 获取方式：
    + localStorage：window.localStorage;
    + sessionStorage：window.sessionStorage;
+ ==应用场景==
    + ==localStoragese==：常用于长期登录（+判断用户是否已登录），适合长期保存在本地的数据。
    + ==sessionStorage==：敏感账号一次性登录；
    + ==cookie==
        + （1）判断用户是否登陆过网站，以便下次登录时能够实现自动登录（或者记住密码）。如果我们删除cookie，则每次登录必须从新填写登录的相关信息。
        + （2）保存上次登录的时间等信息。
        + （3）保存上次查看的页面
        + （4）浏览计数

##### webStorage相对于cookie的优点
1. 存储空间更大
2. 节省网络流量：减少了客户端和服务器端的交互
3. 快速显示：有的数据存储在WebStorage上，再加上浏览器本身的缓存。获取数据时可以从本地获取会比从服务器端获取快得多，所以速度更快；
4. 安全性：WebStorage不会随着HTTP header发送到服务器端，所以安全性相对于cookie来说比较高一些，不会担心截获，但是仍然存在伪造问题；
5. WebStorage提供了一些方法，数据操作比cookie方便；
    + setItem (key, value) 
    + getItem (key) 
    + removeItem (key) 
    + clear () ——  删除所有的数据
    + key (index) —— 获取某个索引的key
#### 前端安全
+ 参考https://segmentfault.com/a/1190000007059639（用大白话谈谈XSS与CSRF）
+ XSS：跨站脚本（Cross-site scripting).通过客户端脚本语言（最常见如：JavaScript）
在一个论坛发帖中发布一段恶意的JavaScript代码就是脚本注入，如果这个代码内容有请求外部服务器，那么就叫做XSS！
+ CSRF/XSRF:跨站请求伪造（英语：Cross-site request forgery）.冒充用户发起请求（在用户不知情的情况下）,完成一些违背用户意愿的请求（如恶意发帖，删帖，改密码，发邮件等）。CSRF更偏向于一个攻击结果，只要发起了冒牌请求那么就算是CSRF了。
+ 通常来说CSRF是由XSS实现的，所以CSRF时常也被称为XSRF[用XSS的方式实现伪造请求]（但CSRF还可以直接通过命令行模式（命令行敲命令来发起请求）直接伪造请求[只要通过合法验证即可]）。条条大路（XSS路，命令行路）通罗马（CSRF马，XSRF马）。
#####  如何防范XSS？
+ 参考https://www.cnblogs.com/wangyuyu/p/3388180.html（总结 XSS 与 CSRF 两种跨站攻击）
+  AJAX 技术所使用的 XMLHttpRequest 对象的同源策略
+  如果我们不需要用户输入 HTML 而只想让他们输入纯文本，那么把所有用户输入进行 HTML 转义输出是个不错的做法。
+  在一些场合我们要允许用户输入 HTML，又要过滤其中的脚本。简单的方法就是白名单重新整理。用户输入的 HTML 可能拥有很复杂的结构，但我们并不将这些数据直接存入数据库，而是使用HTML解析库遍历节点，获取其中数据（之所以不使用XML解析库是因为HTML要求有较强的容错性）。然后根据用户原有的标签属性，重新构建HTML元素树。构建的过程中，所有的标签、属性都只从白名单中拿取。这样可以确保万无一失——如果用户的某种复杂输入不能为解析器所识别，白名单重新整理的策略会直接丢弃掉这些未能识别的部分。
##### 如何防范CSRF？
+ 因为请求可以从任何一方发起，而发起请求的方式多种多样，可以通过 iframe、ajax（这个不能跨域，得先 XSS）、Flash内部发起请求（总是个大隐患）。由于几乎没有彻底杜绝 CSRF 的方式，我们一般的做法，是以各种方式提高攻击的门槛。
+ 首先可以提高的一个门槛，就是改良站内 API 的设计。对于发布帖子这一类创建资源的操作，应该只接受 POST 请求，而 GET 请求应该只浏览而不改变服务器端资源。现在的浏览器基本不支持在表单中使用 PUT 和 DELETE 请求方法，我们可以使用 ajax 提交请求（例如通过 jquery-form 插件，我最喜欢的做法），也可以使用隐藏域指定请求方法，然后用 POST 模拟 PUT 和 DELETE （Ruby on Rails 的做法）。这么一来，不同的资源操作区分的非常清楚，我们把问题域缩小到了非 GET 类型的请求上——攻击者已经不可能通过发布链接来伪造请求了，但他们仍可以发布表单，或者在其他站点上使用我们肉眼不可见的表单，在后台用 js 操作，伪造请求。
+ 另一种方法叫做请求令牌，首先服务器端要以某种策略生成随机字符串，作为令牌（token）， 保存在 Session 里。然后在发出请求的页面，把该令牌以隐藏域一类的形式，与其他信息一并发出。在接收请求的页面，把接收到的信息中的令牌与 Session 中的令牌比较，只有一致的时候才处理请求，否则返回 HTTP 403 拒绝请求或者要求用户重新登陆验证身份。使用请求令牌时应注意：
    + 虽然请求令牌原理和验证码有相似之处，但不应该像验证码一样，全局使用一个 Session Key。
    + 在 ajax 技术应用较多的场合，因为很有请求是 JavaScript 发起的，使用静态的模版输出令牌值或多或少有些不方便。但无论如何，请不要提供直接获取令牌值的 API。这么做无疑是锁上了大门，却又把钥匙放在门口，
    + 请求令牌理论上是可破解的，所以非常重要的场合，应该考虑使用验证码（令牌的一种升级，目前来看破解难度极大），或者要求用户再次输入密码（亚马逊、淘宝的做法）。
    + 无论是普通的请求令牌还是验证码，服务器端验证过一定记得销毁。
