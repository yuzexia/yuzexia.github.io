---
title: 解决vue props拿不到值的问题
catalog: true
date: 2019-01-10 19:35:39
subtitle:
header-img:
tags: 【vue, props, watch】
---

> vue开发中，子组件经常出现props中获取不到父组件传过来的值，发现以下两种方法可以解决这种问题

### 方法一

> 在父组件调用子组件时，设置`v-if="status"`，初始值设置为`false`,在成功获取数据之后设置为`true`

```
// 子组件
 <child :datas="conditionStatisticsData" v-if="status"></child>
// 成功获取数据后 status设置成true
 homeResource.getConditionData().then((res) => {
 this.status = true
  if (res.data.status === 0) {
   console.log('条件', res.data.data)
   this.conditionStatisticsData = res.data.data
  }
 })
```

### 方法二

> watch 监听`props`传递过来的值

```
watch: {
 data: function (val) {
   this.initdata(val)
  }
 }
```

> 希望能够帮得到大家。