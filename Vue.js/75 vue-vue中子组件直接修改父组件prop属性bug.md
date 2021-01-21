在有些时候，子组件直接修改父组件传来的 `prop 对象`的属性会出现不同步的问题。

比如，父组件传过来的一个对象 `checkBoxObj`:

```js
checkBoxObj:{
    checked: false
}
```

将 `checked` 通过v-model绑定给子组件的 checkbox，然后点击这个checkbox，试图改变 `checked` 的值，但是有时候会发现 checkbox 的选中状态和 checked 相反，也就是不同步的问题。



## （尝试）解决办法

将prop的`checkBoxObj`值赋值给data的一个值`checkBoxData`，

```js
this.checkBoxData = this.checkBoxObj;
```

由于是直接赋值，也存在引用关系，修改`checkBoxData`的`checked`就相当于修改了`checkBoxObj`的`checked`。于是这个bug不在出现。


然后直接修改这个checkBoxData的checked属性值，不要直接修改prop传过来的值。