# 一文搞懂 JavaScript 中 DOM 相关的距离

> 阅读本文大概需要 12 分钟。

### 一、问题由来

刚开始学 DOM 操作中对于元素距离元素的距离问题总是迷迷糊糊的，虽然有万能的 getCurrentStyle 方式来取得所需要的属性，但是有时看别人的代码的时候，总会遇到很多简写的方式，或者有的时候为了简洁，关键字的方式更加合适，但是要求我们对这些属性的区别要特别清楚。

比如下面要说的 offset 系列，scroll 系列，client系列的距离，还有事件发生时 offsetX，clientX，pageX 等等的一些距离的总结，可以在我们忘记的时候翻翻一翻这篇文章，然后花最短的时间搞清楚它们之间的区别。

### 二、元素位置之间的距离

先看下示例代码：

```markup
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        textarea {
            width: 200px;
            height: 200px;
            border: 6px solid red;
            padding: 5px;
            margin: 10px;
            /* overflow: scroll; */ /*加不加滚动条*/
            white-space: nowrap;    /*强制不换行*/
        }
    </style>
</head>

<body>
    <textarea>1</textarea>
</body>
<script>
    var textarea = document.querySelector("textarea");

    console.log(textarea.clientWidth, textarea.scrollWidth, textarea.offsetWidth);
    console.log(textarea.clientLeft, textarea.scrollLeft, textarea.offsetLeft);
</script>

</html>
```

