# 6 vue-vue获取后端数据放在created还是mounted方法里面？

## 问题提出

我们知道一般vue使用ajax或者axios来获取后端数据，并且好像放在created里面和mounted里面都可以获取数据并正确渲染。那么放在created里面和mounted里面有什么区别呢？

以下是一些网友的回答：

**一般放到created即可。**

![&#x5728;&#x8FD9;&#x91CC;&#x63D2;&#x5165;&#x56FE;&#x7247;&#x63CF;&#x8FF0;](https://img-blog.csdnimg.cn/20190407140033189.png)

关于 **nextTick** 可以参考下面这篇文章： [Vue.nextTick的原理和用途](https://segmentfault.com/a/1190000012861862)

## 简单来说

如果你修改了某个dom中的数据，视图并不会立即更新。Vue 实现响应式并不是数据发生变化之后 DOM 立即变化，而是按一定的策略进行 DOM 的更新。，此时获取关于此dom的一切操作都是无效的，怎么办？在nextTick的回调中执行即可。$nextTick 是在下次 DOM 更新循环结束之后执行延迟回调，在修改数据之后使用 $nextTick，则可以在回调中获取更新后的 DOM。

