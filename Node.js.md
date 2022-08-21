# 初识 Node.js
Node.js 是基于 Chrome V8引擎的 JavaScript 运行环境

>浏览器是 JavaScript 的前端运行环境
>Node.js 是 JavaScript 的后端运行环境
>Node.js 中无法调用 DOM 和 BOM 等浏览器内置 API 

打开终端，输入 node -v，按下回车，可以看到已安装的 Node.js 的版本号

在 Node.js 环境中执行 JavaScript 代码 ^about
[[终端知识]]
1. 打开终端 cmd、PowerShell
2. 输入  node 要执行的 JS 文件路径
	* 记得要切换存储的位置，切换不同的硬盘 直接写 如 C：
	* 然后输入 cd 空格 切换文件位置
	* **也可以在对应文件夹下面按住 shift + 右键 点击 在此处打开 PowerShell 窗口，直接定位到指定目录**
>PowerShell 比 cmd 出得晚些，功能比 cmd 强大些，但都可以满足需求

# fs 文件系统模块
>fs 模块是 Node.js 官方提供的、用来**操作文件**的模块。它提供了一系列的方法和属性，用来满足用户对文件的操作需求
>const fs = require('fs');  导入模块

## fs.readFile(' ... ') 方法
>读取指定文件中的内容

fs.readFile(path\[, options\], callback)
参数
1. 字符串，表示文件的路径
2. 可选参数，表示以什么编码格式来读取文件
3. 文件读取完成后，通过回调函数拿到的读取的结果 function(err, dataStr)
	* err 对象为 null，则文件读取成功

## fs.writeFile(' ... ')
>向指定文件中写入内容

参数
1. 表示文件的存放路径
2. 要写入的内容
3. 回调函数  如 function(err)
	* 写入成功，err 为 null
	* 失败则 err 的值是一个错误对象

## fs 路径动态拼接的问题
>使用 fs 模块（如上面读取写入方法）时，如果操作路径是以 ./ 或 ../ 开头的**相对路径**时，容易出现路径拼接问题
>因为代码运行时，会以执行 node 命令时**所处的目录**，动态拼接处被操作文件的完整路径

* 解决方案：直接提供完整的路径（绝对路径），不用相对路径
>移植性差，不利于维护

* 使用  \_\_dirname + '文件路径'
>  \_\_dirname  返回的是当前调用文件的路径
>  该方法可以较为方便的定位到准确的路径

# path 路径模块
>path 模块是 Node.js 官方提供的、用来处理**路径**的模块。它提供了一系列的方法和属性，用来满足用户对文件的操作需
>const path = require('path');  导入模块

> ../  表示上级目录  ./  表示当前目录  /  表示根目录

## path.join()  路径拼接
>把多个路径片段拼接为完整的路径字符串

eg.   path.join('/a', '/b')    path.join(\_\_dirname, './files/first.txt')

**以后涉及路径拼接都使用 path.join() 方法进行处理，不直接用 + 进行拼接**
> + 识别不了 ./ 等的字符

## path.basename()  获取路径中的文件名

参数
1. 文件的路径(必选参数)    eg. /a/b/c/index.html 取得 index.html
2. 文件的拓展名（可选）    path.basename('路径', '.html') = index
	* 使用了该参数则返回不带拓展名的文件名

## path.extname(path)  获取路径中的文件拓展名

# http 模块
>http 模块是 Node.js 官方提供的、用来创建 web 服务器的模块。通过 http 模块提供的 http.creatServer() 方法，就能方便的把一台普通的电脑，变成一台 Web 服务器，从而对外提供 Web 资源服务
>const http = require('http');

## IP 地址
[[客户端、服务器]]

## 域名和域名服务器
>IP 地址和域名（平时的网址）是一一对应的关系
>域名服务器（DNS）就是提供 IP 地址和域名之间的转换服务的服务器

>开发测试时期，127.0.0.1 对应的域名是 localhost，都表示自己的电脑

## 端口号
>跟门牌号一样
>每个 web 服务都对应一个唯一的端口号。客户端发送过来的网络请求，通过端口号，可以被准确地交给对应的 web 服务进行处理

注意：
1. 每个端口号不能同时被多个 web 服务占用
2. 实际应用中，URL 中的 80 端口（默认端口）可以被省略

## 创建最近本地 web 服务器
```
//1.导入 http 模块

const http = require('http');

//2.创建 web 服务器实例

const server = http.createServer();

//3.为服务器实例绑定 request 事件，监听客户端的请求

server.on('request', function(req, res) {

    console.log('Someone visit our web server');

})

//4.启动服务器

server.listen(80, function() {

    console.log('server running at http://127.0.0.1:80');

})
```
>参数 res req

