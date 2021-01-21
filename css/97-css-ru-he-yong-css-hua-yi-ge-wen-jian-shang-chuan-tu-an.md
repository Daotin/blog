# 97 css-如何用css画一个文件上传图案？

如下图，如果是你，你会怎么实现：

![&#x5FAE;&#x4FE1;&#x622A;&#x56FE;\_20191213170143](https://user-images.githubusercontent.com/23518990/70788006-e624eb80-1dca-11ea-97ad-f68a4b3d92e9.png)

通常我们会通过字体图标来显示中间的加号，外层用一个div包裹即可；或者使用伪元素来模拟中间的一横一竖，这都比较麻烦。

其实我们可以直接使用div+css就可以实现。

## 轮廓属性outline

`outline`属性是用来设置一个或多个单独的轮廓属性的简写属性 ， 例如 。

轮廓有下面几个属性：

```css
{
    outline-style: solid;
    outline-width: 10px;
    outline-color: red;
}
```

他们有一种简写形式：

```css
{
    outline: 10px solid red;
}
```

## 轮廓的特点

**轮廓不占据空间，它们被描绘于内容之上。**

可以做到下图的效果：

![](https://raw.githubusercontent.com/Daotin/pic/master/img/20190813101002.png)

我发现，当设置 `outline-offset` 为负值的时候，轮廓会出现在div的内部，如果继续扩大其负值，最终轮廓会收缩成一个“➕”加号，正好可以作为文件上传样式中间的加号。

于是代码就来了：

```css
div {
    margin: 100px;
    width: 100px;
    height: 100px;
    outline: 15px solid #545454;
    outline-offset: -66px;
    border: 2px solid #545454;
}
```

`outline-offset: -66px;` 是关键，它表示**轮廓距div边**的距离，如果为负值则会往里面收缩，最后形成一个加号。具体上传样式的大小和outline的宽度都需要自己慢慢调整已达到大和谐。

> 需要注意的是：
>
> * 容器得是个正方形
> * outline 边框本身的宽度不能太小

（啾咪）

