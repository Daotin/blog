> 本着[不重复造轮子](https://github.com/Daotin/technophile/issues/30)的原则，这篇文章记录链接与概要于此。

## [Vue.js 插件开发详解](https://juejin.im/post/58d9aae02f301e007e8ee278)
1. 插件通常用来为 Vue 添加**全局功能**。Vue.js 的插件应该暴露一个 install 方法。这个方法的第一个参数是 Vue 构造器，第二个参数是一个可选的选项对象：

```js
MyPlugin.install = function (Vue, options) {
  // 1. 添加全局方法或属性
  Vue.myGlobalMethod = function () {
    // 逻辑...
  }

  // 2. 添加全局资源
  Vue.directive('my-directive', {
    bind (el, binding, vnode, oldVnode) {
      // 逻辑...
    }
    ...
  })

  // 3. 注入组件选项
  Vue.mixin({
    created: function () {
      // 逻辑...
    }
    ...
  })

  // 4. 添加实例方法
  Vue.prototype.$myMethod = function (methodOptions) {
    // 逻辑...
  }
}
```

2. 开发一个消息通知框 `vue-toast`，可以传入不同参数在上下左右位置显示。


## 参考链接
- [Vue.js 插件开发详解](https://juejin.im/post/58d9aae02f301e007e8ee278)

