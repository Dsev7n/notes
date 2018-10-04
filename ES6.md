## 2. let和const命令
#### 2.1 let命令
+ let声明的变量只在花括号内有效,`let`没有声明提升，一定要先声明后使用
+ 暂时性死区：ES6 明确规定，如果区块中存在let和const命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。凡是在声明之前就使用这些变量，就会报错（就算有同名的全局变量）。
+ for循环还有一个特别之处，就是设置循环变量的那部分是一个父作用域，而循环体内部是一个单独的子作用域。
```
for (let i = 0;i <3; i++) {
    let i = 'abc';
    console.log(i);
}
```
上面代码正确运行，输出了 3次abc。这表明==函数内部的变量i与循环变量i不在同一个作用域==，有各自单独的作用域。
+ 不允许重复声明：但是`let`不允许在相同作用域内，重复声明同一变量。如函数内部、函数的参数和实际值，都属于同一作用域。
+ `let`实际上为JS新增了==块级作用域==

#### 2.2 块级作用域
+ 考虑到环境导致的行为差异太大，应该避免在块级作用域内声明函数。如果确实需要，也应该写成函数表达式，而不是函数声明语句。
```
//函数声明语句
{
    let a = 'secret';
    function f(){
        return a;
    }
}

//函数表达式
{
    let a = 'secret';
    let f = function () {
        return a;
    };
}
```

#### 2.3 `const`命令
+ `const`一旦声明变量，就必须==立即初始化==,而且该值就不能再改变。
+ 只在块级作用域内有效
+ 不存在变量提升，同样存在暂时性死区，不可重复声明。
+ const实际上保证的，并不是变量的值不得改动，而是变量指向的那个内存地址不得改动。对于简单类型的数据（数值、字符串、布尔值），值就保存在变量指向的那个内存地址，因此==等同于常量==。但对于复合类型的数据（主要是对象和数组），变量指向的内存地址，保存的只是一个指针，const只能保证这个==指针是固定的==，至于它指向的数据结构是不是可变的，就完全不能控制了
---


## 3.变量的解构赋值
+ 不完全解构：等号左边的模式，只匹配一部分的等号右边的数组。这种情况下，解构依然可以成功。
+ 只要某种数据结构具有 Iterator 接口，都可以采用数组形式的解构赋值。如`fibs`和`Set`都可以。
+ 默认值可以引用解构赋值的其他变量，但该变量必须已经声明。
```
let [x = 1, y = x] = [];//x=1;y=1
let [x = 1, y = x] = [1,2];//x=1;y=2
```
#### 3.2 对象的解构赋值
+ 对象解构时，对象的属性没有次序，变量必须与属性同名，才能取到正确的值。
+ 如果变量名与属性名不一致，必须写成
```
let {foo: baz} = { foo: 'aaa', bar: 'bbb' };
baz//"aaa"
```
+ 默认值生效的条件是，对象的属性值严格等于undefined
#### 字符串的解构赋值
+ 字符串在解构时被转换成了一个类似数组的对象，此时还有一个`length`属性，可以对这个属性解构赋值如下：
```
let {length : len} = 'hello';
len //5
```
#### 3.4数值和布尔值的解构赋值
+ 解构赋值时，如果等号右边是数值和布尔值，则会先转为对象。

#### 3.5 函数参数的解构赋值

#### 3.6 圆括号问题
+ 不能使用圆括号的情况

#### 3.7 用途
+ 交换变量的值
+ 从函数返回多个值
+ 函数参数的定义，可以方便地将一组参数与变量名对应起来。
+ 提取JSON数据
+ 函数参数的默认值
+ 遍历Map结构：任何部署了 Iterator 接口的对象，都可以用for...of循环遍历。Map 结构原生支持 Iterator 接口，配合变量的解构赋值，获取键名和键值就非常方便。
+ 输入模块的指定方法
---

## 4.字符串的扩展
#### 4.1 字符的Unicode表示法
+ \uxxxx表示一个字符，表示范围\u0000~\uFFFF，超出范围用双字节表示。
+ ES6中，只要将码点放入大括号就好。
```
“\u{20BB7}” 
```
#### 4.2 codePointAt()
+ codePointAt方法会正确返回 32 位的 UTF-16 字符的码点。而charCodeAt只能返回16位的。
+ `codePointAt`方法是测试一个字符由两个字节还是由四个字节组成的最简单方法。
```
function is32bBit(c){
    return c.codePointAt(0) > 0xFFFF;
}
```
#### 4.3 String.fromCodePoint()
+ ES5提供的`String.fromCharCode`方法不能识别32位。
+ 作用与`codePointAt`相反

#### 4.4 字符串的遍历器端口
+ ES6为字符串添加了遍历器接口，使得字符串可以被`for...of`循环遍历
```
for (let codePoint of 'foo') {
    cosole.log(codePoint)
}
```
+ 除了遍历字符串，这个遍历器最大的优点是可以识别大于0xFFFF的码点，传统的for循环无法识别这样的码点。

#### 4.5 at()
+ ES5 对字符串对象提供`charAt`方法，返回字符串给定位置的字符。该方法不能识别码点大于0xFFFF的字符。而`at`可以。

#### 4.6 normalize()
+ 两种等价表示法：一种是直接提供带重音符号的字符，比如`Ǒ`（\u01D1）。另一种是提供合成符号（combining character），比如`O`（\u004F）和`ˇ`（\u030C）合成`Ǒ`（\u004F\u030C）。
+ ES6 提供字符串实例的normalize()方法， 将Unicode 正规化
```
'\u01D1'.normalize() === '\u004F\u030c'.nomalize()
//true
```
+  `normalize`可以接受一个参数，四个可选值
    + `NFC`,默认。表示“标准等价合成”（Normalization Form Canonical Composition），返回多个简单字符的合成字符。
    + `NFD`，表示“标准等价分解”（Normalization Form Canonical Decomposition），即在标准等价的前提下，返回合成字符分解的多个简单字符。
    + `NFKC`，表示“兼容等价合成”（Normalization Form Compatibility Composition），返回合成字符。所谓“兼容等价”指的是语义上存在等价，但视觉上不等价，
    + `NFKD`，表示“兼容等价分解”（Normalization Form Compatibility Decomposition），即在兼容等价的前提下，返回合成字符分解的多个简单字符。

#### 4.7 includes(), startsWith(), endsWith()
+ 以前的`indexOf()`方法用来确定一个字符串是否包含在另一个字符串中
+ ES6这三个方法都支持第二个参数，对于`includes`和`startsWith`,表示开始搜索的位置;对于`endsWith`表示结束搜索的位置：
    + `includes()`：返回布尔值，表示是否找到了参数字符串。
    + `startsWith()`：返回布尔值，表示参数字符串是否在原字符串的头部。
    + `endsWith()`：返回布尔值，表示参数字符串是否在原字符串的尾部。

#### 4.8 repeat()
+ `repeat()`方法将原字符串重复`n`次。参数如果是小数，会被取整；如果是是负数或者`Infinity`，会报错。

#### 4.9 padStart(), padEnd()
+ ES2017 引入了字符串补全长度的功能。`padStart()`用于头部补全，`padEnd()`用于尾部补全。
```
'x'.padStart(5, 'ab') // 'ababx'
'x'.padEnd(5, 'ab') // 'xabab'
```
+ 如果原字符串的长度，等于或大于指定的最小长度，则返回原字符串。
+ 如果省略第二个参数，默认使用空格补全长度。
+ `padStart`常用语为数值补全指定位数和提示字符串格式。
```
'12'.padStart(10, 'YYYY-MM-DD') // "YYYY-MM-12"
'09-12'.padStart(10, 'YYYY-MM-DD') // "YYYY-09-12"
```
#### 4.10 matchAll()
+ 该方法返回一个正则表达式在当前字符串的所有匹配

#### 4.11 模板字符串
```
$('#result').append(`
  There are <b>${basket.count}</b> items
   in your basket, <em>${basket.onSale}</em>
  are on sale!
`);
```
+ 如果在模板字符串中需要使用反引号，则前面要用反斜杠转义。
+ 表示多行字符串，所有的空格和缩进都会被保留在输出之中。`<ul>`标签前面会有一个换行。如果你不想要这个换行，可以使用`trim`方法消除它。
```
$('#list').html(`
<ul>
  <li>first</li>
  <li>second</li>
</ul>
`.trim());
```
+ 模板字符串中嵌入变量，需要将变量名写在`${}`之中。大括号内部可以放入任意的 JavaScript 表达式，可以进行运算，以及引用对象属性,还能调用函数。
+ 如果模板字符串中的变量没有声明，将报错。如果大括号内部是一个字符串，将会原样输出。