**req 请求对象**
只要服务器接收到了客户端的请求，就会调用通过 server.on() 为服务器绑定的 request 事件处理函数

```
server.on('request', (req) => {

    const url = req.url;

    const method = req.method;

    const str = `Your request url is ${url}, and requert method is ${method}`;

    console.log(str);

})
```
>在事件处理函数中访问与客户端相关的数据或属性

**res 响应对象**
调用 res.end(内容) 方法，向客户端响应一些内容，并结束此次请求的处理过程

## 解决中文乱码问题
调用 res.end(内容) 方法，向客户端发送中文内容的时候，会出现乱码问题，需要手动设置内容的编码格式
>res.setHeader('Content-Type', 'text/html; charset=utf-8');

## 根据不同的 url 响应不同的 html 内容
[[根据不同的 url 响应不同的 html 内容]]

## 时钟 web 服务器案例
https://www.bilibili.com/video/BV1a34y167AZ?p=18

>优化资源的请求路径
>希望访问的原始 url 就是首页，可以手动在代码添加路径

---

# 模块化
>提高了代码的复用性
>提高了代码的可维护性
>可以实现按需加载

## 分类
* 内置模块（Node.js 官方提供，如 fs、path、http）
* 自定义模块（用户创建的每个 .js 文件）
* 第三方模块（使用前要先下载）

## 加载模块
>require()  方法 加载模块
>除了自定义模块需要加路径（可省 .js），其他两个直接输入模块名字符串
>使用 require() 方法加载其他模块时，会执行被加载模块中的代码

## 模块作用域
* 防止了全局变量污染的问题

## module 对象
>向外共享模块作用域中的成员
>每个 .js 自定义模块中都有一个 module 对象，它里面存储了和当前模块有关的信息

* module.exports 对象
	* 该对象将模块内的成员共享出去，供外界使用
	* 外界用 require() 方法导入自定义模块时，得到的就是 module.exports 所指向的对象，默认情况下为空对象
	* eg. module.exports.username = 'le';  挂载属性
		* 该对象写在被导入的模块下，导入该模块的模块就可以得到该对象

* exports 对象
	* 默认情况下，exports 和 module.exports 指向同一个对象。可以使用 exports 代替 module.exports
	* 使用 require() 模块时，**得到的永远是 module.exports 指向的对象**，即代表用 exports 重新指向*新对象*时，若有 module.exports 已经有指向的*对象*了，以 module.exports 指向的对象为准

>为了防止混乱，建议不要在同一个模块中同时使用 exports 和 module.exports

## 模块化规范
>CommonJS 规定了模块的特性和各模块之间如何相互依赖

---

# npm 与包
[[npm 与包]]
**在项目中安装指定名称的包**
* npm install 包的完整名称  or  npm i 完整包名称

>第三方模块又叫做包，为同一个概念

>http://www.npmjs.com  全球最大的包共享平台（检索包）
>http://registry.npmjs.org/  下载包

>Node Package Manage  npm 包管理工具（随着 Node.js 安装包被一起安装到电脑上）  npm -v  在终端查看版本号



## moment 格式化时间的包
>npm i moment    在终端中下载包
>自动安装最新版本，若要安装指定版本，通过 @ 符号指定具体的版本

>node_modules 文件夹用来存放所有已安装到项目中的包
>package-lock.json 配置文件用来记录 node_modules 目录下的每一个包的下载信息
>注意：不要手动修改文件中的任何代码，npm 包管理工具会自动维护它们

## 包管理配置文件
package.json
>记录了与项目有关的一些配置信息

>注意：今后在项目开发中，一定要把 node_modules 文件夹，添加到 .gitignore 忽略文件中（node_modules 文件过大，不方便团队成员之间共享项目源代码），package.json 可以记录项目安装过的包，方便共享项目的源代码

## 快速创建 package.json
npm init -y
>上述命令只能在英文的目录下成功运行，不要用中文命名，不能出现空格
>运行 npm install 安装命令安装包，npm 包管理工具会自动把包的名称和版本号记录到 package.json 中

>**json 对象中不能用单引号**

## dependencies 节点
>专门记录了用 npm install 命令安装了哪些包

## 一次性安装所有的包
npm install 或 npm i
>把 node_modules 忽略的时候，需要将所有用上了的包安装下来，使用上面的命令即可将包重新安装下来

## 卸载包
npm uninstall 命令卸载指定的包
>会把卸载的包自动从 package.json 的 dependencies 中移除掉

## devDependencies 节点
>如果某些包只在项目开发阶段会用到，在项目上线之后不会用到，则建议把这些包记录到 devDependencies 节点中

