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
注意
- v-if与JavaScript中的if结构类似，else-if和else可以省略
- data可以是JS数据/表达式/vue实例data对象中的变量

#### v-show
>根据条件判断成立与否，决定显示/隐藏html元素
>使用v-show性能消耗较小，相当于为元素设置display:none

```html
<div v-show="data"></div>
```
注意
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
	<li v-for="(value, key, i) in obj">{{ value }}</li>
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
注意
- data可以是JS数据/表达式/vue实例data对象中的变量
- v-text之后，插值表达式不会生效

#### v-html
>相当于为元素动态设置innerHTML属性值

```html
<div v-html="data"></div>
```
注意
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
注意
- `v-bind:class`简写形式为`:class`
- 以对象形式绑定值时，根据exp判断值是否会绑定在属性上

#### v-model
>双向绑定：将值绑定到html表单元素上，同时值会随着表单元素的更新而更新

**input/textarea/select表单元素的双向绑定**

```html
<input type="text" v-model="data" />
```
注意
- 使用v-model时，表单元素的初始值需要在vue实例的data对象中设置
- 如果设置了value/checked/selected属性值，则这些属性值会被忽略
- v-model之后，插值表达式不会生效

**radio（单选框）数据的双向绑定**

```html
<div>
	<label><input type="radio" v-model="data" value="1">1</label>
	<label><input type="radio" v-model="data" value="2">1</label>
	<label><input type="radio" v-model="data" value="3">1</label>
</div>
```
注意
- 同一个v-model会对应多个不同的元素，其name属性也会被忽略
- v-model的值即当前选中的单选框的value属性值

#### v-on
>为html元素绑定DOM事件

```html
<button v-on:click="method"></button>
<button v-on:keyup.enter="method"></button>
```
注意
- `v-on:click`简写形式为`@click`
- method可以是一段JS代码/vue实例methods对象的方法
- v-on可以为元素绑定多个DOM事件，并可以添加修饰符

### 事件修饰符

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
//使用Vue.set()方法的别名
app.$set(app.b,'age',20)  
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
        //computed主要用来封装复杂的逻辑
		computed: {
			sum(){return this.nums.reduce((a, b) => a + b, 0);}
		}
	})
</script>
```
注意
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
                handler: "add",
                //回调将会在侦听开始之后被立即调用
                immediate: true,
                //深度监听，回调会在任何被侦听的对象的 property 改变时被调用，不论其被嵌套多深
                deep: true
            },
			//对象定义：创建新的方法
			d: {
                handler: function(val, oldVal){...code},
                immediate: true,
                deep: true
            },
            //数组定义：传入回调数组并逐一调用
            e: [
                "add",
                function(val, oldVal){...code},
                {
                    handler: function(val, oldVal){...code},
                    ...code
                }
            ]
            //侦听对象的某个属性时，需要使用引号包裹
            "f.g": {...code}
		}
	})
</script>
```
注意
- watch适合处理异步操作
- watch可以传入参数，保存旧值

### filters（过滤器）
>filters处理数据的一种快捷方式。filters不能使用this来访问data的变量/methods的方法，是纯函数，通过传递参数的形式处理并输出数据。

```html
<div id="app">{{ name | upperCase | add}}</div>
<script>
    //全局中定义的过滤器，需要在Vue实例创建之前定义
    Vue.filter("lowerCase", function(str){
        return str.toLowerCase()
    })
    //在vue实例的filters属性中定义过滤器，仅该实例可以使用
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
注意
- 可以叠加多个filters，要注意使用顺序
- filters可以使代码重复少/易读/维护性更好
- filters只可以在插值表达式和v-bind中使用

### directives(指令)
>directives用来创建自定义指令，内部包含一系列钩子函数。即可以用于全局自定义指令，也可用于局部自定义指令的创建。
>
```html
<div id="app">
    <input type="text" v-foucs />
    <input type="text" v-hide />