#### 4.13 标签模板
+ 一个重要应用，就是过滤 HTML 字符串，防止用户输入恶意内容。另一个应用，就是多语言转换（国际化处理）。甚至可以使用标签模板，在 JavaScript 语言之中嵌入其他语言。

#### 4.14 String.raw()
#### 4.15 模板字符串的限制
---
## 5.正则的扩展
#### 5.1 RegExp构造函数
+ ES6中，如果RegExp构造函数第一个参数是一个正则对象，那么可以使用第二个参数指定修饰符。
```
new RegExp(/abc/ig, 'i').flags
// "i"
```
#### 5.2 字符串的正则方法
红宝书P126
#### 5.3 u修饰符
+ ES5 RegExp实例方法
    + exec()：专门为捕获组设计的方法，传入字符串。返回包含第一个匹配项的数组，该数组有两个额外属性：`index`和`input`。
    + test():调用方法
    `pattern.test(text)`，在模式与改参数匹配的情况下返回true。
    + toLocaleString()、toString():返回正则表达式的字面量
    + valueOf():返回正则表达式本身。
```
var text = "mom and dad and baby";
var pattern = /mom( and dad( and baby)?)?/gi;

var matches = pattern.exec(text);
```
+ `u`修饰符表示“Unicode模式”，会正确处理四个字节UTF-16编码
```
/^\uD83D/u.test('\uD83D\uDC2A') // false
/^\uD83D/.test('\uD83D\uDC2A') // true
```
+ 点字符（.)表示出了换行符以外的任意单个字符

var s = '𠮷';

/^.$/.test(s) // false ,^表示字符串的起始位置，$表示字符串的结束位置
/^.$/u.test(s) // true
```
+ Unicode字符表示法：ES6 新增了使用大括号表示 Unicode 字符，这种表示法在正则表达式中必须加上u修饰符，才能识别当中的大括号，否则会被解读为量词。
+ `\S`是预定义模式，匹配所有非空白字符。只有加了u修饰符，它才能正确匹配码点大于0xFFFF的 Unicode 字符。
```
#### 5.4 y修饰符
+ `y`表示“粘连”（sticky),匹配必须从第一个位置开始，才会返回true。
+ 
---

## 6. 数值的扩展
####  6.1 二进制和八进制表示法
+ 用前缀0b（或0B）表示二进制，用前缀0o（或0O）表示八进制
+ 用`Number`方法将其他进制转换为十进制。

#### 6.2 Number.isFinite(), Number.isNaN()
+ `Number.isFinite()`用来检查一个数值是否为有限的（finite），即不是`Infinity`。如果参数类型不是数值，`Number.isFinite`一律返回`false`。
+ 它们与传统的全局方法`isFinite()`和`isNaN()`的区别在于，传统方法先调用`Number()`将非数值的值转为数值，再进行判断，而这两个新方法只对数值有效

#### 6.3  Number.parseInt(), Number.parseFloat()
+ ES6 将全局方法parseInt()和parseFloat()，移植到Number对象上面，行为完全保持不变。这样做的目的，是逐步减少全局性方法，使得语言逐步模块化。

#### 6.4 Number.isInteger()
+ 整数和浮点数采用的是同样的储存方法，所以 25 和 25.0 被视为同一个值。
```
Number.isInteger(25.0) // true
```
+ 如果对数据精度的要求较高，不建议使用`Number.isInteger()`判断一个数值是否为整数。

#### 6.5 Number.EPSILON
+ 表示 1 与大于 1 的最小浮点数之间的差。误差如果小于这个值，就可以认为已经没有意义了，即不存在误差了。

#### 6.6 安全整数和Number.isSafeInteger()
+ JavaScript 能够准确表示的整数范围在-2^53到2^53之间（不含两个端点），超过这个范围，无法精确表示这个值。ES6 引入了`Number.MAX_SAFE_INTEGER`和`Number.MIN_SAFE_INTEGER`这两个常量，用来表示这个范围的上下限。
---

#### 6.7 Math对象的扩展
#### Math.trunc()
+ 该方法用于去除一个数的小数部分，返回整数部分。对于非数值，`Math.trunc`内部使用`Number`方法将其先转为数值。对于空值和无法截取整数的值，返回`NaN`。对于没有部署这个方法的环境，可以用下面的代码模拟。
```
Math.trunc = Math.trunc || function(x) {
  return x < 0 ? Math.ceil(x) : Math.floor(x);
};
```
#### Math.sign()
+ 对于非数值，会先将其转换为数值。
    + 参数为正数，返回+1；
    + 参数为负数，返回-1；
    + 参数为 0，返回0；
    + 参数为-0，返回-0;
    + 其他值，返回NaN。

#### Math.cbrt(),计算立方根
+ `Math.cbrt`方法内部也是先使用`Number`方法将其转为数值。
+ 对于没有部署这个方法的环境，可以用下面的代码模拟。
```
Math.cbrt = Math.cbrt || function(x) {
  var y = Math.pow(Math.abs(x), 1/3);
  return x < 0 ? -y : y;
};
```
#### Math.clz32
+ `clz32`这个函数名就来自”count leading zero bits in 32-bit binary representation of a number“（计算一个数的 32 位二进制形式的前导 0 的个数）的缩写。
```
Math.clz32(1000) // 22,1000 的二进制形式是0b1111101000，一共有 10 位
```
+ 左移运算符（`<<`）与`Math.clz32`方法直接相关。
+ 对于空值或其他类型的值，Math.clz32方法会将它们先转为数值，然后再计算。
```
Math.clz32() // 32
Math.clz32(NaN) // 32
Math.clz32(Infinity) // 32
Math.clz32(null) // 32
Math.clz32('foo') // 32
Math.clz32([]) // 32
Math.clz32({}) // 32
Math.clz32(true) // 31
```
#### Math.imul()
+ 如果只考虑最后 32 位，大多数情况下，`Math.imul(a, b)`等同于`(a * b)|0`的效果（超过 32 位的部分溢出）。
+ 区别：对于那些很大的数的乘法，低位数值往往都是不精确的，Math.imul方法可以返回正确的低位数值。

#### Math.fround()
+ 主要作用，是将64位双精度浮点数转为32位单精度浮点数。如果小数的精度超过24个二进制位，返回值就会不同于原值，否则返回值不变（即与64位双精度值一致）。

#### Math.hypot()
+ 返回所有参数的平方和的平方根。只要有一个参数无法转为数值，就会返回 NaN。用于计算方差。

#### 对数方法
+ `Math.expm1()`: 返回 `e^x - 1`，即`Math.exp(x) - 1`。
+ `Math.log1p(x)`: 返回`Math.log(1 + x)`。如果x小于-1，返回`NaN`。
+ `Math.log10(x)`: 返回以 10 为底的x的对数。如果x小于 0，则返回 `NaN`。
+ `Math.log2(x)`: 返回以 2 为底的x的对数。如果x小于 0，则返回 `NaN`。

#### 双曲函数方法
+ 新增了6个：`Math.sinh(x)`、`Math.cosh()`、`Math.tanh(x)`、`Math.asinh()`、`Math.acosh(x)`、`Math.atanh()`

#### 6.8 指数运算符（**）
+ 与等号结合，形成一个新的赋值运算符（`**=`）。
```
2 ** 2 // 4
2 ** 3 // 8

let a = 1.5;
a **= 2;
// 等同于 a = a * a;

let b = 4;
b **= 3;
// 等同于 b = b * b * b;
```
---
## 7.函数的扩展
#### 7.1 函数参数的默认值
+ ES5不能指定默认值
```
function log(x, y) {
  if (typeof y === 'undefined') {
    y = 'World';
   }
  console.log(x, y);
}
```
+ ES6 允许为函数的参数设置默认值，即直接写在参数定义的后面。优点：首先，阅读代码的人，可以立刻意识到哪些参数是可以省略的，不用查看函数体或文档；其次，有利于将来的代码优化，即使未来的版本在对外接口中，彻底拿掉这个参数，也不会导致以前的代码无法运行。
+ 参数变量x是默认声明的，在函数体中，不能用let或const再次声明。
+ 参数默认值时惰性求值的，每次调用的时候才开始计算。
+ 定义了默认值的参数，应该放在函数参数的尾部。
+ 函数`length`属性的含义是，该函数预期传入的参数个数。指定了默认值以后，函数的`length`属性，将返回没有指定默认值的参数个数。如果设置了默认值的参数不是尾参数，那`length`属性也不再计入后面的参数了。

