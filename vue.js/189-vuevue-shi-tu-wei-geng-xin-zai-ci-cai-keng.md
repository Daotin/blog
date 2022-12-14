# 189 vue-Vue视图未更新再次踩坑

> 原文首发地址：[https://github.com/Daotin/fe-blog/issues/](https://github.com/Daotin/fe-blog/issues/)
>
> 你也可以从下面地方找到我：
>
> * [Github](https://github.com/Daotin/) （求star! ✨✨✨）
> * [CSDN](https://blog.csdn.net/lvonve) 
> * [博客园](https://www.cnblogs.com/lvonve/) 
> * [掘金](https://juejin.im/user/2084329777534216) 
> * 微信公众号：[前端队长](https://images.cnblogs.com/cnblogs_com/lvonve/1607764/o_gzh.jpg)

今天遇到一个Vue数据更新了，但是视图未更新的问题，折腾了我2小时才搞定，有必要记录下来，防止日后再次踩坑。

## 问题描述

我需要显示一个列表，而且列表是可编辑的。比如可以修改列表每一项的名称等。

我从后端获取列表后，对其中的每一项数据进行初始化，增加一个`editing` 可编辑属性。

```javascript
me.groupList.forEach(item=>{
    item.editing = false;
});
```

之后在模板中使用v-for遍历groupList，然后每个item增加一个icon，点击icon后，修改editing的状态，根据`editing`的状态显示item的名称或者`input`标签。

但是在我操作的时候，发现使用`Vue.set` 也无法使得页面更新，加上`this.$forceUpdate()` 之后才可以。

```javascript
menuEdit(menu) {
    this.$set(menu, 'editing', true);
    console.log('edting----->', menu.editing); // 可以打印出来数据已经更新
    // this.$forceUpdate(); // 加上视图才会更新
},
```

按照以往的经验，只有直接赋值的时候`editing=false`，才会数据更新，但是视图未更新，但是我现在已经使用了`this.$set` 了，为何还是如此？

于是只能借助搜索引擎了。

在网上搜寻的过程中，我发现了有人问，为什么数据更新了，但是Vue Devtools中的数据未更新？

链接如下：

* [https://stackoverflow.com/questions/43838363/why-doesnt-the-data-get-updated-in-vue-dev-tools](https://stackoverflow.com/questions/43838363/why-doesnt-the-data-get-updated-in-vue-dev-tools)
* [https://stackoverflow.com/questions/46606499/vuejs-why-this-data-property-doesnt-updated-on-click-event-in-dev-tools](https://stackoverflow.com/questions/46606499/vuejs-why-this-data-property-doesnt-updated-on-click-event-in-dev-tools)
* [https://github.com/vuejs/vue-devtools/issues/41\#issuecomment-162675083](https://github.com/vuejs/vue-devtools/issues/41#issuecomment-162675083)

其实，如果页面上没有任何可响应的内容，**也就是页面未使用响应式的数据，或者使用了非响应式的数据**，那么数据将无法在Vue Devtools中实时更新，但是你可以点击工具的刷新按钮，这时候可以看到数据进行了更新。

小插曲完。

## 问题解决

就在我一筹莫展的时候，我突然发现，最开始初始化的时候的`editing` 是直接赋值的，我当时一个激灵就知道，问题就在这里。

```javascript
me.groupList.forEach(item=>{
    this.$set(item, 'editing', false);
});
```

改成上面代码后，问题迎刃而解。

## 总结

* 问题是个小问题，也是我知道的问题。只是没想到它在最根源的地方犯了错，后面即使正确操作，也是于事无补。
* 要相信，当使用`this.$forceUpdate()` 的时候，99%的情况都是自己错了。
* 如果页面未使用响应式的数据，或者使用了非响应式的数据，Vue DevTools的数据是不会更新的。

