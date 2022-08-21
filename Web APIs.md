# 什么是 Web APIs
Application Programming Interface
**应用程序编程接口**
是一些预定的函数，一种轻松实现功能的工具，可以理解为接口

>Web APIs 是一个页面交互功能，是浏览器提供的操作浏览器功能（BOM）和页面元素（DOM）的API
>其中一般都有输入和输出，很多都是方法（函数）

# DOM 操作
>文档对象模型 Document Object Model
>处理可扩展标记语言（HTML 或 XML）的标准编程接口（处理文档接口）
>可以改变内容、结构、样式

**DOM树**
* 文档：一个页面就是一个文档，DOM 中使用 document 表示
* 元素：页面中的所有标签都是元素，DOM 中使用 element 表示
* 节点：网页中的所有内容都是节点（标签、属性、文本、注释等），DOM 中使用 node 表示
>DOM 把以上内容都看做是对象

## 获取元素
*一般声明变量接收获取过来的对象*
console.dir()  打印返回的元素对象，可以看到里面的属性和方法

* document.getElementById('id')  （*需要加引号引起 id*）返回的是元素对象（object），获取相应 id 的全部内容包括标签...
* element.getElementsByTagName('标签名')  返回的是元素对象的集合，以伪数组的形式存储，可以用 for ... in ... 方式遍历打印

* document.getElementsByClassName('')  根据类名获得元素
* document.querySelector('选择器')  返回指定选择器的第一个元素对象
* document.querySelectorAll('选择器')  返回指定选择器的所有对象
>以上的 选择器 若是 class 则需要加  . 
>若是 id 则需要加  # 
>标签则直接写标签名

特殊元素的获取（都不需要加括号）
* document.body  返回 body 元素对象
* document.documentElement  返回 html 元素对象

## 事件基础
>可以被 JavaScript 侦测到的行为    触发--相应机制

**组成**
* 事件源：事件被处罚的对象
* 事件类型：如何触发、什么事件
* 事件处理程序：通过函数赋值的方式完成
**步骤**
1. 获取事件源
2. 注册事件（绑定事件）
3. 添加事件处理程序

## 操作元素
*eg. var ele = document.querySelector('div');
	  var input = document.querySelector('input');*
>对获取过来的对象进行进一步地操作

### 改变元素内容
* ele.innerText = 内容    该操作会去除掉空格和换行，且不识别 HTML 标签
* ele.innerHTML = 内容    保留空格和换行，并且识别内容中的 HTML 标签
>同为可读写操作

常用的元素属性
>src  |  href    图片、超链接修改
>id  |  alt  |  title  

### 表单属性操作
>innerHTML 不能影响表单属性
* ~.type  |  ~.value 表单里的值 |  ~.checked 是否打上勾 |  ~.selected 是否被选中 |  ~.disable 按钮能否按下 ...

### 样式属性操作
* ele.style.样式类型  行内样式操作，权重值较高
	* 样式名使用驼峰命名法调用 eg. backgroundColor    fontSize
	* 后面的属性除非是数字型，否则需要加引号
>修改的样式较少时可用该方法
* ele.className = '类名1 类名2 ...' 
	* 原理是将一些样式写在 style 标签里，需要用上里面的样式的时候就使用这个方法给标签用上 class 类名
	* 因为 class 是保留字，所以用的是 className 做属性名
	* 使用该方法会替代原先的类名，如果想要保留原先的类名，在赋值的时候在引号里面用**空格**隔开原先的类名和加入的类名
>需要修改的样式多的时候用这个方法

## 属性值操作
*eg. var ele = document.querySelector('div');*

### 获取|设置|移除 自定义属性值
* ele.id | ele.class    获取元素的内置属性
* ele.getAttribute('id | class | ...');    可获取自定义的属性

* ele.id = 'first';    设置内置属性值
* ele.setAttribute('属性', '值');    可设置自定义属性

* ele.removeAttribute('属性');

>有些自定义属性容易引起歧义，不容易判断是内置属性还是自定义属性

### H5 的自定义属性的设置
H5 提供了设置自定义属性，以 data- 开头作为属性名并且赋值
eg. <div data-index=“1”></div>
或使用 JS 设置 ele.setAttribute('data-index', '2');
* 获取 H5 自定义属性的方法
	* ele.dataset.index | ele.dataset\['index'\] 
	* 只能获取 data- 开头的
	* 若自定义属性里有多个 - 连接，则采用驼峰命名法获取
	* eg. 设置属性名为 data-list-name    获取时ele.dataset.listName

---

## 节点操作
*eg. var ul = document.querySelector('ul');*

