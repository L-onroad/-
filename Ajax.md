>上网的本质目的：通过互联网的形式来获取和消费资源

服务器
>上网过程中，负责存放和对外提供资源的电脑，叫做服务器

客户端
>上网过程中，负责获取和消费资源的电脑，叫做客户端

# URL 地址
>URL（全称是UniformResourceLocator）中文叫统一资源定位符，用于标识互联网上每个资源的唯一存放位置。浏览器只有通过URL地址，才能正确定位资源的存放位置，从而成功访问到对应的资源

## 组成部分
* 客户端与服务器之间的**通信协议**    http
* 存有该资源的**服务器名称**    www.baidu.com
* 资源在服务器上**具体的存放位置**    后面的内容即为存放地址

https://www.baidu.com/s?wd=URL&rsv_spt=1&rsv_iqid=0xdea2bb5b00005b08&issp=1&f=8&rsv_bp=1&rsv_idx=2&ie=utf-8&tn=baiduhome_pg&rsv_enter=1&rsv_dl=ib&rsv_sug3=5&rsv_sug1=4&rsv_sug7=100&rsv_sug2=0&rsv_btype=i&inputT=1422&rsv_sug4=6086

# 客户端与服务器的通信过程
客户端与服务器之间的通信过程，分为 请求 – 处理 – 响应 三个步骤。

网页中的每一个资源，都是通过 请求 – 处理 – 响应 的方式从服务器获取回来的

# 资源

## 获取资源
如果要在网页中请求服务器上的数据资源，则需要用到 XMLHttpRequest 对象。
XMLHttpRequest（简称 xhr）是浏览器提供的 js 成员，通过它，可以请求服务器上的数据资源。

最简单的用法 var xhrObj = new XMLHttpRequest()

## 资源的请求方式
客户端请求服务器时，请求的方式有很多种，最常见的两种请求方式分别为 get 和 post 请求。

* get 请求通常用于获取服务端资源（向服务器要资源）
      例如：根据 URL 地址，从服务器获取 HTML 文件、css 文件、js文件、图片文件、数据资源等
* post 请求通常用于向服务器提交数据（往服务器发送资源）
      例如：登录时向服务器提交的登录信息、注册时向服务器提交的注册信息、添加用户时向服务器提交的用户信息等各种数据提交操作

# Ajax
>Ajax能让我们轻松实现网页与服务器之间的数据交互。

Ajax 的全称是 Asynchronous Javascript And XML（异步 JavaScript 和 XML）。
通俗的理解：在网页中利用 **XMLHttpRequest 对象和服务器**进行数据交互的方式，就是Ajax。

>Fetch/XHR 中监听 Ajax 请求

## jQuery 中的 Ajax
 * $.get()    专门用来发起 get 请求，从而将服务器上的资源请求到客户端来进行使用
	 * `$.get(url, [data], [callback])`
	 * url 要请求的资源地址
	 * data 要携带的参数，对象形式
	 * 请求成功时的回调函数
 * $.post()    专门用来发起 post 请求，从而向服务器提交数据
	 * `$.post(url, [data], [callback])`
	 * url 提交数据的地址
	 * data 要提交的数据，对象形式
	 * 数据提交成功时的回调函数
 * $.ajax()
```
$.ajax({
   type: '', // 请求的方式，例如 GET 或 POST
   url: '',  // 请求的 URL 地址
   data: { },// 这次请求要携带的数据
   success: function(res) { } // 请求成功之后的回调函数
})
```

res 响应回的数据

# 接口
>使用 Ajax 请求数据时，被请求的 URL 地址，就叫做数据接口（简称接口）。同时，每个接口必须有请求方式

## postman

**测试 GET 接口**
步骤：
* 选择请求的方式
* 填写请求的URL地址
* 填写请求的参数
* 点击 Send 按钮发起 GET 请求
* 查看服务器响应的结果

**测试 POST 接口**
步骤：
* 选择请求的方式
* 填写请求的URL地址
* **选择 Body 面板并勾选数据格式**
* 填写要发送到服务器的数据
* 点击 Send 按钮发起 POST 请求
* 查看服务器响应的结果

## 接口文档
>接口文档，即就是接口的说明文档，它是我们调用接口的依据。
>好的接口文档包含了对接口URL，参数以及输出内容的说明，参照接口文档就能方便的知道接口的作用，以及接口如何进行调用。

一个合格的接口文档一般包括以下六项内容
 * 接口名称：用来标识各个接口的简单说明，如登录接口，获取图书列表接口等。
 * 接口URL：接口的调用地址。
 * 调用方式：接口的调用方式，如 GET 或 POST。
 * 参数格式：接口需要传递的参数，每个参数必须包含参数名称、参数类型、是否必选、参数说明这4项内容。
 * 响应格式：接口的返回值的详细描述，一般包含数据名称、数据类型、说明3项内容。
 * 返回示例（可选）：通过对象的形式，例举服务器返回数据的结构。

# form 表单

## form 表单的基本使用
>表单在网页中主要负责数据采集功能。HTML中的`<form>`标签，就是用于采集用户输入的信息，并通过`<form>`标签的提交操作，把采集到的信息提交到服务器端进行处理