npm i 包名 -D
>npm install 包名 --save-dev

## 解决下包速度慢的问题
>国外网站下载（http://registry.npmjs.org/），海底光缆长，速度慢

淘宝 NPM 镜像服务器
>镜像（Mirroring）是一种文件存储形式，一个磁盘上的数据在另一个磁盘上存在一个完全相同的副本即为镜像

### 切换 npm 的下包镜像源
1. 查看当前的下包镜像源  npm config get registry
2. 将下包的镜像源切换为淘宝镜像源  npm config set registry=https://registry.npm.taobao.org/
3. 检查镜像源是否下载成功

### nrm 
>为了更方便的切换下包的镜像源，可以安装 nrm 小工具

1. 通过 npm 包管理器，将 nrm 安装为全局可用的工具    npm i nrm -g
2. 查看所有可用的镜像源    nrm ls
3. 将下包的镜像源切换为 taobao 镜像    nrm use taobao

## 包的分类
### 项目包
>安装到 node_modules 目录中的包都是项目包，分为两类
1. 开发依赖包    在 devDependencies 节点中
2. 核心依赖包    在 dependencies 节点中

### 全局包  -g
执行 npm install 命令时，如果提供了 -g 参数，则会把包安装为全局包
>会被安装到 C:\Users\用户目录\AppData\Roaming\npm\node_modules  
  目录下（隐藏目录）
* npm i 包名 -g    全局安装指定的包
* npm uninstall 包名 -g    卸载全局安装的包

>只有工具性质的包，才有全局安装的必要性，因为它们提供了好用的终端命令
>判断是否需要全局安装才能使用，可以参考官方说明

### i5ting_toc
>可以把 md 文档转为 html 页面
1. 将 i5ting_toc 安装为全局包    npm install -g i5ting_toc
2. 调用 i5ting_toc，轻松实现 md 转换 html 功能    
	* i5ting_toc -f 要转换的 md 文件路径 -o

## 规范的包结构
1. 包必须以单独的目录而存在
2. 包的顶级目录下必须包含 package.json 这个包管理配置文件
3. package.json 中**必须**包含 name、version、main 这三个属性，分别代表 包的名字、版本号、包的入口

## 开发属于自己的包
1. 新建一个文件夹作为包的根目录
2. 在文件夹中新建如下三个文件
	* package.json  包管理配置文件
	* index.js  包的入口文件
	* README.md  包的说明文档
3. 初始化 package.json（听说  npm init -y  可以一键生成）
4. 在 index.js 中定义方法
···
7. 将不同的功能进行模块化拆分
8. 将说明写入 README.md 说明文档

## 发布包
1. 注册在官网注册
2. 登录在终端登录
	* npm login 
3. 将终端切到根目录后，运行 npm publish 命令，即可将包发布到 npm 上（包名不能与已存在的包相同）
4. 运行 npm unpublish 包名 --force 命令，即可从 npm 删除已发布的包
	* 只能删除 72 小时以内发布的包
	* 在 24 小时以内不允许重复发布通过该命令删除的包

---

# 模块加载机制

## 优先从缓存中加载
* 模块在第一次加载之后会被缓存，多次调用 require() 不会导致模块的代码被多次执行
>三种模块都会优先从缓存中加载，从而提高模块的加载效率

## 内置模块的加载机制
* 内置模块加载优先级最高

## 自定义模块的加载机制
* 使用 require() 加载自定义模块时，必须以 ./ 或 ../ 开头的路径标识符，如果没有，将会被 node 当做内置模块或第三方模块进行加载
* 如果忽略了拓展名，会按一下顺序尝试加载
	* 按确切文件名进行加载
	* 补全 .js 进行加载
	* 补全 .json 进行加载
	* 补全 .node 进行加载
	* 加载失败，终端报错

## 第三方模块的加载机制
* 传递给 require() 的模块标识符不是一个内置模块，也没有 ./ 或 ../ 开头，则 Node.js 会从当前模块的父目录开始，尝试从 /node_modules 文件夹中加载第三方模块
* 如果没找到对应第三方模块，则会移动到再上一层父目录中（都是在 /node_modules 文件夹里）进行加载，直到文件系统的根目录

## 目录作为模块
* 目录作为模块先在目录下找 package.json 文件，并寻找 main 属性，作为 require() 加载入口
* 如果目录里没有  package.json 文件，或者 main 入口不存在或无法解析，则 Node.js 将会试图加载目录下的 index.js 文件
* 都失败则打印错误信息

---

# Express
*const app = express();*

