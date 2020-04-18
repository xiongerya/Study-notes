# Vue.js学习笔记
## Vue.js引用
**在html文件中引入Vue**
- 开发环境版本：包含帮助的命令行警告
```html
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```
- 生产环境版本：优化了尺寸和速度
```html
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

## Vue.js基础
### Template(模板)
#### 插值表达式
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
```html
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
### Directive(指令)
#### v-if
>根据条件判断成立与否，决定插入/移除html元素
>使用v-if性能消耗较大，每次插入/移除元素时必须生成元素内部DOM树

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

```html
<div v-show="data"></div>
```
**注意**

- data可以是JS数据/表达式/vue实例data对象中的变量
- 元素频繁切换显示/隐藏状态时，推荐使用v-show

#### v-for
>通过循环一个数组/对象，将其渲染（循环输出）到html页面上

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

```html
<div v-text="data"></div>
```
**注意**
- data可以是JS数据/表达式/vue实例data对象中的变量
- v-text之后，插值表达式不会生效

#### v-html
>相当于为元素动态设置innerHTML属性值

```html
<div v-html="data"></div>
```
**注意**
- data可以是JS数据/表达式/vue实例data对象中的变量
- v-html之后，插值表达式不会生效

#### v-bind
>属性绑定：将值绑定到html元素的属性上
>单向数据绑定，值的变化会反映在绑定的元素属性上

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
>Vue 会在初始化实例时对属性执行 getter/setter 转化，存在于data对象中的所有属性都是响应式的。
>当把一个普通的 JavaScript 对象传入 Vue 实例作为 data 选项，Vue 将遍历此对象所有的属性，并使用 Object.defineProperty() 把这些属性全部转为 getter/setter。
>Vue 不允许为已经创建的实例动态添加根级别的响应式属性。但是，可以使用 Vue.set(object, propertyName, value) 方法向嵌套对象添加响应式属性。
>Vue的响应式能力除了在使用插值表达式将数据输出到页面有效外，在data对象中的属性作为指令值时也同样有效。

**对于对象**
Vue 无法检测 property 的添加或移除。
```js
let app = new Vue({
	el: "#app",
	data: {
		a: 1,
		b: {name: "Bob"}
	}
});
//data中的属性是响应式的，可以动态改变
app.a = 2;   
app.b.name = "Tom";
//vue实例中无法添加根属性
app.c = 2;   //
//vue实例中的对象添加的属性是非响应式的
app.b.age = 25;   //b.age === undenfined
//使用Vue.set()方法添加响应式属性
Vue.set(app.b, "age", 20)
app.$set(app.b,'age',20)   //Vue.set()方法的别名
//Object.assign()为对象添加多个新属性
app.b = Object.assign(
	{},
	app.b,
	{color: "white", pet: "dog"}
)
```
**对于数组**
Vue不能检测数组index任意项和length的变动
```js
let app = new Vue({
	el: "#app",
	data: {
		a: [1, 2]
	}
});
//使用Vue.set()动态移除/添加数组元素
Vue.set(app.a, 0, 20)
app.$set(app.a, 3,3)  
//使用splice(index, num, items...)动态移除/添加数组元素
app.a.splice(0, 1);   //删除元素
app.a.splice(0, 1, "0");   //替换元素
app.a.splice(0, 0, 1);   //插入元素
app.a.splice(1);   //缩短数组
```

### computed(计算)
>computed是基于它们的响应式依赖进行缓存的；介于data和methods之间，像访问data中数据一样访问它，但以methods中的方式定义，但不能传入参数。

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
>侦听器可以监听data/computed中属性的变化，需要监听的data/computed属性只需要在watch中设置的与其同名的属性即可，可以传入参数，可以使用对象/函数/数组等方式定义。

```html
<div id="app">{{ name }}</div>
<script>
	new Vue({
		el: "#app",
		data: {
			a: 1,
            b: 2,
            c: 3,
            d: {},
            e: {},
        	f: {g: ""}
		},
        methods: {
            add(){...code}
        },
		watch: {
     		//直接引用methods中的方法
            //val和oldVal将作为参数隐式按序传入
        	a: "add",
			//方法定义：可以引入参数
			b(val, oldVal){...code},
			//对象定义：引用methods中的方法
			c: {
                //val和oldVal将作为参数隐式按序传入
                hander: "add",
                //回调将会在侦听开始之后被立即调用
                immediate: true,
                //深度监听，回调会在任何被侦听的对象的 property 改变时被调用，不论其被嵌套多深
                deep: true
            },
			//对象定义：创建新的方法
			d: {
                hander: function(val, oldVal){...code},
                immediate: true,
                deep: true
            },
            //数组定义：传入回调数组并逐一调用
            e: [
                "add",
                function(val, oldVal){...code},
                {
                    hander: function(val, oldVal){...code},
                    ...code
                }
            ]
            //侦听对象的某个属性时，需要使用引号包裹
            "f.g": {...code}
		}
	})
</script>
```
**注意**
- watch适合处理异步操作
- watch可以传入参数，保存旧值

