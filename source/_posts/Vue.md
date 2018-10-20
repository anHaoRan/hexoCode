---
title: Vue
date: 2017-06-29 12:09:55
tags: Vue
toc: true
---
# Vue
## 介绍
### Vue.js 是什么
>一套构建用户界面的渐进式框架。与其他重量级框架不同的是，Vue 采用自底向上增量开发的设计。Vue 的核心库只关注视图层，它不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与单文件组件和 Vue 生态系统支持的库结合使用时，Vue 也完全能够为复杂的单页应用程序提供驱动。

### 简单起步
>双向数据绑定，所有的元素都是响应式的。
~~~html
    <style>
        [v-cloak]{
            visibility: hidden; 
        }
    </style>
    <div id="app">
        {{ message }}
        <span v-bind:title="message"></span>
        <!--
            Vue的数据绑定方法有：
            双花括号：{{}}
            V开头的bind：v-bind
        -->
    </div>
    <script src="https://unpkg.com/vue/dist/vue.js"></script>
    <script>
        var app = new Vue({
            el: '#app',
            data: {
                message: 'Hello Vue!'+ new Date()
            }
        })
    </script>
~~~
<!--more-->
### Vue指令
>看到的以v-开头的属性，被称为指令。
>v-bind，用处表示“将这个元素节点的title属性和Vue实例中的massage属性保持一致。
>可以简写为‘:’，:title。”
>v-on,指令监听 DOM 事件来触发一些 JavaScript 代码。
>可以简写为‘@’，@click