#### 7.2 rest参数
+ 形式：`...变量名`
+ 作用：用于获取函数任意数目的参数，这样就不需要使用`arguments`对象了，rest参数对应的是一个数组。
+ `arguments`对象不是数组，而是一个类似数组的对象。所以为了使用数组的方法，必须使用`Array.prototype.slice.call`先将其转为数组
```
// arguments变量的写法
function sortNumbers() {
  return Array.prototype.slice.call(arguments).sort();
}

// rest参数的写法
const sortNumbers = (...numbers) => numbers.sort();
```
+ 利用 rest 参数改写数组`push`方法的例子:
```
function push(array, ...items) {
  items.forEach(function(item) {
    array.push(item);
    console.log(item);
  });
}

var a = [];
push(a, 1, 2, 3)
```
+ 注意，rest 参数之后不能再有其他参数，否则会报错。
+ 函数的`length`属性，不包括 rest 参数。

#### 7.3 严格模式
+ ES6规定，规定只要函数参数使用了默认值、解构赋值、或者扩展运算符，那么函数内部就不能显式设定为严格模式，否则会报错。这样规定的原因是，函数内部的严格模式，同时适用于函数体和函数参数。但是，函数执行的时候，先执行函数参数，然后再执行函数体。这样就有一个不合理的地方，只有从函数体之中，才能知道参数是否应该以严格模式执行，但是参数却应该先于函数体执行。
+ 两种方法可以规避这种限制。第一种是设定全局性的严格模式；第二种是把函数包在一个无参数的立即执行函数里面。

#### 7.4 name属性
+ 作用：返回函数的函数名
```
var f = function () {};//匿名函数

// ES5
f.name // ""

// ES6
f.name // "f"



const bar = function baz() {};//具名函数

// ES5
bar.name // "baz"

// ES6
bar.name // "baz"
```
+ `Function`构造函数返回的函数实例，`name`属性的值为`anonymous`。
+ `bind`返回的函数，`name`属性值会加上`bound`前缀。
```
(new Function).name // "anonymous"

function foo() {};
foo.bind({}).name // "bound foo"

(function(){}).bind({}).name // "bound "
```
#### 7.5 箭头函数
+ 格式：`（参数）=> 返回值`
```
var f = v => v;

// 等同于
var f = function (v) {
  return v;
};


var sum = (num1, num2) => num1 + num2;
// 等同于
var sum = function(num1, num2) {
  return num1 + num2;
};
```
+ 如果箭头函数的代码块部分多于一条语句，就要使用大括号将它们括起来，并且使用return语句返回。如果箭头函数直接返回一个对象，必须在对象外面加上括号，否则会报错。
+ 函数体内的`this`对象，就是定义时所在的对象，而不是使用时所在的对象。`this`对象的指向是可变的，但是在箭头函数中，它是固定的。
+ 不可以使用`arguments`对象，该对象在箭头函数体内不存在
+ 箭头函数里的`this`绑定定义时所在的作用域，而普通函数的`this`指向运行时所在的作用域。
+ this指向的固定化，并不是因为箭头函数内部有绑定this的机制，实际原因是箭头函数根本没有自己的this，导致==内部的this就是外层代码块的this==。正是因为它没有this，所以也就不能用作构造函数。
+ 除了`this`，以下三个变量在箭头函数之中也是不存在的，指向外层函数的对应变量：`arguments`、`super`、`new.target`。


#### 7.6 双冒号运算符
+ 函数绑定运算符是并排的两个冒号`::`，双冒号左边是一个对象，右边是一个函数。该运算符会自动将左边的对象，作为上下文环境（即`this`对象），绑定到右边的函数上面。
---
## 8. 数组的扩展
#### 8.1 扩展运算符
+ 数组的扩展运算符三个点（...)，是rest参数的逆运算，将一个数组转为用逗号分隔的==参数序列==。该运算符主要用于函数调用。
```
function add(x, y){
    return x + y;
}
const numbers = [4, 38];
add(...numbers);  //42
```
##### 替代函数的apply方法
+ 由于扩展运算符可以展开数组，所以不再需要apply方法，将数组转为函数的参数了。
```
// ES5 的写法
function f(x, y, z) {
  // ...
}
var args = [0, 1, 2];
f.apply(null, args);

// ES6的写法
function f(x, y, z) {
  // ...
}
let args = [0, 1, 2];
f(...args);
```
+ ES5 写法中，push方法的参数不能是数组，所以只好通过apply方法变通使用push方法。有了扩展运算符，就可以直接将数组传入push方法。
```
// ES5的 写法
var arr1 = [0, 1, 2];
var arr2 = [3, 4, 5];
Array.prototype.push.apply(arr1, arr2);

// ES6 的写法
let arr1 = [0, 1, 2];
let arr2 = [3, 4, 5];
arr1.push(...arr2);
```
##### 扩展运算符的应用
+ （1）复制数组：数组是复合的数据类型，直接复制的话，只是复制了数组的指针，而不是克隆一个全新的数组。修改其中一个数组，会直接导致另一个数组的变化。
```
ES5 只能用变通方法来复制数组。
const a1 = [1, 2];
const a2 = a1.concat();


扩展运算符提供了复制数组的简便写法。
const a1 = [1, 2];
// 写法一
const a2 = [...a1];
// 写法二
const [...a2] = a1;
```
+ (2)合并数组：不过，这两种方法都是浅拷贝，它们的成员都是对原数组成员的引用，如果修改了原数组的成员，会同步反映到新数组。
```
const arr1 = ['a', 'b'];
const arr2 = ['c'];
const arr3 = ['d', 'e'];

// ES5 的合并数组
arr1.concat(arr2, arr3);
// [ 'a', 'b', 'c', 'd', 'e' ]

// ES6 的合并数组
[...arr1, ...arr2, ...arr3]
// [ 'a', 'b', 'c', 'd', 'e' ]
```
+ (3) 与解构赋值结合
+ （4）字符串：
```
[...'hello']
// [ "h", "e", "l", "l", "o" ]
```
+ (5)实现了Iterator接口的对象
+ （6）Map和Set结构，Generator函数

#### 8.2 Array.from()
+ Array.from方法用于将两类对象转为真正的数组：类似数组的对象（array-like object）和可遍历（iterable）的对象（包括 ES6 新增的数据结构 Set 和 Map）。
+ 扩展运算符背后调用的是遍历器接口（Symbol.iterator），如果一个对象没有部署这个接口，就无法转换。Array.from方法还支持类似数组的对象。所谓类似数组的对象，本质特征只有一点，即必须有length属性。

#### 8.3 Array.of()
+ 这个方法的主要目的，是弥补数组构造函数Array()的不足。Array（）方法参数个数只有一个时，实际上是指定数组的长度。Array.of基本上可以用来替代Array()或new Array()，并且不存在由于参数不同而导致的重载。它的行为非常统一
```
Array(3)  //[, , ,]
Array.of(3)  //[3]
Array.of(3,11,8) //[3,11,8]

Array.of(undefined)  //[undefined]
```
+ Array.of方法可以用下面的代码模拟实现
```
function ArrayOf(){
  return [].slice.call(arguments);
}
```
#### 8.5 数组实例的copyWithin()
+ 该方法将指定位置的元素复制到其他位置（会覆盖原有成员），然后返回当前数组，会改变当前数组
```
Array.prototype.copyWith(target, start = 0, end = this.length)
```
+ 他接收三个参数
    + target(必须)：从该位置开始替换数据。如果为负值，表示倒数。
    + start(可选)： 从该位置开始读取数据，默认为0，如果为负值，表示倒数
    + end(可选)：从该位置前停止读取数据，默认等于数组长度。
