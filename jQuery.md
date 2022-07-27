# JavaScript 库
**许多函数的集合**

>query 快速的

### 下载
到jQuery官网下选择文件[Download the compressed, production jQuery 3.6.0](https://code.jquery.com/jquery-3.6.0.min.js)
复制粘贴至js文件中

**第一次使用**

	 $('div').hide();  //隐藏元素（写在文档下面）

	$(document).ready(function() {
            $('div').hide();
        }；//执行完页面DOM入口

	$(function() {

            $('div').hide();

        })
	//推荐使用


## 基本使用

jQuery 顶级对象 ' \$ ' ，是jQuery的别称
相当于原生 JS 中的 window 

用原生 JS 获取的对象是 DOM 对象
jQuery 方法获取的元素就是jQuery 对象
jQuery 对象的本质是利用 $ 对 DOM 对象包装后产生的对象
==*jQuery 对象只能用 jQuery 方法，DOM 对象使用原生的JS属性和方法*==
>如 element.style.display; 只能对 DOM 对象使用
>不能用例如 hide() 的方法在 DOM 对象中

## jQuery 对象和 DOM 对象转换

* DOM 转换为 jQuery 
	* 直接用 $ 符号对 DOM 对象进行包装 eg.   $('video');    *注：括号里已经是DOM对象时不要加引号*

* jQuery 转换为 DOM
	* $('div')\[index\] index 是索引号
	* $('div').get(0).\[index\] index 是索引号

# jQuery 常用 API

## jQuery 选择器

* 基础选择器
	* $("选择器");  获取某元素   *里面直接写css选择器即可，需要加引号 "#id" 、".class" ...* 


* 层级选择器
	* $("ul>li");   获取子元素层级的元素
	* $("ul li");   后代选择器，获取 ul 后的所有 li

---

### 隐式迭代

> eg.  $("div").css("background", "pink");

*将匹配的所有元素内部进行遍历循环，执行相应的方法*

---

[[下拉菜单]]  *.show() 显示元素； .hide() 隐藏元素*  [[排他按钮]]

### 筛选选择器

* :first   :last        获取第一|第二个 li 元素    $('li:first')
* :eq(index)        获取对应索引号的 li 元素    $("li:eq(2)")
* :odd    :even        选择索引号为奇数|偶数的元素

### 筛选方法

**父子级**
* parent()        查找最近一级父级    $("li").parent();
* children(selector)        最近一级子级    $("ul").children("li");  *相当于 $("ul>li");*
* find(selector)         后代选择器    $("ul").find("li");  *相当于 $("ul li");*
* parents(selector”)        查找指定父级
* ...

**兄弟级**
* siblings(selector)        查找除自身以外的兄弟节点    $("first").siblings("li")
* nextAll(\[expr\])        查找当前元素之后的所有同辈元素    $(".first").nextAll()
* prevtAll(\[expr\])        查找当前元素之前的所有同辈元素
* eq(index)        相当于 $("ul li:eq(2)")  index 从0开始    $("ul li").eq(2) *推荐写法*
* hasClass(class)        检查当前元素是否含有某个特定类，有则返回 true    $('div').hasClass("protected") *了解即可

**重点记住： parent()   children()  find()  sibling()  eq()**


### 链式编程
**为了节省代码量，看起来更优雅** *(使用时要注意是哪个对象执行样式)*

`$(this).css('color', 'red').siblings('button').css('color', '')`

	以上代码使指定的元素添加红色字体，其余兄弟元素不添加。

---

## jQuery 样式操作

* css方法 *注意引号的使用，不是数字则需要加引号*
```javascript
$("div").css("width", 300)  |  $("div").css("width", "300px")
	 $("div").css(function() {
		width: 400,
		height: 400,
		backgroundColor: "red"  *复合属性采取驼峰命名法*
		})
```

* 设置类样式方法  *Class已经说明是类型了，类名不需要加点*
```javascript
$("div").addClass("类名")  添加类
$("div").removeClass("类名")  删除类
$("div").toggleClass("类名")  切换类 有这个类的时候则删除，没有则添加
```

>*原生 JS 的 className 会覆盖元素原先的类名
>jQuery 的类操作只是对指定类进行操作，不影响原先的类名*

## jQuery 动画效果
*[[jQuery实现动画效果]]*

### 显示隐藏效果

* show(\[speed, easing, fn\])    *参数都可省， speed： 三种预定速度 slow， normal， fast 或者毫秒数  |  easing： 切换效果，默认 swing， 可用参数 linear  |  fn 回调函数*
* hide(\[speed, easing, fn\])    *参数同上*
* toggle(\[speed, easing, fn\])    切换显示隐藏 *参数同上*

### 上拉下拉效果

* slideDown(\[speed, easing, fn\])    下拉效果
* slideUp(\[speed, easing, fn\])    上拉效果
* slideToggle(\[speed, easing, fn\])    切换效果

### ***事件切换效果***
hover(\[ovee,\]out)    over: 鼠标经过触发的函数  out: 鼠标移出触发的函数  只写一个时经过和离开都会触发效果  *[[切换效果]]*

### 淡入淡出效果

* fadeIn(\[speed, easing, fn\])    淡入效果
* fadeOut(\[speed, easing, fn\])    淡出效果
* fadeToggle(\[speed, easing, fn\])    淡入淡出切换
* fadeTo(speed, opacity, \[easing, fn\])    调整透明度 *speed 和 opacity 必须写，opacity取值0~1之间*

### 动画队列及其停止效果

>**动画或者效果一旦触发就会执行，多次触发会造成排队效果**

### stop() 停止动画或效果
*必须写到动画的前面，相当于结束上一次的动画  eg. $('div').stop().slideToggle(100);*

### 自定义动画

* animate(params, \[speed\], \[easing\], \[fn\])    
	* params *更改的样式属性，以对象形式传递，必须写。属性名可以不带引号，复合属性用驼峰命名法*

---

## 属性操作

### 设置、获取元素**固有**属性**值**

* $("div").prop("属性名", "属性值")    

### 设置、获取元素**自定义**属性**值**

* $("div").attr("属性名", "属性值")    *类似原生setAttribute()*

### 数据缓存

* $("div").data("属性名", "属性值")    **里面的数据存放在元素内存里，需要调用的时候可以使用，不会修改 DOM 元素结构，页面刷新则数据清除**
>该方法获取 index-index h5 自定义属性，不需要写 data- ，如$("div").data("index") 
>返回值为数字型

___

## 内容文本值
*[[价格加减操作]]*

* 普通元素内容  *相当于原生 innerHtml*
	* html()  获取元素内容
	* html("内容")  修改元素内容
* 普通元素文本内容  *相当于原生  innerText*
	* text()  获取元素文本内容
	* text("内容")  修改元素文本内容
* 表单的值  *相当于原生的 value*
	* val()  获取表单的 value 值
	* val("内容")  修改表单的 value 值的内容

## 元素操作

### 遍历元素
#### each() 遍历对象

* *$("div").each(function (index, domEle) { ...; })*    [[each() 遍历对象用法]]
>该方法主要用 DOM 处理
>index 为每个元素索引号，可以**自定义**属性名
>demEle 是 DOM 元素对象，不是 jQuery 对象，所以需要**转换**为 jQuery 对象$("domEle")

#### $.each() 

* *$.each(object, function (index, domEle) { ...; })*      
 [[$.each() 用法示例]]
>该方法可用于遍历任何对象。主要用于数据处理，eg. 数组、对象

**遍历 DOM 对象 \$("div").each() 更合适，遍历数组、对象，$.each() 更合适**

### 创建元素

* 语法 var li = $("\<li\>...\<\\li\>");    动态创建出了一个 li 标签，并存放在变量 li 中

### 添加元素

* **内部添加**
	* $("ul").*append*(li);        把内容放在 ul 标签内部的*最后*
	* $("ul").*prepend*(li);        把内容放在 ul 标签内部的*最前*
* **外部添加**
	* $("ul").*after*(li);        在 ul 的后面添加
	* $("ul").*before*(li);        在 ul 前面添加

### 删除元素

* $("ul").remove;        删除匹配的元素（包括自身）
* $("ul").empty;        删除匹配元素里面的*子节点*（不包括自身）
* $("ul").html("");        效果同上，清空匹配元素内容

---

## 尺寸、位置操作

### 尺寸方法

* width() / height()        取得元素宽度|高度值，*只返回宽度|高度*
* innerWidth() / innerHeight()         包含了**padding**
* outterWidth() / outerHeight()        包含**padding**和**border**
* outterWidth(true) / outerHeight(true)        包含了padding、border、**margin**

>以上参数为空，则是获取相应的值，返回数字型  （无单位）
>参数为数字，则修改为相应值

### 位置方法

* offset()        设置（有参数）或者返回被选元素相对*文档*的偏移坐标，与父级有无定位无关
	* offset().left  |  offset().top     获取距离文档左侧|顶部的距离
	* offset({ top: 10, left: 30 })        设置元素的偏移

* position()       获取距离带有定位的父级的偏移（位置），无父级则以文档为准  *只能获取，不能设置偏移*

* scrollTop/scrollLeft()        设置或获取元素被卷去的头部和左侧
>animate() 动画方法效果中也有 scrollTop 方法，对**元素（非文档）** 进行动画效果的到顶部 eg.    $("body, html").stop().animate({
>		scrollTop: 0
>})    *滑动到页面的顶部*

## 节流阀|互斥锁
>有时候程序执行会有一个效果触发两个事件的情况，这时候要在相应的事件里添加节流阀|互斥锁，这样以便达到执行完一种事件之后，不影响另一个事件而能使两个事件正常运行

eg. 声明 flag 变量设置为 true | false ，执行其中一个事件的时候改变 flag 的值，在该事件执行完后，将 flag 的值变回原来的状态。  *案例在 js 教程中的电梯导航案例里*

---

# jQuery 事件

## 单个事件注册

eg. $("div").click(function() { ... })  *与原生JS类似*

## on() 事件处理（多个事件注册）
>以对象的方式执行函数

>[[发布微博案例]]

$("div").on(events, \[selector\], fn)
* event  一个或者多个用空格分隔的事件类型
* selector  目标元素子元素的选择器
* fn  回调函数

```javascript
$("div").on({
	mouseenter: function() {
		$(this).css("background", "skyblue")
	}  //事件与函数用 ":" 隔开，与对象相同
	mouseleave: function() {
		$(this).css("background", "blue")
	}
})
/* -------- */
$("div").on("mouseenter mouseleave", function() {
	$(this).toggleClass("current");  //鼠标经过和离开给元素增加 current
})
```

### **on() 可以做事件委派的操作**
*可以将原来加给子元素的事件绑定在父元素上，即将事件委派给父级*

$("ul").on("click", "li", function() {
	alert('hello world!')
})
>以前：$("ul li").click();    给每一个 li 都添加事件

### **可以给动态生成的元素绑定事件**
*on() 最大的优势*

## off() 解绑事件

* $("ul").off()     解绑 ul 元素的所有绑定事件
* $("ul").off("click")    解绑 ul 上的点击事件
* $("ul").off("click", "li")    解绑事件委托

> .one()    只触发一次的绑定事件

## 自动触发事件

* eg. div 绑定点击事件后
	* 第一种 在下一行直接写  $("div").click();
	* 第二种  $("div").trigger("click");
	* $("div").triggerHandler("click");    *该方法不会触发元素的默认行为*

## 事件对象
>只要事件被触发，就会有事件对象的产生

element.on(events, \[selector\], function(event) {})
*event 为事件对象*
> event.preventDefault() | return false    阻止默认行为
> event.stopPropagetion()    阻止冒泡

# 其他方法

## 拷贝对象

$.extend(\[deep\], target, object1, \[objectN\])
* deep  设置为 true 为深拷贝，默认为 false 浅拷贝
* target  要拷贝给的目标对象
* object1  待拷贝到的第一个对象的对象
* objectN 待拷贝到的第N个对象

>浅拷贝是把被拷贝的对象的地址拷贝给目标对象，修改目标对象也会影响原来的被拷贝对象
>深拷贝，前面加 true ，完全克隆，修改目标对象不会影响被拷贝的对象，目标对象里有不冲突的属性，则会合并到一起

## 多库共存
>jQuery 使用 $ 作为标识符，久而久之，其他 JS 库也会用 $ 作为标识符，这样会引起冲突
>让其他库不存在冲突，可以同时存在，这就叫多库共存

**jQuery 解决方案**
* 将 $ 符号统一改为 jQuery
* jQuery 规定新的名称，使用 \$.noConflict();    eg. var mingzi=$.noConflict();
	* 该方法让 jQuery 释放了对 $ 的控制权，可以自己定义标识符

# jQuery 插件
jQuery 功能比较有限，想要更复杂的效果可以通过插件完成

* jQuery 插件库  https://www.jq22.com/
* jQuery 之家  http://www.htmleaf.com/

## 插件演示
* 瀑布流
* 图片懒加载    需要在程序最后才写 JS
* 全屏滚动插件    https://github.com/alvarotrigo/fullPage.js    插件地址
	* https://www.dowebok.com/demo/2014/77/    中文文件说明

---

# jQuery 总结性案例 -todolist

