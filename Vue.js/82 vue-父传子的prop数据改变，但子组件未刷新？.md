## 问题描述

如果子组件接收到父组件传递的`prop`数据在created 或 mounted 中处理（比如赋值）的话，当改变传入的prop数据的话，子组件的视图未发生改变。



## 原因分析

子组件并没有重新加载，所以不会再次执行created或者mounted，子组件也就未处理改变的prop数据，视图也就不会更新。

## 解决方法

1、（**推荐**）在子组件上加上一个唯一的`key`标识，当传入的props不同时，使key值也不同，这样当切换prop数据的时候，由于key不同，vue会认为是两个不同的组件，就会重新执行组件的created或者mounted中的操作。

示例：

```js
<set-text :data="data" :key="data.id"></set-text>
```



2、在子组件使用 watch 监听（可能需要深度监听）props传过来的值，如果发现改变，再次执行created或者mounted中的操作。