* 使用节点操作操作元素更简单，逻辑性强（利用父子兄节点关系获取元素）
* 网页所有内容均为节点，在 DOM 中，节点用 node 来表示
* 节点至少有三个基本属性：nodeType 属性类型、nodeName 属性名、nodeValue 属性值
>元素节点 nodeType 为 1，属性节点 nodeType 为 2，文本节点 nodeType 为 3
>在实际开发中，节点操作主要操作的是元素节点

### 节点层级

* 父级节点
	* ul.parentNode    返回距离最近的一个父节点，无则返回 null

* 子节点
	* ul.childNodes （标准写法，用得少）返回子节点集合，包括元素节点、文本节点（元素节点、换行节点 ...）
	* ul.firstChild  |  ul.lastChild    获取所有子节点集合中的 第一|最后 一个
	* ul.children    为**只读**属性，返回所有的**子元素**节点（重点、常用）
	* ul.firstElementChild | ul.lastElementChild    获取子元素中的 第一|最后 一个节点    （写起来麻烦，且有兼容问题，ie9 以上才能用）
	* 实际开发中，使用  ul.children\[索引号\] 来获取子元素节点中任意一个节点

* 兄弟节点
	* ul.nextSibling  |  ul.previousSibling    返回当前元素 下一个|上一个 兄弟节点（包括所有节点）
	* ul.nextElementSibling  |  ul.previousSibling    返回当前元素 下一个|上一个 兄弟元素节点  （ie9 以上兼容）

>重点去记父节点和子节点，基本就够用了

### 创建添加节点
>想要页面添加一个新的元素，首先要先创建元素，再进行添加
* 创建节点
	* li.createElement('li')
* 添加节点
	* ul.appendChild(li);    追加元素，像数组中的 push()
	* ul.insertBefore(li, 指定元素);    添加到指定元素前面

### 删除节点
* ul.removeChild(节点变量)    删除父节点中的一个子节点，返回删除掉的节点

### 复制节点
* ul.cloneNode();    括号为空或者为 false，则为浅拷贝，只复制标签，不复制内容；为 true，则是深拷贝，会复制节点本身以及里面所有的子节点 [[深拷贝浅拷贝]]

### 三种动态创建元素的区别
* document.write();    用得少，如果页面文档流加载完毕，再调用这句话会导致页面全部重绘（了解即可）
* innerHTML    将内容写入某个 DOM 节点，不会导致页面全部重绘，创建多个元素效率更高（不采用拼接字符串，采取数组形式拼接），结构稍微复杂 [[innerHTML 拼接效率（数组）]]
* document.createElement()    创建多个元素效率稍微低一点点，但结构更清晰 [[createElement 拼接效率测试]]

## DOM 重点核心
>W3C 已经定义了一系列的 DOM 接口，通过这些 DOM 接口可以改变网页的内容、结构和样式
>对于 JavaScript，为了能够使 JavaScript 操作 HTML，JavaScript 就有了一套自己的 DOM 编程接口
>对于 HTML，DOM 使得 HTML 形成一颗 DOM 树，包含了文档、元素、节点

***获取过来的 DOM 元素是一个对象 object，所以称为文档对象模型***

主要针对元素的 创建、增、删、改、查、属性操作、事件操作

---

# 事件高级
*eg. var ele = document.querySelector('div');*

## 注册事件（绑定事件）
### 传统方式
* 利用 on 开头的事件  eg. onclick、onfocus ...
* ele.onclick = function() { ... }
* 有唯一性：同一元素同一事件只能设置一个处理函数，最后注册的处理函数将会覆盖前面注册的处理函数

### 方法监听注册方式
* w3c 标准，推荐方式
* addEventListener()  是一个方法
* ie9 之前可用 attachEvent() 代替（非标准，了解即可）
* 同一个元素同一个事件可注册多个监听器
* ele.addEventListener(type, listener\[, useCapture\])
	* type  事件类型字符串 如：click、mouseover ... ，不需要在前面加 on
	* listener  事件处理函数  function() { ... } （可外部定义函数，在内部调用）
	* useCapture  

>兼容性处理的原则：首先照顾大多数浏览器，再处理特殊浏览器

## 删除事件
### 传统注册方式
* ele.onclick = null;  一般添加在事件里

方法监听注册方式
* ele.removeEventListener(type, listener\[, useCapture\])

## 事件流
>事件流描述的是从页面中接收事件的顺序
>事件发生时会在元素节点之间按照特定的顺序传播，这个传播过程即 DOM 事件流


DOM 事件流分为3个阶段
1. 捕获阶段
2. 当前目标阶段
3. 冒泡阶段