>基于 Node.js 平台，快速、开放、极简的 Web 开发框架（类似 http 模块）
>能方便、快速地创建 Web 网站的服务器或 API 接口的服务器
>第三方包，基于内置的 http 模块进一步封装出来的
[[express 初使用]]

## 创建基本的 web 服务器
```
//导入express
const express = require('express');
//创建web服务器
const app = express();
//调用app.listen（端口号，启动成功后的回调函数），启动服务器
app.listen(80, () => {
    console.log('express server running at http://127.0.0.1');
})
```

## 监听 get|post 请求 | app.get()|app.post()
>参数1：客户端请求的 url 地址
>参数2：请求对应的处理函数  function(req, res) { ... }
>			 req 请求对象   res 响应对象    都包含了相关的属性和方法

### res.send()  请求函数内使用
>可以把处理好的内容，发送给客户端

### req.query  获取 URL 中携带的查询参数
>可以访问到客户端通过查询字符串的形式，发送到服务器的参数（默认为空）

eg.  URL 中末尾输入  ?name=gu&age=22
console.log(req.query);
返回  { name: 'gu', age: '22' }

### req.params 访问 URL 中，通过 ' :xx ' 匹配到的动态参数
>默认为空对象
>xx 作为参数名，URL 中最后接收到的内容作为属性值，存储在 req.params 对象中
>可以有多个动态参数，以 / 分割

```
app.get('/user/:id', (req, res) => {    // : 后面可以自定义属性名

    console.log(req.params);

    res.send(req.params);

})
```

## express.static()  托管静态资源
>app.use(express.static(文件目录))  app.use 函数中的 express.static() 方法
可以方便地创建一个静态资源服务器，可以将文件目录下的文件向外开放访问

>如果要托管多个静态资源目录，多次调用 app.use(express.static(文件目录)) 即可
>注意：按照添加顺序查找所需文件，找到了就不再向下找了

### 挂载路径前缀
>app.use('/前缀名', express.static(文件目录))
访问文件时需要在 URL 中加上前缀

eg. `http://127.0.0.1/前缀名/文件名.后缀`

## nodemon
[[nodemon]]

## Express 路由
>路由：映射关系
>Express 路由：客户端的请求与服务器处理函数之间的映射关系

由三部分组成：app.请求的类型('请求的 url 地址', 处理函数)
>上面讲过的例子 get | post 就是一个路由

### 匹配过程
1. 按照定义的**先后顺序**进行匹配
2. **请求类型**和**请求的 URL**同时匹配成功才会调用对应的处理函数（成功后不向后进行匹配了）

## 模块化路由  express.Router()
>为了方便对路由进行模块化的管理，推荐将路由抽离为单独的模块

### 步骤
1. 创建路由模块对应 .js 文件
2. 调用 **express.Router()** 函数创建路由对象
3. 向路由对象上挂载具体路由
4. 使用 **module.exports** 向外共享路由对象
5. 使用 **app.use()** 函数注册路由模块
```creat
var express = require('express');
//创建路由对象
var router = express.Router();
router.get('/list', function(req, res) {
    res.send('Get list');
})
router.post('/add', function(req, res) {
    res.send('Add new user');
})
//向外导出路由对象
module.exports = router;
```
>创建了模块化的路由

```use
const express = require('express');
const app = express();
//导入路由模块
const router = require('./路由模块化');
//注册路由模块
app.use(router);
app.listen(80, () => {
    console.log('http://127.0.0.1');
})
```
>使用路由模块

>app.use( ... )  用来注册全局中间件

### 添加访问前缀
app.use('/访问前缀', router)；

## Express 中间件
>业务流程的中间处理环节（Middleware）

>当一个请求到达 Express 的服务器之后，可以连续调用多个中间件，从而对这次请求进行预处理
>上一个中间件的输出会作为下一个中间件的输入进行处理

Express 中间件的格式  eg.  app.get('/', function(req, res, **next**) { next(); })
中间件函数的形参列表中，必须包含 next 参数
>而路由处理函数中只包含 req 和 res

### next 函数作用
实现多个中间件连续调用的关键，它表示把留转关系转交给下一个中间件或路由

### 定义中间件函数
const mw = function(req, res, next) { ...;    next(); }
add.use(mw);

### 实际作用
多个中间件之间，**共享同一份** req 和 res，基于这样的特性，我们可以在上游中间件中，**统一为** req 或 res 对象添加自定义的属性和方法，供下游的中间件或路由使用

### 可以定义多个中间件，按定义的顺序执行

### 局部生效的中间件
>不使用 app.use() 定义的中间件，叫做局部生效的中间件

