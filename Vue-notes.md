> 参考链接：https://cn.vuejs.org/v2/guide/

## 第1章 介绍
+ Vue是一套用于构建用户界面的==渐进式框架==，与其他大型框架不同的是，VUE被设计为可以自底向上逐层应用。
+ Vue.js的核心是一个允许采用简洁的模板语法来声明式的将数据渲染进DOM的系统。
+ 控制切换一个元素是否显示，用`v-if`
+ 我们不仅可以把数据绑定到DOM==文本或特性==，还可以绑定到DOM==结构==。？？
+ 用`<button v-on:click="reverseMessage">你转消息</button>`添加事件监听器，其中响应函数写在`methods：{}`里面
+ `v-model`实现表单输入与应用状态之间的双向绑定，即输即显示。
```
<input v-model="message">
```
此时，data里的message定义的是默认值，当在输入框输入其他值，会实时显示改变。
+ 在Vue中注册组件:
```
Vue.component('todo-item',{
    //`prop`类似于一个自定义属性
    props:[todo],
    template:'<li>{{todo.text}}</li>'
})

<ol>
    <todo-item 
        v-for="item in groceryList"
        v-bind:todo="item"
        v-bind:key="item.id">
    </todo-item>
<ol>
```
---
## 第2章 Vue实例
#### 2.1 创建一个Vue实例
+ 每个Vue应用都是通过`Vue`函数创建一个新的Vue实例开始的。
```
var vm = new Vue({
    data:data,
    
    //可选的如下：
    el: '#replace',                            //挂载的目标
    template: '<p class="bar">replaced</p>'     //挂载的内容
})
```
+ 在文档中经常会使用`vm`这个变量名表示Vue实例。
+ 当创建一个VUE实例时，可以传入一个**选项对象**。可以在**API文档**中浏览完整的选项列表。
+ 一个vue应用有一个通过`new Vue`创建的根Vue实例，以及可选的嵌套的、可复用的组件树组成。

##### 选项/DOM
+ **el**
    + 只在由`new`创建的实例中遵守
    + 提供一个在页面上已存在的DOM元素作为Vue实例的==挂载目标==。可以是CSS选择器，也可以是一个HTMLElement实例
    + 在实例挂载之后，元素可以用`vm.$el`访问
    + 如果这个选项在实例化时有作用，实例将立即进入编译过程，否则，需要显式调用`vm.$mount()`手动开启编译。
    + 提供的元素只能作为挂载点，不同于Vue.1.x,所有的挂载元素会被Vue生成的DOM替换，因此不推荐挂载root实例到\<html>或者<body>上。
    + 如果`render`函数和`template`属性都不存在，挂载DOM元素的HTML会被提取出来用作模板，此时，必须使用Runtime+Compiler构建的Vue库。
+ **template**
    + 一个==字符串模板==，将会**替换挂载的元素**。被挂载元素的内容都将被忽略，除非模板的内容有分发插槽。
    + 如果值以#开始，则他将被用作选择符，并使用匹配元素的innerHTML作为模板。常用的技巧是用`<script type="x-template">`包含模板。
    + 如果Vue选项中包含渲染函数，该模板将会被忽略。
```
<div id="replace" class="foo"></div>

new Vue({
 el: '#replace',
 template: '<p class="bar">replaced</p>'
})


结果
<p class="foo bar" id="replace">replaced</p>
```

