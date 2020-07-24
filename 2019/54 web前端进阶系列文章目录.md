## HTML 相关文章

## CSS 相关文章

[9个css技巧](https://mp.weixin.qq.com/s/MeWRCGwuK-Tw73BZcd1xHg)

- 建议使用 padding 代替 margin
- position:fixed 降级问题
- 合理使用 px | em | rem | % 等单位
- 合理使用css变量
- 1px 细线问题
- 从 html 元素继承 box-sizing
- 内联首屏关键 CSS （[自动提取工具](https://github.com/addyosmani/critical)）

- 文字超出省略、文字两端对齐
- ...

[position:sticky粘性定位](https://www.zhangxinxu.com/wordpress/2018/12/css-position-sticky/)

[你未必知道的49个CSS知识点](https://mp.weixin.qq.com/s/u7yQCPcbuspTJsRME2uYpA)

- 负margin效果
- shape-outside属性
- flex布局下margin:auto的神奇用法
- input的默认宽度取决于size属性，可用width修改
- 绝对定位和固定定位时，同时设置 left 和 right 等同于隐式地设置宽度
- z-index层级只在父元素内有效
- position:sticky 粘性定位
- css绘制三角形
- display:table实现多列等高布局
- 实现居中的一种简单方式（父元素：display:auto; 子元素：margin:auto;）
- 角向渐变
- outline描边，不占地方，它甚至可以在里面
- tab-size 指定tab宽度
- 设置按钮禁用时鼠标状态
- 背景虚化filter
- fill-available可以解决box-sizing无法解决的margin问题

- fit-content使得宽度正好为内容宽度
- css自定义属性
- min-content/max-content
- page-break-before属性来表示打印时是否需要另起新页
- 普通元素resize
- 使用before伪元素实现面包屑
- 水波效果原理

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<style type="text/css">
			* {
				padding: 0;
				margin: 0;
			}
            .c {
                margin: 100px auto;
                width: 100px;
                height: 100px;
                border: 1px solid #000;
                border-radius: 50%;
                position: relative;
                overflow: hidden;
            }
            .c1 {
                position: absolute;
                left: -40px;
                top: 60px;
                width: 200px;
                height: 200px;
                background-color: #238cee;
                border-radius: 80px;
                opacity: 0.7;
                /* z-index: -1; */
                animation: move 8s linear infinite;
            }
            .c2 {
                position: absolute;
                left: -40px;
                top: 60px;
                width: 200px;
                height: 200px;
                background-color: #238cee;
                border-radius: 60px;
                opacity: 0.5;
                /* z-index: -1; */
                animation: move 6s linear infinite;
            }
            .c3 {
                position: absolute;
                left: -40px;
                top: 60px;
                width: 200px;
                height: 200px;
                background-color: #238cee;
                border-radius: 50px;
                opacity: 0.2;
                /* z-index: -1; */
                animation: move 5s linear infinite;
            }
            @keyframes move {
                /*百分比是指整个动画耗时的百分比*/
                0% {
                    transform: rotate(0deg);
                }
                100% {
                    transform: rotate(360deg);
                }
            }
		</style>
	</head>
	<body>
        <div class="c">
            <div class="c1"></div>
            <div class="c2"></div>
            <div class="c3"></div>
        </div>
	</body>
</html>

```

- 弹球动画效果
- ...


z-index层级问题

> - z-index只能影响position值不是static的元素。
> - 在一组层叠元素中，其子元素的z-index值是相对于该父元素而不是 document 设置的。每个层叠上下文完全独立于它的兄弟元素。如果元素 B 位于元素 A 之上，则即使元素 A 的子元素 C 具有比元素 B 更高的z-index值，元素 C 也永远不会在元素 B 之上。


## JS 相关文章

[移动端触发touchend后阻止click事件](https://blog.csdn.net/heeng4688/article/details/83305079)

```vue
// vue里面简单的处理方式，可以同时兼容PC和移动端

<div @touchend.stop.prevent="doSomething" @click.stop.prevent="doSomething">
```


## Vue 相关文章

- [vue-router实现原理](https://segmentfault.com/a/1190000018584560)
- [vue-router查缺补漏](https://mp.weixin.qq.com/s/wNmdY6rmd5Kr9F3vTP6gJg)

- [用vue构建多页面应用](https://github.com/JaneSu/multiple-vue-page)

