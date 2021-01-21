当发现fixed定位不是以视口左上角位置为基准定位时，可能的原因如下：

1. 使用了 `iframe`，定位基准是iframe的左上角。
2. 使用了 `translate3D` 变换。解决办法：`transform: none !important;`

