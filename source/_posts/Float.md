---
title: Float
catalog: true
date: 2018-12-13 23:45:18
subtitle:
header-img:
tags: [html, css, float, clear, BFC]
---
## 浮动

> 浮动的本质，就是实现文字环绕效果

### 浮动的表现形式
- 元素block化
- 破坏性造成的紧密排列特性(去空格化)

![](https://img2.mukewang.com/5c1216fb00014ca912800720-156-88.jpg)

### 砌砖布局的问题

- 容错性比较糟糕，容易出问题
- 这种布局需要元素固定尺寸，很难重复使用
- 在低版本IE下会有很多问题



### 浮动与流体布局

- 文字环绕
```
float:left;
```
- 左右浮动，中间居中布局
```
左：float: left
右：float: right
中间：text-align: center
```
- 文字环绕衍生-单侧固定
```
固定元素：width+float;
右侧元素：padding-left/margin-left;
```
- DOM与显示位置匹配的单侧固定布局

```
左侧父元素：width：100% + float
左侧子元素：padding-left/margin-left
右侧固定元素：width + float + margin负值
```

- 智能自适应布局

```
左侧：float
右侧：display:table-cell(IE8+)
      *display:inline-block(IE7)
```


### Float与兼容性


#### 让IE7飙泪的浮动问题
1. 含clear的浮动元素包裹不正确的问题
> IE7的表现   

![](https://img1.mukewang.com/5c121f290001f54b12800720.jpg)

> IE8+的表现   

![](https://img2.mukewang.com/5c121f710001230212800720.jpg)

2. 浮动元素倒数2哥莫名垂直间距问题
> IE7的表现   

![](https://img3.mukewang.com/5c121fcf0001f57512800720.jpg)

> IE8+的表现   

![](https://img.mukewang.com/5c12204500013fc612800720.jpg)



3. 浮动元素最后一个自负重复问题
> IE7的表现   

![](https://img4.mukewang.com/5c12208400012d9212800720.jpg)

> IE8+的表现   

![](https://img4.mukewang.com/5c1220c90001f59112800720.jpg)




4. 浮动元素楼梯排列问题
> IE7的表现   

![](https://img3.mukewang.com/5c1221150001017312800720.jpg)

> IE8+的表现   

![](https://img3.mukewang.com/5c12213e0001d12e12800720.jpg)




5. 浮动元素和文本不在同一行的问题
> IE7的表现   

![](https://img1.mukewang.com/5c1221b90001680b12800720.jpg)

> IE8+的表现   

![](https://img2.mukewang.com/5c1221da000104e512800720.jpg)