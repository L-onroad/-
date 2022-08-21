# let 变量声明

* 变量不能重复声明
* 块级作用域
* 不存在变量提升
* 不影响作用域链

# const 声明常量
* 一定要赋初始值
* 常量一般用大写表示（约定俗成）
* 值不能修改
* 块级作用域
* 修改常量定义的数组和对象里的元素不会报错
>所以一般用来声明数组和对象

# 变量的解构赋值
```
        //数组的解构
        // const F = ['A','B','C','D'];
        // let [a,b,c,d] = F;
        // console.log(a);
        // console.log(b);
        // console.log(c);
        // console.log(d);
        
        //对象的解构
        const Z = {
            name: 'li',
            age: 18,
            ab: function(){
                console.log('play game');
            }
        }
        // let {name, age, ab} = Z;
        // console.log(name);
        // console.log(age)
        // console.log(ab);
        // ab();
        let {ab} = Z;
        ab();   //简略写法
```

# 模板字符串
>ES6 引入新声明字符串的方式   \`字符串\`  反引号

* 内容中可以直接出现换行符
* 拼接字符串
	* \`${变量}...其他内容\`    将外部变量的值与后面的部分直接拼接

# 对象的简化写法
```
    <script>
        let name = 'zhao';
        let age = 18;
        let fn = function() {
            console.log('定义的函数');
        }
        const obj = {
            name,
            age,
            fn,
            fn1(){
                console.log('对象函数的简写');
            }
        }
    </script>
```

# 箭头函数及声明特点

^bf15ef

## 声明：
`let fn = (形参) => {`
`    函数体`
`}`

## 特点
* this 是静态的，箭头函数的 this 指向始终指向**函数声明时所在作用域**下的 this
* 不能构造实例化对象（构造函数）
* 不能使用 arguments 变量（存储实参的变量）

## 简写
* 当形参有且只有一个的时候可以简写
* 当代码体只有一条语句的时候，可以省略花括号
	* 注意：此时 return 也必须省略，语句执行结果就是函数的返回结果
`let pow = n => n * n;`
`console.log(pow(3));`

## 适用场景
* 适合用于与 this 无关的回调    eg. 定时器，数组方法的回调
* 不适合用于与 this 有关的回调    eg. 事件回调，对象方法

# 函数参数默认值设置

`function(a, b, c = 10) { ... }`
此时调用函数若无第三个参数传入，则 c 默认为10
（具有默认值的参数一般位置靠后）

结构赋值的运用
```
    <script>
        function obj({ host, username, password, port }) {      //属性名要一一对应（相同）
            console.log(host);
            console.log(username);
            console.log(password);
            console.log(port);
        }
        obj({
            host: '127.0.0.1',
            username: 'root',
            password: 'root',
            port: 3306
        })
    </script>
```

# rest 参数（es6 新增）
形式  `function fn(...args) { ... }`
意义和 arguments 相似
没被接收的实参都放在 `...args` 里，以数组的形式存储
必须放到最后
```
        function fn(a,b,...ok){
            console.log(a);
            console.log(b);
            console.log(ok);    //ok为[3,4,5,6]
        }
        fn(1,2,3,4,5,6);
```

# 扩展运算符
## 形式 `...实参`
调用函数传带有扩展运算符的实参，在函数内部将会被分割为参数序列
```
        const arr = ['a','b','c','d','e'];
        function fn() {
            console.log(arguments);
        }
        fn(...arr);
        //打印出来的是五个内容，不加 ... 则打印的是一个数组
```

## 运用
* 合并数组
```
        const a = ['hei','hi'];
        const b = ['o','oi'];
        // const c = a.concat(b);       //旧方法
        const c = [...a,...b];      //可以合并数组
        console.log(c);
```
* 克隆数组（浅克隆）
```
        const a = ['r','e','d'];
        const b = [...a];
        console.log(b);
```
* 将伪数组转为真正的数组
```
<body>
    <div></div>
    <div></div>
    <div></div>
    <script>
        const divs = document.querySelectorAll('div');   