### filters（过滤器）
>filters处理数据的一种快捷方式。filters不能使用this来访问data的变量/methods的方法，时纯函数，通过传递参数的形式处理并输出数据。

```html
<div id="app">{{ name | upperCase | add}}</div>
<script>
    //全局中定义的过滤器
    Vue.filters("lowerCase", function(str){
        return str.toLowerCase()
    })
    //在vue实例中定义过滤器
	new Vue({
		el: "#app",
		data: {
			name: "Tom"
		},
		filters: {
			upperCase(str){ return str.toUpperCase()}，
			add(str){ return "Hello, " + str},
		}
	})
</script>
```
**注意**
- 可以叠加多个filters，要注意使用顺序
- filters可以使代码重复少/易读/维护性更好
- filters只可以在插值表达式/v-bind中使用

### 特殊的attribute
- key：用在 Vue 的虚拟 DOM 算法
```html
<ul>
  <li v-for="item in items" :key="item.id">...</li>
</ul>
```
- ref：用来给元素或子组件注册引用信息
```html
<!-- `vm.$refs.p` will be the DOM node -->
<p ref="p">hello</p>

<!-- `vm.$refs.child` will be the child component instance -->
<child-component ref="child"></child-component>
```
- is：用于动态组件且基于 DOM 内模板的限制来工作
```html
<!-- 当 `currentView` 改变时，组件也跟着改变 -->
<component v-bind:is="currentView"></component>

<!-- 这样做是有必要的，因为 `<my-row>` 放在一个 -->
<!-- `<table>` 内可能无效且被放置到外面 -->
<table>
  <tr is="my-row"></tr>
</table>
```

## 生命周期钩子
![lifecycle](./images/vue/lifecycle.png)
### create
**beforeCreate**
在vue实例初始化之前被触发

```js
new Vue({
	el: "app",
	data: {},
	methods: {},
	beforeCreate(){
        ...code
    }
})
```
**created**
在vue实例初始化之后，被添加到DOM之前触发

```js
new Vue({
	el: "app",
	data: {},
	methods: {},
	created(){
        ...code
    }
})
```
### mount
**beforeMount**
在元素已经准备好被添加到DOM，但是还未被添加的时候触发

```js
new Vue({
	el: "app",
	data: {},
	methods: {},
	beforeMount(){
        ...code
    }
})
```
**mounted**
在元素被创建之后触发（但不一定已经添加到DOM，可以使用nextTick来保证）

```js
new Vue({
	el: "app",
	data: {},
	methods: {},
	mounted(){
      this.$nextTick(function () {
      	...code
      })
    }
})
```
### update
**beforeUpdate**
在数据更新将对DOM做出更改时触发

```js
new Vue({
	el: "app",
	data: {},
	methods: {},
	beforeUpdate(){
    	...code
    }
})
```
**updated**
DOM的更改已经完成之后触发

```js
new Vue({
	el: "app",
	data: {},
	methods: {},
	updated(){
      this.$nextTick(function () {
		...code
      })
    }
})
```
### destroy
**beforeDestroy**
在组件即将被销毁并且从DOM上移除时触发
```js
new Vue({
	el: "app",
	data: {},
	methods: {},
	beforeDestory(){
        ...code
    }
})
```
**destoryed**
在组件被销毁之后触发
```js
new Vue({
	el: "app",
	data: {},
	methods: {},
	destoryed(){
        ...code
    }
})
```
## component(组件)
### 组件注册
```html
///在html中使用组件
<element-ui></element-ui>
<div id="app">
    <component-a></component-a>
    <component-b></component-b>
</div>
<script>
//在全局中注册组件
Vue.component("element-ui", {
    //template相当于一段innerHtml字符串
    template: "<p>正整数是：1， 2， 3，4……</p>"
});
//在vue实例中注册组件
//先通过一个普通的JavaScript对象定义组件
let component1 = {...code},
    component2 = {...code};
new Vue({
    el: "#app",
    data: {},
    methods: {},
    //然后将对象传递给component的属性
    components: {
        "component-a": component1,
        "component-b": component2
    }
})
//注意局部注册的组件在其子组件中不可用
//如果希望component2在component1中可用
//需要将代码改写为以下形式
//let component1 = {...code},
//    component2 = {
//        compontents:{"compontent-b": component2},
//        ...code
//	  };
//new Vue({
//    el: "#app",
//    data: {},
//    methods: {},
//    components: {
//        "component-a": component1,
//        ...code
//    }
//})
</script>
```
### 组件属性
- **data**
data返回一个包含了组件中定义数据的对象