![&#x56FE;&#x7247;](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e8b934ebfd8d48d381fef6b7ee941063~tplv-k3u1fbpfcp-zoom-1.image)

> textarea.clientWidth = 200\(可见区域\) + 5\(padding\) + 5\(padding\)
>
> textarea.scrollWidth = 200\(可见区域\) + 5\(padding\) + 5\(padding\)
>
> textarea.offsetWidth = 200\(可见区域\) + 5\(padding\) + 5\(padding\) + 6\(border\) + 6\(border\)
>
> textarea.clientLeft = 6\(border-left\)
>
> textarea.scrollLeft = 0\(横向滚动条滚动的距离\)
>
> textarea.offsetLeft = 10\(元素左外border距离父元素左内border的距离\)

当我把滚动条加上的时候：

![&#x56FE;&#x7247;](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5afb815136b248eab0a3d0e56d466983~tplv-k3u1fbpfcp-zoom-1.image)

> textarea.clientWidth = 200\(可见区域\) + 5\(padding\) + 5\(padding\) - 17\(滚动条宽度\)
>
> textarea.scrollWidth = 200\(可见区域\) + 5\(padding\) + 5\(padding\) - 17\(滚动条宽度\)
>
> textarea.offsetWidth = 200\(可见区域\) + 5\(padding\) + 5\(padding\) + 6\(border\) + 6\(border\)
>
> textarea.clientLeft = 6\(border-left\)
>
> textarea.scrollLeft = 0\(横向滚动条滚动的距离\)
>
> textarea.offsetLeft = 10\(元素左外border距离父元素左内border的距离\)

当文字过长滚动条可以滑动的时候：

![&#x56FE;&#x7247;](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b6c549b9299c4249abfb17e83654ac67~tplv-k3u1fbpfcp-zoom-1.image)

> textarea.clientWidth = 200\(可见区域\) + 5\(padding\) + 5\(padding\) - 17\(滚动条宽度\)
>
> textarea.scrollWidth = 280\(整个内容，包括不可见区域\) + 5\(padding\) + 5\(padding\) - 17\(滚动条宽度\)
>
> textarea.offsetWidth = 200\(可见区域\) + 5\(padding\) + 5\(padding\) + 6\(border\) + 6\(border\)
>
> textarea.clientLeft = 6\(border-left\)
>
> textarea.scrollLeft = 0\(横向滚动条滚动的距离\)
>
> textarea.offsetLeft = 10\(元素左外border距离父元素左内border的距离\)

由于每次打开时，滚动条的位置不变，所以我需要 js 设置滚动滚动条的时候，看每个值的变化：

```javascript
textarea.onscroll = function () {
    console.log(textarea.clientWidth, textarea.scrollWidth, textarea.offsetWidth);
    console.log(textarea.clientLeft, textarea.scrollLeft, textarea.offsetLeft);
};
```

![&#x56FE;&#x7247;](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/75e9928ea06f49da95b20c5073ffde11~tplv-k3u1fbpfcp-zoom-1.image)

我们可以发现，只有 scrollLeft 是在改变的。

上面是 width 系列 和 left 系列的一些值的情况，那么相应的 height 系列和 top 系列的值也是一样的。

#### 1、总结一下

#### 1.1、client系列

> **clientWidth = width（可见区域）+ padding - 滚动条宽度（如果有）**
>
> **clientHeight = height（可见区域）+ padding - 滚动条宽度（如果有）**
>
> **clientLeft：相当于元素左border\(border-left\)的宽度**
>
> **clientTop：相当于元素上border（border-top）的宽度**

#### 1.2、scroll系列

> **scrollWidth = width（内容实际宽度，包括不可见区域） + padding**
>
> **scrollHeight = height（内容实际高度，包括不可见区域） + padding**
>
> **scrollLeft：指当前元素可见区左部，到完整内容左部的距离（也就是横向滚动条滚动的距离）。**
>
> **scrollTop：指当前元素可见区顶部，到完整内容顶部的距离（也就是纵向滚动条滚动的距离）。**

#### 1.3、offset系列

在此之前，我们先看看一个属性：offsetParent。

offset是偏移的意思，既然是偏移就要有一个参照物，这个参照物就是 offsetParent。它指的是距离当前元素最近的定位父元素（position != static），这个定位父元素就是我们计算所有offset属性的参照物。

元素的 offsetParent 的获取方式：

* 通过元素的`offsetParent`属性直接获取。
* 元素`position:fixed`，则其`offsetParent`的值为`null`，此时相对视口定位。
* 元素非`fixed`定位，其父元素无位设置定位，则`offsetParent`均为。
* 元素非`fixed`定位，其父元素中有设置定位的，则其中离当前元素最近的父元素为`offsetParent`。
* 的`offsetParent`为`null`，相对视口定位。

> **offsetWidth = width（可见区域） + padding + border**
>
> **offsetHeight = height（可见区域） + padding + border**
>
> **offsetLeft：元素左外边框距离父元素左内边框的距离（简单来说就是元素相对父元素左边的距离）**
>
> **offsetTop：元素上外边框距离父元素上内边框的距离（简单来说就是元素相对父元素上边的距离）**

下面有张图对上面的内容进行了总结，并给出了不同浏览器下的兼容性：

![&#x56FE;&#x7247;](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5691b8c9b80449d7847a53fd9667c219~tplv-k3u1fbpfcp-zoom-1.image)

### 三、鼠标事件相关的坐标距离

鼠标事件中有很多描述鼠标事件发生时的坐标信息的，给大家介个图看看：

![&#x56FE;&#x7247;](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/87f0d1576eda4f88a6e11c996fc9a7e4~tplv-k3u1fbpfcp-zoom-1.image)

这么多的坐标位置到底有什么区别呢？下面两张图（来自网络）带你一眼看穿它们之间的区别：

![&#x56FE;&#x7247;](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/75b62085f6e74c579639e04c7baf852d~tplv-k3u1fbpfcp-zoom-1.image)

![&#x56FE;&#x7247;](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/89daca8f462f4d9994ee581e77ddb6aa~tplv-k3u1fbpfcp-zoom-1.image)

#### 1、总结一下

> **clientX** = 鼠标点击位置距离浏览器**可视区域**左边的距离
>
> **offsetX** = 鼠标点击位置距离元素左边的距离，不包括左border。
>
> **pageX** = scrollLeft + clientX （**但是IE8不支持**）
>
> **layerX** = offsetX + 左border + 左边滚动条滚动的距离
>
> **x** = 鼠标点击位置距离浏览器可视区域的左边距离（相当于 clientX）。
>
> **screenX** = 鼠标点击位置距离电脑屏幕左边的距离。

同样，上面都是 X 系列的位置比较，Y的方向上也是一样的。

看完这些，你对 DOM 元素的距离相关的属性都了解了吗？

其实我们之间的问题，从来都不是距离。

想看更多精彩内容，关注公众号「前端队长」，获取更多前端技术与个人成长相关内容。

![&#x5FAE;&#x4FE1;&#x56FE;&#x7247;\_20210427113225.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2d86f15e00c34770a384948abe084785~tplv-k3u1fbpfcp-watermark.image)