eg.  将中间件定义在 mw 中，通过 app.get() 的第二个参数调用
```
const mw = (req, res, next) => {
    console.log('局部生效了');
    next()
}
app.get('/', mw, (req, res) => {
    res.send('ok, 开始调用');
})
```

>可以从第二个参数开始添加多个局部中间件，用逗号隔开。同时也可以用中括号把局部中间件括起来

### 使用中间件的注意事项
1. 一定要在路由之前注册中间件
2. 客户端发送过来的请求，可以连续调用多个中间件进行处理
3. 执行完中间件的业务代码之后，不要忘记调用 next() 函数
4. 为了防止代码逻辑混乱，调用 next() 函数后不要再写额外的代码
5. 连续调用多个中间件时，多个中间件之间，共享 req 和 res 对象

## 中间件的分类

### 应用级别中间件
通过 app.use() 或 app.get() 或 app.post()，绑定到 app 实例上的中间件，叫做应用级别的中间件

### 路由级别中间件
绑定到 express.Router() 实例上的中间件，叫做路由级别的中间件，用法和应用级别的没有任何区别

### **错误级别中间件**
>专门捕获整个项目中发生的异常错误，从而防止项目异常崩溃的问题

错误级别中间件的 function 处理函数中，必须有 4 个形参，形参顺序从前到后，分别是 (err, req, res, next)
```
app.get('/', function(req, res) {
    throw new Error('服务器内部发生了错误！');
    res.send('Home Page');
})
app.use(function(err, req, res, next) {
    console.log('发生了错误：' + err.message);
    res.send('Error!' + err.message);
})
```

>错误级别的中间件必须注册在所有路由之后

### Express 内置的中间件
>内置三个常用中间件
* express.static  快速托管静态资源的内置中间件
* express.json  解析 json 格式的请求体数据
	* app.use(express.json())
* express.urlencoded  解析 URL-encoded 格式的请求体数据
	* app.use(express.urlencoded( { extended: false } ))
```
const express = require('express');
const app = express();
app.use(express.json());    //解析客户端传来的 json 格式的请求体数据
app.use(express.urlencoded( { extended: false } ));    //解析客户端传来的 URL-encoded 格式的请求体数据
app.post('/', function(req, res) {
    console.log(req.body);
    res.send('ok');
})
app.post('/book', function(req, res) {
    console.log(req.body);
    res.send('ok');
})
app.listen(80, () => {
    console.log('http://127.0.0.1');
})
```
在 postman 上选择 body 标签，选择下方的 raw 或 x-www-form-urlencoded，然后输入相应的数据

### 第三方中间件

## 自定义中间件

## 使用 Express 写接口

## CORS 跨域资源共享
>GET 和 POST 接口不支持跨域请求
>浏览器**同源安全策略**默认会阻止网页‘跨域’获取资源
>如果接口服务器配置了 CORS 相关的 HTTP 响应头，就可以解除浏览器端的跨域访问限制

1. 运行 npm install cors 安装中间件
2. 使用 const cors = require('cors'); 导入中间件
3. 在路由之前调用 app.use(cors()); 配置中间件

主要在服务器端进行配置，客户端浏览器不需要做额外的配置

### CORS 响应头部 
Access-Control-Allow-**Origin**
>origin 参数的值指定了允许访问该资源的外域 URL
>如果指定了字段的值为**通配符** * ，表示允许来自任何域的请求

eg.  res.setHeader('Access-Control-Allow-Origin', '\*')

### Access-Control-Allow-**Headers**
CORS 仅支持客户端向服务器发送的 9 个请求头

如果发送了额外的请求头，则要在服务器端通过 Access-Control-Allow-Headers 对额外的请求头进行声明，否则请求会失败

### Access-Control-Allow-**Methods**
默认情况 CORS 仅支持客户端发起 GET、POST、HEAD 请求

如果客户端希望通过 PUT、DELETE 等方式请求服务器资源，则要在服务器端通过 Access-Control-Allow-MMethods 来指明实际请求所允许使用的 HTTP 方法

res.setHeader('Access-Control-Allow-MMethods', 'GET, POST, HEAD, DELETE');
res.setHeader('Access-Control-Allow-MMethods', '\*');  允许所有 HTTP 方法

### 简单请求
>客户端与服务器之间只会发生一次请求
### 预检请求
>客户端与服务器之间会发生两次请求，OPTION 预检请求成功后，才会发起真正的请求

## JSONP 接口
>浏览器通过` <script> </script>`标签的 src 属性，请求服务器上的数据，同时，服务器返回一个函数的调用，这种请求数据的方式叫做 JSONP