```
[1, 2, 3, 4, 5].copyWithin(0, 3)
// [4, 5, 3, 4, 5]
```
#### 8.5 数组实例的find()和findIndex()
+ 数组实例的find方法，用于找出第一个符合条件的数组成员。它的参数是一个回调函数，所有数组成员依次执行该回调函数，直到==找出第一个返回值为true的成员，然后返回该成员==。如果没有符合条件的成员，则返回undefined。
```
[1, 4, -5, 10].find((n) => n < 0)  // -5
```
+ 数组实例的findIndex方法，返回第一个符合条件的数组成员的位置，如果所有成员都不符合条件，则返回-1。
```
[1, 5, 10, 15].findIndex(function(value, index, arr) {
  return value > 9;
}) // 2
```
+ 这两个方法都可以接受第二个参数，用来==绑定回调函数的this对象==。
```
function f(v){
  return v > this.age;
}
let person = {name: 'John', age: 20};
[10, 12, 26, 15].find(f, person);    // 26
```
+ indexOf方法无法识别数组的NaN成员，但是findIndex方法可以借助Object.is方法做到。
```
[NaN].indexOf(NaN)    // -1

[NaN].findIndex(y => Object.is(NaN, y))   // 0
```
#### 8.6 数组实例的fill()
+ 该方法用给定值填充一个数组，用于空数组的初始化非常方便，数组中已有的元素，会被全部抹去。该方法还可以接受第二个和第三个参数，用于指定填充的起始位置和结束位置。
```
['a', 'b', 'c'].fill(7)
// [7, 7, 7]

new Array(3).fill(7)
// [7, 7, 7]

['a', 'b', 'c'].fill(7, 1, 2)
// ['a', 7, 'c']
```
+ 注意，如果填充的类型为对象，那么被赋值的是同一个内存地址的对象，而不是深拷贝对象。
```
let arr = new Array(3).fill({name: "Mike"});
arr[0].name = "Ben";
arr
// [{name: "Ben"}, {name: "Ben"}, {name: "Ben"}]

let arr = new Array(3).fill([]);
arr[0].push(5);
arr
// [[5], [5], [5]]
```
#### 8.7 数组实例的entries(),keys()和values()
+   ES6 提供三个新的方法——entries()，keys()和values()——用于遍历数组。它们都返回一个遍历器对象（详见《Iterator》一章），可以用for...of循环进行遍历，唯一的区别是keys()是对键名的遍历、values()是对键值的遍历，entries()是对键值对的遍历。
```
for (let index of ['a', 'b'].keys()) {
  console.log(index);
}
// 0
// 1

for (let elem of ['a', 'b'].values()) {
  console.log(elem);
}
// 'a'
// 'b'

for (let [index, elem] of ['a', 'b'].entries()) {
  console.log(index, elem);
}
// 0 "a"
// 1 "b"
```

#### 8.8 数组实例的includes()
```
[1, 2, 3].includes(2)     // true
[1, 2, 3].includes(4)     // false
[1, 2, NaN].includes(NaN) // true
```
+ 没有该方法之前，我们通常使用数组的indexOf方法，检查是否包含某个值。indexOf方法有两个缺点，一是不够语义化，它的含义是找到参数值的第一个出现位置，所以要去比较是否不等于-1，表达起来不够直观。二是，它内部使用严格相等运算符（===）进行判断，这会导致对NaN的误判。
+ Map 和 Set 数据结构有一个has方法，需要注意与includes区分。
    + Map 结构的has方法，是用来查找键名的，比如Map.prototype.has(key)、WeakMap.prototype.has(key)、Reflect.has(target, propertyKey)。
    + Set 结构的has方法，是用来查找值的，比如Set.prototype.has(value)、WeakSet.prototype.has(value)。

#### 8.9 数组的空位
+ in运算符用于判断数组的某一个位置有没有值。in前面的值时位置索引
```
0 in [undefined, undefined, undefined] // true
0 in [, , ,] // false
```
+ 由于空位的处理规则非常不统一，所以建议避免出现空位。
---

## 9. 对象的扩展
#### 9.1 属性的简洁表示法
+ ES6允许直接写入变量和函数，作为对象的属性和方法。在对象之中，直接写变量名。这时，属性名为变量名，属性值为变量的值。
+ 方法也可以简写，就是省略`：function`
```
let birth = '2000/01/01';

const Person = {

  name: '张三',

  //等同于birth: birth
  birth,

  // 等同于hello: function ()...
  hello() { console.log('我的名字是', this.name); }

};
```
+ 这种写法用于函数的返回值，将会非常方便。
```
function getPoint() {
  const x = 1;
  const y = 10;
  return {x, y};
}

getPoint()
// {x:1, y:10}
```
#### 9.2 属性名表达式
+ ES6新增了一种定义对象属性的方法，当用表达式作为属性名时，将表达式放在方括号之内。
```
obj['a' + 'bc'] = 123;
//相当于
var obj={
    abc:123
}

```
+ 表达式还可以用于定义方法名
+ 注意，属性名表达式与简洁表示法，不能同时使用，会报错
```
// 报错
const foo = 'bar';
const bar = 'abc';
const baz = { [foo] };

// 正确
const foo = 'bar';
const baz = { [foo]: 'abc'};
```
#### 9.3 方法的name属性
+ 函数的name属性，返回函数名。如果对象的方法使用了取值函数（getter）和存值函数（setter），则name属性不是在该方法上面，而是该方法的属性的描述对象的get和set属性上面，返回值是方法名前加上get和set。
```
const obj = {
  get foo() {},
  set foo(x) {}
};

obj.foo.name
// TypeError: Cannot read property 'name' of undefined

const descriptor = Object.getOwnPropertyDescriptor(obj, 'foo');

descriptor.get.name // "get foo"
descriptor.set.name // "set foo"
```
+ 有两种特殊情况：bind方法创造的函数，name属性返回bound加上原函数的名字；Function构造函数创造的函数，name属性返回anonymous。
```
(new Function()).name // "anonymous"

var doSomething = function() {
  // ...
};
doSomething.bind().name // "bound doSomething"
```
#### 9.4 Object.is()
+ ES5 比较两个值是否相等，只有两个运算符：相等运算符（==）和严格相等运算符（===）。它们都有缺点，前者会自动转换数据类型，后者的NaN不等于自身，以及+0等于-0。
```
+0 === -0 //true
NaN === NaN // false

Object.is(+0, -0) // false
Object.is(NaN, NaN) // true
```
#### 9.5 Object.assign()
+ 该方法用于对象的合并，如果目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性。
```
const target = { a: 1, b: 1 };

const source1 = { b: 2, c: 2 };
const source2 = { c: 3 };

Object.assign(target, source1, source2);
target // {a:1, b:2, c:3}
```
+ 如果该参数不是对象，则会先转成对象，然后返回.由于undefined和null无法转成对象，所以如果它们作为参数，就会报错。
```
typeof Object.assign(2) // "object"
Object.assign(undefined) // 报错
Object.assign(null) // 报错
```
##### 注意点
+ （1）Object.assign方法实行的是浅拷贝，而不是深拷贝。也就是说，如果源对象某个属性的值是对象，那么目标对象拷贝得到的是这个对象的引用。
+ （2）Object.assign可以用来处理数组，但是会把数组视为对象。
```
Object.assign([1, 2, 3], [4, 5])   
// [4, 5, 3]
```
上面代码中，Object.assign把数组视为属性名为 0、1、2 的对象，1、2、3分别为目标数组的属性值，4、5为源数组的属性值，源数组的前两个属性值覆盖了目标数组的属性值。
+ （4）取值函数的处理：只能进行值的复制，如果要复制的值是一个取值函数，那么将求值后再复制。
```
const source = {
  get foo() { return 1 }
};
const target = {};

Object.assign(target, source)
// { foo: 1 }
```
##### 常见用途
+ （1）为对象添加属性
```
class Point {
  constructor(x, y) {
    Object.assign(this, {x, y});
  }
}
```
上面方法通过Object.assign方法，将x属性和y属性添加到Point类的对象实例。
+ （2）为对象添加方法
```
Object.assign(SomeClass.prototype, {
  someMethod(arg1, arg2) {  //使用了对象属性的简洁表示法
    ···
  },
  anotherMethod() {
    ···
  }
});

// 等同于下面的原型模式
SomeClass.prototype.someMethod = function (arg1, arg2) {
  ···
};
SomeClass.prototype.anotherMethod = function () {
  ···
};

```
+ （3）克隆对象
+ （4）合并多个对象
+ （5）为属性指定默认值
#### 9.6 属性的可枚举性和遍历
##### 可枚举

##### 属性的遍历
ES6一共有5种方法可以遍历对象的属性
+ （1）for..in：循环遍历对象自身的和继承的可枚举属性（不含Symbol属性）
+ （2）Object.keys(obj):返回一个数组，包括对象自身的（不含继承的）所有可枚举属性（不含Symbol属性）的==键名==
+ （3）Object.getOwnPropertyNames(obj):返回一个数组，包含对象自身的所有属性（不含 Symbol 属性，但是包括不可枚举属性）的键名。
+ （4）Object.getOwnPropertySymbols(obj):返回一个数组，包含对象自身的所有 Symbol 属性的键名。
+ (5)Reflect.ownKeys(obj):返回一个数组，包含对象自身的所有键名，不管键名是 Symbol 或字符串，也不管是否可枚举。
+ 以上的 5 种方法遍历对象的键名，都遵守同样的属性遍历的次序规则。
    + 首先遍历所有数值键，按照数值升序排列。
    + 其次遍历所有字符串键，按照加入时间升序排列
    + 最后遍历所有 Symbol 键，按照加入时间升序排列。