</div>
<script>
    //定义一个全局的v-focus新定义指令
    //全局自定义指令需要在vue实例之前创建
    Vue.directive("focus", {
		inserted: function(el){
            el.foucus();
        }
    })
    //简写形式：在 bind 和 update 时触发相同行为，而不关心其它的钩子
    Vue.directive('color-swatch', function (el, binding) {
      el.style.backgroundColor = binding.value
    })
    //定义一个局部自定义指令
    new Vue({
        el: "#app",
        data: {},
        directives: {
            hide: {
                inserted: function(el){ el.style.display = "none"}
            }
        }
    })

</script>
```
钩子函数
- bind：只调用一次，指令第一次绑定到元素时调用
- inserted：被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中）
- update： 所在组件的 VNode 更新时调用，**但是可能发生在其子 VNode 更新之前**。指令的值可能发生了改变，也可能没有。
- componentUpdated： 指令所在组件的 VNode **及其子 VNode** 全部更新后调用 
- unbind： 只调用一次，指令与元素解绑时调用

钩子函数参数
- `el`：指令所绑定的元素，可以用来直接操作 DOM。
- `binding`：一个对象，包含以下 property：
  - `name`：指令名，不包括 `v-` 前缀。
  - `value`：指令的绑定值，例如：`v-my-directive="1 + 1"` 中，绑定值为 `2`。
  - `oldValue`：指令绑定的前一个值，仅在 `update` 和 `componentUpdated` 钩子中可用。无论值是否改变都可用。
  - `expression`：字符串形式的指令表达式。例如 `v-my-directive="1 + 1"` 中，表达式为 `"1 + 1"`。
  - `arg`：传给指令的参数，可选。例如 `v-my-directive:foo` 中，参数为 `"foo"`。
  - `modifiers`：一个包含修饰符的对象。例如：`v-my-directive.foo.bar` 中，修饰符对象为 `{ foo: true, bar: true }`。
- `vnode`：Vue 编译生成的虚拟节点。移步 [VNode API](https://cn.vuejs.org/v2/api/#VNode-接口) 来了解更多详情。
- `oldVnode`：上一个虚拟节点，仅在 `update` 和 `componentUpdated` 钩子中可用。

### config(全局配置)
> `Vue.config` 是一个对象，包含 Vue 的全局配置

```html
<div id="app">
    <input type="button" />
</div>
<script>
    //取消 Vue 所有的日志与警告
    Vue.config.silent = true;
    //给 v-on 自定义键位别名
    Vue.config.keyCodes.f2 = 113;
    Vue.config.keyCodes = {
      v: 86,
      f1: 112,
      // camelCase 不可用
      mediaPlayPause: 179,
      // 取而代之的是 kebab-case 且用双引号括起来
      "media-play-pause": 179,
      up: [38, 87]
    }
</script>
```
[全局配置API](https://cn.vuejs.org/v2/api/#%E5%85%A8%E5%B1%80%E9%85%8D%E7%BD%AE)

### 特殊的attribute
- key：用在 Vue 的虚拟 DOM 算法
```html
<ul>
  <li v-for="item in items" :key="item.id">...</li>
</ul>
```
- ref：用来给元素或子组件注册引用信息
```html
<div id="app">
    <p ref="p">hello</p>
	<child-component ref="child"></child-component>
</div>
<script>
    new Vue({
        el: "#app",
        data: {},
        methods: {
            alert(){
                this.$refs.p.style.color = "red";
                alert("hello");
            }
        }
    })
