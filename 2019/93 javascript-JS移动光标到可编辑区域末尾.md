此方法适用于:
- `input` 输入框
- 设置了 `contenteditable="true"`的标签（如：div，p，span等）

```js
// 光标移动到最后
moveCursorToEnd(dom) {
    if (window.getSelection) {//ie11 10 9 ff safari
        var range = window.getSelection();//创建range
        range.selectAllChildren(dom);//range 选择obj下所有子内容
        range.collapseToEnd();//光标移至最后
    }
    else if (document.selection) {//ie10 9 8 7 6 5
        var range = document.selection.createRange();//创建选择对象
        //var range = document.body.createTextRange();
        range.moveToElementText(dom);//range定位到obj
        range.collapse(false);//光标移至最后
        range.select();
    }
}
```