#### 9.7 Object.getOwnPropertyDescriptors()
+ 返回指定对象所有自身属性（非继承属性）的描述对象。
```
const obj = {
  foo: 123,
  get bar() { return 'abc' }
};

Object.getOwnPropertyDescriptors(obj)
// { foo:
//    { value: 123,
//      writable: true,
//      enumerable: true,
//      configurable: true },
//   bar:
//    { get: [Function: get bar],
//      set: undefined,
//      enumerable: true,
//      configurable: true } 
```
#### 9.8 __proto__属性，Object.setPrototypePf(),Objcect.getPrototypeOf()
##### __proto__属性（前后各两个划线）
+ 该属性用来读取或设置当前对象的prototype对象。

##### Object.setPrototypeOf()
+ 它是 ES6 正式推荐的设置原型对象的方法
```
// 格式
Object.setPrototypeOf(object, prototype)

// 用法
const o = Object.setPrototypeOf({}, null);

//等同于
function (obj, proto) {
  obj.__proto__ = proto;
  return obj;
}
```
+ 如果第一个参数不是对象，会自动转为对象。但是由于返回的还是第一个参数，所以这个操作不会产生任何效果。由于undefined和null无法转为对象，所以如果第一个参数是undefined或null，就会报错。
##### 9.9 Object.getPrototypeOf()
+ 该方法与Object.setPrototypeOf方法配套，用于读取一个对象的原型对象。
#### 9.9 super关键字
+ super的用法和this相似，不过super指向当前对象的原型对象
```
const proto = {
  foo: 'hello'
};

const obj = {
  foo: 'world',
  find() {
    return super.foo;
  }
};

Object.setPrototypeOf(obj, proto);
obj.find() // "hello"
```
+ super关键字表示原型对象时，只能用在对象的方法之中，用在其他地方都会报错。
```
// 报错
const obj = {
  foo: super.foo
}

// 报错
const obj = {
  foo: () => super.foo
}

// 报错
const obj = {
  foo: function () {
    return super.foo
  }
}
```
+ 上面三种super的用法都会报错，因为对于 JavaScript 引擎来说，这里的super都没有用在对象的方法之中。第一种写法是super用在属性里面，第二种和第三种写法是super用在一个函数里面，然后赋值给foo属性。目前，只有对象方法的简写法可以让 JavaScript 引擎确认，定义的是对象的方法。？？？
+ JavaScript 引擎内部，super.foo等同于Object.getPrototypeOf(this).foo（属性）或Object.getPrototypeOf(this).foo.call(this)（方法）。

#### 9.10 Object.keys(),Object.values(),Object.entries()
##### Object.keys()
+ ES2017 引入了跟Object.keys配套的Object.values和Object.entries，作为遍历一个对象的补充手段，供for...of循环使用。
```
let {keys, values, entries} = Object;
let obj = { a: 1, b: 2, c: 3 };

for (let key of keys(obj)) {
  console.log(key); // 'a', 'b', 'c'
}

for (let value of values(obj)) {
  console.log(value); // 1, 2, 3
}

for (let [key, value] of entries(obj)) {
  console.log([key, value]); // ['a', 1], ['b', 2], ['c', 3]
}
```
##### Object.values()
+ 方法返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键值。返回数组的成员顺序，与本章的《属性的遍历》部分介绍的排列规则一致。

##### Object.entries
+ 方法返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键值对数组。如果原对象的属性名是一个 Symbol 值，该属性会被忽略。
+ Object.entries的基本用途是遍历对象的属性。
```
let obj = { one: 1, two: 2 };
for (let [k, v] of Object.entries(obj)) {
  console.log(
    `${JSON.stringify(k)}: ${JSON.stringify(v)}`
  );
}
// "one": 1
// "two": 2
```
+ Object.entries方法的另一个用处是，将对象转为真正的Map结构。
```
const obj = { foo: 'bar', baz: 42 };
const map = new Map(Object.entries(obj));
map // Map { foo: "bar", baz: 42 }
```
+ 实现Object.entries方法，非常简单。
```
// Generator函数的版本
function* entries(obj) {
  for (let key of Object.keys(obj)) {
    yield [key, obj[key]];
  }
}

// 非Generator函数的版本
function entries(obj) {
  let arr = [];
  for (let key of Object.keys(obj)) {
    arr.push([key, obj[key]]);
  }
  return arr;
}
```
#### 9.11 对象的扩展运算符

## 10.Symbol
#### 10.1 概述
+ S6 引入了一种新的原始数据类型Symbol，表示独一无二的值。它是 JavaScript 语言的第七种数据类型，前六种是：undefined、null、布尔值（Boolean）、字符串（String）、数值（Number）、对象（Object）。
+ ES5 的对象属性名都是字符串，这容易造成属性名的冲突。比如，你使用了一个他人提供的对象，但又想为这个对象添加新的方法（mixin 模式），新方法的名字就有可能与现有方法产生冲突。如果有一种机制，保证每个属性的名字都是独一无二的就好了，这样就从根本上防止属性名的冲突。这就是 ES6 引入Symbol的原因。
+ 注意，Symbol函数前不能使用new命令，否则会报错。这是因为生成的 Symbol 是一个原始类型的值，不是对象。也就是说，由于 Symbol 值不是对象，所以不能添加属性。基本上，它是一种类似于字符串的数据类型。
+ Symbol函数可以接受一个字符串作为参数，表示对 Symbol 实例的描述，主要是为了在控制台显示，或者转为字符串时，比较容易区分。
```
let s1 = Symbol('foo');
let s2 = Symbol('bar');

typeof s1 //"Symbol"
s1 // Symbol(foo)
s2 // Symbol(bar)

s1.toString() // "Symbol(foo)"
s2.toString() // "Symbol(bar)"
```
+ 如果 Symbol 的参数是一个对象，就会调用该对象的toString方法，将其转为字符串，然后才生成一个 Symbol 值。
+ 注意，Symbol函数的参数只是表示对当前Symbol值的描述，因此相同参数的Symbol函数的返回值是不相等的
+ Symbol 值不能与其他类型的值进行运算，会报错。
```
let sym = Symbol('My symbol');

"your symbol is " + sym
// TypeError: can't convert symbol to string
```
+ ymbol 值可以显式转为字符串。Symbol值也可以转为布尔值，但是不能转为数值。
```
let s = Symbol('My symbol');
String(s) // 'Symbol(My symbol)'

let sym = Symbol();
Boolean(sym) // true
!sym  // false

if (sym) {
  // ...
}

Number(sym) // TypeError
sym + 2 // TypeError
```
#### 10.2 作为属性名的Symbol
+ Symbol 值作为对象属性名时，不能用点运算符。在对象的内部，使用 Symbol 值定义属性时，Symbol值必须放在方括号之中。如果s不放在方括号中，该属性的键名就是字符串s，而不是s所代表的那个 Symbol 值。
```
let mySymbol = Symbol();

// 第一种写法
let a = {};
a[mySymbol] = 'Hello!';

// 第二种写法
let a = {
  [mySymbol]: 'Hello!'
};

// 第三种写法
let a = {};
Object.defineProperty(a, mySymbol, { value: 'Hello!' });

// 以上写法都得到同样结果
a[mySymbol] // "Hello!"
```
+ 常量使用 Symbol值最大的好处，就是其他任何值都不可能有相同的值了，因此可以保证上面的switch语句会按设计的方式工作。
```
const COLOR_RED    = Symbol();
const COLOR_GREEN  = Symbol();

function getComplement(color) {
  switch (color) {
    case COLOR_RED:
      return COLOR_GREEN;
    case COLOR_GREEN:
      return COLOR_RED;
    default:
      throw new Error('Undefined color');
    }
}
```
#### 10.4 属性名的遍历
+ Symbol 作为属性名，该属性不会出现在for...in、for...of循环中，也不会被Object.keys()、Object.getOwnPropertyNames()、JSON.stringify()返回。但是，它也不是私有属性，有一个Object.getOwnPropertySymbols方法，可以获取指定对象的所有 Symbol 属性名。
```
const obj = {};
let a = Symbol('a');
let b = Symbol('b');

obj[a] = 'Hello';
obj[b] = 'World';

const objectSymbols = Object.getOwnPropertySymbols(obj);

objectSymbols
// [Symbol(a), Symbol(b)]
```
+ 另一个新的 API，Reflect.ownKeys方法可以返回所有类型的键名，包括常规键名和 Symbol 键名。

