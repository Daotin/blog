## 什么是图片瀑布流

用一张花瓣网页的图片布局可以很清楚看出图片瀑布流的样子：

![1](https://user-images.githubusercontent.com/23518990/68454712-756f3a00-0234-11ea-8657-2e4c75be7ee6.png)



简单来说，就是有很多图片平铺在页面上，每张图片的宽度相同，但是高度不同，这样错落有致的排列出 n 列的样子很像瀑布，于是就有了瀑布流图片一说。



## 实现原理

### 1、第一种方式

> 第一种方式前提是：图片的宽度固定，但是列可变（根据屏幕大小）

通过上面的介绍，我们知道要实现瀑布流的前提是宽度一致（假如为100px），高度可以不相同。

我们首先确定排布的列数（假如为4列），那么第一行只能放4张图片，然后将每个图片的高度放入一个数组中（假如为 `heightArr = [100,50, 200,30]`），当我们在放入下一张图片的时候就要判断这个数组中哪个高度是最小的（这里是30），然后还要知道最小的高度所在高度数组的索引（这里是i = 3），然后我们就可以对这张图片进行定位：

```css
{
    position: absolute;
    left: i*100 + 'px';
    top: 30 + 'px'
}
```

如此遍历剩下的图片即可。



### 实现代码

下面是未处理的原始代码，图片之间间隔很多空白，影响美观。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .box {
            position: relative;
        }
        img {
            width: 200px;
            vertical-align: top;
            padding: 5px;
        }
    </style>
</head>
<body>
    <div class="box">
        <img src="./images/img/2-.jpg" alt="">
        <img src="./images/img/3-.jpg" alt="">
        <img src="./images/img/4-.jpg" alt="">
        <img src="./images/img/5-.jpg" alt="">
        <img src="./images/img/6-.jpg" alt="">
        <img src="./images/img/7-.jpg" alt="">
        <img src="./images/img/8-.jpg" alt="">
        <img src="./images/img/9-.jpg" alt="">
        <img src="./images/img/10-.jpg" alt="">
        <img src="./images/img/11-.jpg" alt="">
        <img src="./images/img/12-.jpg" alt="">
        <img src="./images/img/13-.jpg" alt="">
        <img src="./images/img/14-.jpg" alt="">
        <img src="./images/img/15-.jpg" alt="">
        <img src="./images/img/16-.jpg" alt="">
    </div>
</body>
</html>
```

![2](https://user-images.githubusercontent.com/23518990/68454723-7f913880-0234-11ea-9afa-1e31ee1d3960.png)


下面是处理后的代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .box {
            position: relative;
        }
        img {
            width: 200px;
            vertical-align: top;
            padding: 5px;
        }
    </style>
</head>
<body>
    <div class="box">
        <img src="./images/img/2-.jpg" alt="">
        <img src="./images/img/3-.jpg" alt="">
        <img src="./images/img/4-.jpg" alt="">
        <img src="./images/img/5-.jpg" alt="">
        <img src="./images/img/6-.jpg" alt="">
        <img src="./images/img/7-.jpg" alt="">
        <img src="./images/img/8-.jpg" alt="">
        <img src="./images/img/9-.jpg" alt="">
        <img src="./images/img/10-.jpg" alt="">
        <img src="./images/img/11-.jpg" alt="">
        <img src="./images/img/12-.jpg" alt="">
        <img src="./images/img/13-.jpg" alt="">
        <img src="./images/img/14-.jpg" alt="">
        <img src="./images/img/15-.jpg" alt="">
        <img src="./images/img/16-.jpg" alt="">
    </div>
</body>
<script src="./jquery.min.js"></script>
<script>
    $(function () {
        // 获取图片的宽度(200px)
        let imgWidth = $('img').outerWidth(); // 200

        waterfallHandler();

        // 瀑布流处理
        function waterfallHandler() {
            // 获取图片的列数
            let column = parseInt($(window).width() / imgWidth);

            // 高度数组
            let heightArr = [];
            for(let i=0; i<column; i++) {
                heightArr[i] = 0;
            }

            // 遍历所有图片进行定位处理
            $.each($('img'), function (index, item) {
                // 当前元素的高度
                let itemHeight = $(item).outerHeight();
                // 高度数组最小的高度
                let minHeight = Math.min(...heightArr);
                // 高度数组最小的高度的索引
                let minIndex = heightArr.indexOf(minHeight);
                
                $(item).css({
                    position: 'absolute',
                    top: minHeight + 'px',
                    left: minIndex * imgWidth + 'px'
                });

                heightArr[minIndex] += itemHeight;
            });
        }
        
        // 窗口大小改变
        $(window).resize(function () {
            waterfallHandler();
        });
    });
</script>
</html>
```

![1](https://user-images.githubusercontent.com/23518990/68454739-8c159100-0234-11ea-9c6e-9641368141db.gif)


### 2、第二种方式

> 第二种方式前提是：列是固定的个数，然后图片根据屏幕的宽度进行自适应缩放。

这种方式由于图片是可以缩放的，宽高不好确定，因此不好用定位的方式处理。

我们可以这样处理，既然知道了列，那么每一列做一个容器。然后遍历图片，将图片放入容器高度最小的容器中即可。

> 这里我们使用js来添加图片，而不是事先写好在html中了。



### 实现代码

```html
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

        ul li {
            list-style: none;
            float: left;
        }
    </style>
</head>

<body>
</body>
<script src="./jquery.min.js"></script>
<script>
    $(function () {
        const COLUMN = 4; // 4列
        let arr = []; // 存储4列li
        let minHeight = [] // 存储4列的高度

        create();

        function create() {
            // 创建ul li作为每一列的容器
            $("<ul></ul>").appendTo($("body")).css("width", "100%");
            for (let i = 0; i < COLUMN; i++) {
                var li = document.createElement("li");
                $(li).appendTo($("ul"))
                    .css({
                        "width": "24%",
                        "margin": "0 0.5%"
                    });
                arr.push(li);
                // console.log(arr);
                minHeight.push(0);
            }
            createImg();
        }

        function createImg() {
            let img = new Image();
            img.num = 2;
            img.src = `./images/img/${img.num}-.jpg`; // 素材图片的规律为 2-.jpg 3-.jpg 4-.jpg ...
            $(img).css("width", "100%");
            // 当图片加载完后
            $(img).on("load", loadHandler);
        }

        function loadHandler() {
            // 高度数组的最小值
            let min = Math.min.apply(null, minHeight);
            // 高度数组的最小值索引
            let minIndex = minHeight.indexOf(min);
            // 克隆一份图片
            let im = this.cloneNode(true);
            // 将图片假如对应最小值索引的容器中
            arr[minIndex].append(im);
            // 更新最小值索引的容器的高度
            minHeight[minIndex] += im.getBoundingClientRect().height;

            this.num++;

            // 图片的个数只有79张
            if (this.num > 79) {
                $(this).off("load", loadHandler);
                return;
            }
            this.src = `./images/img/${this.num}-.jpg`;
        }
    });
</script>

</html>
```

![2](https://user-images.githubusercontent.com/23518990/68454756-98015300-0234-11ea-8a30-22f7121ebfe2.gif)


本文完。

（啾咪 ^.<）

