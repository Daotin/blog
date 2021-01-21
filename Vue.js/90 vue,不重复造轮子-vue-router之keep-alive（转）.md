> 本着[不重复造轮子](https://github.com/Daotin/technophile/issues/30)的原则，这篇文章记录链接与概要于此。

## [keep-alive：组件级缓存](https://juejin.im/post/5b407c2a6fb9a04fa91bcf0d)
- 讲解keep-alive基本使用
- 选择某些组件缓存。（使用include/exclude属性）
- 条件缓存。（比如：详情页到列表页需要缓存，但是首页到列表页就不需要，如何处理？）

## [Vue 返回记住滚动条位置详解](https://juejin.im/post/5c7ce50b6fb9a049b13eef08)
- 先获取`scrollTop`并保存起来。
- 在回到该组件时会触发`activated`，然后设置之前保存的`scrollTop`值。

## Q&A
Q：文章 `Vue 返回记住滚动条位置详解` 是点击列表的时候保存滚动条位置，然后再跳转，其实我感觉可以直接在组件守卫中进行保存。

A：我想的是在B中使用组件守卫 `beforeRouteLeave ` 进行滚动条位置保存。

```js
// 在路由跳转时,如果离开当前组件,则会触发该守卫
beforeRouteLeave (to, from, next) {
     // 如果下一个页面是详情页（C），则缓存列表页（B）的滚动条位置
     if (to.name === 'C') {
         this.scroll = this.$refs.wrapper.scrollTop;
     }
     next(true);
},
```
具体效果如何还未检验，等到用时再说。

