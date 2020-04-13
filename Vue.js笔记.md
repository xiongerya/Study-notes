# Vue.js学习笔记
## Vue.js引用
**在html文件中引入Vue**

- 开发环境版本：包含帮助的命令行警告
	- `<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>` 
- 生产环境版本：优化了尺寸和速度
	- `<script src="https://cdn.jsdelivr.net/npm/vue"></script>`

## Vue.js基础
### Template（模板）
#### 插值表达式
**基础结构**

```html
<div id="app">
	<p>{{ value }}</p>
	<p>{{ "This is a" + value }}</p>
	<p>This is a {{ value }}</p>
</div>
<script>
	new Vue({
		el: "#app",
		data: {value: "123"}
	})
</script>
```
#### 数据和方法
```vue
<script>
	//创建一个vue实例，并赋值给变量app
	let app = new Vue({
		//el表示选择的DOM元素，使用css选择器进行选择
		//实例中定义的变量和方法在DOM元素及其后代元素中均可使用
		el: "#app",
		//data对象定义变变量/数据
		//数据可以是任意JS数据类型
		data: {value: 123},
		//methods对象定义方法，以下两种方式均可定义方法
		//方法中使用this访问实例内部的变量/数据
		methods: {
			add() {...code},
			sub: function(){...code}
		}
	})
	//获取vue实例中的数据/方法
	console.log(app.value, app.add)
</script>
```
### Directive（指令）
#### v-if
>根据条件判断成立与否，决定插入/移除html元素
>使用v-if性能消耗较大，每次插入/移除元素时必须生成元素内部DOM树

**基本结构**
```html
<div v-if="data"></div>
<div v-else-if="data"></div>
<div v-else="data"></div>
```
**注意**
- v-if与JavaScript中的if结构类似，else-if和else可以省略
- data可以是JS数据/表达式/vue实例data对象中的变量

#### v-show
>根据条件判断成立与否，决定显示/隐藏html元素
>使用v-show性能消耗较小，相当于为元素设置display:none

**基本结构**
```html
<div v-show="data"></div>
```
**注意**
- data可以是JS数据/表达式/vue实例data对象中的变量
- 元素频繁切换显示/隐藏状态时，推荐使用v-show

#### v-for
>通过循环一个数组/对象，将其渲染（循环输出）到html页面上

**基本结构**
- v-for循环一个数组，i（数组索引）可省略
```html
<ul>
	<li v-for="(item, i) in arr">{{ item }}</li>
</ul>
```
- v-for循环一个对象，key（对象属性）可省略
```html
<ul>
	<li v-for="(value, key) in obj">{{ value }}</li>
</ul>
```
- v-for实现简单的计数器，n从1开始计数
```html
<ul>
	<li v-for="n in 10">{{ item }}</li>
</ul>
```
#### v-text
>相当于为元素动态设置textContent属性值

**基本结构**
```html
<div v-text="data"></div>
```
**注意**

- data可以是JS数据/表达式/vue实例data对象中的变量
- v-text之后，插值表达式不会生效

#### v-html
>相当于为元素动态设置innerHTML属性值

**基本结构**
```html
<div v-html="data"></div>
```
**注意**
- data可以是JS数据/表达式/vue实例data对象中的变量
- v-html之后，插值表达式不会生效

#### v-bind
>属性绑定：将值绑定到html元素的属性上
>单向数据绑定，值的变化会反映在绑定的元素属性上

**基本结构**
```html
<div v-bind:class="data"></div>
//以对象形式将值绑定在属性上`
<div v-bind:class="{data: exp}"></div>
```
**注意**

- `v-bind:class`简写形式为`:class`
- 以对象形式绑定值时，根据exp判断值是否会绑定在属性上

#### v-model
>双向绑定：将值绑定到html表单元素上，同时值会随着表单元素的更新而更新

**基本结构**
- input/textarea/select表单元素的双向绑定
```html
<input type="text" v-model="data" />
```
**注意**
	- 使用v-model时，表单元素的初始值需要在vue实例的data对象中设置
	- 如果设置了value/checked/selected属性值，则这些属性值会被忽略
	- v-model之后，插值表达式不会生效
- radio（单选框）数据的双向绑定
```html
<div>
	<label><input type="radio" v-model="data" value="1">1</label>
	<label><input type="radio" v-model="data" value="2">1</label>
	<label><input type="radio" v-model="data" value="3">1</label>
</div>
```
**注意**
	- 同一个v-model会对应多个不同的元素，其name属性也会被忽略
	- v-model的值即当前选中的单选框的value属性值

#### v-on
>为html元素绑定DOM事件

**基础结构**
```html
<button v-on:click="method"></button>
<button v-on:keyup.enter="method"></button>
```
**注意**
- `v-on:click`简写形式为`@click`
- method可以是一段JS代码/vue实例methods对象的方法
- v-on可以为元素绑定多个DOM事件，并可以添加修饰符

**修饰符**
- 事件修饰符：`.stop`，`.prevent`，`.capture`，`.self`，`.once`，`.passive`
- 按键修饰符：`.enter`，`.tab`，`.delete`，`.esc`，`.up`，`.down`，`.left`，`.right`，按键码等
- 系统修饰符：`.ctrl`，`.alt`，`.shift`，`.meta`，`.exact`，`.left`，`.middle`，`.right`
### 响应式数据
### computed（计算）
>computed是基于它们的响应式依赖进行缓存的；介于data和methods之间，像访问data中数据一样访问它，但以methods中的方式定义，但不能传入参数。

**基础结构**
```html
<div id="app">{{ sum }}</div>
<script>
	new Vue({
		el: "#app",
		data: {
			nums: [1, 2, 3]}
		},
		computed: {
			sum(){return this.nums.reduce((a, b) => a + b, 0);}
		}
	})
