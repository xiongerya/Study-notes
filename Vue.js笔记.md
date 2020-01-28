# VUE学习笔记
## Vue引用方式
**在html文件中引入Vue**
- 开发环境版本：包含帮助的命令行警告
	- `<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>` 
- 生产环境版本：优化了尺寸和速度
	- `<script src="https://cdn.jsdelivr.net/npm/vue"></script>`

**Vue模板语法格式**
**html结构**
`<div id="name">`
	`//插值表达式的用法`
	`{{ message }}`
	`{{ message + "!!!" }}`
	`This is a {{ person.name }}`
	`{{ person.name }}`
	`//过滤器的使用`
	`{{ message || addMsg }}``
`</div>`
**JavaScript结构**
`var app = new Vue({`
	`//el => Vue对象作用的html对象范围`
	`el: '#name',`
	`//data => Vue对象数据，采用键/值对的方式表示`
	`//值可以是任何js数据类型，也可以是Vue对象之外的变量`
	`//在Vue对象内部访问数据需要使用this：this.person`
	`data: {`
		`message: 'Hello World!',`
		`person: {name: "Tom", age: 18}`
	`},`
	`//methods => Vue对象中的方法，可以传递参数`
	`//与事件处理程序一起使用：v-on:click="add"`
	`methods: {`
		`add: function(){code...},`
		`delete(){code...}`
	`}`
	`//filters => Vue对象中的过滤器（私有过滤器）`
	`//将传递的参数进行处理，只能用于插值表达式中`
	`filters: {`
		`formate: function(txt){ code... }`
		`addMsg(msg){ code... }`
	`}`
`})`
**注意事项**
- 元素选择可以使用css选择器，id/class/tag等选择器均可
- data可以是JavaScript中任意的数据类型（基础/复杂数据类型）
- data中的数据可以在el元素及其所有后代元素范围内使用
- 应用在html标签中的{{ message }}，称为**插值表达式**

___
## Vue指令语法
### v-text
>相当于textContent，为元素添加文本内容

**html结构**
`<div id="name" v-text="message"></div>`
也可拼接字符：
`<div id="name" v-text="message + 'good'"></div>`
**JavaScript结构**
`var app = new Vue({`
	`el: '#name',`
	`data: {`
		`message: 'Hello World!',`
	`}`
`})`

### v-html
>相当于innerHTML属性，即可添加文本也可识别html标签

**html结构**
`<div id="name" v-html="message"></div>`
**JavaScript结构**
`var app = new Vue({`
	`el: '#name',`
	`data: {`
		`message: '<h2>Hello World!</h2>',`
	`}`
`})`

### v-bind
> 设置html元素的属性（比如：src/title/class等）

**html结构**
`<div id="name" v-bind:class="message"></div>`
简写方式：
`<div id="name" :class="message"></div>`
表达式：
`<div id="name" :class="message ? message : 'active'"></div>`
**JavaScript结构**
`var app = new Vue({`
	`el: '#name',`
	`data: {`
		`message: 'active'`
	`}`
`})
### v-show
>决定html元素显示/隐藏，操纵css样式对性能消耗较小
>需要重复切换显示/隐藏的元素时，使用v-show

**html结构**
布尔值
`<div id="name" v-show="true"></div>`
变量值
`<div id="name" v-show="bool"></div>`
表达式
`<div id="name" v-show="age > 18"></div>`
**JavaScript结构**
`var app = new Vue({`
	`el: '#name',`
	`data: {`
		`bool: false,`
    	`age: 19,`
	`},`
	`methods: {`
		`//添加一个事件改变bool值`
		`doThing: function(){`
			`this.bool = !this.bool;`
		`}`
	`}`
`})`
### v-if
>条件判断元素显示/隐藏，操纵dom树对性能消耗较大
>隐藏时从dom树中移除，显示时重新添加到dom树中

**html结构**
布尔值
`<div id="name" v-if="true"></div>`
变量值
`<div id="name" v-if="bool"></div>`
表达式
`<div id="name" v-if="age > 18"></div>`
**JavaScript结构**
`var app = new Vue({`
	`el: '#name',`
	`data: {`
		`bool: false,`
    	`age: 19,`
	`},`
	`methods: {`
		`//可以添加一个事件改变bool值`
		`doThing: function(){`
			`this.bool = !this.bool;`
		`}`
		`//其他添加事件的方式`
		`doIt(){`
			`console.log("This is a test.")`
		`}`
	`}`
`})`

### v-for
>循环结构展示所需数据据

**html结构**
`<div id="name">`
	`<ul>`
		`<li v-for="(item, index) in arr">{{ item }}</li>`
	`</ul>`
`</div>`
**JavaScript结构**
`let app = new Vue({`
	`el: "#name",`
	`data: {`
		`arr: [1, 2, 3, 4, 5],`
		`obj: [{name:"one"}, {name:"two"}]`
	`}`
`})`
### v-on
>添加事件监听程序，比如onclick/onmouseover等

**html结构**
`<div id="name" v-on:click="doThing"></div>`
简写方式：
`<div id="name" @click="doThing"></div>`
表达式：
`<div id="name" @click="message += '!'"></div>`
**JavaScript结构**
`var app = new Vue({`
	`el: '#name',`
	`data: {`
		`message: 'Hello World!'`
	`},`
	`methods: {`
		`doThing: function(){`
			`message += "!";`
		`}`
	`}`
`})`
### v-model
>实现表单输入和应用状态之间的双向绑定，便捷的获取和设置表单元素的值

**html结构**
`<div id="name">`
	`<input type="text" v-model="message" />`
`</div>`
**JavaScript结构**
`var app = new Vue({`
	`el: '#name',`
	`data: {`
		`message: 'Hello World!',`
	`}`
`})`
*当input表单元素内容改变时，message相应做出改变*

### v-once
>只渲染元素和组件一次。随后的重新渲染，元素/组件及其所有的子节点将被视为静态内容并跳过。这可以用于优化更新性能。

**html结构**
`<div id="name" v-once>{{ message }</div>`
**JavaScript结构**
`var app = new Vue({`
	`el: '#name',`
	`data: {`
		`message: 'Hello World!',`
	`}`
`})`

### v-cloak
>这个指令保持在元素上直到关联实例结束编译，用于解决插值表达式网络延迟时出现的闪烁问题。

**html结构**
`<div id="name" v-cloak>{{ message }</div>`
**JavaScript结构**
`var app = new Vue({`
	`el: '#name',`
	`data: {`
		`message: 'Hello World!',`
	`}`
`})`

___
## Vue修饰符
**一般事件修饰符**
eg：`<input @click.stop.once="" />`
表示元素上点击事件触发时阻止冒泡行为，但仅阻止一次
- `.stop`：阻止事件冒泡行为
- `.prevent`：阻止默认事件 行为
- `.capture`：添加事件侦听器时使用事件捕获模式
- `.self`：事件只有在该元素上触发时才发生
- `.once`：事件只执行一次

**键盘事件修饰符**
eg：`<input @keyup.enter="" />`
表示当键盘点击的是enter键时，才执行该事件

___
## Vue注册组件
**html结构**
`<div id="name">`
	`<ul>`
		`<todo-item v-for="item in foods" v-bind:todo="item" v-bind:key="item.id"></todo-item>`
	`</ul>`
`</div>`
**JavaScript结构**
`//此处开始创建Vue自定义组件`
`Vue.component('todo-item', {`
	`//pros数组内的是自定义组件内部的attribute（属性）`
	`'props': ['todo'],`
	`'template': '<li>{{ todo.text }}</li>'`
`})`

`var app = new Vue({`
	`el: '#name',`
	`data: {`
		`message: 'Hello World!',`
		`foods: [`
			`{id: 1, text: "111111"},`
			`{id: 2, text: "222222"}`
		`]`
	`}`
`})`
___
## Vue生命周期


[Vue官网学习](https://cn.vuejs.org/v2/guide/)