事件冒泡：IE 最早提出，事件开始时由最具体的元素接收，然后**逐级向上**传播到 DOM 最顶层节点的过程
事件捕获：网景最早提出，由 DOM 最**顶层节点**开始，然后逐级向下传播到最具体的元素接收的过程

>*JS 代码中只能执行捕获或者冒泡其中的一个阶段*
>
>onclick 和 attachEvent 只能得到冒泡阶段
>
>addEventListener 第三个参数如果为 **true**，表示在事件**捕获阶段调用**事件处理程序；如果是 **false 或者空**，表示在**冒泡阶段调用**事件处理程序
>
>实际开发中很少使用事件捕获，更关注事件冒泡
>
>有些事件是没有冒泡的，如 onblur、onfocus、onmouseenter、onmouseleave（触发了就开始执行，不需要冒泡阶段）

## 事件对象
ele.onclick = function(event) { ... }
ele.addEventListener('click', function(event) { ... })
>其中的 event 就是事件对象，一般也喜欢写成 e 或者 evt
>代表事件的状态，比如键盘按键的状态、鼠标的位置等等

**事件发生后，与事件相关的一系列信息数据的集合都被放到这个对象（event）里面，其具有许多的属性和方法**

### 常见属性和方法
* e.target    返回**触发**事件的对象（标准）
* e.srcElement    返回触发事件的对象（非标准，ie6-8 使用）
* e.type    返回事件的类型，如 click、mouseover
* e.cancelBubble = true   该属性阻止冒泡（非标准，ie6-8 使用）
* e.returnValue    该**属性**阻止默认行为，非标准
* e.preventDefault()    该**方法**阻止默认行为，标准，如阻止连接跳转
* e.stopPropagation()    阻止冒泡（标准）
>return false 也能阻止默认行为，但是 return 后面的代码就不执行了，且仅限于传统的注册方式
>
>this 和 target ：this 返回的是**绑定事件**的对象，谁绑定了指向谁
>                          event.target 返回的是**触发事件**的对象，谁触发指向谁
>
>currentTarget 与 this 相似（了解即可）

## 事件委托
>不给每个子节点单独设置事件监听，而是事件监听器设置在父节点上，利用**冒泡原理**影响设置每个子节点

只操作了一次 DOM，提高了程序的性能（？常跟 e.target 配合使用？）

## 鼠标键盘事件及其对象
### 常用鼠标事件
* onclick    点击触发
* onmouseover    经过触发
* onmouseout    离开触发
* onfocus    获得焦点触发
* onblur    失去焦点触发
* onmousemove    移动触发
* onmouseup    弹起触发
* onmousedown    按下触发
>mouseenter 只会在经过自身盒子才触发，和 mouseleave 搭配使用，两者都没有冒泡阶段
>mouseover 经过自身盒子会触发，经过子盒子也会触发，和 mouseout 搭配使用
* 右键菜单    contextmenu  主要控制何时显示上下文菜单
* 鼠标选中    selectstart  开始选中
>上面两个方法可以搭配 e.preventDefault();  达到禁止右键和选中文本
>[[禁止右键和选中文字]]

### 鼠标事件对象
* e.clientX
* e.clientY
>返回鼠标相对于浏览器窗口**可视区**的 X|Y 坐标
* e.pageX
* e.pageY
>返回鼠标相对于**文档页面**的 X|Y 坐标  ie9+ 支持
* e.screenX
* e.screenY
>返回鼠标相对于**电脑屏幕**的 X|Y 坐标

### 常用键盘事件
* onkeyup    键盘某个按键松开触发
* onkeydown    按键被按下时触发
* onkeypress    按键被按下时触发 （不识别功能键 如 ctrl shift 箭头等）
>addEventListener 中不需要加 on
>执行顺序  keydown -- keypress -- keyup

### 键盘事件对象
* e.keyCode    返回该键的 ASCII 值
>onkeydown 和 onkeyup 不区分字母大小写，onkeypress 区分字母大小写
>实际开发中更多使用 keydown 和 keyup 

>e.key    返回对应的键（不是 ASCII 码）

---

# BOM
## BOM 介绍
>浏览器对象模型 Brower Object Model
>提供了独立于内容而与浏览器窗口进行交互的对象，其核心对象是**window**

* 缺少标准，兼容性较差
* BOM 包含了 DOM

window 对象是浏览器的顶级对象，具有双重角色
* 它是 JS 访问浏览器窗口的一个接口
* 它是一个全局对象，定义在全局作用域中的变量、函数都会变成 window 对象的属性和方法
>在调用的时候可以省略 window，之前学习的对话框都属于 window 对象方法，如 alert()、prompt() 等
>注意：window 下的一个特殊属性 window.name，声明变量最好不要用 name做关键字