```html
<element-ui></element-ui>
<script>
Vue.component("element-ui", {
	template: "<p>{{ message }} <span>{{ person.name }}</span></p>",
	data(){
		return {
			message: "Hello",
			person: {name: "Tom", age: 20}
		}
	}
})
</script>
```

- **methods**
methods定义了一系列方法，可在组件中使用
```html
<element-ui></element-ui>
<script>
Vue.component("element-ui", {
	template: "<p v-on:click='say'>{{ message }}</p>",
	data(){
		return {
			message: "Hello",
			person: {name: "Tom", age: 20}
		}
	},
	methods: {
		say(){
			alert("Hello" + this.person.name);
		}
	}
})
</script>
```

- **computed**
computed计算并缓存组件中的数据，只有当数据改变时才重新计算

```html
<element-ui></element-ui>
<script>
Vue.component("element-ui", {
	template: "<p>奇数的个数为：{{ odd.length }}</p>",
	data(){
		return {
			numbers: [1, 3, 4, 23, 3, 6]
		}
	},
	computed: {
		odd(){
			return this.numbers.filter(x => x % 2 === 1);
		}
	}
})
</script>
```

- **props**
props传递数据，由element-ui向组件内部传递数据
```html
//使用element-ui的属性上传递数据到组件内
<element-ui color="red"></element-ui>
//使用v-bind动态向组件内部传递数据
<element-ui v-bind:title="red"></element-ui>
<script>
Vue.component("element-ui", {
	template: "<p v-bind:style='style1'>{{ message }} <span>{{ title }}</span></p>",
	//props数组包含与element-ui属性同名的字符串
	props: ['color', 'title'],
	data(){
		return {
			message: "Hello",
			person: {name: "Tom", age: 20}
		}
	},
	computed: {
		style1(){
			return {backgroundColor: this.color};
		}
	}
})
</script>
```
props验证数据，为传入的数据指定类型，类型不符会抛出警告
```html
<element-ui price="123" unit="'$'"></element-ui>
<script>
Vue.component("element-ui", {
    template: "<p>{{ price }}</p>",
    //props为传入数据指定而类型
    props: {
        price: Number,
        unit: String
	}
	//props还可以为传入数据指定默认值/函数等
	//props: {
    //    price: {
    //    	type: Number,
    //    	required: true,
    //    	valid(value){
    //    		return value >= 0;
    //    	}
    //    },
    //    unit: {
    //    	type: String,
    //    	default: "$"
    //    }
	//}
})
</script>
```

## Vue.js样式
### 绑定class
```html
//数组语法为class添加/删除类名改变样式
<div id="app" v-bind:class="[class1, class2]"></div>
<div id="app" v-bind:class="[class1, {class2: flag}]"></div>
//对象语法为class添加/删除类名改变样式
<div id="app" v-bind:class="{class1: true, class2: flag}"></div>
<div id="app" v-bind:class="classes"></div>
<script>
new Vue({
    el: "#app",
    data: {
        flag: false,
        class1: "fade",
        classes:{
            "fade": true,
            "red-text": this.flag
        }
    },
    computed: {
        class2(){
            return "red-text"
        }
    }
})
</script>
```
### 绑定style
```html
//对象语法v-bind绑定style改变元素样式
<div id="app" v-bind:style="{color: 'red', fontWeight: 'bold'}"></div>
<div id="app" v-bind:style="style1"></div>
<div id="app" v-bind:style="style2"></div>
//数组语法v-bind绑定style改变元素样式
<div id="app" v-bind:style="[style1, style2]"></div>
//多重值绑定，设置浏览器最终支持的值
<div id="app" v-bind:style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
<script>
new Vue({
    el: "#app",
    data: {
        flag: false,
        color: "yellow",
        style1: {
            fontSize: "30px",
            backgroundColor: "green"
        }
    },
    computed: {
        style2(){
            return {color: this.color}
        }
    }
})
</script>
```
## 过渡和动画
### css过渡&动画
```html
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
- `{name}-leave-to`： 定义离开过渡的结束状态。在离开过渡被触发之后下一帧生效 (与此同时 v-leave 被删除)，在过渡/动画完成之后移除。
### JavaScript动画
```html
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
**JS动画钩子**html
- `beforeEnter`：动画开始前被触发，设置合适的初始值
- `enter`：动画开始时被触发，运行动画，并使用done回调表明动画已完成
- `afterEnter`：动画执行完成时被触发
- `enterCancelled`：动画被取消时触发
- `beforeLeave`：离开动画开始前被触发
- `leave`：离开动画开始时被触发，运行离开动画
- `afterLeave`：离开动画执行完成时被触发
- `leaveCancelled`：离开动画被取消时触发

## render & JSX
### render函数
```html

```


[Vue官网学习](https://cn.vuejs.org/v2/guide/)

