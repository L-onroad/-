# 类 class

^45894a

>类抽象了对象的公共部分，它泛指某一大类（class）
>对象特指某一个，通过类实例化一个具体的对象   

## 面向对象的特点
抽取（抽象）对象共用的属性和行为组织(封装)成一个类(模板)
对类进行实例化, 获取类的对象

## 创建类
```
class 类名 {
	// class body
}
```

var xxx = **new** name();
>**类必须使用 new 实例化对象

## constructor 构造函数
>constructor() 方法是类的构造函数(默认方法)，用于传递参数,返回实例对象，通过 new 命令生成对象实例时，自动调用该方法。如果没有显示定义, 类内部会自动给我们创建一个constructor() 

```
class 类名 {
	constructor(形参) {
		
	}
	函数2() {
		
	}
}
```

类中的方法（函数）不需要加逗号分割

## extends 类的继承
>子类可以继承父类的属性和方法
>class Fa { ... }
>class Son extends Fa { ... }

### super 关键字
>super 关键字用于**访问**和**调用**对象父类上的函数。可以调用父类的构造函数，也可以调用父类的普通函数

```
        class Fa {
            constructor(x, y) {
                this.i = x;
                this.j = y;
            }
            sum() {
                console.log(this.i + this.j);
            }
        }
        class Son extends Fa {
            constructor(x, y) {
                super(x, y);  //调用了父类中的 constructor
                this.x = x;
                this.y = y;
            }
        }
        var son = new Son(1, 2);
        son.sum();
```

>在继承中，如果子类有父类同名的方法，则执行的是子类自己的方法（就近原则）

 注意: **子类**在构造函数中**使用super**，必须**放到 this 前面**（必须先调用父类的构造方法，再使用子类构造方法）

 super.方法()
>手动在子类中调用父类的属性和方法

## 注意事项
* 在 ES6 中类没有变量提升（不像 function 之类），所以必须先定义类，才能通过类实例化对象
* 类里面的共有属性和方法一定要**加 this 使用**
* constructor 里面的 this 指向实例对象，方法里面的this 指向这个方法的调用者

---

# 构造函数和原型
>构造函数是一种特殊的函数，主要用来初始化对象，即为对象成员变量赋初始值，它总与 new 一起使用。我们可以把对象中一些公共的属性和方法抽取出来，然后封装到这个函数里面

new 在执行时会做四件事情：
* 在内存中创建一个新的空对象。
* 让 this 指向这个新的对象。
* 执行构造函数里面的代码，给这个新对象添加属性和方法。
* 返回这个新对象（所以构造函数里面不需要 return ）。


静态成员：在构造函数上添加的成员称为静态成员，只能由构造函数本身来访问 
实例成员：在构造函数内部创建的对象成员称为实例成员，只能由实例化的对象来访问
```
function Star(uname, age) {
	this.uname = uname;
	this.age = age;
}
//uname, age 为实例成员
var li = new Star('li', 20);
//实例成员通过实例化对象访问，在此即通过 li 访问，不能用 Star 访问
Star.sex = 'nan';
//上面代码即静态成员
```

构造函数有浪费内存的问题

## 构造函数原型 prototype
>每一个构造函数都有一个 prototype 属性，指向另一个对象。
>注意：这个 prototype 就是一个对象，这个对象的所有属性和方法，都会**被构造函数所拥有**。

一般情况下，**公共属性**定义到**构造函数**里面；**公共的方法**放到**原型对象**身上

```
    <script>
        function Star(uname, age) {
            this.uname = uname;
            this.age = age;
        }
        Star.prototype.sing = function () {
            console.log('我很强')
        }
        var li = new Star('li', 18);
        var zhao = new Star('zhao', 18);
        console.log(li.sing === zhao.sing);
        li.sing();
        zhao.sing();
    </script>
```

## 对象原型 \_\_proto\_\_
>对象都会有一个属性 \_\_proto\_\_ 指向构造函数的 prototype 原型对象，之所以对象可以使用构造函数 prototype 原型对象的属性和方法，就是因为对象有 \_\_proto\_\_ 原型的存在

* \_\_proto\_\_对象原型和原型对象 prototype 是等价的
* \_\_proto\_\_对象原型的意义就在于为对象的查找机制提供一个方向，或者说一条路线，但是它是一个非标准属性，因此实际开发中，不可以使用这个属性，它只是内部**指向**原型对象 prototype