如果已经配置了 CORS 跨域资源共享，为防止冲突，必须在配置 CORS 中间件之前声明 JSONP 的接口，否则 JSONP 接口会被处理成开启了 CORS 的接口

# 数据库与身份认证
>数据库 database 组织、存储和管理数据的仓库
* MySQL Server  专门用来提供数据存储和服务的软件
* MySQL Workbench  可视化的 MySQL 管理工具

## 数据组织结构
>传统型数据库中，数据的组织结构分为 数据库（database）、数据表（table）、数据行（row）、字段（field）四大部分组成

## 创建数据库

### 创建数据表
每个表里都会有 id 字段
用户信息唯一标识

* DataType 数据类型
	* int  整数
	* varchar(len)  字符串
	* tinyint(1)  布尔值

* 字段的特殊标识
	* PK(Primary Key)  主键、唯一标识
	* NN(Not Null)  值不允许为空
	* UQ(Unique)  值唯一
	* AI(Auto Increment)  值自动增长

### 向表中写入数据

## 使用 SQL 管理数据库
>SQL (Structured Query Language) 是结构化查询语言，专门用来访问和处理数据库的编程语言。能够让我们以编程的形式，操作数据库里面的数据。
>SQL 是一门数据库编程语言


>SQL 语言只能在关系型数据库中使用（例如 MySQL、Oracle、SQL Server）。非关系型数据库（例如 Mongodb）不支持 SQL 语言

使用 SQL 从数据表中：查询数据（select） 、插入数据（insert into） 、更新数据（update） 、删除数据（delete） 

额外需要掌握的 4 种 SQL 语法：
where 条件、and 和 or 运算符、order by 排序、count(\*) 函数

>需要向哪个表写入数据就在哪个表右键选第一个选项

>可选择创建新文件，在新文件下 增删改查
### select 语句  查
>SQL 语句中的关键字对大小写不敏感。SELECT 等效于 select，FROM 等效于 from

* select * from 表名称    查询指定表中所有的数据
* select 列名称 from 表名称    查询指定列名称的数据

### insert into 语句  增
insert into 表名称 (列名称1, 列名称2, ...) values (值1, 值2, ...)
>列名和值一一对应

### update 语句  改
update 表名称 set 列名称='新值' where 需更改的位置
>不要忘了加 where 条件，否则会导致整张表的数据发生改变

eg.  update users set password='admin123', status=1 where id=2
>更改多个列使用逗号隔开

### delete 语句  删
delete from 表名称 where 需要删除的位置

### where 子句
>WHERE 子句用于限定选择的标准。在 SELECT、UPDATE、DELETE 语句中，皆可使用 WHERE 子句来限定选择的标准

限定条件
 ... where 列 运算符 值

and 和 or 运算符

### order by 子句
>ORDER BY 语句用于根据指定的列对结果集进行排序

ORDER BY 语句默认按照升序（asc）对记录进行排序。
如果希望按照降序对记录进行排序，可以使用 DESC 关键字。

eg.  select * from users order by status
降序 eg.  select * from users order by id desc

多重排序 eg.  select * from users order by status desc, username asc
逗号分割（升序降序写在每个排序的列后）

### count(\*) 函数
>COUNT(\*) 函数用于返回查询结果的总数据条数
>需要加条件限制

select count(\*) from 需统计的列名
eg.  select count(\*) from users where status=0

>可以使用关键字 as 给列名起名字
>eg.  select count(\*) as total from users where status=0
>为统计出来的列名起了 total 名称

### as 可以设置别名
eg.  select 列名 as 别名, 列名 as 别名 from 表名

# 在项目中操作 MySQL
>npm install mysql
* 在 express 项目中安装操作 MySQL 数据库的第三方模块（mysql）
* 通过 mysql 模块连接到 MySQL 数据库
* 通过 mysql 模块执行 SQL 语句

使用 mysql 模块需先进行必要的配置
```
//导入mysql模块
const mysql = require('mysql');

//建立与MySQL数据库的连接关系
const db = mysql.createPool({
    host: '127.0.0.1',
    user: 'root',
    password: 'admin123',
    database: 'my_db_01'
})
```

```
//测试mysql模块能否正常工作
db.query('select 1', (err, results) => {
    if(err) return console.log(err.message);
    console.log(results);
})
```

## 查询数据
```
db.query('select * from users', (err, results) => {
    if(err) return console.log(err.message);
    //注意：如果执行的是 select 查询语句，则执行的结果是数组
    console.log(results);
})
```