</script>
```
**注意**
- methods的方法多次调用时，每次调用都会执行一次
- computed的值多次调用时，代码只执行一次，每次调用使用缓存的值
- computed的依赖（如data中的数据）发生变化时，代码再次执行

### watch（侦听器）
>侦听器可以监听data/computed中属性的变化，需要监听的data/computed属性只需要在watch中设置的与其同名的属性即可，且可以传入参数

**基础结构**
```vue
<div id="app">{{ name }}</div>
<script>
	new Vue({
		el: "#app",
		data: {
			name: "Tom",
			obj: {age: 18}
		},
		watch: {
			//val等于当前的this.name
			nums(val, oldVal){...code},
			//监听对象的某个属性
			"obj.age"(){...code},
			//监听整个对象，称作深度监听
			obj(){
				...code
				deep: true
			}
		}
	})
</script>
```
**注意**

- watch适合处理异步操作
- watch可以传入参数，保存旧值
- watch监听对象的某个属性，需要引号包裹
- `deep:true`开启对整个对象的深度监听

### filters（过滤器）
>filters处理数据的一种快捷方式。filters不能使用this来访问data的变量/methods的方法，时纯函数，通过传递参数的形式处理并输出数据。

**基础结构**
```vue
<div id="app">{{ name | upperCase | add}}</div>
<script>
	new Vue({
		el: "#app",
		data: {
			name: "Tom"
		},
		//局部定义的过滤器
		filters: {
			upperCase(str){ return str.toUpperCase()}，
			add(str){ return "Hello, " + str},
		}
	})
	//全局定义的过滤器
    Vue.filters("lowerCase", function(str){
        return str.toLowerCase()
    })
</script>
```
**注意**
- 可以叠加多个filters，使用顺序很重要
- filters可以使代码重复少/易读/维护性更好
- filters只可以在插值表达式/v-bind中使用

### ref访问元素
## 过渡和动画
### css过渡&动画
**基础结构**
```vue
//transition组件包裹html元素
<transition name="fade">
	<div v-if="data">message</div>
</transition>
//transition组件添加样式
.fade-enter-active, .fade-leave-active{
	transition: opacity .5s;
}
.fade-enter, .fade-leave-to{
	opacity: 0;
}
```
**工作原理**
- Vue获取transition组件的name属性值
- 使用name属性值在过渡的各个节点为包含的元素添加class
- 当元素添加/删除时，分别应用enter和leave过渡
![transition](./images/vue/transition.png)
**过渡类名**
- `{name}-enter`：定义进入过渡的开始状态。在元素被插入之前生效，在元素被插入之后的下一帧移除。
- `{name}-enter-active`：定义进入过渡生效时的状态。在整个进入过渡的阶段中应用，在元素被插入之前生效，在过渡/动画完成之后移除。这个类可以被用来定义进入过渡的过程时间，延迟和曲线函数。
- `{name}-enter-to`：定义进入过渡的结束状态。在元素被插入之后下一帧生效 (与此同时 v-enter 被移除)，在过渡/动画完成之后移除。
- `{name}-leave`：定义离开过渡的开始状态。在离开过渡被触发时立刻生效，下一帧被移除。
- `{name}-leave-active`：定义离开过渡生效时的状态。在整个离开过渡的阶段中应用，在离开过渡被触发时立刻生效，在过渡/动画完成之后移除。这个类可以被用来定义离开过渡的过程时间，延迟和曲线函数。
- `{name}-eave-to`： 定义离开过渡的结束状态。在离开过渡被触发之后下一帧生效 (与此同时 v-leave 被删除)，在过渡/动画完成之后移除。
### JavaScript动画
**基础结构**
```
<transition
    v-on:before-enter="beforeEnter"
    v-on:enter="enter"
    v-on:after-enter="afterEnter"
    v-on:enter-cancelled="enterCancelled"

    v-on:before-leave="beforeLeave"
    v-on:leave="leave"
    v-on:after-leave="afterLeave"
    v-on:leave-cancelled="leaveCancelled"
>
	<div v-if="data">message</div>
</transition>
```
**JS动画钩子**
- `beforeEnter`：动画开始前被触发，设置合适的初始值
- `enter`：动画开始时被触发，运行动画，并使用done回调表明动画已完成
- `afterEnter`：动画执行完成时被触发
- `enterCancelled`：动画被取消时触发
- `beforeLeave`：离开动画开始前被触发
- `leave`：离开动画开始时被触发，运行离开动画
- `afterLeave`：离开动画执行完成时被触发
- `leaveCancelled`：离开动画被取消时触发

## 生命周期钩子
![lifecycle](./images/vue/lifecycle.png)
### beforeCreate和created
### beforeMoount和mounted
### beforeUpdate和updated
### beforeDestroy和destroyed
## Vue.js组件
## Vue.js样式
[Vue官网学习](https://cn.vuejs.org/v2/guide/)