#### 10.5 Symbol.for(),Symbol.keyFor()
+ Syombol.for()会被登记在全局环境中供搜索，而Symnol不会，Symbol.for()不会每次调用就返回一个新的Symbol类型的值，而是会先检查给定的key是否已经存在，如果不存在才会新建一个值。
```
Symbol.for("bar") === Symbol.for("bar")
// true

Symbol("bar") === Symbol("bar")
// false
```
+ 由于Symbol()写法没有登记机制，所以每次调用都会返回一个不同的值。Symbol.keyFor方法返回一个已登记的 Symbol 类型值的key。
```
let s1 = Symbol.for("foo");
Symbol.keyFor(s1) // "foo"

let s2 = Symbol("foo");
Symbol.keyFor(s2) // undefined
```

#### 10.7 内置的Symbol值
##### Symbol.hsaInstance

## 11. Set和Map数据结构
#### 11.1 Set
+ ES6 提供了新的数据结构Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。通过add方法向Set结构加入成员,Set函数可以接受一个数组（或者具有iterable接口的其他数据结构）作为参数，用来初始化。
```
// 例一
const set = new Set([1, 2, 3, 4, 4]);
[...set]
// [1, 2, 3, 4]

// 例二
const items = new Set([1, 2, 3, 4, 5, 5, 5, 5]);
items.size // 5

// 例三
const set = new Set(document.querySelectorAll('div'));
set.size // 56
```

+ 向 Set 加入值的时候，不会发生类型转换，所以5和"5"是两个不同的值。Set 内部判断两个值是否不同，使用的算法叫做“Same-value-zero equality”，它类似于精确相等运算符（===），主要的区别是NaN等于自身，而精确相等运算符认为NaN不等于自身。另外，两个对象总是不相等的。

##### Set实例的属性和方法
+ Set 结构的实例有以下属性。
    + Set.prototype.constructor：构造函数，默认就是Set函数。
    + Set.prototype.size：返回Set实例的成员总数。
+ Set 实例的方法分为两大类：操作方法（用于操作数据）和遍历方法（用于遍历成员）。下面先介绍四个操作方法。
    + add(value)：添加某个值，返回 Set 结构本身。
    + delete(value)：删除某个值，返回一个布尔值，表示删除是否成功。
    + has(value)：返回一个布尔值，表示该值是否为Set的成员。
    + clear()：清除所有成员，没有返回值。
+ 在判断是否包括一个键上面，Object结构和Set结构的写法不同。
```
// 对象的写法
const properties = {
  'width': 1,
  'height': 1
};

if (properties[someName]) {
  // do something
}

// Set的写法
const properties = new Set();

properties.add('width');
properties.add('height');

if (properties.has(someName)) {
  // do something
}
```
+ Array.from方法可以将 Set 结构转为数组。
+ 一种去除数组重复成员的方法
```
// 去除数组的重复成员
[...new Set(array)]
```
##### 遍历操作
+ Set 结构的实例有四个遍历方法，可以用于遍历成员。
    + keys()：返回键名的遍历器
    + values()：返回键值的遍历器
    + entries()：返回键值对的遍历器
    + forEach()：使用回调函数遍历每个成员
+ Set的遍历顺序就是插入顺序，也就是编写代码的前后顺序。
##### （1） keys(),values(),entries()
+ 由于 Set 结构没有键名，只有键值（或者说键名和键值是同一个值），所以keys方法和values方法的行为完全一致。entries方法返回的遍历器，同时包括键名和键值，所以每次输出一个数组，它的两个成员完全相等。
```
let set = new Set(['red', 'green', 'blue']);

for (let item of set.keys()) {
  console.log(item);
}
// red
// green
// blue

for (let item of set.values()) {
  console.log(item);
}
// red
// green
// blue

for (let item of set.entries()) {
  console.log(item);
}
// ["red", "red"]
// ["green", "green"]
// ["blue", "blue"]
```
+ Set 结构的实例默认可遍历，它的默认遍历器生成函数就是它的values方法。这意味着，可以省略values方法，直接用for...of循环遍历 Set。
```
let set = new Set(['red', 'green', 'blue']);

for (let x of set) {
  console.log(x);
}
// red
// green
// blue
```
##### (2)forEach()
+ Set 结构的实例与数组一样，也拥有forEach方法，用于对每个成员执行某种操作，没有返回值。
```
set = new Set([1, 4, 9]);
set.forEach((value, key) => console.log(key + ' : ' + value))
// 1 : 1
// 4 : 4
// 9 : 9
```
##### (3)遍历的应用
+ 扩展运算符（...）内部使用for...of循环，所以也可以用于 Set 结构。
```
let set = new Set(['red', 'green', 'blue']);
let arr = [...set];
// ['red', 'green', 'blue']
```
+ 数组的map和filter方法也可以间接用于 Set 
```
let set = new Set([1, 2, 3]);
set = new Set([...set].map(x => x * 2));
// 返回Set结构：{2, 4, 6}

let set = new Set([1, 2, 3, 4, 5]);
set = new Set([...set].filter(x => (x % 2) == 0));
// 返回Set结构：{2, 4}
```
+ 使用 Set 可以很容易地实现并集（Union）、交集（Intersect）和差集（Difference）。
```
let a = new Set([1, 2, 3]);
let b = new Set([4, 3, 2]);

// 并集
let union = new Set([...a, ...b]);
// Set {1, 2, 3, 4}

// 交集
let intersect = new Set([...a].filter(x => b.has(x)));
// set {2, 3}

// 差集
let difference = new Set([...a].filter(x => !b.has(x)));
// Set {1}
```
+ 如果想在遍历操作中，同步改变原来的 Set 结构，目前没有直接的方法，但有两种变通方法。一种是利用原 Set 结构映射出一个新的结构，然后赋值给原来的 Set 结构；另一种是利用Array.from方法。
```
// 方法一
let set = new Set([1, 2, 3]);
set = new Set([...set].map(val => val * 2));
// set的值是2, 4, 6

// 方法二
let set = new Set([1, 2, 3]);
set = new Set(Array.from(set, val => val * 2));
// set的值是2, 4, 6
```
#### 11.2 WeakSet
+ WeakSet 结构与 Set 类似，也是不重复的值的集合。但是，它与 Set 有两个区别。
    + 首先，WeakSet 的成员只能是对象，而不能是其他类型的值
    + 其次，WeakSet 中的对象都是弱引用，即垃圾回收机制不考虑 WeakSet 对该对象的引用，也就是说，如果其他对象都不再引用该对象，那么垃圾回收机制会自动回收该对象所占用的内存，不考虑该对象还存在于 WeakSet 之中。
+ 由于上面这个特点，WeakSet 的成员是不适合引用的，因为它会随时消失。另外，由于 WeakSet 内部有多少个成员，取决于垃圾回收机制有没有运行，运行前后很可能成员个数是不一样的，而垃圾回收机制何时运行是不可预测的，因此 ES6 规定 WeakSet 不可遍历。这些特点同样适用于本章后面要介绍的 WeakMap 结构。
##### 语法
+ WeakSet 是一个构造函数，可以使用new命令，创建 WeakSet 数据结构。作为构造函数，WeakSet可以接受一个数组或类似数组的对象作为参数。
```
const a = [[1, 2], [3, 4]];
const ws = new WeakSet(a);
// WeakSet {[1, 2], [3, 4]}
```
+ WeakSet结构有三个方法：add、delete和has
+ WeakSet 的一个用处，是储存DOM节点，而不用担心这些节点从文档移除时，会引发内存泄漏。

