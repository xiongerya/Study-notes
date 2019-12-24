# JSON笔记
> JSON(JavaScript Object Notation)
是一种轻量级的数据交换格式。它使得人们很容易的进行阅读和编写。同时也方便了机器进行解析和生成。

## JSON数据结构
- 简单值：字符串/数值/布尔值/null，JSON不支持undefined，JSON的字符串必须使用双引号
- 复杂值：
	- 数组：有序值的列表，值(value)可以是简单值/复杂值，
	- 对象：无序的键值对，属性名(key)必须是加上双引号的字符串，值可以是简单值/复杂值(array/object/function/date等)，最后一个属性后不能有逗号。

## JSON解析和序列化
**eval()函数解析——字符串转换为javascript值**
>eval() 函数会将传入的字符串当做 JavaScript 代码进行执行，因此eval()函数可以解析/解释并返回javascript值（简单值/对象/数组）。

语法：eval(string)

**JSON.parse()——JSON字符串转换为javascript值**

>JSON.parse() 方法用来解析JSON字符串，构造由字符串描述的JavaScript值或对象。提供可选的 reviver 函数用以在返回之前对所得到的对象执行变换(操作)。

语法：JSON.parse(text\[, reviver]) [MDN解析](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse)

**JSON.stringify()——javascript值转换为JSON字符串**

>JSON.stringify() 方法将一个 JavaScript 值（对象或者数组）转换为一个 JSON 字符串，如果指定了 replacer 是一个函数，则可以选择性地替换值，或者如果指定了 replacer 是一个数组，则可选择性地仅包含数组指定的属性。

语法：JSON.stringify(value\[, replacer\[, space]]) [MDN解析](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify)

## Ajax和JSON
Ajax请求json格式数据
`var request = new XMLHttpRequest();`
`request.open('get', url);`
`request.responseType = 'json';`

`request.onload = function(){`
	`var json = request.response;`
`};`

`request.send();`
此时的Ajax传输的json数据为原始数据结构，并非json字符串


