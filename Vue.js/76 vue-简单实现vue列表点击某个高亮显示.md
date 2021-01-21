比如ul下有4个li元素。

给每个li绑定点击事件`@click="select_li(index)`，然后这个点击时间会将一个全局变量 `selectLi` 赋值为 index 的值。

然后在每个li上加上class样式

```html
:class="[selectLi ==index?'active':'']"
```


即可。