#### 11.3 Map
+ JS的对象，实质上是键值对的集合（Hash结构），但是传统上只能用字符串当做键，这给他的使用带来了很大的限制。
```
const data = {};
const element = document.getElementById('myDiv');

data[element] = 'metadata';
data['[object HTMLDivElement]']//"metadata"
```
上面代码原意是讲一个DOM节点作为对象data的键，但是由于对象只接受字符串作为键名，所以element将自动转为字符串
+ Map数据结构时键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当做键。也就是说，Object结构提供了“字符串--值”的对应，Map结构提供了“值--值”的对应，是一种更完善的Hash结构实现。
```
const m = new Map();
const o = {p: 'Hello World'};

m.set(o, 'content')
m.get(o) // "content"

m.has(o) // true
m.delete(o) // true
m.has(o) // false
```
+ 作为构造函数，Map 也可以接受一个数组作为参数。==该数组的成员是一个个表示键值对的数组。==
```
const map = new Map([
  ['name', '张三'],        //指定了两个键name和title。
  ['title', 'Author']
]);

map.size // 2
map.has('name') // true
map.get('name') // "张三"
map.has('title') // true
map.get('title') // "Author"
```
+ 只有对同一个对象的引用，Map结构才将其视为同一个键。这一点要非常小心。下面代码的`set`和`get`方法，表面是针对同一个键，但实际上这是两个值，内存地址是不一样的，因此get方法无法读取该键，返回`undefined`。
```
const map = new Map();

map.set(['a'], 555);
map.get(['a']) // undefined
```
+ 对象是引用类型，变量里面保存的其实是对象的内存地址，
```
['a'] === ['a'] // false
var a = ['a']
var b = a
a === b // true
```
+ Map 的键实际上是跟内存地址绑定的，只要内存地址不一样，就视为两个键。这就解决了同名属性碰撞（clash）的问题，我们扩展别人的库的时候，如果使用对象作为键名，就不用担心自己的属性与原作者的属性同名。
+ 如果 Map 的键是一个简单类型的值（数字、字符串、布尔值），则只要两个值严格相等，Map 将其视为一个键，比如`0`和`-0`就是一个键，布尔值`true`和字符串`true`则是两个不同的键。另外，`undefined`和`null`也是两个不同的键。虽然`NaN`不严格相等于自身，但 Map 将其视为同一个键。
+ **实例的属性和操作方法**
+ `size`属性返回Map结构的成员总数
+ `set(key,value)`,设置然后返回整个Map结构。可以采用链式写法。
```
let map = new Map()
  .set(1, 'a')
  .set(2, 'b')
  .set(3, 'c');
```
+ get(key):读取键值
+ has(key)返回一个布尔值
+ delete(key):删除某个键，返回`true`。如果删除失败，返回`false`。
+ clear():清除所有成员，没有返回值
```
let map = new Map();
map.set('foo', true);
map.set('bar', false);

map.size // 2
map.clear()
map.size // 0
```
#####  遍历方法
+ Map 结构原生提供三个遍历器生成函数和一个遍历方法。Map遍历的顺序就是插入顺序。
    + `keys()`：返回键名的遍历器。
    + `values()`：返回键值的遍历器。
    + `entries()`：返回所有成员的遍历器。
    + `forEach()`：遍历 Map 的所有成员。
```
const map = new Map([
  ['F', 'no'],
  ['T',  'yes'],
]);

for (let key of map.keys()) {
  console.log(key);
}
// "F"
// "T"

for (let value of map.values()) {
  console.log(value);
}
// "no"
// "yes"

for (let item of map.entries()) {
  console.log(item[0], item[1]);
}
// "F" "no"
// "T" "yes"

// 或者
for (let [key, value] of map.entries()) {
  console.log(key, value);
}
// "F" "no"
// "T" "yes"

// 等同于使用map.entries()
for (let [key, value] of map) {
  console.log(key, value);
}
// "F" "no"
// "T" "yes"
```
+ Map 结构转为数组结构，比较快速的方法是使用扩展运算符（`...`）。
```
const map = new Map([
  [1, 'one'],
  [2, 'two'],
  [3, 'three'],
]);

[...map.keys()]
// [1, 2, 3]

[...map.entries()]
// [[1,'one'], [2, 'two'], [3, 'three']]

[...map]
// [[1,'one'], [2, 'two'], [3, 'three']]
```
+ 结合数组的`map`方法、`filter`方法，可以实现Map的遍历和过滤。
```
const map0 = new Map()
    .set(1, 'a')
    .set(2, 'b')
    .set(3, 'c');

const map1 = new Map{
    [...map0].filter([k,v] => k < 3)
};
// 产生 Map 结构 {1 => 'a', 2 => 'b'}

const map2 = new Map(
    [...map0].map([k,v]) => [k * 2,'_' + v]
);
// 产生 Map 结构 {2 => '_a', 4 => '_b', 6 => '_c'}
```
+ Map的forEach方法与数组的forEach方法类似，也可以实现遍历。
```
map.forEach(function(value, key, map) {
  console.log("Key: %s, Value: %s", key, value);
});
```
+ forEach方法还可以接受第二个参数，用来绑定this。
```
const reporter = {
  report: function(key, value) {
    console.log("Key: %s, Value: %s", key, value);
  }
};

map.forEach(function(value, key, map) {
  this.report(key, value);
}, reporter);
```
##### 与其他数据结构的转换
+ （1）Map转化为数组:最简便的方法就是用扩展运算符（...）
+ （2）数组转为Map:将数组传入Map构造函数
```
new Map([
  [true, 7],
  [{foo: 3}, ['abc']]
])
// Map {
//   true => 7,
//   Object {foo: 3} => ['abc']
// }
```
+ (3)Map转为对象：如果所有Map的键都是字符串，它可以无损地转为对象。
```
function strMapToObj(strMap) {
  let obj = Object.create(null);  //创建一个空对象
  for (let [k,v] of strMap) {    //遍历Map数据结构
    obj[k] = v;
  }
  return obj;
}

const myMap = new Map()
  .set('yes', true)
  .set('no', false);
strMapToObj(myMap)
// { yes: true, no: false }
```
+ （4）对象转为Map
```
function objToStrMap(obj) {
  let strMap = new Map();
  for (let k of Object.keys(obj)) {
    strMap.set(k, obj[k]);
  }
  return strMap;
}

```
+ (5)Map转为JSON
+ Map 转为 JSON 要区分两种情况。一种情况是，Map的键名都是字符串，这时可以选择转为对象 JSON。
```
function strMapToJson(strMap) {
  return JSON.stringify(strMapToObj(strMap));
}

let myMap = new Map().set('yes', true).set('no', false);
strMapToJson(myMap)
// '{"yes":true,"no":false}'
```
+ 另一种情况是，Map 的键名有非字符串，这时可以选择转为数组 JSON。
```
function mapToArrayJson(map) {
  return JSON.stringify([...map]);
}

let myMap = new Map().set(true, 7).set({foo: 3}, ['abc']);
mapToArrayJson(myMap)
// '[[true,7],[{"foo":3},["abc"]]]'
```
+ (6)JSON转为Map：正常情况下，所有键名都是字符串。
```
function jsonToStrMap(jsonStr) {
  return objToStrMap(JSON.parse(jsonStr));
}

jsonToStrMap('{"yes": true, "no": false}')
// Map {'yes' => true, 'no' => false}
```
+ 但是，有一种特殊情况，整个JSON就是一个数组，且每个数组成员本身，又是一个有两个成员的数组。这时，它可以一一对应地转为 Map。这往往是 Map 转为数组 JSON 的逆操作。
```
function jsonToMap(jsonStr) {
  return new Map(JSON.parse(jsonStr));
}

jsonToMap('[[true,7],[{"foo":3},["abc"]]]')
// Map {true => 7, Object {foo: 3} => ['abc']}
```
#### 11.4 WeakMap