先在实例对象上是否有相应的方法，若没有，则调用的是构造函数原型对象 prototype 里寻找

## constructor 构造函数
>对象原型（ \_\_proto\_\_）和构造函数（prototype）原型对象里面都有一个 **constructor 属性** ， 该属性被称为构造函数，因为它指回构造函数本身

主要用于记录该对象引用于哪个构造函数，它也可以让原型对象重新指向原来的构造函数

```
/*         Star.prototype.sing = function() 
            console.log('我会唱歌');
        } */
        Star.prototype = {
            //该操作会覆盖原来的原型对象
            constructor: Star,      //重新指向 Star 这个构造函
            sing: function() {
                console.log('我会唱歌');
            },
            song: function() {
                console.log('我会演戏');
            }
        }
```

## 原型链

实例对象（new 出来的对象） --（实例对象.\_\_proto\_\_）--  原型对象.prototype（function 对象() { ... }）--（原型对象.prototype.\_\_proto\_\_）--  Object 原型对象.prototype --（Object 原型对象.prototype.\_\_proto\_\_）-- null

## 原型对象 this 指向
谁调用指向谁

## 拓展内置对象方法
```
        Array.prototype.sum = function() {
            var sum = 0;
            for(var i = 0; i < this.length; i++) {
                sum += this[i];
            }
            return sum;
        }
        console.log(Array.prototype);
        var arr = [1, 2, 5];
        console.log(arr.sum());
```

```
Array.prototype = {
	sum: function() {
		    var sum = 0;
            for(var i = 0; i < this.length; i++) {
                sum += this[i];
            }
            return sum;
	}
}
//该方法直接覆盖掉了原来的对象，constructor 也没有了，调用的时候也找不到该方法，不能这样拓展对象
```

# 继承

## call 方法
```
        function fn(x, y) {
            console.log(this);
            console.log(x + y);
        }
        var o = {
            name: 'li'
        }
	    fn.call();    //直接调用函数
	    fn.call(o, 2, 5);    //this 指向 o，2和5 是参数
```

## 利用父构造函数继承**属性**
```
        //父构造函数
        function Father(uname, age) {
            this.uname = uname;
            this.age = age;
        }
        //子构造函数
        function Son(uname, age) {
            Father.call(this, uname, age);
            //this.uname = uname;  这时直接添加了该属性
        }
        var son = new Son('li', 18);
        console.log(son);
```

## 利用原型对象继承**方法**
```
        //父构造函数
        function Father(uname, age) {
            this.uname = uname;
            this.age = age;
        }
        //子构造函数
        function Son(uname, age, score) {
            Father.call(this, uname, age);  //this.uname = uname;  这时直接添加了该属性
            this.score = score;
        }
        Son.prototype = new Father();       //让子原型对象指向父构造函数的实例对象，可让子构造函数也有父构造函数的方法
        Son.prototype.constructor = Son;    //但需要指回原来的构造函数
        Son.prototype.hei = function() {
            console.log('heihei');
        }
        var son = new Son('li', 18, 99);
        console.log(son);
        console.log(Son);
```

## 类的本质
>一个函数

1. class本质还是function.
2. 类的所有方法都定义在类的 prototype 属性上
3. 类创建的实例,里面也有__proto__指向类的 prototype 原型对象
4. ES6的类它的绝大部分功能，ES5都可以做到，新的class写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已。
5. 所以ES6的类其实就是语法糖.
>语法糖：一种便捷写法，即有两种方法可以实现同样的功能, 但是一种写法更加清晰、方便，那么这个方法就是语法糖


# ES5 中的新增方法

## 数组方法
迭代(遍历)方法：forEach()、map()、filter()、some()、every()

### forEach()
array.forEach(function(currentValue, index, arr) { ... })
* currentValue：数组**当前项**的值
* index：数组当前项的索引
* arr：数组对象本身

### filter()
array.filter(function(currentValue, index, arr) { ... })
 * filter() 方法创建一个新的数组，新数组中的元素是通过检查指定数组中符合条件的所有元素,主要用于**筛选数组**，它直接返回一个新数组
 * 需要 return 值(true or false)

### some()
array.some(function(currentValue, index, arr) { ... })
* some() 方法用于检测数组中的元素是否满足指定条件
* 注意它返回值是布尔值, 如果查找到这个元素, 就返回true ,  如果查找不到就返回false
* 如果找到第一个满足条件的元素,则终止循环. 不再继续查找.
>在 some 里，遇到 return true 就终止遍历，迭代效率更高，而 forEach、filter 则不会直接终止

