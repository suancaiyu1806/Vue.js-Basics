## Vue.js 学习摘要

### 一、介绍

1. 构建用户界面的**渐进式框架**（渐进式框架的意思就是可以只用框架的一部分，而不是只用了一点就必须用所有部分）；
2. 核心库只关注视图层，目标是通过尽可能简单的 API 实现**响应的数据绑定**和**组合的视图组件**；
3. [Vue.js 2.0 与其他框架的区别](https://www.w3cschool.cn/vuejs2/vuejs2-comparison.html);

### 二、起步

1. 声明式渲染

   Vue.js 的核心是一个允许采用简洁的模板语法来声明式的将数据渲染进 DOM 的系统。数据和 DOM 已经被绑定在一起，所有的元素都是**响应式的**。

   ```html
   <script src="https://unpkg.com/vue"></script>
   
   <div id="app">
     <p>{{ message }}</p>
   </div>
   ```

   ```javascript
   new Vue({
     el: '#app',
     data: {
       message: 'Hello Vue.js!'
     }
   })
   ```

2. 指令

   `v-` 属性被称为**指令**。指令带有前缀 `v-`，表示它们是 Vue.js 提供的特殊属性。它们会在渲染过的 DOM 上应用特殊的响应式行为。这个指令的简单含义是说：将这个元素节点的 `title` 属性和 Vue 实例的 `message` 属性绑定到一起。

   ```html
   <div id="app-2">
     <span v-bind:title="message">
       Hover your mouse over me for a few seconds to see my dynamically bound title!
     </span>
   </div>
   ```

   ```javascript
   var app2 = new Vue({
     el: '#app-2',
     data: {
       message: 'You loaded this page on ' + new Date()
     }
   })
   ```

3. 条件与循环

   * if 条件

     Vue 不仅可以绑定 DOM 文本到数据，也可以绑定 DOM **结构**到数据。而且，Vue.js 也提供一个强大的过渡效果系统，可以在 Vue 插入/删除元素时自动应用[过渡效果](https://www.w3cschool.cn/vuejs2/transitions.html)。

   ```html
   <div id="app-3">
     <p v-if="seen">Now you see me</p>
   </div>
   ```

   ```javascript
   var app3 = new Vue({
     el: '#app-3',
     data: {
       seen: true
     }
   })
   ```

   * for 循环

     

   ```html
   <div id="app-4">
     <ol>
       <li v-for="todo in todos">
         {{ todo.text }}
       </li>
     </ol>
   </div>
   ```

   ```javascript
   var app4 = new Vue({
     el: '#app-4',
     data: {
       todos: [
         { text: 'Learn JavaScript' },
         { text: 'Learn Vue' },
         { text: 'Build something awesome' }
       ]
     }
   })
   ```

4. 处理用户输入

   * 监听事件 `v-on`

   ```html
   <div id="app-5">
     <p>{{ message }}</p>
     <button v-on:click="reverseMessage">Reverse Message</button>
   </div>
   ```

   ```javascript
   var app5 = new Vue({
     el: '#app-5',
     data: {
       message: 'Hello Vue.js!'
     },
     methods: {
       reverseMessage: function () {
         this.message = this.message.split('').reverse().join('')
       }
     }
   })
   ```

   * `v-model` 指令使得在表单输入和应用状态中做双向数据绑定变得非常轻巧

   ```html
   <div id="app-6">
     <p>{{ message }}</p>
     <input v-model="message">
   </div>
   ```

   ```javascript
   var app6 = new Vue({
     el: '#app-6',
     data: {
       message: 'Hello Vue!'
     }
   })
   ```

5. 用组件构建（应用）

   ```html
   <div id="app-7">
     <ol>
       <!-- Now we provide each todo-item with the todo object    -->
       <!-- it's representing, so that its content can be dynamic -->
       <todo-item v-for="item in groceryList" v-bind:todo="item"></todo-item>
     </ol>
   </div>
   ```

   ```javascript
   Vue.component('todo-item', {
     props: ['todo'],
     template: '<li>{{ todo.text }}</li>'
   })
   var app7 = new Vue({
     el: '#app-7',
     data: {
       groceryList: [
         { text: 'Vegetables' },
         { text: 'Cheese' },
         { text: 'Whatever else humans are supposed to eat' }
       ]
     }
   })
   ```

### 三、实例

1. 构造器

   Vue 中构造器可以通过扩展，从而用预定义选项创建可复用的组件构造器（类似继承）。尽管可以命令式地创建扩展实例，不过在多数情况下建议将组件构造器注册为一个自定义元素，然后声明式地用在模板中。

    Vue.js 组件都是被扩展的 Vue 实例。

   ```javascript
   var MyComponent = Vue.extend({
     // 扩展选项
   })
   // 所有的 `MyComponent` 实例都将以预定义的扩展选项被创建
   var myComponentInstance = new MyComponent()
   ```

2. 属性与方法

   + Vue 可以代理 Vue 实例 data 中的属性，被代理的属性是响应式的，属性的改变会引起使徒的改变。但如果想在实例中添加新属性，则将由于未被代理无法响应，既不能引起使徒的变化。
   + 像是 el, data 等 Vue 暴露的属性与方法在调用时都有前缀 eg:`vm.$data`，以便与代理的 data 属性（即 data 后大括号中内容）区分。

3. 生命周期

   暂未学习

### 四、模板语法

1. `v-once` 可以实现一次性插值，数据改变时，插值处的内容不会更新；

2. `v-html` 可实现纯 HTML 的实现方式，但不常用也不建议，故不介绍；

3. 想要在 HTML 属性中使用 Mustache ，应使用 v-bind 指令进行绑定，可对某节点（ id ）或某属性进行绑定；

4. Vue.js 对所有的数据绑定均提供了完全的 JavaScript 支持（[注：只支持单个表达式，不支持语句]()）：

   ```javascript
   {{ number + 1 }}
   {{ ok ? 'YES' : 'NO' }}
   {{ message.split('').reverse().join('') }}
   <div v-bind:id="'list-' + id"></div>
   ```

5. 过滤器：Vue.js 允许你自定义过滤器，被用作一些常见的文本格式化（eg:`{{ message | capitalize }}`），过滤器函数总接受表达式的值作为第一个参数，过滤器是 JavaScript 函数，可以接受参数（eg:`{{ message | filterA('arg1', arg2) }}`，其中 arg1 作为第二个参数传递给过滤器），可以串联（eg:`{{ message | filterA | filterB }}`）;

   ```javascript
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
   ```

6. 指令，`v-if v-for v-on v- bind` 等，一些指令能接受一个“参数”，在指令后以冒号指明。

   + 例一

     ```html
     <a v-bind:href="url"></a>
     ```

     href 是参数，将该元素的 href 属性与表达式 url 的值绑定。

   + 例二

     ```html
     <a v-on:click="doSomething">
     ```

     参数 click 是监听的事件名。

7. 修饰符，用于指出一个指定应该以特殊方式绑定；

   ```html
   <form v-on:submit.prevent="onSubmit"></form>
   ```

   .prevent 修饰符告诉 v-on 指令对于触发的事件调用 event.preventDefault()。

8. 缩写

   + `v-bind 缩写`

     ```html
     <!-- 完整语法 -->
     <a v-bind:href="url"></a>
     <!-- 缩写 -->
     <a :href="url"></a>
     ```

   * `v-on` 缩写

     ```html
     <!-- 完整语法 -->
     <a v-on:click="doSomething"></a>
     <!-- 缩写 -->
     <a @click="doSomething"></a>
     ```

   [该部分均较简单，参见`介绍`部分即可了解，这里只列出部分重点。]()

### 五、计算属性

+ 在模板中绑定表达式较为便利，但放入较多的逻辑则会由于过于笨重而难以维护。因此涉及较复杂逻辑时会使用计算属性。

1. 计算缓存 vs Methods

   + 前例的计算属性实现

   ```html
   <div id="example">
     <p>Original message: "{{ message }}"</p>
     <p>Computed reversed message: "{{ reversedMessage }}"</p>
   </div>
   ```

   ```javascript
   var vm = new Vue({
     el: '#example',
     data: {
       message: 'Hello'
     },
     computed: {
       // a computed getter
       reversedMessage: function () {
         // `this` points to the vm instance
         return this.message.split('').reverse().join('')
       }
     }
   })
   ```

   + 前例的 method 实现

   ```html
   <div id="example">
     <p>Original message: "{{ message }}"</p>
     <p>Computed reversed message: "{{ reverseMessage() }}"</p>
   </div>
   ```

   ```javascript
   var vm = new Vue({
     el: '#example',
     data: {
       message: 'Hello'
     },
     methods: {
     reverseMessage: function () {
       return this.message.split('').reverse().join('')
     }
   })
   ```

   计算属性是基于它的依赖缓存。计算属性只有在它的相关依赖发生改变时才会重新取值。这就意味着只要 message 没有发生改变，多次访问 reversedMessage 计算属性会立即返回之前的计算结果，而不必再次执行函数。

   + 再例

   ```javascript
   computed: {
     now: function () {
       return Date.now()
     }
   ```

   该计算属性则不会更新，因为 Date.now() 不是响应式依赖，其只与系统时间有关。相比而言，每当重新渲染的时候，method 调用总会执行函数。

2. 计算属性 vs Watched Property

   用于观察 Vue 实例上的数据变动，用于一些数据需要根据其它数据变化时。

   + 非计算属性实现

     ```html
     <div id="demo">{{ fullName }}</div>
     ```

     ```javascript
     var vm = new Vue({
       el: '#demo',
       data: {
         firstName: 'Foo',
         lastName: 'Bar',
         fullName: 'Foo Bar'
       },
       watch: {
         firstName: function (val) {
           this.fullName = val + ' ' + this.lastName
         },
         lastName: function (val) {
           this.fullName = this.firstName + ' ' + val
         }
       }
     })
     
     ```

   + 计算属性实现

     ```javascript
     var vm = new Vue({
       el: '#demo',
       data: {
         firstName: 'Foo',
         lastName: 'Bar'
       },
       computed: {
         fullName: function () {
           return this.firstName + ' ' + this.lastName
         }
       }
     })
     ```

3. setter

   默认有 getter ，可设置 setter 属性：

   ```javascript
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
   ```

   运行 vm.fullName = 'John Doe' 时， setter 会被调用， vm.firstName 和 vm.lastName 也会被对应更新。

4. 观察 Watchers

   ...

### 六、Class 与 Style 绑定

1. class 对象语法

   传给 `v-bind:class` 一个对象，以动态地切换 class：

   ```html
   <div v-bind:class="{ active: isActive }"></div>
   ```

   上例中 class 是否为 active 取决于 isActive 的值。

   也可在对象中传入更多属性用来动态切换多个 class ，可以与普通的 class 属性共存：

   ```html
   <div class="static"
        v-bind:class="{ active: isActive, 'text-danger': hasError }">
   </div>
   ```

   ```javascript
   data: {
     isActive: true,
     hasError: false
   }
   ```

   也可直接绑定数据中的一个对象：

   ```html
   <div v-bind:class="classObject"></div>
   ```

   ```javascript
   data: {
     classObject: {
       active: true,
       'text-danger': false
     }
   }
   ```

   效果等同于：

   ```html
   <div class="static active"></div>
   ```

2. 绑定返回对象的计算属性

   ```html
   <div v-bind:class="classObject"></div>
   ```

   ```javascript
   data: {
     isActive: true,
     error: null
   },
   computed: {
     classObject: function () {
       return {
         active: this.isActive && !this.error,
         'text-danger': this.error && this.error.type === 'fatal',
       }
     }
   }
   ```

3. 数组语法

   可以把一个数组传给 v-bind:class ，从而应用一个 class 列表：

   ```html
   <div v-bind:class="[activeClass, errorClass]">
   ```

   ```javascript
   data: {
     activeClass: 'active',
     errorClass: 'text-danger'
   }
   ```

   渲染为：

   ```html
   div class="active text-danger"></div>
   ```

   可使用三元表达式根据条件切换列表中的 class：

   ```html
   <div v-bind:class="[isActive ? activeClass : '', errorClass]">
   ```

   也可在数组中使用对象语法：

   ```html
   <div v-bind:class="[{ active: isActive }, errorClass]">
   ```

4. 用在组件上

   ...

5. CSS 对象语法

   ```html
   <div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
   ```

   ```javascript
   data: {
     activeColor: 'red',
     fontSize: 30
   }
   ```

   绑定一个样式对象更清晰：

   ```html
   <div v-bind:style="styleObject"></div>
   ```

   ```javascript
   data: {
     styleObject: {
       color: 'red',
       fontSize: '13px'
     }
   }
   ```

6. CSS 数组语法

   ```html
   <div v-bind:style="[baseStyles, overridingStyles]">
   ```

### 七、组件

+ 条件渲染、列表渲染、事件处理器及表单控件绑定详细内容不再提及

1. 定义

   组件可以扩展 HTML 元素，封装可重用的代码。在较高层面上，组件是自定义元素， Vue.js 的编译器为它添加特殊功能。在有些情况下，组件也可以是原生 HTML 元素的形式，以 `is` 特性扩展。

2. 注册