+ 在ES6之前，有时我们想在某个对象上面存放一些数据，但是这会形成对于这个对象的引用，一旦不再需要这两个对象，我们就必须手动删除这个引用，否则垃圾回收机制就不会释放e1和e2占用的内存,一旦忘了写，就会造成内存泄漏。
```
const e1 = document.getElementById('foo');
const e2 = document.getElementById('bar');
const arr = [
  [e1, 'foo 元素'],
  [e2, 'bar 元素'],
];

// 不需要 e1 和 e2 的时候
// 必须手动删除引用
arr [0] = null;
arr [1] = null;
```
+ WeakMap与Map的区别有两点。首先，WeakMap只接受对象作为键名（null除外,会报错），不接受其他类型的值作为键名。其次，WeakMap的键名所指向的对象，不计入垃圾回收机制。
+ 一个典型应用场景是，在网页的DOM元素上添加数据，就可以使用WeakMap结构。当该 DOM 元素被清除，其所对应的WeakMap记录就会自动被移除。
```
const wm = new WeakMap();

const element = document.getElementById('example');

wm.set(element, 'some information');
wm.get(element) // "some information"
```
+ 注意，WeakMap 弱引用的只是键名，而不是键值。键值依然是正常引用。
```
const wm = new WeakMap();
let key = {};
let obj = {foo: 1};

wm.set(key, obj);
obj = null;
wm.get(key)
// Object {foo: 1}
```
##### WeakMap的语法
+ WeakMap 与 Map 在API上的区别主要是两个，一是没有遍历操作（即没有keys()、values()和entries()方法），也没有size属性。二是无法清空，即不支持clear方法。因此，WeakMap只有四个方法可用：get()、set()、has()、delete()。
##### WeakMap的用途
+ 应用的典型场合就是DOM节点作为键名。
```
let myElement = document.getElementById('logo');
let myWeakmap = new WeakMap();

myWeakmap.set(myElement,{timesCliecked:0});

myElement.addEventListener("click",function(){
    let logoData = myWeakmap.get(myElement);
    logoData.timesClicked++;
},false);
```
+ WeakMap的另一个作用是部署私有属性。
```
const _counter = new WeakMap();
const _action = new WeakMap();

class Countdown {
    constructor(counter, action){
        _counter.set(this, counter);
        _action.set(this,action);
    }
    dec(){
        let counter = _counter.get(this);
        if(counter < 1)return;
        counter--;
        _counter.set(this, counter);
        if(counter ===0){
            _action.get(this)();
        }
    }
}

const c = new Countdown(2,()=>console.log('DONE'));

c.dec()
c.dec()
//DONE
```
***
## 12. Proxy
#### 12.1 概述
## 14. Promise对象
#### 14.1 Promise的含义
+ Promise是异步编程的一种解决方案，比传统的解决方案--回调函数和事件--更合理和更强大。
+ 所谓`Promise`,简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果，从语法上说，Promise是一个对象，从它可以获取异步操作的消息。Promise提供统一的API，各种异步操作都可以用同样的方法进行处理。
+ `Promise`对象有两个特点：
    + （1）对象的状态不受外界影响。`Promise`对象代表一个异步操作，有三种状态：`pending`(进行中)、`fulfiled`（已成功）和`rejected`(已失败)。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。这也是`Promise`这个名字的由来。
    + （2）一旦状态改变，就不会再变，任何时候都可以得到这个结果。`Promise`对象的状态改变，只有两种可能：从`pending`变为`fulfilled`和从`pending`变为`rejected`。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就成为resolved(已定型)。如果改变已经发生了，你再对`Promise`对象添加回调函数，也会立即得到这个结果，这与事件（Event)完全不同，事件的特点是，如果你错过了他，再去监听，是得不到结果的。
+ 注意，为了行文方便，本章后面的`resolved`统一指`fulfilled`状态，不包含`rejected`状态。
+ 缺点：首先，无法取消`Promise`,一旦新建他就会立即执行，无法中途取消。其次，如果不设置回调函数，`Promise`内部抛出的错误，不会反应到外部。第三，当处于`pending`状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。如果某些事件不断的反复发生，一般来说，使用`Stream`模式时比部署`Promise`更好的选择。

#### 基本用法
+ `Promise`对象是一个构造函数，用来生成`Promise`实例。
```
const promise = new Promise(function(resolve,reject){
    //  ... some code

  if (/* 异步操作成功 */){
    resolve(value);
  } else
  
  {
    reject(error);
  }
    
}
```
+ 参考博客：
    + 1.大白话讲解Promise（一）https://www.cnblogs.com/lvdabao/p/es6-promise-1.html
+ Promise是一个构造函数，自己身上有all、reject、resolve这几个眼熟的方法，原型上有then、catch等同样很眼熟的方法。
+ Promise的构造函数接收一个参数，是函数，并且传入两个参数：resolve，reject，分别表示异步操作执行成功后的回调函数和异步操作执行失败后的回调函数。其实这里用“成功”和“失败”来描述并不准确，按照标准来讲，resolve是将Promise的状态置为fullfiled，reject是将Promise的状态置为rejected。
+ 只是new了一个对象，并没有调用它，我们传进去的函数就已经执行了，这是需要注意的一个细节。所以我们用Promise的时候一般是包在一个函数中，在需要的时候去运行这个函数，如：
```
function runAsync(){
    var p = new Promise(function(resolve, reject){
        //做一些异步操作
        setTimeout(function(){
            console.log('执行完成');
            resolve('随便什么数据');
        }, 2000);
    });
    return p;            
}
runAsync()
```
+ 在我们包装好的函数最后，会return出Promise对象，也就是说，执行这个函数我们得到了一个Promise对象。还记得Promise对象上有then、catch方法吧？这就是强大之处了，
```
runAsync().then(function(data){
    console.log(data);
    //后面可以用传过来的数据做些其他操作
    //......
});
```
+ 原来then里面的函数就跟我们平时的回调函数一个意思，能够在runAsync这个异步任务执行完成之后被执行。这就是Promise的作用了，简单来讲，就是能把原来的回调写法分离出来，在异步操作执行完后，用链式调用的方式执行回调函数。
+ 把回调函数封装一下，给runAsync传进去不也一样吗，有多层回调该怎么办？Promise的优势在于，可以在then方法中继续写Promise对象并返回，然后继续调用then来进行回调操作。

#### all的用法
+ Promise的all方法提供了并行执行异步操作的能力，并且在所有异步操作执行完后才执行回调。all接收一个数组参数，里面的值最终都算返回Promise对象。这样，三个异步操作的并行执行的，等到它们都执行完后才会进到then里面。
```
Promise
.all([runAsync1(), runAsync2(), runAsync3()])
.then(function(results){
    console.log(results);
});
```

















***
## 22.Module的语法
#### 22.3 export命令
+ export 命令规定的是对外的接口.
```
// 报错
export 1;

// 报错
var m = 1;
export m;

// 写法一
export var m = 1;

// 写法二
var m = 1;
export {m};

// 写法三
var n = 1;
export {n as m};
```
+ 输出function 的方法
```
// 报错
function f() {}
export f;

// 正确
export function f() {}  ;

// 正确
function f() {}
export {f};
```
#### 22.4 import命令
+ `import`命令输入的变量都是只读的,不允许在加载模块的脚本里面，改写接口。
```
import {a} from './xxx.js'

a = {};//语法错误
```
但是如果a是一个对象,那么改写属性时允许的.但是建议不要轻易改
+ `import`是静态执行,所以不能使用表达式,变量和if结构
+ 加载模块
    `import 'lodash';`

#### 22.6 export default命令
+ 用`export default`命令输出时,可以不给输出的函数命名,在输入时命名,并且名字不用花括号
```
// export-default.js
export default function () {
  console.log('foo');
}

// import-default.js
import customName from './export-default';
customName(); // 'foo'
```
***
## 22.0 require.js的加载
+ 概念：require.js是一个js脚本加载器，它遵循AMD(==Asynchronous Module Definition==)规范，实现js脚本的异步加载。
`<script src="js/require.js" defer async="true" data-main="js/main"></script>`
+ data-main之后的主模块会第一个被加载。由于require.js默认的文件后缀名是js，所以可以把main.js简写成main。
+ 解决网页失去响应的方法：1.aysnc属性表明这个文件需要异步加载，避免网页时区相应。IE不支持async,只支持defer。2.把它放在网页底部加载。

#### 主模块的写法
+ 如果主模块依赖于其他模块，要使用AMD规范定义的require()函数
```
//main.js

require(['moduleA', 'moduleB', 'moduleC'],function(moduleA, moduleB, moduleC) {
    
    //some code here
    
});
```
function规定的是回调函数，当前面的模块加载成功后，它将被调用。
+ require.config()方法，我们可以对模块的加载行为进行自定义。paths属性指定各个模块的加载路径（这里的min是子模块里面返回的一个函数）。
```
require.config({

    paths: {
    
        "jquery": "jquery.min",
        "underscore": "underscore.min",
        "backbone": "backbone.min"
        
    }
    
)};
```
上面的代码的三个模块默认路径与main.js在同一个目录（js子目录），如果在js/lib目录，有两种写法：
```
require.config({

    //第一种，逐一指定路径
    paths: {
        
        "jquery": "lib/jquery.min",
        "backbone": "lib/backbone.min"
        
    }
    
    
    //第二种，直接改变基目录
    baseUrl: "js/lib",
    
    paths: {
        
        "jquery": "jquery.min",
        "backbone": "backbone.min"
        
    }
    
});
```
+ 模块必须采用特定的==define()==函数来定义。如果一个模块不依赖其他模块，那么可以直接定义在define()函数之中。
```
//math.js

define(function() {
    
    var add = function(x,y) {
        
        return x+y;
        
    };
    
    return {
        
        add: add
        
    };
    
});


//main.js

require(['math'], function (math){
    
    alert(math.add(1,1));
    
});
```
+ 如果这个模块还依赖其他模块，那么define()函数的第一个参数，必须是一个数组，指明该模块的依赖性。
```
define(['myLib'], function(myLib){
    
    function foo(){
        
        myLib.doSomething();
        
    }
    
    return {
        
        foo: foo
        
    };
    
});
```
---