## 字符串方法

### trim() 去除字符串左右两边空格
>不会影响原字符串，返回一个新字符串


## 对象方法

### Object.keys() 用于获取对象自身所有的属性
Object.keys(obj)
>返回一个由属性名组成的数组

### Object.defineProperty() 定义对象中新属性或修改原有的属性
Object.defineProperty(obj, prop, descriptor)
* obj：必需。目标对象 
* prop：必需。需定义或修改的属性的名字
* descriptor：必需。目标属性所拥有的特性（对象形式）
	* value: 设置属性的值  默认为undefined
	* writable: 值是否可以重写。true | false  默认为false
	* enumerable: 目标属性是否可以被枚举。true | false 默认为 false
	* configurable: 目标属性是否可以被删除或是否可以再次修改特性 true | false  默认为false

---

# 函数进阶

## 函数定义
* 命名函数    function fn() { ... }
* 匿名函数    var fn = function() { ... }
* 利用 new Function    var fn = new Function('参数1','参数2'..., '函数体')    （执行效率低，不方便书写，少用）
>所有函数都是 Function 的实例(对象)
>函数也属于对象

## 函数的调用
```
        //普通函数
        function fn() {
            console.log('调用函数');
        }
        fn(); fn.call();
        //对象的方法
        var o = {
            sayHi: function () {
                console.log('调用函数');
            }
        }
        o.sayHi();
        //构造函数
        function Star() {};
        new Star();
        //绑定事件函数
        btn.onclick = function() {};
        //定时器函数
        setInterval(function(){}, 1000);
        //立即执行函数
        (function(){
            console.log('调用函数');
        })();
```

## 函数内 this 的指向

### 改变函数内部 this 指向
>常用的有 bind()、call()、apply() 三种方法

**apply()
fun.apply(thisArg, \[argsArray\])**
* thisArg：在fun函数运行时指定的 this 值
* argsArray：传递的值，必须包含在数组里面
* 返回值就是函数的返回值，因为它就是调用函数
* 因此 apply 主要跟数组有关系，比如使用 Math.max() 求数组的最大值
```
        var arr = [21, 36, 15, 32, 13, 20];
        var max = Math.max.apply(Math, arr);
        var min = Math.min.apply(Math, arr);
        console.log(max);
        console.log(min);
```

**bind()    重点记住
fun.bind(thisArg, arg1, arg2, ...)**
* thisArg：在 fun 函数运行时指定的 this 值
* arg1，arg2：传递的其他参数
* 返回由指定的 this 值和初始化参数改造的**原函数拷贝**
* 因此当我们只是想改变 this 指向，并且不想调用这个函数的时候，可以使用 bind
>***重点***
```
<body>
    <button>按钮</button>
    <button>按钮</button>
    <button>按钮</button>
    <script>
        var btns = document.querySelectorAll('button');
        for (var i = 0; i < btns.length; i++) {
            btns[i].addEventListener('click', function () {
                this.disabled = true;
                setTimeout(function () {
                    this.disabled = false;
                }.bind(this), 3000)     //在定时器函数外，this 指向的是 btns 这个对象
            })
        }
    </script>
</body>
```

### call apply bind 总结

主要应用场景
* call 经常做继承. 
* apply 经常跟数组有关系.  比如借助于数学对象实现数组最大值最小值
* bind  不调用函数,但是还想改变this指向. 比如改变定时器内部的this指向


# 严格模式
* 消除了 Javascript 语法的一些不合理、不严谨之处，减少了一些怪异行为。
* 消除代码运行的一些不安全之处，保证代码运行的安全。
* 提高编译器效率，增加运行速度。
* 禁用了在 ECMAScript 的未来版本中可能会定义的一些语法，为未来新版本的 Javascript 做好铺垫。比如一些保留字如：class, enum, export, extends, import, super 不能做变量名

*为脚本开启*
为整个脚本文件开启严格模式，需要在所有语句之前放一个特定语句“use strict”;（或‘use strict’;）
```
<script>
　　"use strict";
　　console.log("这是严格模式");
</script>
```
*为函数开启*
要给某个函数开启严格模式，需要把“use strict”;  (或 'use strict'; ) 声明放在函数体所有语句之前
```
function fn(){
　　"use strict";
　　return "这是严格模式。";
}
```

禁止删除已声明变量

