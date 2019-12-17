# Ajax学习笔记
>Ajax（Asynchronous JavaScript and XML）是从服务器获取某些数据（例如 HTML, XML, JSON, 或纯文本) 来更新部分网页而不用加载整个页面的技术。
>这是通过使用诸如 XMLHttpRequest 之类的API或者 — 最近以来的 Fetch API 来实现. 这些技术允许网页直接处理对服务器上可用的特定资源的 HTTP 请求，并在显示之前根据需要对结果数据进行格式化。
>

**Ajax的优点**
- 页面更新速度更快，您不必等待页面刷新，这意味着该网站体验感觉更快，响应更快。
- 每次更新都会下载更少的数据，这意味着更少地浪费带宽。

## 基本的Ajax请求
### XMLHttpRequest
- 要开始创建XHR请求，需要使用 XMLHttpRequest() 的构造函数创建一个新的请求对象
	- `var request = new XMLHttpRequest();`
- 使用 open() 方法来指定用于从网络请求资源的 HTTP request method , 以及url
	- `request.open('get', url);`
- 接下来，设置我们期待的响应类型 —  这是由请求的 responseType 定义的(text/json/html等)
	- `request.responseType = 'text';`
- 从网络获取资源是一个 asynchronous "异步" 操作, 这意味着您必须等待该操作完成（例如，资源从网络返回），然后才能对该响应执行任何操作，否则会出错,将被抛出错误。 XHR允许你使用它的 onload 事件处理器来处理这个事件 — 当onload 事件触发时（当响应已经返回时）这个事件会被运行。 发生这种情况时， response 数据将在XHR请求对象的响应属性中可用。
	- `request.onload = function(){};`
- XHR请求设置完成，通过send（）完成
	- `request.send();`

### Feach API
将Feach代替为XHR方法对比：
**XHR实现Ajax**
`var request = new XMLHttpRequest();`
`request.open('get', url);`
`request.responseType = 'text';`
` `
`request.onload = function(code……){};`
` `
`request.send();`

**Fetch实现Ajax**
`fetch(url).then(function(response){
	response.text().then(function(text){
		code……
	});
});`

流程对比：
- 调用fetch()方法，将要获取的url传递给它。这相当于XHR中的request.open(),但不需要任何等效的send()方法。
- 然后then()方法连接到了fetch()末尾-这个方法是Promises的一部分，是一个用于执行异步操作的现代JavaScript特性。fetch()返回一个promise，它将解析从服务器发回的响应。我们使用then()来运行一些后续代码，这是我们在其内部定义的函数。这相当于XHR版本中的onload事件处理程序。
- 当fetch() promise 解析时，这个函数会自动将响应从服务器传递给参数。在函数内部，我们获取响应并运行其text()方法。这基本上将响应作为原始文本返回，这相当于在XHR版本中的`request.responseType = 'text'`。
- 你会看到 text() 也返回了一个 promise, 所以我们连接另外一个 then() 到它上面, 在其中我们定义了一个函数来接收 text() promise解析的生文本。

*传递给 then() 是一段不会立即执行的代码 — 而是当返回响应时代码会被运行。*

[MDN解析Ajax](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Client-side_web_APIs/Fetching_data)