//伪数组
        const divArr = [...divs];   //经过转换变为真数组
        console.log(divArr);
    </script>
</body>
```

# Symbol 数据类型
* Symbol 的值是唯一的，用来解决命名冲突的问题
* Symbol 值不能与其他数据进行运算
* Symbol 定义的对象属性不能使用 for…in 循环遍历，但是可以使用 Reflect.ownKeys 来获取对象的所有键名
>与身份证差不多含义

创建
`let s = Symbol('描述字符串');`
`let s = Symbol.for();`

```
    <script>
        let s1 = Symbol();
        console.log(s1, typeof s1);
        let s2 = Symbol('a');
        let s3 = Symbol('a');
        console.log(s2 === s3);     //返回 false
        let s4 = Symbol.for('a');
        let s5 = Symbol.for('a');
        console.log(s4 === s5);     //返回 true
    </script>
```

利用 Symbol 要用 `[]`括起来
>使用 Symbol 数据作为对象的属性都要用 `[]` 而不是用` .`

Symbol 内置属性
看文档

# 迭代器
>遍历器（Iterator）就是一种机制。它是一种接口，为各种不同的数据结构提供统一的访问机制。任何数据结构只要部署 Iterator 接口，就可以完成遍历操作

`for ( let v of 数组) { ... }`
该方法可以遍历数组（涉及 Symbol.iterator 方法）
>for ... in ...    获取的是键名
>for ... of ...    获取的是键值

原生具备 iterator 接口的数据(可用 for of 遍历)
Array、Arguments、Set、Map、String、TypedArray、NodeList

## 工作原理
1. 创建一个指针对象，指向当前数据结构的起始位置
2. 第一次调用对象的 next 方法，指针自动指向数据结构的第一个成员
3. 接下来不断调用 next 方法，指针一直往后移动，直到指向最后一个成员
4. 每调用 next 方法返回一个包含 value 和 done 属性的对象
*注: 需要自定义遍历数据的时候，要想到迭代器*

```
    <script>
        //声明一个对象
        const banji = {
            name: "终极一班",
            stus: [
                'xiaoming',
                'xiaoning',
                'xiaotian',
                'knight'
            ],
            //调用 Symbol.iterator 方法
            [Symbol.iterator]() {
                //索引变量
                let index = 0;
                let _this = this;
                return {
                    next: function () {
                        if (index < _this.stus.length) {
                            const result = { value: _this.stus[index], done: false };
                            //下标自增
                            index++;
                            //返回结果
                            return result;
                        }else{
                            return {value: undefined, done: true};
                        }
                    }
                };
            }
        }
        //遍历这个对象 
        for (let v of banji) {
            console.log(v);
        }
    </script>
```

# 生成器
>特殊的函数、异步编程 纯回调函数 node ajax mongodb

形式
`function * gen() { ... }`
```
    <script>
        function * gen() {
            // console.log('a');
            yield '第一个分隔符'
            // console.log('b');
            yield '第二个分隔符'
            // console.log('c');
            yield '第三个分隔符'
            // console.log('d');
        }
    let iterator = gen();
    console.log(iterator.next());
    console.log(iterator.next());
    console.log(iterator.next());
    console.log(iterator.next());
    </script>
    
    //输出结果
     {value: '第一个分隔符', done: false}
	 {value: '第二个分隔符', done: false}
	 {value: '第三个分隔符', done: false}
	 {value: undefined, done: true}
```

内部有 next 属性，迭代器
靠 `iterator.next()` 执行函数内部结果

yield 函数代码分隔符，依靠 next() 依次调用
iterator.next() 返回结果为 yield 的 value 以及 done 的状态

## 参数的传递
iterator.next(内容)
内容将一起作为上一个 yield 的整体返回结果

## 解决回调地狱
```
    function one() {
        setTimeout(() => {
            console.log(111);
            iterator.next();
        }, 1000);
    }
    function two() {
        setTimeout(() => {
            console.log(222);
            iterator.next();
        }, 2000);
    }
    function third() {
        setTimeout(() => {
            console.log(333);
            iterator.next();
        }, 3000);
    }
    function * gen(){
        yield one();
        yield two();
        yield third();
    }
    //调用生成器函数
    let iterator = gen();
    iterator.next();