## 安装
### CDN
>推荐：[unpkg](https://unpkg.com/vue@2.3.4/dist/vue.js), 会保持和 npm 发布的最新的版本一致。可以在 [unpkg.com/vue/](https://unpkg.com/vue/) 浏览 npm 包资源。
>也可以从 [jsdelivr](https://cdn.jsdelivr.net/vue/2.1.3/vue.js) 或 [cdnjs](https://cdnjs.cloudflare.com/ajax/libs/vue/2.1.3/vue.js) 获取，不过这两个服务版本更新可能略滞后。

### NPM
>在用 Vue.js 构建大型应用时推荐使用 NPM 安装， NPM 能很好地和诸如 [Webpack](http://webpack.github.io/) 或 [Browserify](http://browserify.org/) 模块打包器配合使用。 Vue.js 也提供配套工具来开发单文件组件。

~~~js
$ npm install vue
~~~
### 构建方式
#### 概念
>有两种构建方式，独立构建和运行构建。它们的区别在于前者包含模板编译器而后者不包含。

#### 模板编译器
>模板编译器的职责是将模板字符串编译为纯 JavaScript 的渲染函数。如果你想要在组件中使用 template 选项，你就需要编译器。

#### 模板字符串：template

####el
>提供一个在页面上已存在的 DOM 元素作为 Vue 实例的挂载目标。可以是 CSS 选择器，也可以是一个 HTMLElement 实例。

 

#### template
>一个字符串模板作为 Vue 实例的标识使用。模板将会 替换 挂载的元素。挂载元素的内容都将被忽略，除非模板的内容有分发 slot。

#### render
>字符串模板的代替方案，允许你发挥 JavaScript 最大的编程能力。render 函数接收一个 createElement 方法作为第一个参数用来创建 VNode。

#### 区别
>独立构建包含模板编译器并支持 template 选项。 它也依赖于浏览器的接口的存在，所以你不能使用它来为服务器端渲染。
运行时构建不包含模板编译器，因此不支持 template 选项，只能用 render 选项，但即使使用运行时构建，在单文件组件中也依然可以写模板，因为单文件组件的模板会在构建时预编译为 render 函数。运行时构建比独立构建要轻量30%，只有 17.14 Kb min+gzip大小。

#### 为什么要使用独立构建和运行时构建？

>Vue.js 的运行过程实际上包含两步。第一步，编译器将字符串模板（template）编译为渲染函数（render），称之为编译过程；第二步，运行时实际调用编译的渲染函数，称之为运行过程
>由于 Vue.js 1.0 的编译过程需要依赖浏览器的 DOM，所以无法（或者说没有意义）将编译器和运行时分开。因此在 Vue.js 1.0 分发包中，编译器和运行时是打包在一起，都在浏览器端执行。
>然而到了 Vue.js 2.0，为了支持服务端渲染（server-side rendering），编译器不能依赖于 DOM，所以必须将编译器和运行时分开。这就形成了独立构建（编译器 + 运行时）和运行时构建（仅运行时）。显而易见，运行时构建要小于独立构建。
>在现代前端工程构建中，通常会使用 vue-loader 和 vueify 预编译模板。在这种情况下，只需要打包运行时，而不需要打包编译器，运行时构建即可满足所需。当然，如果你需要在前端使用 template 选项实时编译模板，那么还是需要使用独立构建将编译器发送到浏览器。

##### 总结：
>两种编译模式是为了服务器端渲染和浏览器执行两种不同环境产生的
一般来讲独立构建适用于服务器端渲染
浏览器 实际运行时为 运行时构建  但是如果需要在前端使用 template 选项实时编译模板，那么还是需要使用独立构建将编译器发送到浏览器。

## 命令行工具
Vue.js 提供一个[官方命令行工具](https://github.com/vuejs/vue-cli)，可用于快速搭建大型单页应用。该工具提供开箱即用的构建工具配置，带来现代化的前端开发流程。只需几分钟即可创建并启动一个带热重载、保存时静态检查以及可用于生产环境的构建配置的项目：
~~~js
    # 全局安装 vue-cli
    $ npm install --global vue-cli
    # 创建一个基于 webpack 模板的新项目
    $ vue init webpack my-project
    # 安装依赖，走你
    $ cd my-project
    $ npm install
    $ npm run dev
~~~
## 实例
### 构造器
~~~js
    var vm = new Vue({
        // 选项
        el:'#app',
        data:{},
        .....
    })
~~~
你可以在构造器的外部用vm来代表vm构造器
>在实例化 Vue 时，需要传入一个选项对象，它可以包含数据、模板、挂载元素、方法、生命周期钩子等选项。全部的选项可以在 [API 文档](https://cn.vuejs.org/v2/api/)中查看。

### 组件构造器
~~~js
    var MyComponent = Vue.extend({
        // 扩展选项
    });
        // 所有的 `MyComponent` 实例都将以预定义的扩展选项被创建
    var myComponentInstance = new MyComponent();
~~~
>尽管可以命令式地创建扩展实例，不过在多数情况下建议将组件构造器注册为一个自定义元素，然后声明式地用在模板中。我们将在后面详细说明[组件系统](https://anhaoran.github.io/2017/06/29/Vue-components/)。现在你只需知道所有的 Vue.js 组件其实都是被扩展的 Vue 实例。

## 属性和方法
每个 Vue 实例都会代理其 data 对象里所有的属性：
~~~js
    var data = { a: 1 }
    var vm = new Vue({
        data: data
    })
    vm.a === data.a // -> true
    // 设置属性也会影响到原始数据
    vm.a = 2
    data.a // -> 2
    // ... 反之亦然
    data.a = 3
    vm.a // -> 3
~~~
注意只有这些被代理的属性是响应的。如果在实例创建之后添加新的属性到实例上，它不会触发视图更新。我们将在后面详细讨论响应系统。
除了 data 属性， Vue 实例暴露了一些有用的实例属性与方法。这些属性与方法都有前缀 $，以便与代理的 data 属性区分。例如：
~~~js
    var data = { a: 1 }
    var vm = new Vue({
        el: '#example',
            data: data
        })
    vm.$data === data // -> true
    vm.$el === document.getElementById('example') // -> true
    // $watch 是一个实例方法
    vm.$watch('a', function (newVal, oldVal) {
        // 这个回调将在 `vm.a`  改变后调用
    })
~~~
>注意，不要在实例属性或者回调函数中（如 vm.$watch('a', newVal => this.myMethod())）使用箭头函数。因为箭头函数绑定父级上下文，所以 this 不会像预想的一样是 Vue 实例，而是 this.myMethod 未被定义。
实例属性和方法的完整列表中查阅 [API](https://anhaoran.github.io/2017/06/29/Vue-api/) 参考。

## 实例生命周期
每个 Vue 实例在被创建之前都要经过一系列的初始化过程。例如，实例需要配置数据观测(data observer)、编译模版、挂载实例到 DOM ，然后在数据变化时更新 DOM 。在这个过程中，实例也会调用一些 生命周期钩子 ，这就给我们提供了执行自定义逻辑的机会。例如，created 这个钩子在实例被创建之后被调用：
~~~js
    var vm = new Vue({
    data: {
        a: 1
    },
    created: function () {
        // `this` 指向 vm 实例
        console.log('a is: ' + this.a)
    }
    })
    // -> "a is: 1"
~~~
也有一些其它的钩子，在实例生命周期的不同阶段调用，如 mounted、 updated 、destroyed 。钩子的 this 指向调用它的 Vue 实例。一些用户可能会问 Vue.js 是否有“控制器”的概念？答案是，没有。组件的自定义逻辑可以分布在这些钩子中。
### 生命周期图示
下图说明了实例的生命周期。你不需要立马弄明白所有的东西，不过以后它会有帮助。
![](https://anhaoran.github.io/ImageServer/Vue/lifecycle.png)
## 模板语法
Vue.js 使用了基于 HTML 的模板语法，允许开发者声明式地将 DOM 绑定至底层 Vue 实例的数据。所有 Vue.js 的模板都是合法的 HTML ，所以能被遵循规范的浏览器和 HTML 解析器解析。
在底层的实现上， Vue 将模板编译成虚拟 DOM 渲染函数。结合响应系统，在应用状态改变时， Vue 能够智能地计算出重新渲染组件的最小代价并应用到 DOM 操作上。
如果你熟悉虚拟 DOM 并且偏爱 JavaScript 的原始力量，你也可以不用模板，直接写渲染（render）函数，使用可选的 JSX 语法。
### 插值
#### 文本
数据绑定最常见的形式就是使用 “Mustache” 语法（双大括号）的文本插值：
~~~html
    <span>Message: {{ msg }}</span>
~~~
Mustache 标签将会被替代为对应数据对象上 msg 属性的值。无论何时，绑定的数据对象上 msg 属性发生了改变，插值处的内容都会更新。
通过使用 v-once 指令，你也能执行一次性地插值，当数据改变时，插值处的内容不会更新。但请留心这会影响到该节点上所有的数据绑定：
~~~html
    <span v-once>This will never change: {{ msg }}</span>
~~~
#### 纯HTML
双大括号会将数据解释为纯文本，而非 HTML 。为了输出真正的 HTML ，你需要使用 v-html 指令：
~~~html
    <div v-html="rawHtml"></div>
~~~
被插入的内容都会被当做 HTML —— 数据绑定会被忽略。注意，你不能使用 v-html 来复合局部模板，因为 Vue 不是基于字符串的模板引擎。组件更适合担任 UI 重用与复合的基本单元。
>你的站点上动态渲染的任意 HTML 可能会非常危险，因为它很容易导致 XSS 攻击。请只对可信内容使用 HTML 插值，绝不要对用户提供的内容插值。

#### 属性
Mustache 不能在 HTML 属性中使用，应使用 v-bind 指令：
~~~html
    <div v-bind:id="dynamicId"></div>
~~~
这对布尔值的属性也有效 —— 如果条件被求值为 false 的话该属性会被移除：
~~~html
    <button v-bind:disabled="someDynamicCondition">Button</button>
~~~
#### 使用JavaScript表达式
迄今为止，在我们的模板中，我们一直都只绑定简单的属性键值。但实际上，对于所有的数据绑定， Vue.js 都提供了完全的 JavaScript 表达式支持。
~~~html
    {{ number + 1 }}
    {{ ok ? 'YES' : 'NO' }}
    {{ message.split('').reverse().join('') }}
    <div v-bind:id="'list-' + id"></div>
~~~
这些表达式会在所属 Vue 实例的数据作用域下作为 JavaScript 被解析。有个限制就是，每个绑定都只能包含单个表达式，所以下面的例子都不会生效。
~~~html
    <!-- 这是语句，不是表达式 -->
    {{ var a = 1 }}
    <!-- 流控制也不会生效，请使用三元表达式 -->
    {{ if (ok) { return message } }}
~~~
>模板表达式都被放在沙盒中，只能访问全局变量的一个白名单，如 Math 和 Date 。你不应该在模板表达式中试图访问用户定义的全局变量。

### 指令
令（Directives）是带有 v- 前缀的特殊属性。指令属性的值预期是单一 JavaScript 表达式（除了 v-for，之后再讨论）。指令的职责就是当其表达式的值改变时相应地将某些行为应用到 DOM 上。让我们回顾一下在介绍里的例子：
~~~html
    <p v-if="seen">Now you see me</p>
~~~
这里， v-if 指令将根据表达式 seen 的值的真假来移除/插入 <p> 元素。
#### 参数
一些指令能接受一个“参数”，在指令后以冒x号指明。例如， v-bind 指令被用来响应地更新 HTML 属性：
~~~html
    <a v-bind:href="url"></a>
~~~
在这里 href 是参数，告知 v-bind 指令将该元素的 href 属性与表达式 url 的值绑定。
另一个例子是 v-on 指令，它用于监听 DOM 事件：
~~~html
    <a v-on:click="doSomething">
~~~
#### 修饰符
修饰符（Modifiers）是以半角句号 . 指明的特殊后缀，用于指出一个指令应该以特殊方式绑定。例如，.prevent 修饰符告诉 v-on 指令对于触发的事件调用 event.preventDefault()：
~~~html
    <form v-on:submit.prevent="onSubmit"></form>
~~~
之后当我们更深入地了解 v-on 与 v-model时，会看到更多修饰符的使用。
### 过滤器
Vue.js 允许你自定义过滤器，可被用作一些常见的文本格式化。过滤器可以用在两个地方：mustache 插值和 v-bind 表达式。过滤器应该被添加在 JavaScript 表达式的尾部，由“管道”符指示：
~~~html
    <!-- in mustaches -->
    {{ message | capitalize }}
    <!-- in v-bind -->
    <div v-bind:id="rawId | formatId"></div>
~~~
>Vue 2.x 中，过滤器只能在 mustache 绑定和 v-bind 表达式（从 2.1.0 开始支持）中使用，因为过滤器设计目的就是用于文本转换。为了在其他指令中实现更复杂的数据变换，你应该使用计算属性。

过滤器函数总接受表达式的值作为第一个参数。
~~~js
    new Vue({
        // ...
        filters: {
            capitalize: function (value) {
            if (!value) return ''
            value = value.toString()
            return value.charAt(0).toUpperCase() + value.slice(1)
            }
        }
    })
~~~
过滤器可以串联：
~~~html
    {{ message | filterA | filterB }}
~~~
过滤器是 JavaScript 函数，因此可以接受参数：
~~~html
    {{ message | filterA('arg1', arg2) }}
~~~
这里，字符串 'arg1' 将传给过滤器作为第二个参数， arg2 表达式的值将被求值然后传给过滤器作为第三个参数。

### 缩写
v- 前缀在模板中是作为一个标示 Vue 特殊属性的明显标识。当你使用 Vue.js 为现有的标记添加动态行为时，它会很有用，但对于一些经常使用的指令来说有点繁琐。同时，当搭建 Vue.js 管理所有模板的 SPA 时，v- 前缀也变得没那么重要了。因此，Vue.js 为两个最为常用的指令提供了特别的缩写：
#### v-bind缩写
~~~html
    <!-- 完整语法 -->
    <a v-bind:href="url"></a>
    <!-- 缩写 -->
    <a :href="url"></a>
~~~
#### v-on缩写
~~~html
    <!-- 完整语法 -->
    <a v-on:click="doSomething"></a>
    <!-- 缩写 -->
    <a @click="doSomething"></a>
~~~
它们看起来可能与普通的 HTML 略有不同，但 : 与 @ 对于属性名来说都是合法字符，在所有支持 Vue.js 的浏览器都能被正确地解析。而且，它们不会出现在最终渲染的标记。缩写语法是完全可选的，但随着你更深入地了解它们的作用，你会庆幸拥有它们。