# 81 css-如何设置input输入框的宽度随文字的输入长度而改变？

如何实现下面input输入框的宽度随文字的输入长度而改变？

![GIF](https://user-images.githubusercontent.com/23518990/70308909-3f2ed580-1847-11ea-8704-035c24956b33.gif)

我们知道，`<input>` 输入框，如果不给一个宽度的话，那么它的宽度是固定的，想让它伸展是不可能的，那怎么办呢？

## 方法

我们可以给input外面套一层div，设置**input的宽高为100%**，让它随着父元素宽度改变而改变。那父元素div的宽高如何确定呢？

再在这个父元素div中塞一个span标签，**用span的宽高撑起div的宽高**。由于span标签的宽高就可以根据它内部的内容的宽高动态改变，所以div的宽高就可以动态改变，然后input的宽高也可以动态改变。

然后再**将span的内容和input 的内容同步**，再将span标签隐藏在input下面，暗搓搓的操纵全局。

## 代码

```markup
<div class="add-tag tag-item">
    <span>{{ inputValue }}</span>
    <input  v-model="inputValue" />
</div>

<style>
     .add-tag {
        height: 32px;
        position: relative;

        span {
            display: inline-block;
            width: 100%;
            height: 100%;
            visibility: hidden;
        }

        input {
            width: 100%;
            height: 100%;
            display: inline-block;
            position: absolute;
            left: 0;
            top: 0;
            border: none;
            border-bottom: 1px dashed #ccc;
        }
    }
</style>
```

## 更简单的方法

_**（added in 20210315）**_

> 通过获取input标签的`scrollWidth`，然后动态设置input的宽度即可。

scrollWidth：对象的实际内容的宽度，不包边线宽度，会随对象中内容超过可视区后而变大。 clientWidth：对象内容的可视区的宽度，不包滚动条等边线，会随对象显示大小的变化而改变。 offsetWidth：对象整体的实际宽度，包滚动条等边线，会随对象显示大小的变化而改变。

![](../.gitbook/assets/image%20%283%29.png)

在input内，scrollWidth也就相对于input中value值的长度。clientWidth相当于input默认宽度（能看到的宽度）。

```javascript
$('.fillin-input').on('input', function (e) {
    let inputDom = e.target;
    
    // input 输入框的最小宽度设置为120px
    if(inputDom.scrollWidth > 120) {
        $(inputDom).width(120-18);
        $(inputDom).width(inputDom.scrollWidth - 18); // -18是兼容误差
    }else {
        $(inputDom).width(120-18);
    }
})
```

## 其他方法（未经过测试）

**通过计算文本宽度然后设置input宽度。**

由于我们输入的文本有中文和英文，字体宽度不是一样的，所以首先要转换成等宽字体，然后计算。

原理是利用html提供的标签 `<pre>`，向dom中动态添加标签，标签里的内容就是要测试长度的文本，获取完长度之后再删除刚才添加的标签，从而可取到文本的大概长度了。

为什么要用标签而不用其他标签呢，那来看看标签的特性吧：pre 元素可定义预格式化的文本。被包围在 pre 元素中的文本通常会保留空格和换行符；而文本也会呈现为等宽字体。 

```javascript
//获取文本宽度
var textWidth = function(text){ 
    var sensor = $('<pre>'+ text +'</pre>').css({display: 'none'}); 
    $('body').append(sensor); 
    var width = sensor.width();
    sensor.remove(); 
    return width;
};
```

有了上面这个函数，input标签根据输入字符动态自适应宽度的实现就简单了：

```javascript
//input宽度自适应
$("input").on('input', function(e){
    $(e.target).width(textWidth($(e.target).val()));
});
```

（完）