## 插入数据
```
const user = {username: 'Spider-Man', password: 'm12345'}

//定义待执行的 SQL 语句
//? 占位符，接收后面传进的真正的参数
const sqlStr = 'insert into users (username, password) values (?, ?)';

//执行 SQL 语句
//[user.username, user.password] 通过数组的形式传参数进 value
db.query(sqlStr, [user.username, user.password], (err, results) => {
    if(err) return console.log(err.message);

    //注意：如果执行的是 insert into 插入语句，则 results 是一个对象
    //可以通过 affectedRows 属性，来判断是否插入成功
    if(results.affectedRows === 1) {
        console.log('插入数据成功！');
    }
})
```

### 插入数据快捷方式
>通过语句 
>const sqlStr = 'insert into users set ?';
>query 方法第二个参数匹配对应占位符

```
const user = {username: 'X-Man', password: 'h12345'};

//定义待执行 SQL 语句，简便写法
const sqlStr = 'insert into users set ?';

//执行 SQL 语句
//第二个参数直接写待插入的数据
db.query(sqlStr, user, (err, results) => {
    if(err) return console.log(err.message);

    if(results.affectedRows === 1) {
        console.log('插入数据成功！');
    }
})
```

## 更改数据
```
const user = { id: 5, username: 'aaa', password: '000000'};

//占位符等待插入数据
const sqlStr = 'update users set username=?, password=? where id=?';

//通过数组形式逐个插入占位符中
db.query(sqlStr, [user.username, user.password, user.id], (err, results) => {
    if(err) return console.log(err.message);
    
    //执行 update 语句后执行的结果也是一个对象
    if(results.affectedRows === 1) {
        console.log('更新数据成功！');
    }
})
```

>便捷方式
>const sqlStr = 'update users set ? where id=?';
>通过 set 来控制内容
>query 方法第二个参数确定更改内容
```
const user = { id: 5, username: 'aa', password: '00000'};

//占位符等待插入数据
const sqlStr = 'update users set ? where id=?';

//通过数组形式逐个插入占位符中
db.query(sqlStr, [user, user.id], (err, results) => {
    if(err) return console.log(err.message);
    //执行 update 语句后执行的结果也是一个对象
    if(results.affectedRows === 1) {
        console.log('更新数据成功！');
    }
})
```

## 删除数据
>建议根据 id 这样的唯一标识来删除对应的数据

```
const sqlStr = 'delete from users where id=?';
db.query(sqlStr, 7, (err, results) => {
    if(err) return console.log(err.message);
    if(results.affectedRows === 1) {
        console.log('删除数据成功！');
    }
})
```

### 标记删除
>使用 DELETE 语句，会把真正的把数据从表中删除掉。为了保险起见，推荐使用标记删除的形式，来模拟删除的动作

>实际操作中，可以使用 status 字段的状态改变来标记当前的数据是否被删除
>当用户执行了删除的动作时，不是执行 DELETE 语句把数据删除掉，而是执行了 UPDATE 语句，将 status 字段改变，从0改为1
```
const sqlStr = 'update users set status=? where id=?';
db.query(sqlStr, [1, 6], (err, results) => {
    if(err) return console.log(err.message);
    if(results.affectedRows === 1) {
        console.log('删除数据成功！');
    }
})
```

# 前后端的身份认证

## Web 开发模式
* 基于服务端渲染的传统 Web 开发模式
	* 服务器发送给客户端的 HTML 页面，是在服务器通过字符串的拼接，动态生成的，因此，客户端不需要使用 Ajax 这样的技术额外请求页面的数据
	* 优点：1.前端耗时少。因为服务器端负责动态生成 HTML 内容，浏览器只需要直接渲染页面即可。尤其是移动端，更省电。2.有利于SEO。因为服务器端响应的是完整的 HTML 页面内容，所以爬虫更容易爬取获得信息，更有利于 SEO
	*  缺点：1.占用服务器端资源。即服务器端完成 HTML 页面内容的拼接，如果请求较多，会对服务器造成一定的访问压力。2.不利于前后端分离，开发效率低。使用服务器端渲染，则无法进行分工合作，尤其对于前端复杂度高的项目，不利于项目高效开发。

* 基于前后端分离的新型 Web 开发模式
	* 前后端分离的开发模式，依赖于 Ajax 技术的广泛应用。
	* 优点：1.开发体验好。前端专注于 UI 页面的开发，后端专注于api 的开发，且前端有更多的选择性。2.用户体验好。Ajax 技术的广泛应用，极大的提高了用户的体验，可以轻松实现页面的局部刷新。3.减轻了服务器端的渲染压力。因为页面最终是在每个用户的浏览器中生成的。
	* 缺点： 不利于 SEO。因为完整的 HTML 页面需要在客户端动态拼接完成，所以爬虫对无法爬取页面的有效信息。（解决方案：利用 Vue、React 等前端框架的 SSR （server side render）技术能够很好的解决 SEO 问题）