```

## 实例2
```
        function getUsers() {
            setTimeout(() => {
                let data = '用户数据';
                iterator.next(data);
            }, 1000)
        }
        function getOrders() {
            setTimeout(() => {
                let data = '订单数据';
                iterator.next(data);
            }, 1000)
        }
        function getGoods() {
            setTimeout(() => {
                let data = '商品数据';
                iterator.next(data);
            }, 1000)
        }
        function * gen(){
            let users = yield getUsers();
            console.log(users);
            let orders = yield getOrders();
            console.log(orders);
            let goods = yield getGoods();
            console.log(goods);
        }
        let iterator = gen();
        iterator.next();
```

# Promise
>Promise 是 ES6 引入的异步编程的新解决方案。语法上 Promise 是一个**构造函数**，用来**封装异步操作**并可以获取其成功或失败的结果

```
        // 实例化 Promise 对象
        const p = new Promise(function (resolve, reject) {
            setTimeout(function () {
                // let data = '数据库中的用户数据';
                // resolve(data);

                let err = '数据读取失败'
                reject(err);
            }, 1000)
        })
        
        //调用 Promise 对象的 then 方法
        p.then(function(value){
            console.log(value);
        },function(reason){
            console.error(reason);
        })
```

Promise 对象中的参数是一个函数
该函数中有两个参数，用来改变 Promise 对象的状态
resolve()  表示成功
reject()  表示失败

调用 Promise 对象的 then 方法，该方法的两个参数都是函数形式，这两个函数里都有一个形参，成功的一般设为 value，失败的一般设为 reason 

Promise 状态为成功的时候（即调用了 resolve 方法的时候），则执行第一个函数的代码，此时 value 接收的值为 resolve 里面的内容
失败时（调用了 reject 方法），则执行第二个函数的代码，reason 的值即 reject 里面的内容

## Promise.prototype.then 方法
调用 then 方法，返回结果也是 Promise 对象，对象状态由回调函数的执行结果决定
1. 如果回调函数中返回的结果是 非Promise 类型的属性，状态为成功，返回值为对象的成功的值
2. 返回的为 Promise 对象，则返回的状态决定了 then 的状态
>这样的特性可以达到链式调用的效果

```
p.then(value=>{

}, reason=>{

}).then(value=>{

}, reason=>{

}).then(value=>{

}, reason=>{

})
```

## 读取多个文件
```
const { rejects } = require('assert');
const fs = require('fs');
const { resolve } = require('path');

//回调地狱
// fs.readFile('./真es6/读取对象/鹅鹅鹅.md', (err, data1) => {
//     fs.readFile('./真es6/读取对象/悯农.md', (err, data2) => {
//         fs.readFile('./真es6/读取对象/岳阳楼记.md', (err, data3) => {
//             let result = data1 + '\r\n' + data2 + '\r\n' + data3;
//             console.log(result);
//         })
//     })
// });

//使用 Promise 实现
const p = new Promise((resolve, rejects) => {
    fs.readFile('./真es6/读取对象/鹅鹅鹅.md', (err, data) => {
        resolve(data);
    })
})
p.then(value => {
    return new Promise((resolve, rejects) => {
        fs.readFile('./真es6/读取对象/悯农.md', (err, data) => {
            resolve([value, data]);
        })
    })
}).then(value => {
    return new Promise((resolve, rejects) => {
        fs.readFile('./真es6/读取对象/岳阳楼记.md', (err, data) => {
            value.push(data);
            resolve(value);
        })
    })
}).then(value => {
    console.log(value.toString());
})
```

## catch 方法
如果 p 对象状态为失败，
p.catch(function(reason){
		console.warn(reason);
})
可以不需要带 value函数 参数

# 数据结构 Set （集合）
## 声明
`let s = new Set();`
`let s1 = new Set(['a','b','c','a'])  //会自动去重，该数组长度为3`

类似数学上的集合概念，中括号里为元素

