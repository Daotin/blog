# 7 javascript-jquery中attr和prop区别

## 相同点：

都是jq中的自定义属性的方法。

## 区别：

prop：attr 方法针对单选框和复选框的是否选中问题操作复杂 ==（ 元素.attr\("checked"\) 选中返回值为 checked，未选中返回值为 undefined，不是直接显示 true 或者 false 那么简单）==

## 示例：

`元素.prop("checked"); // 获取这个元素是否选中` （可以直接显示true或false），更加方便。