</script>
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
vue实例初始化，但是data和methods等数据尚未初始化

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
data和methods等数据已被初始化，被添加到DOM之前触发
如果要调用methods中的方法，最早只能在created中操作

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
在元素被创建之后触发（但不一定已添加到DOM，可以使用nextTick来保证）

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
- 全局组件：在全局注册，需Vue实例创建之前定义，所有Vue实例中均可使用
```html
///在html中使用组件
<div id="app">
    <element1></element1>
</div>
<script>
//在全局中注册组件是可复用
Vue.component("element1", {
    template: "<p>正整数是：1， 2， 3，4……</p>"
});
//
new Vue({el: "#app"})
</script>
```
- 局部组件：在Vue实例内部注册的，局部组件仅该Vue实例中可以使用
```html
<div id="app">
    <component-a></component-a>
    <component-b></component-b>
</div>
<script>
//先通过一个普通的JavaScript对象定义组件
let component1 = {...code},
    component2 = {...code};
new Vue({
    el: "#app",
    data: {},
    methods: {},
    //然后将对象传递给components的属性
    components: {
        "component-a": component1,
        "component-b": component2
    }
})
//注意局部注册的组件在其子组件中不可用
//如果希望component2在component1中可用
//需要将代码改写为以下形式
let component1 = {...code},
    component2 = {
        compontents:{"compontent-b": component2},
        ...code
	  };
new Vue({
    el: "#app",
    data: {},
    methods: {},
    components: {
        "component-a": component1
    }
})
</script>
```
- `Vue.extend()`创建全局组件
```html
<div id="mount-point"></div>
<script>
    // 创建构造器
    let Profile = Vue.extend({
      template: '<p>{{firstName}} {{lastName}} aka {{alias}}</p>',
      //data必须是函数，避免数据共享问题
      data: function () {
        return {
          firstName: 'Walter',
          lastName: 'White',
          alias: 'Heisenberg'
        }
      }
    })
    // 创建 Profile 实例，并挂载到一个元素上。
    new Profile().$mount('#mount-point')
</script>
```

### 组件属性
- **template**
template是一串html字符串，相当于innerHTML，其中只能包含一个根元素
```html
<div is="app">
    <element-ui></element-ui>
</div>
<script>
Vue.component("element-ui", {
	template: "<p>{{ message }} <span>{{ person.name }}</span></p>"
})
new Vue({el: "#app"});
```

- **data**
data必须是一个函数，返回一个包含了组件中定义数据的对象，避免组件共享数据的问题
```html
<div is="app">
    <element-ui></element-ui>
</div>
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
new Vue({el: "#app"});
</script>
```

- **methods**
methods定义了一系列方法，可在组件中使用
```html
<div id="app">
    <element-ui></element-ui>
</div>
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
new Vue({el: "#app"});
</script>
```

- **computed**
computed计算并缓存组件中的数据，只有当数据改变时才重新计算
```html
<div id="app">
    <element-ui></element-ui>
</div>
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
new Vue({el: "#app"});
</script>
```

### slot(插槽)
>slot插槽是用于向组件传递内容的，分为默认插槽/具名插槽/做由于插槽。
>vue2.6中使用v-slot指令（简写为#），取代了原先的slot和slot-scope属性。

- v-slot示例，v-slot只能在`template`上使用
```html
<div id="app">
    <!--默认插槽-->
	<element1>
        <!--默认default可以省略-->
		<template v-slot:default>
            <p>This is a element1</p>
        </template>
	</element1>
    <!--具名插槽:拥有名称的slot，允许一个组件拥有多个插槽-->
    <element2>
        <!--简写为#header-->
		 <template v-slot:header>
            <p>This is elemen2</p>
        </template>
	</element2>
     <!--作用域插槽:在作用域上绑定属性来将子组件的信息传给父组件使用 -->  
     <element3>
        <!--简写为#footer="slotPros"-->
		 <template v-slot:footer="user">
            <p>This is {{user.name}}</p>
        </template>
	</element3>  
</div>
<script>
Vue.component('element1', {
	template: '<p>This is : <slot>默认内容</slot></p>'
})
Vue.component('element2', {
	template: '<p>This is : <slot name="header">默认内容</slot></p>'
})
Vue.component('element3', {
	template: '<p>This is : <slot name="footer" v-bind:user="user">默认内容</slot></p>',
    data: () => ({
        user: {name: "Bob"}
    })
})
new Vue({
	el: "#app"
})
</script>
```
- slot示例（已废弃）
```html
<div id="app">
    <!--默认插槽-->
    <element1>
        <p>This is a element1</p>
    </element1>
    <!--使用template标签包裹-->
	<element1>
		<template>
            <p>This is a element1</p>
        </template>
	</element1>
    <!--具名插槽-->
    <element2>
        <p slot="header">This is element2.</p>
	</element2>
    <!--使用template标签包裹-->
    <element2>
        <!--简写为#header-->
		 <template slot="header">
            <p>This is element2.</p>
        </template>
	</element2>
     <!--作用域插槽-->
    <element3>
        <p slot-scope="person">This is {{person.name}}</p>
	</element3>  
    <!--使用template标签包裹-->
     <element3>
        <!--简写为#footer="slotPros"-->
		 <template slot-scope="user">
            <p>This is {{user.name}}</p>
        </template>
	</element3>  
</div>
<script>
Vue.component('element1', {
	template: '<p>This is : <slot>默认内容</slot></p>'
})
Vue.component('element2', {
	template: '<p>This is : <slot name="header">默认内容</slot></p>'
})
Vue.component('element3', {
	template: '<p>This is : <slot name="footer" v-bind:user="user">默认内容</slot></p>',
    data: () => ({
        user: {name: "Bob"}
    })
})
new Vue({
	el: "#app"
})
</script>
```

