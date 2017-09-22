---
title: 'string-function.md'
date: 2017-09-22 09:58:05
tags: [javascript, string]
---

### 操作方法

##### 字符串截取sub,substing,slice

- 一个参数

```javascript
 var str = 'what are you doing?';
 //参数为正数
 str.sub(1);        //'hat are you doing?'
 str.substring(1);  //'hat are you doing?'
 str.slice(1);      //'hat are you doing?'
 //参数为负数
 str.sub(-1);       //'?'
 str.substring(-1); //'what are you doing?'
 str.slice(-1);     //'?'
```
> sub,slice支持参数为正数和负数(等价于字符串长度+负数)，
substring只支持参数为正数，如果为负数会返回整个字符串

<!-- more -->

- 两个参数

```javascript
var str = 'what are you doing?';
// 两个参数都为正数
 str.substr(1,4);      //'hat '
 str.substring(1,4);   //'hat'
 str.slice(1,4);       //'hat'

// 有负数参数
 str.substr(-5,4);  //'oing'
 str.substring(-5,4);//'what'
 str.slice(-5,4);    //''
```

> substr，第二个参数代表的是截取字符串的长度，第二个参数不能为负数;
substing，参数为负数会自动转换为0，并且数值小的为起点。
slice，第二个参数的数值小于第一个参数的数值返回空值。