## window 对象的常见事件

### 窗口加载事件
* 窗口加载事件  当文档内容完全（包括图像、脚本文件、CSS 文件等）加载完成会触发该事件
	* window.onload = function() { ... }
	* window.addEventListener("load", function() { ... })
>**有了 window.onload 就可以把 JS 代码写到页面元素的上方**，因为 onload 是等页面内容全部加载完毕，再去执行处理函数
>传统注册只能执行最后注册的一个，监听注册没有次数限制

* document.addEventListener('DOMContentLoaded', function() { ... })
	* 该事件触发时，仅当 DOM 加载完成，不包括样式表，图片等
	* ie9 以上支持
>如果页面的图片很多的话，从用户访问到 onload 触发可能需要较长的时间，交互效果就不能实现，影响体验，此时用该事件比较合适

### 调整窗口大小事件
* window.onresize = function() { ... }
* window.addEventListener("resize", function() { ... })
>调整窗口大小触发事件

### 定时器
* window.setTimeout(调用函数, \[延迟毫秒数\])    （window 可省略）
	* 在定时器到时间后执行调用函数
	* 调用函数可以直接写函数，或者函数名，或者采用字符串'函数名()'（不推荐）
	* 延迟毫秒数省略默认为0，单位是毫秒
* window.clearTimeout(timeoutID)
	* 该方法可以取消先前通过调用 setTimeout() 建立的定时器

* setInterval(回调函数, \[间隔毫秒数\])    [[倒计时]]
	* 该方法重复调用一个函数，每隔间隔毫秒的时间，就调用一次回调函数
* window.clearInterval(intervalID)
	* 取消先前通过调用 setInterval() 建立的定时器

>定时器可能有很多，所以经常会给定时器赋值一个标识符

插入知识点 [[JS 执行机制]]

### location 对象及常见方法
>window 对象给我们提供了一个 **location 属性**用于**获取或设置窗体的URL**，并且可以用于解析 URL。[[URL]]
>因为这个属性返回的是一个对象，所以又将这个属性称为 **location 对象**

location 对象的属性
* location.href    获取或设置整个 URL
* location.host    返回主机（域名）
* location.port    返回端口号，如果未写返回，则为空字符串
* location.pathname    返回路径
* location.search    返回参数
* location.hash    返回片段

重点记住 href 和 search
>案例在 P283 获取 URL 参数里

location 对象的方法
* location.assign()    跟 href 一样，可以跳转页面
* location.replace()    替换当前页面（不记录历史，不能后退页面）
* location.reload()    重新加载页面，相当于刷新按钮，如果参数为 true 则强制刷新（重新再下载页面图片之类的数据）

### navigator 对象
>navigator 对象包含有关浏览器的信息，其具有很多属性，最常用的为 userAgent，该属性可以返回由客户机发送服务器的 user-agent 头部的值

应用：判断客户端类型，跳转到不同的页面  [[判断客户端跳转页面]]

### history 对象（较为少用，OA办公系统）
* history.back()    后退功能
* history.forward()    前进功能
* history.go(参数)    参数是 1 则为前进一个页面，-1 为后退一个页面

---

# PC 端网页特效

## 元素偏移量 offset
[[拖动模态框]]

>该系列相关属性可以**动态地**获得该元素的位置（偏移）、大小等
>返回的数值都不带单位
>*只读属性，不能赋值*

* element.offsetParent    返回作为该元素**带有定位**的父级**元素**，如果父级都没有定位则返回 body
* element.offsetTop    返回元素相对带有定位父元素上方的偏移
* element.offsetLeft    返回元素相对带有定位父元素左边的偏移
* element.offsetWidth    返回自身*包括 padding、边框、内容区*的宽度
* element.offsetHeight    返回自身*包括 padding、边框、内容区*的高度

### offset 与 style 的区别
* offset
	* 可以得到任意样式表中的样式值
	* 获得的数值没有单位
	* offsetWidth 包含 padding + border + width 
	* 只读属性，只能获取不能赋值
	* ***想要获取元素大小位置，用 offset 更合适***
* style
	* 只能得到行内样式表中的样式值
	* style.width 获得的是带有单位的字符串
	* style.width 获得不包含 padding 和 border 的值
	* style.width 是可读写属性，可以获取也可以赋值
	* ***想要给元素更改值，则需要用 style 改变***

