---
title: Location-Object
date: 2017-05-09 15:49:34
tags: javascript
---

- 它提供了与当前窗口中加载的文档有关的信息，还提供了一些导航功能。
- `location`对象很特别，它既是`window`对象的属性，又是`document`对象的属性。
```
window.location === document.location  //true
```
- location对象的用处不只表现在它保存当前文档和信息，还表现在它将URL解析为独立的片段。

<!-- more -->

> 属性

属性名 | 例子 | 说明
---|--- | ---
hash | "#contents" | 返回URL中hash(#号后跟零或多个字符)，如果URL中不包含散列，则返回空字符串
host | "www.qxy.com:80" | 返回服务器名称和端口号（如果有）
hostname | "www.qxy.com" | 返回不带端口号的服务器名称
href | "http://www.qxy.com" | 返回当前加载页面的完整URL。而location对象的toString()方法也返回这个值
pathname | "/blog/" | 返回URL中的目录和（或）文件名
port | "8080" | 返回URL中指定的端口号。如果URL中不包含端口号，则这个属性返回空字符串
protocol | "http:" | 返回页面使用的协议。通常是http:或https:
search | "?q=javascript" | 返回URL的查询字符串。这个字符串以问号开头

#### 查询字符串参数

虽然通过上面的属性可以访问到location 对象的大多数信息，但其中访问URL 包含的查询字符
串的属性并不方便。尽管location.search 返回从问号到URL 末尾的所有内容，但却没有办法逐个
访问其中的每个查询字符串参数。为此，可以像下面这样创建一个函数，用以解析查询字符串，然后返
回包含所有参数的一个对象：
```
function getQueryStringArgs(){
    //取得查询字符串并去掉开头的问号
    var qs = (location.search.length > 0 ? location.search.substring(1) : "" ),
        //保存数据的对象
        args = {},
        //取得每一项
        items = qs.length ? qs.split('&') : [],
        item = null,
        name = null,
        value = null,
        //在for循环中使用
        i = 0,
        len = items.length;
        
    //逐个将每一项添加到args对象中
    
    for(i = 0;i < len; i++){
        item = items[i].split(':');
        name = decodeURIComponent(item[0]);
        value = decodeURIComponent(item[1]);
        
        if(name.length){
            args[name] = value;
        }
    }
    return args;
}
```

#### 位置操作

使用location 对象可以通过很多方式来改变浏览器的位置。首先，也是最常用的方式，就是使用
assign()方法并为其传递一个URL，如下所示。
```
location.assign("http://www.qxy.com");
```

这样，就可以立即打开新URL 并在浏览器的历史记录中生成一条记录。如果是将`location.href`
或`window.location` 设置为一个URL 值，也会以该值调用`assign()`方法。例如，下列两行代码与
显式调用`assign()`方法的效果完全一样。
```
window.location = "http://www.wrox.com";
location.href = "http://www.wrox.com";
```
在这些改变浏览器位置的方法中，最常用的是设置`location.href `属性。

修改location 对象的其他属性也可以改变当前加载的页面
```
//假设初始URL 为http://www.wrox.com/WileyCDA/
//将URL 修改为"http://www.wrox.com/WileyCDA/#section1"
location.hash = "#section1";
//将URL 修改为"http://www.wrox.com/WileyCDA/?q=javascript"
location.search = "?q=javascript";
//将URL 修改为"http://www.yahoo.com/WileyCDA/"
location.hostname = "www.yahoo.com";
//将URL 修改为"http://www.yahoo.com/mydir/"
location.pathname = "mydir";
//将URL 修改为"http://www.yahoo.com:8080/WileyCDA/"
location.port = 8080;
```
每次修改location 的属性（hash 除外），页面都会以新URL 重新加载。
> 在IE8、Firefox 1、Safari 2+、Opera 9+和Chrome 中，修改hash 的值会在浏览
器的历史记录中生成一条新记录。在IE 的早期版本中，hash 属性不会在用户单击“后
退”和“前进”按钮时被更新，而只会在用户单击包含hash 的URL 时才会被更新。

> replace()方法。这个方法
只接受一个参数，即要导航到的URL；结果虽然会导致浏览器位置改变，但不会在历史记录中生成新记
录。在调用replace()方法之后，用户不能回到前一个页面
```
setTimeout(function () {
location.replace("http://www.wrox.com/");
}, 1000);
```

> reload()，作用是重新加载当前显示的页面

如果调用reload()
时不传递任何参数，页面就会以最有效的方式重新加载。也就是说，如果页面自上次请求以来并没有改
变过，页面就会从浏览器缓存中重新加载。如果要强制从服务器重新加载，则需要像下面这样为该方法
传递参数true。
```
location.reload(); //重新加载（有可能从缓存中加载）
location.reload(true); //重新加载（从服务器重新加载）
```
位于reload()调用之后的代码可能会也可能不会执行，这要取决于网络延迟或系统资源等因素。
为此，最好将reload()放在代码的最后一行。