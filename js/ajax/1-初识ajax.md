### 初识ajax

------

[TOC]

------



### 1.简介

- AJAX = Asynchronous JavaScript and XML（异步的 JavaScript 和 XML）。
- AJAX 不是新的编程语言，而是一种使用现有标准的新方法。
- AJAX 是与服务器交换数据并更新部分网页的艺术，在不重新加载整个页面的情况下。
- AJAX 的核心是 XMLHttpRequest 对象。

### 2.ajax 编程步骤

- 创建XMLHttpRequest对象。

- 设置请求方式。

- 发送请求。

- 调用回调函数。

  

​      b.设置请求方式

```
第二步
new XMLHttpRequest().open(参数1, 参数2, 参数3)

参数1：设置请求类型（GET 或 POST），GET与POST的区别请自己百度一下，这里不做解释；
参数2：文件在服务器上的位置；
参数3：是否异步处理请求，是为true，否为false。
```

​		get和post方式对比，

​		与 POST 相比，GET 更简单也更快，并且在大部分情况下都能用。然而，在以下情况中，请使用 POST 请求

> 无法使用缓存文件（更新服务器上的文件或数据库）
> 向服务器发送大量数据（POST 没有数据量限制）
> 发送包含未知字符的用户输入时，POST 比 GET 更稳定也更可靠

​	 c.发送请求

​		当请求方式为GET的时候，发送请求为xhr.send(null).
​        当请求方式为POST的时候，发送请求为xhr.send(请求数据)。
​         此外使用POST的时候必须在new XMLHttpRequest().send（请求数据）上面添加传输格式

```js
new XMLHttpRequest().setRequestHeader("Content-Type", "application/x-www-form-urlencoded"); 
```

 	d.注册（调用）回调函数

​		如果是异步请求就会要有回调函数。，XMLHttpRequest对象有一个onreadystatechange属性，这个属性返回的是一个匿名的方法，所以回调函数就在这里写new XMLHttpRequest().onreadystatechange=function{},function{}内部就是回调函数的内容。

```js
//第三步：注册回调函数
new XMLHttpRequest().onreadystatechange = function() {
    if (new XMLHttpRequest().readyState == 4) {
        if (new XMLHttpRequest().status == 200) {
            var obj = document.getElementById(id);
            obj.innerHTML = new XMLHttpRequest().responseText;
        } else {
            alert("AJAX服务器返回错误！");
        }
    }
}
```

   	上面readyState从 0 到 4 发生变化。0: 请求未初始化。1: 服务器连接已建立。2: 请求已接收。3: 请求处理中。4: 请求已完成，且响应已就绪。所以这里我们判断只有当xmlHttp.readyState为4的时候才可以继续执行。

​	总的如下

```js
//var xmlHttp=new XMLHttpRequest();
//第一步：创建XMLHttpRequest对象
var xmlHttp;
if (window.XMLHttpRequest) {            //非IE
    xmlHttp = new XMLHttpRequest();
} else if (window.ActiveXObject) {       //IE
    xmlHttp = new ActiveXObject("Microsoft.XMLHTTP")
}
//第二步
xmlHttp.open("POST", url, true)

//第三步
new XMLHttpRequest().send(null)//get
new XMLHttpRequest().send(parm)//post

//第四步：注册回调函数
new XMLHttpRequest().onreadystatechange = function() {
    if (new XMLHttpRequest().readyState == 4) {
        if (new XMLHttpRequest().status == 200) {
            var obj = document.getElementById(id);
            obj.innerHTML = new XMLHttpRequest().responseText;
        } else {
            alert("AJAX服务器返回错误！");
        }
    }
}

```

最后各种封装的ajax[【vue学习】axios - 简书 (jianshu.com)](https://www.jianshu.com/p/d771bbc61dab)