## 元素可视区 client
>client 系列相关属性可以获取*元素可视区*的相关信息
>通过该系列相关属性可以动态地得到该元素的边框大小、元素大小等

* element.clientTop    返回元素*上边框*的大小
* element.clientLeft    返回元素*左边框*的大小
* element.clientWidth    返回自身 包括padding、内容区的宽度，*不含边框*，返回数值不带单位
* element.clientHeight    返回自身 包括padding、内容区的高度，*不含边框*，返回数值不带单位

## 元素滚动 scroll
>使用 scroll 系列相关属性可以动态地得到该元素（实际）的大小、滚动距离等
>返回数值都不带单位

* element.scrollTop    返回被卷去的上侧距离
* element.scrollLeft    返回被卷去的左侧距离
* element.scrollWidth    返回自身实际的宽度，不含边框（包含 padding）
* element.scrollHeight    返回自身实际的高度，不含边框（包含 padding）
>scrollTop 和 scrollLeft 使用得最多

>注意：若是**页面**被卷去则使用 **window.pageYOffset**

>页面卷去属性有兼容性问题
>声明了 DTD 即 <!DOCTYPE html>  用document.documentElement.scrollTop
>未声明则使用  document.body.scrollTop
>新方法：window.pageXOffset | window.pageYOffset    ie9 开始支持

## 三大系列总结
1. offset 系列经常用于获得**元素位置**  offsetLeft、offsetTop
2. client 经常用于获取**元素大小**  clientWidth、clientHeight
3. scroll 经常用于获取**滚动距离**  scrollTop、scrollLeft
4. 页面滚动距离通过 **window.pageXOffset**

## 动画原理
>核心原理：通过定时器 setInterval() 不断移动盒子位置

实现步骤
1. 获得盒子当前位置
2. 让盒子在当前位置加上1个移动距离
3. 利用定时器不断重复这个操作
4. 加一个结束定时器的条件
5. 注意此元素需要添加定位，才能使用 element.style.left

### 动画函数简单封装
>需要传递两个参数，目标对象和目标位置
>定时器定义使用对象名属性的方法可以减少开辟的内存，也能明确定时器的对象  obj.timer = setInterval(function() { ... }, 毫秒数)

>使用定时器时，为避免执行过程中同一个定时器同时执行，常常先清除以前启动的定时器（在程序开头写 clearInterval(定时器名);），只保留一个定时器执行

### 缓动效果原理
[[缓动动画效果]]
>让元素运动速度有所变化，最常见的是让速度慢慢停下来

思路
1. 让盒子每次移动的距离慢慢变小，速度就会慢慢落下来
2. 核心算法：（目标值 - 现在的位置）/ 10  做为每次移动的距离的步长
3. 停止条件：让当前盒子位置等于目标位置就停止定时器
4. 注意步长值需要取整    step = step > 0 ? Math.ceil(step) : Math.floor(step);

定时器回调函数写在定时器结束的位置

### 回调函数
>函数可以作为一个参数，将这个函数作为参数传到另一个函数里面，当那个函数执行完之后，再执行传进去的这个函数，这个过程就叫做回调

### 网页轮播图

节流阀：防止轮播图按钮连续点击造成播放过快
目的：先执行完毕动画，再执行下一个动画，让事件无法连续触发
关键：
1. 外部设置一个 flag
2. 执行点击事件时判断 flag 的状态，为 true 时执行后续代码，然后设置 flag = false
3. 在动画封装函数中添加函数，使动画播完后 flag = true

核心思路：利用回调函数，**添加一个变量**来控制，锁住函数和解锁函数

---

# 移动端网页特效
>移动端浏览器兼容性较好，不需要考虑以前 JS 兼容性问题

## 触屏 touch 事件
* touchstart    手指触摸到一个 DOM 元素时触发
* touchmove    手指在一个 DOM 元素上滑动时触发
* touchend    手指从一个 DOM 元素上移开时触发

## 触摸事件对象  TouchEvent
>描述手指在触摸平面的状态变化的事件，用于描述一个或多个触点，可以检测触点的移动、增加和减少等
* touches    正在触摸屏幕的所有手指的一个列表
* targetTouches    正在触摸当前 DOM 元素上的手指的一个列表
* changedTouches    手指状态发生了改变的列表，从无到有，从有到无变化

## 移动端常见插件
* Swiper
* superslide
* iscroll

插件的使用
1. 确认插件实现的功能
2. 去官网查看使用说明
3. 下载插件
4. 打开 demo 案例文件，查看需要引入的相关文件，并引入
5. 复制 demo 实例文件中的 HTML 结构，CSS 样式以及 JS 代码

## 移动端开发框架 bootstrap

...