* 以前在全局作用域函数中的 this 指向 window 对象。
* 严格模式下全局作用域中函数中的 this 是 undefined。
* 以前构造函数时不加 new也可以 调用,当普通函数，this 指向全局对象
* 严格模式下,如果 构造函数不加new调用, this 指向的是undefined 如果给他赋值则 会报错
* new 实例化的构造函数指向创建的对象实例。
* 定时器 this 还是指向 window 。
* 事件、对象还是指向调用者。

# 高阶函数
>高阶函数是对其他函数进行操作的函数，它接收函数作为参数或将函数作为返回值输出
>函数也是一种数据类型，同样可以作为参数，传递给另外一个参数使用。 最典型的就是作为回调函数。
>同理函数也可以作为返回值传递回来


# 闭包
>1. 函数内部可以使用全局变量。
>2. 函数外部不可以使用局部变量。
>3. 当函数执行完毕，本作用域内的局部变量会销毁。

闭包（closure）指有权访问另一个函数作用域中变量的**函数**

>延伸了函数的作用范围


# 递归函数
>如果一个函数在内部可以调用其本身，那么这个函数就是递归函数
>递归函数的作用和循环效果一样
>由于递归很容易发生“栈溢出”错误（stack overflow），所以必须要加退出条件 return

## 浅拷贝
拷贝对象级别的数据是拷贝地址，更改数据时会使地址里原来的数发生改变

Object.assign(目标对象, 被拷贝对象)
```
    <script>
        var obj = {
            name: 'li',
            age: 18,
            msg: {
                height: 170
            }
        };
        var o = {};
        //for in 遍历浅拷贝
/*         for (var k in obj) {
            o[k] = obj[k];
        }
        console.log(o);
        o.msg.height = 175;
        console.log(obj); */
        console.log('----------');
        //浅拷贝语法糖 Object.assign(o, obj)
        Object.assign(o, obj);
        console.log(o);
        o.msg.height = 175;
        console.log(obj);
    </script>
```

## 深拷贝
```
    <script>
        var obj = {
            name: 'li',
            age: 18,
            msg: {
                height: 170
            },
            color: ['black', 'blue', 'red']
        };
        var o = {};
        function deepCopy(newObj, oldObj){
            for(var k in oldObj) {
                var item = oldObj[k];
                if(item instanceof Array) {
                    newObj[k] = [];
                    deepCopy(newObj[k], item);
                } else if (item instanceof Object) {
                    newObj[k] = {};
                    deepCopy(newObj[k], item);
                } else {
                    newObj[k] = item;
                }
            }
            return newObj;
        }
        console.log(deepCopy(o, obj)); 
        o.msg.height = 175;
        console.log(obj);
    </script>
```

# 正则表达式
>正则表达式（ Regular Expression ）是用于匹配字符串中字符组合的模式。
>在 JavaScript中，正则表达式也是对象。

**匹配、替换、提取**

## 创建正则表达式

* var 变量名 = new RegExp(/表达式/);        通过调用 RegExp 对象的构造函数创建
*  var 变量名 = /表达式/;        通过字面量创建

## 测试正则表达式 test
>test() 正则对象方法，用于检测字符串是否符合该规则，该对象会返回 true 或 false，其参数是测试字符串

正则表达式.test(字符串)

## 组成
>一个正则表达式可以由简单的字符构成，比如 /abc/，也可以是简单和特殊字符的组合，比如 /ab\*c/ 。其中特殊字符也被称为元字符，在正则表达式中是具有特殊意义的专用符号，如 ^ 、$ 、+ 等

特殊字符非常多，可以参考： 
* MDN: https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions
* jQuery 手册：正则表达式部分
* 正则测试工具: http://tool.oschina.net/regex

### 边界符
* ^    表示匹配行首的文本（以谁开始）
* $    表示匹配行尾的文本（以谁结束）
>如果 ^ 和 $ 在一起，表示必须是精确匹配

### 字符类
>字符类表示有一系列字符可供选择，只要匹配其中一个就可以了。所有可供选择的字符都放在方括号内

`[]
`var rg = /[abc]/
匹配字符串中只要含有 a 或 b 或 c 任意一个都返回 true
（多选一）
``var rg1 = /^[abc]$/
`rg1.test('abc')  // false
只有开头和结尾 是 a，b，c 的**其中一个**才返回 true

`[-]  
方括号内部 范围符-
`[a-z]
`var rg = /^[a-z]$/
>表示 26 个小写字母中的任意**一个**都返回 true