## 如何选择开发模式
根据具体的应用场景选择

* 主要功能是展示而没有复杂的交互，并且需要良好的 SEO，则这时我们就需要使用服务器端渲染
* 类似后台管理项目，交互性比较强，不需要考虑 SEO，那么就可以使用前后端分离的开发模式
>具体使用何种开发模式并不是绝对的，为了同时兼顾了首页的渲染速度和前后端分离的开发效率，一些网站采用了首屏服务器端渲染 + 其他页面前后端分离的开发模式。

## 身份认证
* 服务端渲染推荐使用 Session 认证机制
* 前后端分离推荐使用 JWT 认证机制

>HTTP 协议的无状态性，指的是客户端的每次 HTTP 请求都是独立的，连续多个请求之间没有直接的关系，服务器**不会主动保留**每次 HTTP 请求的状态

## Cookie
>Cookie 是存储在用户浏览器中的一段不超过 4 KB 的字符串。它由一个名称（Name）、一个值（Value）和其它几个用于控制 Cookie 有效期、安全性、使用范围的可选属性组成
>不同域名下的 Cookie 各自独立，每当客户端发起请求时，会自动把当前域名下所有未过期的 Cookie 一同发送到服务器
>
>特性：自动发送、域名独立、过期时限、4KB 限制

客户端第一次请求服务器的时候，服务器通过响应头的形式，向客户端发送一个身份认证的 Cookie，客户端会自动将 Cookie 保存在浏览器中。
随后，当客户端浏览器每次请求服务器的时候，浏览器会自动将身份认证相关的 Cookie，通过请求头的形式发送给服务器，服务器即可验明客户端的身份。

### Cookie 不具有安全性
> Cookie 是存储在浏览器中的，而且浏览器也提供了读写 Cookie 的 API，因此 Cookie 很容易被伪造
> 不建议服务器将重要的隐私数据，通过 Cookie 的形式发送给浏览器

## Session 认证
>npm install express-session

配置 session 中间件
```
const session = require('express-session');
app.use(
    session({
        secret: 'load',
        resave: false,
        saveUninitialized: true
    })
)
```

### 向 session 中存数据
>当 express-session 中间件配置成功后，即可通过 req.session 来访问和使用 session 对象，从而存储用户的关键信息

### 向 session 中取数据

### 清空 session
调用 req.session.destroy() 函数，即可清空服务器保存的 session 信息

## JWT 认证
>原理：用户的信息通过 Token 字符串的形式，保存在客户端浏览器中。服务器通过还原 Token 字符串的形式来认证用户的身份

>当前端请求后端接口不存在跨域问题的时候，推荐使用 Session 身份认证机制
>当前端需要跨域请求后端接口的时候，不推荐使用 Session 身份认证机制，推荐使用 JWT 认证机制

>JWT(JSON Web Token) 是目前最流行的**跨域**认证解决方案

### 组成
JWT 通常由三部分组成，分别是 Header（头部）、Payload（有效荷载）、Signature（签名）

Header.Payload.Signature

* Payload 部分是真正的用户信息，是用户信息经过加密后生成的字符串
* Header 和 Signature 是安全性相关的部分，保证 Token 的安全性

### 使用方式

### 安装
npm install jsonwebtoken express-jwt
>jsonwebtoken 用于生成 JWT 字符串
>express-jwt 用于将 JWT 字符串解析还原成 JSON 对象

### 定义 secret 密钥
>const secretKey = '任意字符串';
>secret 密钥的本质就是一个字符串
* 生成 JWT 字符串的时候，用 secret 密钥对用户信息进行加密，得到加密好的 JWT 字符串
* 把 JWT 字符串解析还原成 JSON 对象时，需要使用 secret 密钥进行解密

### 在登录成功后生成 JWT 字符串
>调用 jsonwebtoken 包提供的 sign() 方法，将用户的信息加密成 JWT 字符串，响应给客户端
>参数1：用户的信息对象
>参数2：加密的密钥
>参数3：配置对象，可以配置当前 token 的有效期

### 将 JWT 字符串还原为 JSON 对象
>客户端每次在访问那些有权限接口的时候，都需要主动通过请求头中的 Authorization 字段，将 Token 字符串发送到服务器进行身份认证
>服务器可以通过 express-jwt 这个中间件，自动将客户端发送过来的 Token 解析还原成 JSON 对象

### 获取用户信息
>当 express-jwt 这个中间件配置成功之后，即可在那些有权限的接口中，使用 req.user 对象，来访问从 JWT 字符串中解析出来的用户信息了