表单由三个基本部分组成：
* 表单标签
* 表单域
* 表单按钮

## form 标签的属性
`<form>`标签用来采集数据，`<form>`标签的属性则是用来规定如何把采集到的数据发送到服务器。

### action
* action 属性用来规定当提交表单时，向何处发送表单数据。
* action 属性的值应该是后端提供的一个 **URL 地址**，这个 URL 地址专门负责接收表单提交过来的数据。
* 当 `<form> `表单在未指定 action 属性值的情况下，action 的默认值为当前页面的 URL 地址。
>注意：当提交表单后，页面会立即跳转到 action 属性指定的 URL 地址

### target
>target 属性用来规定在何处打开 action URL。
>它的可选值有5个，默认情况下，target 的值是 \_self，表示在相同的框架中打开 action URL。

* \_blank    在新窗口中打开。
* \_self    默认。在相同的框架中打开。
* \_parent    在父框架集中打开。（很少用）
* \_top    在整个窗口中打开。（很少用）
* framename    在指定的框架中打开。（很少用）

### method
* method 属性用来规定以**何种方式**把表单数据提交到 action URL。
* 它的可选值有两个，分别是 get 和 post。
* 默认情况下，method 的值为 get，表示通过URL地址的形式，把表单数据提交到 action URL。

注意：
* get 方式适合用来提交少量的、简单的数据。
* post 方式适合用来提交**大量的、复杂的、或包含文件上传**的数据。
* 在实际开发中，`<form>`表单的 post 提交方式用的最多，很少用 get。例如登录、注册、添加数据等表单操作，都需要使用 post 方式来提交表单。

### enctype
>enctype 属性用来规定在发送表单数据之前**如何对数据进行编码**。
>它的可选值有三个，默认情况下，enctype 的值为 application/x-www-form-urlencoded，表示在发送前编码所有的字符。

* application/x-www-form-urlencoded    在发送前编码所有字符（默认）
* multipart/form-data    不对字符编码。在使用包含文件上传控件的表单时，必须使用该值。
* text/plain空格转换为 “+” 加号，但不对特殊字符编码。（很少用）

注意：
* 在涉及到文件上传的操作时，必须将 enctype 的值设置为 multipart/form-data
* 如果表单的提交不涉及到文件上传操作，则直接将 enctype 的值设置为 application/x-www-form-urlencoded 即可

## 表单的同步提交及缺点
>通过点击 submit 按钮，触发表单提交的操作，从而使页面跳转到 action URL 的行为，叫做表单的同步提交。

缺点
*  页面会发生跳转
* 页面之前的状态和数据会丢失

解决方案
**表单只负责采集数据，Ajax 负责将数据提交到服务器。**

## 提交表单数据
submit 事件
event.preventDefault() 
阻止表单的默认行为

```
$('#form1').submit(function(e) {
   // 阻止表单的提交和页面的跳转
   e.preventDefault()
})

$('#form1').on('submit', function(e) {
   // 阻止表单的提交和页面的跳转
   e.preventDefault()
})
```

## serialize()函数
为了简化表单中数据的获取操作，jQuery 提供了 serialize() 函数
`$(selector).serialize()`
可以一次性获取到表单中的所有的数据。

注意：在使用 serialize() 函数快速获取表单数据时，必须为每个表单元素添加 name 属性
```
    <form action="/login" id="f1">
        <input type="text" name="username" />
        <input type="password" name="password" />
        <!-- <input type="checkbox" name="remember_me" checked /> -->
        <button type="submit">提交</button>
    </form>

    <script>
        $(function(){
            $('#f1').submit(function(e){
                e.preventDefault();
                var data = $(this).serialize();
                console.log(data);
            })
        })
    </script>
```

