[[本地存储的代码]]
> F12打开控制面板后在Application下方找到Storage可看到数据存储结果

[[记住用户名|案例]]

## 特性
* 数据存储在用户浏览器中
* 设置、读取方便、页面刷新时不丢失数据
* 容量较大，sessionStorage约5M、localStorage约20M
* 只能存储字符串，可以将对象JSON.stringfy()编码后存储

---

## window.sessionStorage对象
* 生命周期为关闭浏览器窗口
* 同一窗口下数据共享（不能在其他窗口使用）
* 键值对形式存储

* **方法**
	* 存储数据 sessionStorage.setItem(属性名, 值)
	* 获取数据 sessionStorage.getItem(属性名)
	* 删除数据 sessionStorage.removeItem(属性名)
	* 删除所有数据 sessionStorage.clear()

---

## window.localStorage对象
* 生命周期**永久**生效，除非手动删除，关闭页面也还会存在
* **多窗口**共享
* 键值对形式存储

* **方法**
	* 存储数据 localStorage.setItem(属性名, 值)
	* 获取数据 localStorage.getItem(属性名)
	* 删除数据 localStorage.removeItem(属性名)
	* 删除所有数据 localStorage.clear()