### 组件传值
父组件向子组件传值（data/methods）
`props`和element-ui上属性向组件内部传递数据
`v-bind`和`props`可以通过element-ui向组件传递实例中的data
`v-on`和`this.$emit()`可以通过element-ui向组件传递实例中的methods
```html
<div id="app">
    //使用element-ui的属性上传递数据到组件内
    <element-ui color="red"></element-ui>
    //使用v-bind动态向组件内部传递数据
    <element-ui color="green" v-bind:title="msg"></element-ui>
    //使用v-on动态向组件内部传递方法
    <element-ui color="yellow" v-on:func="alert"></element-ui>
</div>
<script>
Vue.component("element-ui", {
	template: "<p v-bind:style='style1' @click='alert1'> {{ message }} <span>{{color}}</span> <span>{{ title }}</span> </p>",
	//props数组包含与element-ui属性同名的字符串
	props: ['color','title'],
	data(){
		return {
			message: "Hello",
			person: {name: "Tom", age: 20}
		}
	},
    //methods中的this.emit()方法传递方法
    methods: {
        alert1: function(){
            this.$emit('func')
        }
    }
	computed: {
		style1(){
			return {backgroundColor: this.color};
		}
	}
})
new Vue({
    el: "#app",
    data: {
        msg: "Hello World"
    },
    methods: {
        alert(){alert(this.msg)}
    }
});
</script>
```
props验证数据，为传入的数据指定类型，类型不符会抛出警告
```html
<div id="app">
    <element-ui price="123" unit="'$'"></element-ui>
</div>
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
new Vue({el: "#app"});
</script>
```
### vue-loader
>vue-loader是书写组件的另一种方式，在.vue文件中书写组件，并且可以定义样式style

- 一般组件的注册
```html
<script>
Vue.component('element1', {
    tempalate: '<p>This is a {{number}}</p>',
    props: {
        number: {
            type: number,
            required: true
        }
    },
    data: () => ({
        person: {...code}
    })
})
new Vue({
    el: "#app"
})
</script>
```
- vue-loader组件的注册
```vue
//在.vue文件名相当于组件名，即element1.vue
<tempalate>
    <p class="example">
        This is a {{number}}
    </p>
</tempalate>
<script>
export default{
    name: "element1",
	props: {
        number: {
            type: number,
            required: true
        }
    },
    data: () => ({
        person: {...code}
    })
};
</script>
<style>
.example{
	color: red;
}
</style>
```
```html
//将element1.vue文件导入应用并使用
<div id="app">
    <element1></element1>
</div>
<script>
import element1 from '这里是element1.vue文件的路径'

new Vue({
	el: "#app",
	compontents: {
		element1
	}
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
//对象中：属性/键为驼峰写法的样式属性，值为样式属性对应的值
<div id="app" v-bind:style="{color: 'red', fontWeight: 'bold'}"></div>
<div id="app" v-bind:style="style1"></div>
<div id="app" v-bind:style="style2"></div>
//数组语法v-bind绑定style改变元素样式
//数组中是一个个包含样式语法的对象
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
JS动画钩子
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