# 模板引擎
[[面向对象、ES6#Ajax 正则内容（模板引擎）]]
>模板引擎可以根据程序员指定的模板结构和数据，自动生成一个完整的HTML页面

* 减少了字符串的拼接操作
* 使代码结构更清晰
* 使代码更易于阅读与维护

## art-template模板引擎
>art-template 是一个简约、超快的模板引擎。中文官网首页为 http://aui.github.io/art-template/zh-cn/index.html

art-template的使用步骤
* 导入 art-template
* 定义数据
* 定义模板
* 调用 template 函数
* 渲染HTML结构

```
  <!-- 1. 导入模板引擎 -->
  <!-- 在 window 全局，多一个函数，叫做 template('模板的Id', 需要渲染的数据对象) -->
  <script src="./lib/template-web.js"></script>
  <script src="./lib/jquery.js"></script>
		-----------------------------------
  <div id="container"></div>

  <!-- 3. 定义模板 -->
  <!-- 3.1 模板的 HTML 结构，必须定义到 script 中 -->
  <script type="text/html" id="tpl-user">
    <h1>{{name}}    ------    {{age}}</h1>
    {{@ test}}

    <div>
      {{if flag === 0}}
      flag的值是0
      {{else if flag === 1}}
      flag的值是1
      {{/if}}
    </div>

    <ul>
      {{each hobby}}
      <li>索引是:{{$index}}，循环项是:{{$value}}</li>
      {{/each}}
    </ul>

    <h3>{{regTime | dateFormat}}</h3>
  </script>

  <script>
    // 定义处理时间的过滤器
    template.defaults.imports.dateFormat = function (date) {
      var y = date.getFullYear()
      var m = date.getMonth() + 1
      var d = date.getDate()

      return y + '-' + m + '-' + d
    }


    // 2. 定义需要渲染的数据
    var data = { name: 'zs', age: 20, test: '<h3>测试原文输出</h3>', flag: 1, hobby: ['吃饭', '睡觉', '写代码'], regTime: new Date() }

    // 4. 调用 template 函数
    var htmlStr = template('tpl-user', data)
    console.log(htmlStr)
    // 5. 渲染HTML结构
    $('#container').html(htmlStr)
  </script>
```
## 标准语法
art-template 提供了 {{ }} 这种语法格式，在 {{ }} 内可以进行变量输出，或循环数组等操作，这种 {{ }} 语法在 art-template 中被称为标准语法。

>在 {{ }} 语法中，可以进行变量的输出、对象属性的输出、三元表达式输出、逻辑或输出、加减乘除等表达式输出

### 原文输出
`{{@ value }}`
如果要输出的 value 值中，包含了 HTML 标签结构，则需要使用原文输出语法，才能保证 HTML 标签被正常渲染。

### 条件输出
`{{if value}} 按需输出的内容 {{/if}}`
`{{if v1}} 按需输出的内容 {{else if v2}} 按需输出的内容 {{/if}}`
如果要实现条件输出，则可以在 {{ }} 中使用 if … else if … /if 的方式，进行按需输出。

### 循环输出
```
{{each arr}}
    {{$index}} {{$value}}
{{/each}}
```
如果要实现循环输出，则可以在 {{ }} 内，通过 each 语法循环数组，当前循环的索引使用 $index 进行访问，当前的循环项使用 $value 进行访问。

### 过滤器
>过滤器的本质，就是一个 function 处理函数

`{{value | filterName}}`
过滤器语法类似管道操作符，它的上一个输出作为下一个输入
`template.defaults.imports.filterName = function(value){/*return处理的结果*/}`

# Ajax 加强
## XMLHttpRequest 的基本使用
>XMLHttpRequest（简称 xhr）是浏览器提供的 Javascript 对象，通过它，可以请求服务器上的数据资源。之前所学的 jQuery 中的 Ajax 函数，就是**基于 xhr 对象**封装出来的

### 使用xhr发起GET请求
步骤：
* 创建 xhr 对象
* 调用 xhr.open() 函数
* 调用 xhr.send() 函数
* 监听 xhr.onreadystatechange 事件
```
        //创建 XHR 对象
        let xhr = new XMLHttpRequest();
        //调用 open 函数
        xhr.open('get', 'http://www.liulongbin.top:3006/api/getbooks');
        //调用 send 函数
        xhr.send();
        //监听 xhr.onreadystatechange 事件
        xhr.onreadystatechange = function () {
            //判断条件是固定写法
            if (xhr.readyState === 4 && xhr.status === 200) {
                //获取服务器响应的数据
                console.log(xhr.responseText);
            }
        }
```

### xhr对象的readyState属性
>XMLHttpRequest 对象的 readyState 属性，用来表示当前 Ajax 请求所处的状态。

每个 Ajax 请求必然处于以下状态中的一个：
值    状态        描述
0    UNSENT    XMLHttpRequest 对象已被创建，但尚未调用 open方法。

1    OPENED    open() 方法已经被调用。

2    HEADERS_RECEIVED    send() 方法已经被调用，响应头也已经被接收。

3    LOADING    数据接收中，此时 response 属性中已经包含部分数据。

4    DONE    Ajax 请求完成，这意味着数据传输已经彻底完成或失败。

### 使用xhr发起带参数的GET请求
>在 URL 地址后面拼接的**参数**，叫做查询字符串

? 后面拼接的字符串即查询字符串`xhr.open('GET', 'http://www.liulongbin.top:3006/api/getbooks?id=1')`

### 查询字符串
>查询字符串（URL 参数）是指在 URL 的末尾加上用于向服务器发送信息的字符串（变量）

格式：
将英文的 `?` 放在URL 的末尾，然后再加上 `参数＝值` ，想加上多个参数的话，使用 `&` 符号进行分隔。以这个形式，可以将想要发送给服务器的数据添加到 URL 中
`http://www.liulongbin.top:3006/api/getbooks?id=1&bookname=西游记`

>无论使用 $.ajax()，还是使用 $.get()，又或者直接使用 xhr 对象发起 GET 请求，当需要携带参数的时候，本质上，都是直接将参数以查询字符串的形式，**追加**到 URL 地址的后面，发送到服务器的

### URL编码与解码
>URL 地址中，只允许出现英文相关的字母、标点符号、数字，因此，在 URL 地址中不允许出现中文字符
>如果 URL 中需要包含中文这样的字符，则必须对中文字符进行编码（转义）

URL编码的原则：使用安全的字符（没有特殊用途或者特殊意义的可打印字符）去表示那些不安全的字符。
URL编码原则的通俗理解：使用英文字符去表示非英文字符

encodeURI('需编码的字符串')  编码
decodeURI('需解码的字符串')  解码

更多关于 URL 编码的知识，参考如下博客：
https://blog.csdn.net/Lxd_0111/article/details/78028889

### 使用xhr发起POST请求
步骤：
* 创建 xhr 对象
* 调用 xhr.open() 函数
* 设置 Content-Type 属性（固定写法）
* 调用 xhr.send() 函数，同时指定要发送的数据
* 监听 xhr.onreadystatechange 事件
```
        //创建 xhr 对象
        let xhr = new XMLHttpRequest();
        //调用 open 函数
        xhr.open('post', 'http://www.liulongbin.top:3006/api/addbook');

        //设置 Content-Type 属性（固定写法）
        xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
        //调用 send 函数 同时提交数据（查询字符串形式）
        xhr.send('bookname=水浒传&author=施耐庵&publisher=天津图书出版社');

        //监听事件
        xhr.onreadystatechange = function(){
            if(xhr.readyState===4&&xhr.status===200){
                console.log(xhr.responseText);
            }
        }
```

## 数据交换格式
>数据交换格式，就是服务器端与客户端之间进行数据传输与交换的格式。
>前端领域，经常提及的两种数据交换格式分别是 XML 和 JSON。其中 XML 用的非常少，重点要学习的数据交换格式是 JSON。

### XML
>XML 的英文全称是 EXtensible Markup Language，即可扩展标记语言。因此，XML 和 HTML 类似，也是一种标记语言（但两者之间没有任何的关系）

* HTML 被设计用来**描述**网页上的内容，是**网页内容的载体**
* XML 被设计用来**传输和存储数据**，是**数据的载体**

```XML
<note>
  <to>ls</to>
  <from>zs</from>
  <heading>通知</heading>
  <body>晚上开会</body>
</note>
```

* XML 格式臃肿，和数据无关的代码多，体积大，传输效率低
* 在 Javascript 中解析 XML 比较麻烦

### JSON
>SON 的英文全称是 JavaScript Object Notation，即“JavaScript 对象表示法”。简单来讲，JSON 就是 **Javascript 对象和数组的字符串表示法**，它使用文本表示一个 JS 对象或数组的信息，因此，JSON 的本质是**字符串**。

作用：JSON 是一种轻量级的文本数据交换格式，在作用上类似于 XML，专门用于存储和传输数据，但是 JSON 比 XML 更小、更快、更易解析。
现状：JSON 是在 2001 年开始被推广和使用的数据格式，到现今为止，JSON 已经成为了主流的数据交换格式。

### JSON的两种结构
JSON 就是用字符串来表示 Javascript 的对象和数组。所以，JSON 中包含对象和数组两种结构，通过这两种结构的相互嵌套，可以表示各种复杂的数据结构。

* 对象结构：对象结构在 JSON 中表示为 `{ }` 括起来的内容。数据结构为 `{ key: value, key: value, … }` 的键值对结构。
> 其中，key 必须是**使用英文的双引号包裹**的字符串，value 的数据类型可以是数字、字符串、布尔值、null、数组、对象6种类型。（value 若是字符串也需要双引号包裹，包括数组里的字符串）

* 数组结构：数组结构在 JSON 中表示为` [ ] `括起来的内容。数据结构为` [ "java", "javascript", 30, true … ] `
>数组中数据的类型可以是数字、字符串、布尔值、null、数组、对象6种类型。（是个字符串就都要加双引号）

***JSON语法注意事项***
* 属性名必须使用双引号包裹
* 字符串类型的值必须使用双引号包裹
* JSON 中不允许使用单引号表示字符串
* JSON 中不能写注释
* JSON 的最外层必须是对象或数组格式
* 不能使用 undefined 或函数作为 JSON 的值
JSON 的作用：在计算机与网络之间存储和传输数据。
JSON 的本质：用字符串来表示 Javascript 对象数据或数组数据

###  JSON和JS对象的关系
JSON 是 JS 对象的字符串表示法，它使用文本表示一个 JS 对象的信息，本质是一个字符串
```
//这是一个对象
var obj = {a: 'Hello', b: 'World'}

//这是一个 JSON 字符串，本质是一个字符串
var json = '{"a": "Hello", "b": "World"}'
```

## 序列化和反序列化
* 把**数据对象转换为字符串**的过程，叫做**序列化**，例如：调用 JSON.stringify() 函数的操作，叫做 JSON 序列化。
* 把**字符串转换为数据对象**的过程，叫做**反序列化**，例如：调用 JSON.parse() 函数的操作，叫做 JSON 反序列化

## JSON
###  JSON和JS对象的互转
* 从 JSON 字符串转换为 JS 对象
` JSON.parse(json对象)`
返回一个 JS 对象

`var obj = JSON.parse('{"a": "Hello", "b": "World"}')`
//结果是 `{a: 'Hello', b: 'World'}`

* 现从 JS 对象转换为 JSON 字符串
`JSON.stringify() `

## 封装自己的 Ajax 函数
park1
```
<!-- 1. 导入自定义的ajax函数库 -->
<script src="./itheima.js"></script>

<script>
    // 2. 调用自定义的 itheima 函数，发起 Ajax 数据请求
    itheima({
        method: '请求类型',
        url: '请求地址',
        data: { /* 请求参数对象 */ },
        success: function(res) { // 成功的回调函数
            console.log(res)     // 打印数据
        }
    })
</script>
```
park2
```
/**
 * 处理 data 参数
 * @param {data} 需要发送到服务器的数据
 * @returns {string} 返回拼接好的查询字符串 name=zs&age=10
 */
function resolveData(data) {
  var arr = []
  for (var k in data) {
    arr.push(k + '=' + data[k])
  }
  return arr.join('&')
}
```
park3
```
//拼接请求的数据，监听请求事件并携带返回值
function itheima(options) {
  var xhr = new XMLHttpRequest()
  // 拼接查询字符串
  var qs = resolveData(options.data)

  // 监听请求状态改变的事件
  xhr.onreadystatechange = function() {
    if (xhr.readyState === 4 && xhr.status === 200) {
      var result = JSON.parse(xhr.responseText)
      //返回值
      options.success(result)
    }
  }
}
```
park4
```
  //判断请求类型
  if (options.method.toUpperCase() === 'GET') {
    // 发起 GET 请求
    xhr.open(options.method, options.url + '?' + qs)
    xhr.send()
  } else if (options.method.toUpperCase() === 'POST') {
    // 发起 POST 请求
    xhr.open(options.method, options.url)
    xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded')
    xhr.send(qs)
  }
```

## XMLHttpRequest Level2的新特性

>旧版XMLHttpRequest的缺点
>只支持文本数据的传输，无法用来读取和上传文件
>传送和接收数据时，没有进度信息，只能提示有没有完成

XMLHttpRequest Level2的新功能
* 可以设置 HTTP 请求的时限
* 可以使用 FormData 对象管理表单数据
* 可以上传文件
* 可以获得数据传输的进度信息

### 设置HTTP请求时限
>Ajax 操作很耗时，而且无法预知要花多少时间。如果网速很慢，用户可能要等很久。新版本的 XMLHttpRequest 对象，增加了 timeout 属性，可以设置 HTTP 请求的时限

`xhr.timeout = 3000`
过了这个时限，就自动停止HTTP请求。与之配套的还有一个 timeout 事件，用来指定回调函数
```
xhr.ontimeout = function(event){
     alert('请求超时！')
 }
```

```
        const xhr = new XMLHttpRequest();
        
        //设置超时时间
        xhr.timeout = 3000;
        //超时后的处理函数
        xhr.ontimeout = function(){
            console.log('请求超时了喔！');
        }

        xhr.open('get', 'http://www.liulongbin.top:3006/api/getbooks');
        xhr.send();
        xhr.onreadystatechange = function () {
            if (xhr.readyState === 4 && xhr.status === 200) {
                console.log(xhr.responseText);
            }
        };
```

### FormData对象管理表单数据
>Ajax 操作往往用来提交表单数据。为了方便表单处理，HTML5 新增了一个 FormData **对象**，可以模拟表单操作

```
      // 1. 新建 FormData 对象
      var fd = new FormData()
      // 2. 为 FormData 添加表单项
      fd.append('uname', 'zs')
      fd.append('upwd', '123456')
      // 3. 创建 XHR 对象
      var xhr = new XMLHttpRequest()
      // 4. 指定请求类型与URL地址
      xhr.open('POST', 'http://www.liulongbin.top:3006/api/formdata')
      // 5. 直接提交 FormData 对象，这与提交网页表单的效果，完全一样
      xhr.send(fd)
      xhr.onreadystatechange = function () {
            if (xhr.readyState === 4 && xhr.status === 200) {
                console.log(JSON.parse(xhr.responseText));
            }
        }
```

* FormData对象也可以用来获取网页表单的值
```
       // 获取表单元素
 var form = document.querySelector('#form1')
 // 监听表单元素的 submit 事件
 form.addEventListener('submit', function(e) {
    e.preventDefault()
     // 根据 form 表单创建 FormData 对象，会自动将表单数据填充到 FormData 对象中
     var fd = new FormData(form)  //不需要手动 append 了
     var xhr = new XMLHttpRequest()
     xhr.open('POST', 'http://www.liulongbin.top:3006/api/formdata')
     xhr.send(fd)
     xhr.onreadystatechange = function() {}
})
	...
```

### 上传文件
[[上传文件的信息]]

```
<body>
  <!-- 1. 文件选择框 -->
  <input type="file" id="file1" />
  <!-- 2. 上传文件的按钮 -->
  <button id="btnUpload">上传文件</button>
  <br />
  <!-- 3. img 标签，来显示上传成功以后的图片 -->
  <img src="" alt="" id="img" width="800" />

  <script>
    // 1. 获取到文件上传按钮
    var btnUpload = document.querySelector('#btnUpload')
    // 2. 为按钮绑定单击事件处理函数
    btnUpload.addEventListener('click', function () {
      // 3. 获取到用户选择的文件列表
      var files = document.querySelector('#file1').files
      if (files.length <= 0) {
        return alert('请选择要上传的文件！')
      }
      var fd = new FormData()
      // 将用户选择的文件，添加到 FormData 中
      fd.append('avatar', files[0])

      var xhr = new XMLHttpRequest()
      xhr.open('POST', 'http://www.liulongbin.top:3006/api/upload/avatar')
      xhr.send(fd)

      xhr.onreadystatechange = function () {
        if (xhr.readyState === 4 && xhr.status === 200) {
          var data = JSON.parse(xhr.responseText)
          if (data.status === 200) {
            // 上传成功
            document.querySelector('#img').src = 'http://www.liulongbin.top:3006' + data.url
          } else {
            // 上传失败
            console.log('图片上传失败！' + data.message)
          }
        }
      }
    })
  </script>
</body>
```

### 显示文件上传进度
>新版本的 XMLHttpRequest 对象中，可以通过监听 xhr.upload.onprogress 事件，来获取到文件的上传进度

```
 // 创建 XHR 对象
 var xhr = new XMLHttpRequest()
 // 监听 xhr.upload 的 onprogress 事件
 xhr.upload.onprogress = function(e) {
 
    // e.lengthComputable 是一个布尔值，表示当前上传的资源是否具有可计算的长度
    
    if (e.lengthComputable) {
        // e.loaded 已传输的字节
        // e.total  需传输的总字节
        var percentComplete = Math.ceil((e.loaded / e.total) * 100)
    }
 }
```
>控制台 Network 第二栏 No throttling


# jQuery 高级用法
...
...
Ajax 请求开始时，执行 ajaxStart 函数。可以在 ajaxStart 的 callback 中显示 loading 效果
```
 // 自 jQuery 版本 1.8 起，该方法只能被附加到文档
 $(document).ajaxStart(function() {
     $('#loading').show()
 })
```
>注意： $(document).ajaxStart() 函数会监听当前文档内所有的 Ajax 请求

Ajax 请求结束时，执行 ajaxStop 函数。可以在 ajaxStop 的 callback 中隐藏 loading 效果
```
 // 自 jQuery 版本 1.8 起，该方法只能被附加到文档
 $(document).ajaxStop(function() {
     $('#loading').hide()
 })
```

# axios
>Axios 是专注于网络数据请求的库。
>相比于原生的 XMLHttpRequest 对象，axios 简单易用。
>相比于 jQuery，axios 更加轻量化，**只专注于网络数据请求**

## 发起 get 请求
` axios.get('url', { params: { /*参数*/ } }).then(callback)`

```
            var url = 'http://www.liulongbin.top:3006/api/get';
            var obj = { name: 'zhao', age: 20 };
            axios.get(url, {params: obj}).then(function(res){
	        //请求的参数用 {params: } 包裹着
                var result = res.data;
                console.log(res);
                console.log(result);
            })
```
>注意：res 参数是 axios 里的属性，包含了许多信息，只有 res.data 才是返回的信息

## 发起 post 请求
```
            var url = 'http://www.liulongbin.top:3006/api/post';
            var obj = { address: '广州市', location: '白云区' };
            axios.post(url, obj).then(function(res){
                console.log(res.data);
            })
```

## 直接使用axios发起请求
```
 axios({
     method: '请求类型',
     url: '请求的URL地址',
     data: { /* POST数据 */ },
     params: { /* GET参数 */ }
 }) .then(callback)
```
>**get 提交的参数用 params 指定，post 用 data 指定**

# 跨域与JSONP

## 同源策略
>如果两个页面的**协议，域名和端口**都相同，则两个页面具有相同的源

>同源策略（英文全称 Same origin policy）是浏览器提供的一个安全功能

通俗的理解：浏览器规定，A 网站的 JavaScript，不允许和非同源的网站 C 之间，进行资源的交互，例如：
* 无法读取非同源网页的 Cookie、LocalStorage 和 IndexedDB
* 无法接触非同源网页的 DOM
* 无法向非同源地址发送 Ajax 请求

## 跨域
>同源指的是两个 URL 的协议、域名、端口一致，反之，则是跨域

>出现跨域的根本原因：浏览器的同源策略不允许非同源的 URL 之间进行资源的交互

### 实现跨域数据请求
实现跨域数据请求，最主要的两种解决方案，分别是 JSONP 和 CORS。
* JSONP：出现的早，兼容性好（兼容低版本IE）。是前端程序员为了解决跨域问题，被迫想出来的一种临时解决方案。缺点是**只支持 GET 请求**，不支持 POST 请求。
* CORS：出现的较晚，它是 W3C 标准，属于跨域 Ajax 请求的根本解决方案。支持 GET 和 POST 请求。缺点是不兼容某些低版本的浏览器。

## JSONP
>JSONP (JSON with Padding) 是 JSON 的一种“使用模式”，可用于解决主流浏览器的跨域数据访问的问题。

由于浏览器同源策略的限制，网页中无法通过 Ajax 请求非同源的接口数据。但是`<script> `标签不受浏览器同源策略的影响，可以通过 src 属性，请求非同源的 js 脚本。
因此，JSONP 的实现原理，就是通过` <script> `标签的 src 属性，请求跨域的数据接口，并通过函数调用的形式，接收跨域接口响应回来的数据。
```
    <script>
        function success(data) {
            console.log('JSON返回的数据是:');
            console.log(data);
        }
    </script>

    <script src="http://www.liulongbin.top:3006/api/jsonp?callback=success&name=ls&age=30"></script>
```

## jQuery 中的 JSONP 函数
```
 $.ajax({
    url: 'http://ajax.frontend.itheima.net:3006/api/jsonp?name=zs&age=20',
    // 如果要使用 $.ajax() 发起 JSONP 请求，必须指定 datatype 为 jsonp
    dataType: 'jsonp',
    success: function(res) {
       console.log(res)
    }
 })
```
>默认情况下，使用 jQuery 发起 JSONP 请求，会自动携带一个 callback=jQueryxxx 的参数，jQueryxxx 是随机生成的一个回调函数名称


# 淘宝搜索案例
...
...

## 防抖
>防抖策略（debounce）是当事件被触发后，延迟 n 秒后再执行回调，如果在这 n 秒内事件又被触发，则重新计时

### 输入框的防抖
```
 var timer = null                    // 1. 防抖动的 timer

 function debounceSearch(keywords) { // 2. 定义防抖的函数
    timer = setTimeout(function() {
    // 发起 JSONP 请求
    getSuggestList(keywords)
    }, 500)
 }

 $('#ipt').on('keyup', function() {  // 3. 在触发 keyup 事件时，立即清空 timer
    clearTimeout(timer)
    // ...省略其他代码
    debounceSearch(keywords)
 })
```

## 缓存搜索建议列表
* 定义全局缓存对象
`  // 缓存对象`
`var cacheObj = {}`
* 将搜索结果保存到缓存对象中
```javascript
 // 渲染建议列表
 function renderSuggestList(res) {
    // ...省略其他代码
   // 将搜索的结果，添加到缓存对象中
    var k = $('#ipt').val().trim()
    cacheObj[k] = res
 }
```

## 节流
降低单位时间内的触发频率

## 防抖和节流的区别
防抖：如果事件被频繁触发，防抖能保证只有最有一次触发生效！前面 N 多次的触发都会被忽略！
节流：如果事件被频繁触发，节流能够减少事件触发的频率，因此，节流是有选择性地执行一部分事件！


# HTTP 加强

## 通信
>通信，就是信息的传递和交换

通信三要素：
* 通信的**主体**
* 通信的**内容**
* 通信的**方式**

## 通信协议
>通信协议（Communication Protocol）是指通信的双方完成通信所必须遵守的规则和约定。

* 客户端与服务器之间要实现网页内容的传输，则通信的双方必须遵守网页内容的传输协议
* 网页内容又叫做超文本，因此网页内容的传输协议又叫做超文本传输协议（HyperText Transfer Protocol） ，简称 HTTP 协议。

## HTTP 协议
HTTP 协议即超文本传送协议 (HyperText Transfer Protocol) ，它规定了客户端与服务器之间进行网页内容传输时，所必须遵守的传输格式。

HTTP 协议采用了 请求/响应 的交互模型

## HTTP 请求消息
由于 HTTP 协议属于客户端浏览器和服务器之间的通信协议。因此，**客户端发起**的请求叫做 **HTTP 请求**，**客户端发送**到服务器的**消息**，叫做 **HTTP 请求消息**。

注意：HTTP 请求消息又叫做 HTTP **请求报文**。

### 组成部分
HTTP 请求消息由请求行（request line）、请求头部（ header ） 、空行 和 请求体 4 个部分组成
![[Pasted image 20220803134436.png]]

### 请求行
请求行由**请求方式、URL 和 HTTP 协议版本** 3 个部分组成，他们之间使用空格隔开

### 请求头部
请求头部用来**描述客户端的基本信息**，从而把客户端相关的信息**告知服务器**。比如：**User-Agent** 用来说明当前是什么类型的浏览器；**Content-Type** 用来描述发送到服务器的数据格式；**Accept** 用来描述客户端能够接收什么类型的返回内容；**Accept-Language** 用来描述客户端期望接收哪种人类语言的文本内容。

请求头部由多行 **键/值对** 组成，每行的键和值之间用英文的冒号分隔

头部字段        说明
* Host        要请求的服务器域名
* Connection        客户端与服务器的连接方式(close 或 keepalive)
* Content-Length        用来描述请求体的大小
* ***Accept***        客户端可识别的响应内容类型列表
* ***User-Agent***        产生请求的浏览器类型
* ***Content-Type***        客户端告诉服务器实际发送的数据类型
* Accept-Encoding        客户端可接收的内容压缩编码形式
* ***Accept-Language***        用户期望获得的自然语言的优先顺序

>关于更多请求头字段的描述，可以查看 MDN 官方文档：
>https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers

### 空行
最后一个请求头字段的后面是一个空行，通知服务器请求头部至此结束。
请求消息中的空行，用来分隔请求头部与请求体。

### 请求体
请求体中存放的，是要通过 **POST 方式**提交到服务器的数据。

**注意**：只有 POST 请求才有请求体，GET 请求没有请求体！

## HTTP响应消息
>响应消息就是服务器响应给客户端的消息内容，也叫作响应报文

### 组成部分
HTTP响应消息由**状态行、响应头部、空行 和 响应体** 4 个部分组成
![[Pasted image 20220803140651.png]]

### 状态行
状态行由 **HTTP 协议版本、状态码和状态码的描述文本** 3 个部分组成，他们之间使用空格隔开

### 响应头部
响应头部用来描述**服务器的基本信息**。响应头部由多行 **键/值对** 组成，每行的键和值之间用英文的冒号分隔
>关于更多响应头字段的描述，可以查看 MDN 官方文档：
>https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers

### 空行
在最后一个响应头部字段结束之后，会紧跟一个空行，用来通知客户端响应头部至此结束。
响应消息中的空行，用来分隔响应头部与响应体

### 响应体
响应体中存放的，是服务器响应给客户端的资源内容


## HTTP 请求方法
HTTP 请求方法，属于 HTTP 协议中的一部分，请求方法的作用是：用来**表明要对服务器上的资源执行的操作**。最常用的请求方法是 GET 和 POST

序号        方法        描述
* 1        ***GET***        (查询)发送请求来获得服务器上的资源，请求体中不会包含请求数据，请求数据放在协议头中。
* 2        ***POST***        (新增)向服务器提交资源（例如提交表单或上传文件）。数据被包含在请求体中提交给服务器。
* 3        ***PUT***        (修改)向服务器提交资源，并使用提交的新资源，替换掉服务器对应的旧资源。
* 4        ***DELETE***        (删除)请求服务器删除指定的资源。
* 5        HEAD        HEAD 方法请求一个与 GET 请求的响应相同的响应，但没有响应体。
* 6        OPTIONS        获取http服务器支持的http请求方法，允许客户端查看服务器的性能，比如ajax跨域时的预检等。
* 7        CONNECT        建立一个到由目标资源标识的服务器的隧道。
* 8        TRACE        沿着到目标资源的路径执行一个消息环回测试，主要用于测试或诊断。
* 9        PATCH        是对 PUT 方法的补充，用来对已知资源进行局部更新 。

## HTTP响应状态码
>HTTP 响应状态码（HTTP Status Code），也属于 HTTP 协议的一部分，用来标识响应的状态。

响应状态码会随着响应消息一起被发送至客户端浏览器，浏览器根据服务器返回的响应状态码，就能知道这次 HTTP 请求的结果是成功还是失败了。

### 组成
HTTP 状态码由三个十进制数字组成，第一个十进制数字定义了状态码的类型，后两个数字用来对状态码进行细分。

分类        分类描述
* 1**        信息，服务器收到请求，需要请求者继续执行操作（实际开发中很少遇到 1** 类型的状态码）
* 2**        **成功**，操作被成功接收并处理
* 3**        **重定向**，需要进一步的操作以完成请求
* 4**        **客户端错误**，请求包含语法错误或无法完成请求
* 5**        **服务器错误**，服务器在处理请求的过程中发生了错误

### 2** 成功相关的响应状态码
状态码        状态码英文名称        中文描述
200        OK        请求成功。一般用于 GET 与 POST 请求
201        Created        已创建。成功请求并创建了新的资源，通常用于 POST 或 PUT 请求

### 3** 重定向相关的响应状态码
状态码        状态码英文名称        中文描述
301        Moved Permanently        永久移动。请求的资源已被永久的移动到新URI，返回信息会包括新的URI，浏览器会自动定向到新URI。今后任何新的请求都应使用新的URI代替
302        Found        临时移动。与301类似。但资源只是临时被移动。客户端应继续使用原有URI
304        Not Modified        未修改。所请求的资源未修改，服务器返回此状态码时，不会返回任何资源（响应消息中不包含响应体）。客户端通常会缓存访问过的资源。

### 4** 客户端错误相关的响应状态码
状态码        状态码英文名称        中文描述
400        Bad Request        1、语义有误，当前请求无法被服务器理解。除非进行修改，否则客户端不应该重复提交这个请求。2、请求参数有误。
401        Unauthorized        当前请求需要用户验证。
403        Forbidden        服务器已经理解请求，但是拒绝执行它。
404        Not Found        服务器无法根据客户端的请求找到资源（网页）。
408        Request Timeout        请求超时。服务器等待客户端发送的请求时间过长，超时。

### 5** 服务端错误相关的响应状态码
状态码        状态码英文名称        中文描述
500        Internal Server Error        服务器内部错误，无法完成请求。
501        Not Implemented        服务器不支持该请求方法，无法完成请求。只有 GET 和 HEAD 请求方法是要求每个服务器必须支持的，其它请求方法在不支持的服务器上会返回501
503        Service Unavailable        由于超载或系统维护，服务器暂时的无法处理客户端的请求。

