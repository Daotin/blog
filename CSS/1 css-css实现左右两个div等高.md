## 问题描述

现在有两个div，但是两个div里面内容多少不确定，可能左边多，可能右边多，css要如何设置可以保证左右两边的div等高呢？

##  解决方案
### 方法一
通过父元素设置 `overflow:hidden`, 子元素设置超大 `padding-bottom` 和 `margin-bottom`来实现。
```html
<div id="#warp">
    <div class="left">
        <br>
        <br>
        <br>
        left
    </div>
    <div class="right">right</div>
</div>
```
```css
#wrap {
    overflow: hidden;
    width: 1000px;
    margin: 0 auto;
}
#left,
#center {
    margin-bottom: -10000px;
    padding-bottom: 10000px;
}
#left {
    float: left;
    width: 250px;
    background: #00ffff;
}
#center {
    float: left;
    width: 500px;
    background: #ff0000;
}
```

### 方法二
每个div使用`display:table-cell`
```css
.left,
.right {
    padding: 10px;
    display: table-cell;
    border: 1px solid #f40;
}
```

### 方法三

父元素使用`display:box`
```css
.wrap {
    display: -webkit-box;
}
.left,
.right {
    padding: 10px;
    border: 1px solid #f40;
}
```

## 效果如下图：

![](https://raw.githubusercontent.com/Daotin/pic/master/img/20190813113155.png)

## 参考链接

https://www.cnblogs.com/cbza/p/7145384.html