#### 字符组合
`var rg = /^[a-zA-Z0-9_-]$/
连着写

`[^]
方括号内取反符 ^
`var rg = /^[^a-zA-Z0-9_-]$/

### 量词符
* *    重复零次或更多次
* +    重复一次或更多次
* ?    重复零次或一次
>*写在要匹配的字符后*
`/^a*$/ 允许 a 出现零次或多次（不含其它字符）

* {3}    需要重复3次
* {3,}    大于等于3次
* {3,16}    大于等于3次并且小于16次
`/^a{3,}$/ 允许 a 出现3次以上（不含其它字符）
>大括号内不能有空格

`var rg = /^[a-zA-Z0-9_-]{3,16}$/;
`console.log(rg.test('ef5gf'));    //返回 true

## 括号总结
* 大括号 量词符，里面表示重复次数
* 中括号 字符集合，匹配方括号中的任意字符
* 小括号 表示优先级
`var rg = /^abc{3}$/  //只让 c 重复三次
 `console.log(rg.test('abccc'));  //true
`console.log(rg.test('abcabc'));  //false
`var rg1 = /^(abc){3}$/
`console.log(rg1.test('abcabcabc'));  //true

>https://c.runoob.com/    在线测试网站

## 正则预定义类
`\d   匹配 0-9 之间的数字，相当于 [0-9]
`\D    匹配 0-9 之外的字符，相当于 [^0-9]
`\w    匹配任意的字母、数字和下划线，相当于 [A-Za-z0-9_]
`\W    匹配任意的字母、数字和下划线，相当于 [^A-Za-z0-9_]
`\s    匹配空格（包括换行符、制表符、空格符等），相当于[\t\r\n\v\f]
`\S    匹配非空格的字符，相当于 [^\t\r\n\v\f]

>正则里加 | 表示或

## 正则表达式中的替换 replace()
>replace() 方法可以实现替换字符串操作，用来替换的参数可以是一个字符串或是一个正则表达式

被替换对象.replace(正则表达式或字符串, 替换成的字符串)
>返回一个新字符串

## 正则表达式中的参数

`/表达式/[switch]
switch(也称为修饰符) 按照什么样的模式来匹配. 有三种值：
* g    全局匹配 
* i    忽略大小写 
* gi    全局匹配 + 忽略大小写

-----
# Ajax 正则内容（模板引擎）

## 匹配正则表达式
exec() 函数用于检索字符串中的正则表达式的匹配。
如果字符串中有匹配的值，则返回该匹配值（一个对象），否则返回 null。

RegExpObject.exec(string)
正则表达式.exec(检索的字符串)

## 分组
>正则表达式中( )包起来的内容表示一个分组，可以通过分组来**提取**自己想要的内容

 `var pattern = /{{([a-zA-Z]+)}}/`
`+` 表示大于等于 1

```
        let str = '<div>我是{{name}}</div>';
        let pattern = /{{([a-zA-Z]+)}}/;
        const result = pattern.exec(str);
        console.log(result);
        str = str.replace(result[0], result[1]);
        console.log(str);
```

## 多次 replace
```
        let str = '<div>{{name}}今年{{ age }}岁了</div>'
        let pattern = /{{\s*([a-zA-Z]+)\s*}}/
        console.log(pattern.exec(str));

        let rest1 = pattern.exec(str);
        str = str.replace(rest1[0],rest1[1]);
        console.log(str);

        let rest2 = pattern.exec(str);
        str = str.replace(rest2[0],rest2[1]);
        console.log(str);

        let rest3 = pattern.exec(str);
        // str = str.replace(rest3[0],rest3[1])
        console.log(rest3);
```

## while 循环 replace 多次
```
        let str = '<div>{{name}}今年{{ age }}岁了</div>'
        let pattern = /{{\s*([a-zA-Z]+)\s*}}/
        console.log(pattern.exec(str));

        let result = null;
        while(result = pattern.exec(str)) {
            str = str.replace(result[0],result[1]);
        }
        console.log(str);
```

## 替换为真值
```
        const data = {
            name: 'zhao',
            age: 18
        }
        let str = '<div>{{name}}今年{{ age }}岁了</div>'
        let pattern = /{{\s*([a-zA-Z]+)\s*}}/
        console.log(pattern.exec(str));

        let result = null;
        while(result = pattern.exec(str)) {
            str = str.replace(result[0],data[result[1]]);
        }
        console.log(str);
```