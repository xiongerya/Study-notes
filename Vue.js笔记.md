# VUE学习笔记
## Vue引用方式&基础格式
**在html文件中引入Vue**
- 开发环境版本：包含帮助的命令行警告
	- `<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>` 
- 生产环境版本：优化了尺寸和速度
	- `<script src="https://cdn.jsdelivr.net/npm/vue"></script>`

**Vue模板语法格式**
**html结构**
`<div id="name">`
	`{{ message }}`
	`{{ message + "!!!" }}`
	`{{ person.name }}`
`</div>`
**JavaScript结构**
`var app = new Vue({`
	`el: '#name',`
	`data: {`
		`message: 'Hello World!',`
		`person: {name: "Tom", age: 18}`
	`},`
	`methods: {`
		`m: function(){},`
		`n: function(){}`
	`}`
`})`
**注意事项**
- 元素选择可以使用css选择器，id/class/tag等选择器均可
- data可以是JavaScript中任意的数据类型（基础/复杂数据类型）
- data中的数据可以在el元素及其所有后代元素范围内使用

___
## Vue基础模板语法格式
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
>相当于innerHTMLl属性，即可添加文本也可识别html标签

**html结构**
`<div id="name" v-html="message"></div>`
**JavaScript结构**
`var app = new Vue({`
	`el: '#name',`
	`data: {`
		`message: '<h2>Hello World!</h2>',`
	`}`
`})`
### v-on
>事件绑定，比如onclick/onmouseover等

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
### v-show
>决定html元素显示/隐藏，操纵css样式对性能消耗较小
>需要重复切换显示/隐藏元素时，使用v-show

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
		`//可以添加一个事件改变bool值`
		`doThing: function(){`
			`this.bool = !this.bool;`
		`}`
	`}`
`})`
### v-if
>根据值判断元素显示/隐藏，操纵dom树对性能消耗较大
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
`})`
### v-for
> 


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
### v-mode

## Vue生命周期


[Vue官网学习](https://cn.vuejs.org/v2/guide/)