#### 2.2 数据与方法
+ 只有`data`中存在的属性才是**响应式**的，如果属性一开始为空或不存在，那么仅需要设置一些初始值。
```
data: {
  newTodoText: '',
  visitCount: 0,
  hideCompletedTodos: false,
  todos: [],
  error: null
}
```
+ 这里唯一地例外是使用`Object.freeze()`，这回阻止修改现有的属性，也意味着相应系统无法再**跟踪变化**
+ 除了数据属性，Vue实例还暴露了一些有用的实例属性与方法。他们都有==前缀$==，以便与用户定义的属性区分开。完整的实例属性和方法的列表在**API参考**中。
```
var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})

vm.$data === data // => true
vm.$el === document.getElementById('example') // => true

// $watch 是一个实例方法
vm.$watch('a', function (newValue, oldValue) {
  // 这个回调将在 `vm.a` 改变后调用
})
```
#### 2.3 实例生命周期钩子
+ 每个Vue实例在被创建时都要经过一系列的初始化过程--例如，需要设置数据监听、编译模板、将实例挂载在DOM并在数据变化时更新DOM等。同时在这个过程中也会运行一些叫做生命周期钩子的函数，这给了用户==在不同阶段添加自己的代码==的机会。
+ `created`钩子可以用来在一个实例被创建之后执行代码：
+ 也有一些其它的钩子，在实例生命周期的不同阶段被调用，如 `mounted`、`updated `和 `destroyed`。生命周期钩子的 `this` 上下文指向调用它的 Vue 实例。
+ 不要在Vue实例选项属性或钩子上使用**箭头函数**，因为箭头函数是和父级上下文绑定在一起的，this 不会是如你所预期的 Vue 实例。
![image](https://cn.vuejs.org/images/lifecycle.png)
+ el提供一个在页面上已存在的 DOM 元素作为 Vue 实例的挂载目标。可以是 CSS 选择器，也可以是一个 HTMLElement 实例
+ 生命周期的解释：
    + 什么都还未调用==beforeCreate==函数。
    + 在实例创建后,此时data等数据属性已经初始化，立即调用==created==。此时DOM和el属性还未被创建。
    + 创建好el、template(替换元素）等DOM选项之后，此时数据只是占个坑，还没显示出来，调用==beforeMount==函数。
    + 把数据挂载完成后，即所有的东西都加载完毕，这时调用==mounted==








---
## 第3章 模板语法
+ Vue.js使用了基于HTML的模板语法，允许开发者声明式的将DOM绑定至底层VUE实例的数据。所有 Vue.js 的模板(template)都是合法的 HTML ，所以能被遵循规范的浏览器和 HTML 解析器解析。
+ vue能够只能地计算出最少需要重新渲染多少组件，并把DOM操作次数减到最少。

#### 3.1 插值(数据绑定)
##### 文本
+ 最常见的形式:=="mustache"语法（双大括号）==,论何时，绑定的数据对象上 msg 属性发生了改变，插值处的内容都会更新。
```
<span>Message:{{msg}}</span>
```
+ `v-once`能执行一次性插值，当数据改变时，插值处的内容不会更新
`<span v-once>这个将不会改变：{{msg}}</span>`
##### 原始HTML
+ 双大括号会将数据解释为普通文本，而非 HTML 代码。为了输出真正的 HTML，你需要使用 `v-html `指令：
```
//这里在data中定义了rawHtml='<span style='color:red'>This should be red.</span>'

<p>Using mustaches: {{ rawHtml }}</p>
<p>Using v-html directive: <span v-html="rawHtml"></span></p>
```
![image](https://wx3.sinaimg.cn/mw690/005PH614ly1g0yw1g44zgj30gm02zmx1.jpg)

+ 只能对可信内容使用HTML插值，绝不要对用户提供的内容使用插值，很容易导致XSS攻击。

##### 特性
+ 双大括号语法不能作用在HTML元素属性上，这种情况应该使用`v-bind`指令：
```
<div v-bind:id="dynamicId"></div>
//id是HTML的全局属性
```
+ 在布尔特性的情况下，`v-bind` 工作起来略有不同
```
<button v-bind:disabled="isButtonDisabled">Button</button>
```
如上例子中，如果 `isButtonDisabled` 的值是 `null`、`undefined` 或 `false`，则 `disabled` 特性甚至不会被包含在渲染出来的 `<button>` 元素中。!!

##### 使用JavaScript表达式
+ 实际上，对于所有的数据绑定，Vue.js 都提供了完全的 JavaScript 表达式支持。有个限制就是，每个绑定都只能包含==单个表达式==，不能是语句或流控制。
```
//支持的语法
{{ number + 1 }}
{{ ok ? 'YES' : 'NO' }}
{{ message.split('').reverse().join('') }}
<div v-bind:id="'list-' + id"></div>

//不会生效的两个例子
<!-- 这是语句，不是表达式 -->
{{ var a = 1 }}

<!-- 流控制也不会生效，请使用三元表达式 -->
{{ if (ok) { return message } }}
```

#### 3.2 指令
+ 指令是带有v-前缀的特殊特性，指令特性的值预期是==单个js表达式==（v-for是例外），指令的职责是，当表达式的值改变时，将其产生的连带影响，响应式地作用域DOM。

##### 参数
+ 一些指令能够接收一个“参数”，在指令名称之后以冒号表示。例如，v-bind 指令可以用于双向绑定 HTML 特性，这里的href是参数：
```
<a v-bind:href="url">...</a>
```
+ `v-on`指令用于监听DOM事件，参数是监听的事件名。
```
//click就是事件名
<a v-on:click="doSomething">...</a>
```
##### 动态参数
> 2.6.0新增

+ 可以用方括号括起来的 JavaScript 表达式作为一个指令的参数：
```
<a v-bind:[attributeName]="url"> ... </a>
```
这里的 `attributeName` 会被作为一个 JavaScript 表达式进行动态求值，求得的值将会作为最终的参数来使用。例如，如果你的 Vue 实例有一个 `data` 属性 `attributeName`，其值为 `"href"`，那么这个绑定将等价于 `v-bind:href`。
+ 同样地，可以使用动态参数为一个动态的事件名绑定处理函数：
```
<a v-on:[eventName]="doSomething"> ... </a>
```
+ 对动态参数的值的约束:动态参数预期会求出一个字符串，异常情况下值为 `null`。这个特殊的 `null` 值可以被显性地用于移除绑定。任何其它非字符串类型的值都将会触发一个警告。

##### 修饰符
+ 修饰符是以`.`表示的特殊后缀，用于指出一个指令应该以特殊方式绑定。例如，`.prevent `修饰符告诉 `v-on` 指令对于触发的事件调用 `event.preventDefault()`：
```
//preventDefault() -取消事件的默认行为。如果cancelable是true，则可以使用这个方法。
//这里为什么要取消默认行为？不理解。
<form v-on:submit.prevent="onSubmit">...</form>
```
#### 3.3 缩写
+ `v-bind `缩写
```
<!-- 完整语法 -->
<a v-bind:href="url">...</a>

<!-- 缩写 -->
<a :href="url">...</a>
```
+ `v-on` 缩写
```
<!-- 完整语法 -->
<a v-on:click="doSomething">...</a>

<!-- 缩写 -->
<a @click="doSomething">...</a>
```
---
## 第4章 计算属性和侦听器
#### 4.1 计算属性
+ 为什么要用计算属性？
    + 模板内的表达式非常便利，但是设计他们的初衷是用于简单运算的。在模板中放入太多的逻辑会让模板过重且==难以维护==，而且不利于逻辑的复用，对于任何复杂逻辑，都应当使用`computed`计算属性。
```
<div id="example">
  {{ message.split('').reverse().join('') }}
</div>
```
如上例子，你必须看一段时间才能意识到，这里是想要显示变量 `message` 的翻转字符串。当你想要在模板中多次引用此处的翻转字符串时，就会更加难以处理。
```
//HTML
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>
```
```
//JS 
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // 计算属性的 getter
    reversedMessage: function () {
      // `this` 指向 vm 实例
      return this.message.split('').reverse().join('')
    }
  }
})
```
+ 可以像绑定普通数据一样在模板中绑定计算属性。

##### 计算属性VS方法
```
<p>Reversed message: "{{ reversedMessage() }}"</p>


// 在组件中
methods: {
  reversedMessage: function () {
    return this.message.split('').reverse().join('')
  }
}
```
+ 我们可以通过在表达式中调用方法来达到同样的效果，我们可以将同一个函数定义为一个方法而不是一个计算属性。两种方式的最终结果确实是完全相同的。然而，不同的是==计算属性是基于他们的依赖进行缓存的==。计算属性只有在他的相关依赖发生改变时才会重新求值。这就意味着==只要`message`还没有发生改变，多次访问`reverserMessage`计算属性会立即返回之前的计算结果，而不必再次执行函数。==
+ 这也意味着，像`Date.now()`这种非响应式依赖，如果写在`computed`里面，计算属性将不再更新：
```
computed: {
  now: function () {
    return Date.now()
  }
}
```
+ 相比之下，每当触发重新渲染时，调用方法将**总会**再次执行函数。
+ 我们为什么需要缓存？假设我们有一个==性能开销比较大的计算属性== A，它需要遍历一个巨大的数组并做大量的计算。然后我们可能==有其他的计算属性依赖于 A ==。如果没有缓存，我们将不可避免的多次执行 A 的 getter！

##### 计算属性vs侦听属性
> 参考链接：https://www.jianshu.com/p/631bcfd1b8b5

+ Vue提供了一种更通用的方式来观察和响应Vue实例上的数据变动：**侦听属性**。当你有一些数据需要随着其它数据变动而变动时，你很容易滥用 `watch`——特别是如果你之前使用过 AngularJS。然而，通常更好的做法是使用==计算属性==而不是命令式的`watch`回调。
+ **watch其实是一个对象，里面的函数名其实是需要观察的表达式，相当于watch对象的属性名。而属性值则是当观察的表达式变化时，执行的回调函数**。
```
<div id="demo">{{ fullName }}</div>
```
+ 而侦听器 watch 是侦听一个特定的值，
```
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar',
    fullName: 'Foo Bar'
  },
  watch: {  //命令式且重复的
    firstName: function (val) {
      this.fullName = val + ' ' + this.lastName
    },
    lastName: function (val) {
      this.fullName = this.firstName + ' ' + val
    }
  }
})
```
+ 计算属性具有缓存。这就意味着只要 lastName 和 firstName都没有发生改变，多次访问 fullName 计算属性会立即返回之前的计算结果，而不必再次执行函数。
```
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar'
  },
  computed: {   //好多了
    fullName: function () {
      return this.firstName + ' ' + this.lastName
    }
  }
})
```
+ 也就是说，computed 对于其中变量的依赖是多个的，它的函数中使用了多个 this.xxx ,只要其中一个发生了变化，都会触发这个函数。而 watch 的依赖则是单个的，它每次只可以对一个变量进行监控。 

##### 计算属性的setter
+ 计算属性默认只有getter，不过在需要时你也可以提供一个setter:
```
// ...
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
// ...
```
现在再运行 vm.fullName = 'John Doe' 时，setter 会被调用，vm.firstName 和 vm.lastName 也会相应地被更新。
#### 4.2 侦听器watch
+ 虽然计算属性在大多数情况下更合适，但有时也需要一个自定义的侦听器。当需要在数据变化时==执行异步或开销较大的操作时==，用侦听器是最有用的。

#### 4.3 computed、methods、watch的区别
> 参考链接：https://blog.csdn.net/zifeiyu130/article/details/78908895

+ 1#computed：computed是在HTML DOM加载后马上执行的，如赋值；所有 getter 和 setter 的 this 上下文自动地绑定为 Vue 实例。
+ 2#methods：methods则必须要有一定的触发条件才能执行，如点击事件；可以直接通过VM实例访问这些方法，或者在表达式中调用method。方法中的 this 自动绑定为 Vue 实例。下面的写法等同于computed
+ 3#watch：这是一个观察的动作，是一种更通用的方式来观察和响应 Vue 实例上的数据变动。**watch其实是一个对象，里面的函数名其实是需要观察的表达式，相当于watch对象的属性名。而属性值则是当观察的表达式变化时，执行的回调函数**。Vue 实例将会在实例化时调用 $watch()，遍历 watch 对象的每一个属性。
```
data: {
    question: '',
    answer: 'I cannot give you an answer until you ask a question!'
  },
  watch: {
    // 如果 `question` 发生改变，这个函数就会运行
    question: function (newQuestion, oldQuestion) {
      this.answer = 'Waiting for you to stop typing...'
      this.debouncedGetAnswer()
    }
  },
```
+ 深度监听虽然可以监听到对象的变化,注意：在变异 (不是替换) 对象或数组时，旧值将与新值相同，因为它们的引用指向同一个对象/数组。Vue 不会保留变异之前值的副本。
```
watch:{
      secondChange:{
        handler(oldVal,newVal){
          console.log(oldVal)
          console.log(newVal)
        },
        deep:true
      }
    },
```
+ 什么时候用：
    + 数据量大，需要缓存的时候用 computed ；
    + 每次确实需要重新加载，不需要缓存时用 methods 。方法还有一个好处是可以传参，可以把方法执行过程中的临时变量的值传进方法体进行计算，而计算属性做不到。方法更加灵活的实现了对v-for中的item进行处理。
    + computed 对于其中变量的依赖是多个的，一次可以监听多个属性，它的函数中使用了多个 this.xxx ,只要其中一个发生了变化，都会触发这个函数。而 watch 的依赖则是单个的，它一次只可以对一个变量进行监控。
```
//方法的使用场景举例
<ul v-for='item in obj'>
    <li>{{addHandle(item)}}</li>
</ul>

methods: {
    addHandle(item) {
        return item++
    }
}
```
---
## 第5章 Class 与Style绑定
元素的 class 列表和内联样式都是属性，们可以用 `v-bind` 处理它们：只需要通过表达式计算出字符串结果即可。在将`v-bind`用于`class`和`style`时，vue.js做了专门的增强，表达式结果的类型除了字符串之外，还可以是对象或数组。

#### 5.1 绑定HTML Class
##### 5.1.1 对象语法
+ 我们可以传给v-bind一个==对象==，以动态的切换class
```
<div v-bind:class="{active:isActive}"></div>
```
上面的语法表示 `active`这个class存在与否取决于数据属性`isActive`的`truthiness`
+ 你还可以在对象中传入更多属性来动态切换多个class，此外，`v-bind:class`指令也可以与普通的class属性共存，
```
<div class="static"
     v-bind:class="{ active: isActive, 'text-danger': hasError }">
</div>


data: {
  isActive: true,
  hasError: false
}


//结果渲染为
<div class="static active"></div>
//当 isActive 或者 hasError 变化时，class 列表将相应地更新。
```
绑定的数据对象不必内联定义在模板里：等同于
```
<div v-bind:class="classObject"></div>


data: {
  classObject: {
    active: true,
    'text-danger': false
  }
}
```
+ 一个非常常用且强大的模式:绑定一个返回对象的**计算属性**
```
<div v-bind:class="classObject"></div>


data: {
  isActive: true,
  error: null
},
computed: {
  classObject: function () {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal'
    }
  }
}
```
##### 5.1.2 数组语法
+ 我们可以把一个数组传给 v-bind:class，以应用一个 class 列表：
```
<div v-bind:class="[activeClass, errorClass]"></div>

data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}

//渲染为
<div class="active text-danger"></div>
```
+  可以用==三元表达式==条件切换列表中的class
```
<div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>
```
这样写将始终添加 `errorClass`，同时会在 `isActive` 是 truthy[1] 时添加 `activeClass`。
+ 在 JavaScript 中，Truthy (真值)指的是在 布尔值 上下文中转换后的值为真的值。所有值都是真值，除非它们被定义为 falsy (即除了 false，0，""，null，undefined 和 NaN 外)。JavaScript 中的真值示例如下（将被转换为 true，if 后的代码段将被执行）：
```
if (true)
if ({})
if ([])
if (42)
if ("foo")
if (new Date())
if (-42)
if (3.14)
if (-3.14)
if (Infinity)
if (-Infinity)
```
##### 5.1.3 用在组件上
+ 当在一个自定义组件上使用 class 属性时，这些类将被添加到该组件的根元素上面。这个元素上已经存在的类不会被覆盖。
```
//声明一个组件
Vue.component('my-component', {
  template: '<p class="foo bar">Hi</p>'
})


//使用它
<my-component class="baz boo"></my-component>

//将被渲染为
<p class="foo bar baz boo">Hi</p>
```
#### 5.2 绑定内联样式
##### 5.2.1 对象语法
+ `v-bind:style` 的对象语法十分直观——看着非常像 CSS，但其实是一个 JavaScript 对象。CSS 属性名可以用驼峰式(camelCase) 或短横线分隔 (kebab-case，记得用单引号括起来) 来命名：
```
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>

data: {
  activeColor: 'red',
  fontSize: 30
}
```
+ 直接绑定到一个样式对象通常更好，这会让模板更清晰：
```
<div v-bind:style="styleObject"></div>

data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
```
+ 同样的，对象语法常常结合返回对象的计算属性使用。

##### 5.2.2 数组语法
```
<div v-bind:style="[baseStyles, overridingStyles]"></div>
```
##### 5.2.3 自动添加前缀
+ 当 `v-bind:style` 使用需要添加==浏览器引擎前缀==的 CSS 属性时，如 transform，Vue.js 会自动侦测并添加相应的前缀。
---
## 第6章 条件渲染
#### 6.1 v-if
```
<h1 v-if="awesome">Vue is awesome!</h1>
<h1 v-else>Oh no 😢</h1>
```
##### 6.1.1 在<template>元素上使用v-if条件渲染分组
+ 如果想切换多个元素呢？此时可以把一个 <template> 元素当做不可见的包裹元素，并在上面使用 v-if。最终的渲染结果将不包含 <template> 元素。
```
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```
##### 6.1.2 v-else-if 
> 2.1.0 新增

```
<div v-if="type === 'A'">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else-if="type === 'C'">
  C
</div>
<div v-else>
  Not A/B/C
</div>
<div v-if="type === 'A'">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else-if="type === 'C'">
  C
</div>
<div v-else>
  Not A/B/C
</div>
```
##### 6.1.3 用key管理可复用的元素
+ Vue 会尽可能高效地渲染元素，通常会复用已有元素而不是从头开始渲染。这么做除了使 Vue 变得非常快之外，还有其它一些好处。在下面的代码中切换 loginType 将不会清除用户已经输入的内容。因为两个模板使用了相同的元素，<input> 不会被替换掉——仅仅是替换了它的 placeholder。
```
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address">
</template>
```
+ 这样也不总是符合实际需求，所以 Vue 为你提供了一种方式来表达“这两个元素是完全独立的，不要复用它们”。只需添加一个具有==唯一值==的 `key `属性即可：
```
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username" key="username-input">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address" key="email-input">
</template>
```
#### 6.2 v-show
+ 带有 v-show 的元素始终会被渲染并保留在 DOM 中,只是简单地切换元素的 CSS 属性 display。
+ 注意，v-show 不支持 <template> 元素，也不支持 v-else。
```
<h1 v-show="ok">Hello!</h1>
```
#### 6.3 v-if VS v-show
+ v-if 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。
+ v-if 也是惰性的：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。
+ 相比之下，v-show 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。
+ v-if 有更高的切换开销，而 v-show 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 v-show 较好；如果在运行时条件很少改变，则使用 v-if 较好。

#### 6.4 v-if与v-show一起使用
+ **永远不要把 `v-if` 和 `v-for` 同时用在同一个元素上**。一般我们在两种常见的情况下会倾向于这样做：
    + 为了过滤一个列表中的项目 (比如 `v-for="user in users" v-if="user.isActive"`)。在这种情形下，请将 `users` 替换为一个计算属性 (比如 `activeUsers`)，让其返回过滤后的列表。
    + 为了避免渲染本应该被隐藏的列表 (比如 `v-for="user in users" v-if="shouldShowUsers"`)。这种情形下，请将` v-if` 移动至容器元素上 (比如 `ul`, `ol`)。
```
//反例
<ul>
  <li
    v-for="user in users"
    v-if="user.isActive"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>


<ul>
  <li
    v-for="user in users"
    v-if="shouldShowUsers"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>
```
```
//好例子
<ul>
  <li
    v-for="user in activeUsers"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>


<ul v-if="shouldShowUsers">
  <li
    v-for="user in users"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>
```
+ 当 `v-if` 与 `v-for` 一起使用时，`v-for `具有比` v-if `更高的优先级。这意味着 `v-if` 将分别重复运行于每个 `v-for` 循环中。而如果你的目的是有条件地跳过循环的执行，那么可以将 `v-if` 置于外层元素 (或 `<template>`)上。
```
<ul v-if="todos.length">
  <li v-for="todo in todos">
    {{ todo }}
  </li>
</ul>
<p v-else>No todos left!</p>
```
---
## 第7章 列表渲染
#### 7.1 用v-for把一个数组对应为一组元素
```
//也可以用 of 替代 in 作为分隔符
<ul id="example-1">
  <li v-for="item in items">
    {{ item.message }}
  </li>
</ul>

var example1 = new Vue({
  el: '#example-1',
  data: {
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
```
+ v-for 还支持一个可选的第二个参数为当前项的索引。
```
<ul id="example-2">
  <li v-for="(item, index) in items">
    {{ parentMessage }} - {{ index }} - {{ item.message }}
  </li>
</ul>

var example2 = new Vue({
  el: '#example-2',
  data: {
    parentMessage: 'Parent',
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})

//结果
Parent-0-Foo
Parent-1-Bar
```
#### 7.2 一个对象的v-for
+ 可以用 v-for 通过一个对象的属性来迭代。也可以提供第二个的参数为键名：
```
<ul id="v-for-object" class="demo">
  <li v-for="value in object">
    {{ value }}
  </li>
</ul>

new Vue({
  el: '#v-for-object',
  data: {
    object: {
      firstName: 'John',
      lastName: 'Doe',
      age: 30
    }
  }
})
//结果
John
Doe
30



<div v-for="(value, key) in object">
  {{ key }}: {{ value }}
</div>

//结果
firstName:John
lastName:Doe
age:30
```
+ 第三个参数为索引：
```
<div v-for="(value, key, index) in object">
  {{ index }}. {{ key }}: {{ value }}
</div>

//结果
0.firstName:John
1.lastName:Doe
2.age:30
```
#### 7.3 key
+ 当 Vue.js 用 v-for 正在更新已渲染过的元素列表时，它默认用“就地复用”策略。如果数据项的顺序被改变，Vue 将不会移动 DOM 元素来匹配数据项的顺序， 而是简单复用此处每个元素，并且确保它在特定索引下显示已被渲染过的每个元素。
+ 这个默认的模式是高效的，但是只适用于**不依赖子组件状态或临时 DOM 状态 (例如：表单输入值) 的列表渲染输出**。
+ 为了给 Vue 一个提示，以便它能跟踪每个节点的身份，从而重用和重新排序现有元素，你需要为每项提供一个唯一 key 属性。理想的 key 值是每项都有的唯一 id。它的工作方式类似于一个属性，所以你需要用 v-bind 来绑定动态值 
```
<div v-for="item in items" :key="item.id">
  <!-- 内容 -->
</div>
```
+ 建议尽可能在使用 v-for 时提供 key，除非遍历输出的 DOM 内容非常简单，或者是刻意依赖默认行为以获取性能上的提升。
#### 7.4 数组更新检测
##### 7.4.1 变异方法
##### 












---
## 第9章 组件基础

+ 组件是可复用的Vue实例，我们可以在一个通过`new Vue`创建的Vue根实例中，把这个组件作为自定义元素来使用。**组件与new Vue接收相同的选项**，例如`data`、`computed`、`watch`、`methods`以及生命周期钩子等。**仅有的例外是像`el`这样根实例特有的选项**。
```
// 定义一个名为 button-counter 的新组件
Vue.component('button-counter', {
  data: function () {
    return {
      count: 0
    }
  },
  template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
})

<div id="components-demo">
  <button-counter></button-counter>
</div>

// Vue根实例
new Vue({ el: '#components-demo' })
```
#### 9.1 组件的复用
+ 每用一次组件，就会有一个它的新实例被创建。
+ ==一个组件的`data`选项必须是一个函数==，因此每个实例可以维护一份被返回对象的独立的拷贝。
```
data: function () {
  return {
    count: 0
  }
}
```
#### 9.2 组件的组织
+ 为了能在模板中使用，组件必须先注册：**全局注册**和**局部注册**。
+ **全局注册**
```
Vue.component('my-component-name', {
  // ... options ...
})
```
全局注册的组件可以用在通过`new Vue`)新创建的Vue根实例，也包括其组件树中的所有子组件的模板中。

#### 9.3 通过Prop向子组件传递数据
+ Prop是你可以在组件上注册的一些自定义特性。当一个值传递给一个prop特性的时候，它就变成了那个**组件实例**的一个==属性==(属性可以在代表组件的标签中定义）。为了给博文组件传递一个标题，我们可以用一个props选项将其包含在该组件可接受的prop列表中：
+ 双大括号里的数据有无被注册成prop的区别：没有注册的数据应写在data里面，有注册成PROP的组件可以直接当成属性，在html中改值。
+ 一个组件默认可以拥有任意数量的 prop，任何值都可以传递给任何 prop。我们能够在组件实例中访问这个值，就像访问 data 中的值一样。
```
Vue.component('blog-post', {
  props: ['title'],
  template: '<h3>{{ title }}</h3>'
})
```
```
//一个 prop 被注册之后，你就可以像这样把数据作为一个自定义特性传递进来：
<blog-post title="My journey with Vue"></blog-post>
<blog-post title="Blogging with Vue"></blog-post>
```
+ 在一个典型的应用中，你可能在`data`里有一个博文的数组：
```
new Vue({
  el: '#blog-post-demo',
  data: {
    posts: [
      { id: 1, title: 'My journey with Vue' },
      { id: 2, title: 'Blogging with Vue' },
      { id: 3, title: 'Why Vue is so fun' },
    ]
  }
})
```
```
//并想为每篇博文渲染一个组件
<blog-post
  v-for="post in posts"
  v-bind:key="post.id"
  v-bind:title="post.title"
></blog-post>
```
如上，可以使用 v-for循环，再使用==`v-bind `来动态传递 prop==。这在你一开始不清楚要渲染的具体内容，比如从一个 API 获取博文列表的时候，是非常有用的。

#### 9.4 单个根元素
+ every component must have a single root element (**每个组件必须只有一个根元素**)。可以将模板的内容包裹在一个父元素内。
```
<div class="blog-post">
  <h3>{{ title }}</h3>
  <div v-html="content"></div>
</div>
```
+ 看起来当组件变得越来越复杂的时候，我们的博文不只需要标题和内容，还需要发布日期、评论等等。为每个相关的信息定义一个 prop 会变得很麻烦：
```
<blog-post
  v-for="post in posts"
  v-bind:key="post.id"
  v-bind:title="post.title"
  v-bind:content="post.content"
  v-bind:publishedAt="post.publishedAt"
  v-bind:comments="post.comments"
></blog-post>
```
所以是时候重构一下这个 <blog-post> 组件了，让它变成接受一个单独的 post prop：
```
<blog-post
  v-for="post in posts"
  v-bind:key="post.id"
  v-bind:post="post"
></blog-post>
```
```
Vue.component('blog-post', {
  props: ['post'],
  template: `
    <div class="blog-post">
      <h3>{{ post.title }}</h3>
      <div v-html="post.content"></div>
    </div>
  `
})
```

#### 9.5 监听子组件事件
+ 在我们开发组件时，它的一些功能可能要求我们和父级组件进行沟通，例如我们可能会放大子组件模板的字号，同时让页面的其他部分保持默认的字号。在其父组件中，我们可以通过添加一个`posFontSize`数据属性来支持这个功能。
```
new Vue({
  el: '#blog-posts-events-demo',
  data: {
    posts: [/* ... */],
    postFontSize: 1
  }
})
```
```
//这个style不可以直接写在CSS文件里吗？应该可以
<div id="blog-posts-events-demo">
  <div :style="{ fontSize: postFontSize + 'em' }">
    <blog-post
      v-for="post in posts"
      v-bind:key="post.id"
      v-bind:post="post"
    ></blog-post>
  </div>
</div>
```
+ 上面的例子不是根据用户的输入来确定字号大小，而是根据开发者自己的想法。如果想添加一个按钮，当用户点击时才会放大字号。就涉及到监听的问题了。
+ Vue实例提供了一个自定义事件的系统来解决这个问题，父级组件可以像处理原生DOM事件一样通过`v-on`监听子组件实例的任意事件，同时子组件可以通过调用內建的`$emit`方法来传入事件的名字触发一个事件（这里应该没涉及到父子组件通信）：
```
//子组件调用$emit方法和事件处理程序
Vue.component('blog-post', {
  props: ['post'],
  template: `
    <div class="blog-post">
      <h3>{{ post.title }}</h3>
      <button v-on:click="$emit('enlarge-text')">Enlarge text</button>
      <div v-html="post.content"></div>
    </div>
  `
})


//父组件定义事件处理程序
<blog-post
  ...
  v-on:enlarge-text="postFontSize += 0.1"
></blog-post>
```
##### 子组件使用事件抛出一个值
+ 上面的例子放大多少是由父组件决定的，但有时候我们想让子组件抛出一个特定的值，决定它的文本放大多少，这时可以使用`$emit`的第二个参数来提供这个值（子组件抛出一个值，在父组件中使用，可能这个才叫通信）：
```
//全局注册子组件
Vue.component('blog-post', {
  props: ['post'],
  template: `
    <div class="blog-post">
      <h3>{{ post.title }}</h3>
      <button v-on:click="$emit('enlarge-text'， 0.1)">Enlarge text</button>
      <div v-html="post.content"></div>
    </div>
  `
})

///父级组件监听这个事件时，通过 $event 访问到被抛出的这个值：
//调用组件阶段
<blog-post
  ...
  v-on:enlarge-text="postFontSize += $event"
></blog-post>
```
+ 如果事件处理函数是一个方法，那么这个值将会作为第一个参数传入这个方法：
```
<blog-post
  ...
  v-on:enlarge-text="onEnlargeText"
></blog-post>

methods: {
    //这里的形参就是$emit的第二个参数
  onEnlargeText: function (enlargeAmount) {
    this.postFontSize += enlargeAmount
  }
}
```
#### 总结
+ 父子组件之间如何通信？
    + 父组件向子组件传值：
        + 父组件向子组件传值，在子组件中注册props，props就变为子组件实例属性，父组件在标签中传值。
        + 父组件data中的值可以通过`v-bind:`来动态传递props
    + 子组件向父组件传值：
        + 子组件在模板中调用`v-on:$emit()`,第一个参数传入事件处理程序，第二个参数的值就是传给父组件的值。
        + 父组件像监听原生DOM事件一样监听子组件的事件，用`v-on:handler="(一个语句或一个方法名)"`，如果是一个语句，则用`$event`获取子组件传来的值，如果是定义在methods里的一个方法，则子组件传来的值会成为传入该方法的第一个参数。





##### 在组件上使用v-model
+ 自定义事件也可以用于创建支持`v-model`的自定义输入组件。
```
<input v-model="seatchText">
等价于
<input 
    v-bind:value="seatchText"
    v-on:input="searchText=$exent.target.value"
>

当用在组件上，v-model则会这样

<custom-input
    v-bind:value="searchText"
    v-on:input="search=$event"
></custom-input>
```
为了让它正常工作，这个组件内的<input>必须：
+ 将其value特性绑定在一个名叫`value`的prop上
+ 将其`input`事件被触发时，将新的值通过自定义的`input`事件抛出
```
Vue.component('custom-input',{
    props:['value'],
    template:
        <input
            v-bind:value="value"
            v-on:input="$emit('input',$event.target.value)"
        >
    })

<custom-input v-model="searchText"></custom-input>
```
#### 9.6 通过插槽分发内容
+ 向一个组件传递内容，只要在需要的地方插入插槽就行了。
```
Vue.component('alert-box', {
  template: `
    <div class="demo-alert-box">
      <strong>Error!</strong>
      <slot></slot>
    </div>
  `
})


<alert-box>
  Something bad happened.
</alert-box>
```
#### 9.7 动态组件
+ 有的时候，在不同组件之间进行动态切换是非常有用的，比如在一个多标签的界面里：
```
<!-- 组件会在 `currentTabComponent` 改变时改变 -->
<component v-bind:is="currentTabComponent"></component>
```
+ `currentTabComponent`可以包括已注册组件的名字，或一个组件的选项对象。
###### 解除DOM模板时的注意事项
+ 有些 HTML 元素，诸如 `<ul>`、`<ol>`、`<table>` 和 `<select>`，对于哪些元素可以出现在其内部是有严格限制的。而有些元素，诸如 `<li>`、`<tr>` 和 `<option>`，只能出现在其它某些特定的元素内部。如果把某个自定义组件放在`<table>`里面，会被作为无效的内容提升到外部，并导致渲染结果出错。`is`特性给了我们一个变通的方法。
```
<table>
  <tr is="blog-post-row"></tr>
</table>
```


> 慕课网 vue

+ vue和jquery框架的区别？(看todolist例子的代码找区别)
    + 数据和视图的分离，解耦（开放封闭原则）
    + 以数据驱动视图，只关心数据变化，DOM操作被封装
+ 说一下对MVVM的理解
    + Model-模型、数据
    + View - 视图、模板(视图和模型是分离的)
    + ViewModel - 连接Model和View
+ Vue三要素（如何实现vue)
    + 双向绑定(响应式）：vue如何监听到data的每个属性变化？
    + 模板引擎：vue的模板如何被解析，指令如何处理？
    + 渲染：vue的模板如何被渲染成html？以及渲染过程