## API
`s.size`  元素个数
`s.add()`  添加新元素
`s.delete()`  删除元素
`s.has()`  检测

```
        // let s = new Set();
        // let s1 = new Set(['a','b','c','a']);
        // console.log(s1.size);
        // s1.add('d');
        // console.log(s1);
        // s1.delete('b');
        // console.log(s1);
        // console.log(s1.has('a'));

        //数组去重
        let arr = [1,3,5,2,3,4,5,6,8,7,6,2];
        // let result = [...new Set(arr)];
        // console.log(result);

        //交集
        let arr1 = [3,6,7,12,0];
        // let s2 = new Set(arr1);
        // let result = [...new Set(arr)].filter(item=>{
        //     if(s2.has(item)) {
        //         return true;
        //     } else {
        //         return false;
        //     }
        // });
        // console.log(result);
        // let result = [...new Set(arr)].filter(item=> new Set(arr1).has(item));
        // console.log(result);

        //并集
        let result = [...new Set([...arr,...arr1])];
        console.log(result);

        //差集
        let diff = [...new Set(arr)].filter(item=> !(new Set(arr1).has(item)));
        console.log(diff);
```

# Map 对象
>它类似于对象，也是键值对的集合。但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。Map 也实现了 iterator 接口，所以可以使用『扩展运算符』和『for…of…』进行遍历

## 声明 Map
`let m = new Map();`

## 添加元素
`m.set('name', 'li')  //常规键值对`
`let key = {`
`	school: 'G'`
`};`
`m.set(key, ['广州', '茂名','高州']);  //将对象作为键`

## API
`m.size 返回 Map 的元素个数`
`m.set 增加一个新元素，返回当前 Map`
`m.get('键名') 返回键名对象的键值`
`m.has() 检测 Map 中是否包含某个元素（键），返回 boolean 值`
`m.delete('键名') 删除相应元素`
`m.clear() 清空集合，返回 undefined`


# class
[[面向对象、ES6#^45894a]]

## get-set 读取属性
```
        class Phone {
            get price(){
                console.log('get用来读取属性');
                return 'return 返回值'
            }
            set price(newVal){
                console.log('价格被修改了');
            }
        }
        let s = new Phone();
        console.log(s.price);
        s.price = 'free'
```
使用 get 设置属性，返回值就是 return 里的值
set 设置属性，必须设置一个形参，传入形参时调用
（并不是很清楚用法）

# 数值扩展

Number.EPSILON 是 JavaScript 表示的最小精度
0b 二进制 | 0o 八进制 | 0x 十六进制
`Number.isFinite();` 检测数值是否为有限数
`Number.isNaN();` 判断是否为 NaN
`Number.parseInt() | Number.parseFloat()`    截取整数 | 浮点数
`Math.trunc()` 将小数部分抹掉
`Math.sign()` 判断一个数为正数，负数或者0，正数返回 1，负数返回 -1，0 返回 0

# 对象方法扩展
`Object.is()` 判断两个值是否全等
`Object.assign(对象1,对象2)` 对象的合并，若有相同属性，对象 2 会覆盖对象 1 中共有的属性
`Object.setPrototypeOf(对象1,对象2)` 将对象 2 设为对象 1 的原型
`Object.getPrototypeOf()` 获取对象的原型

# 模块化语法
模块化优点
* 防止命名冲突
* 代码复用
* 高维护性

`<script type="module"></script>` 接收暴露文件需要加 type="module"

export 暴露
* 在需要暴露的内容前面加上 export
* export{ 需要暴露的内容 }
* 默认暴露   
	* export default {
		键: 值
	}

import 导入
* import * as s1 from "路径"    通用导入方式
	* 将导入数据存储在 s1 中
* import { 外部变量名, ... } from "路径"
	* 可以直接使用外部变量名
	* 变量名冲突可以使用 as 起别名
	* import { 外部变量名 as 其他名, ... } from "路径"
	* 对于 default 暴露的使用  import { default as s } from "路径"    必须给 default 起别名
* import a from "路径"    默认暴露的简便写法

## Babel 编译器
>将新语法转换为浏览器能识别的语法

