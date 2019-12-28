# VUE学习笔记
## Vue引用方式&基础格式
**在html文件中引入Vue**
- 开发环境版本：包含帮助的命令行警告
	- `<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>` 
- 生产环境版本：优化了尺寸和速度
	- `<script src="https://cdn.jsdelivr.net/npm/vue"></script>`

**Vue模板语法格式**
html结构：
`<div id="name">`
	`{{ message }}`
`</div>`
JavaScript结构：
`var variable = new Vue({`
	`el: '#name',`
	`data: {`
		`message: 'Hello World!',`
		`number: ''123'`
	`},`
	`methods: {`
		`m: function(){},`
		`n: function(){}`
	`}`
`})`
**注意事项**
- 元素选择可以使用css选择器，id/class/tag等选择器均可
- message可以在选择元素及其所有后代元素范围内使用

## Vue基础模板语法格式
**v-text**
**v-html**
**v-on**
**v-show**
**v-if**
**v-bind**
**v-for**
html结构：
`<div id="list">`
`	<ul>`
`		<li v-for="(item, index) in arr">{{item}}</li>`
`	</ul>`
`</div>`
JavaScript结构：
`let app = new Vue({`
`	el: "#list",`
`	data: {`
`		arr: [1, 2, 3, 4, 5],`
`		obj: [{name:"one"}, {name:"two"}]`
`	}`
`})`
**v-mode**l

## Vue生命周期


[Vue官网学习](https://cn.vuejs.org/v2/guide/)
