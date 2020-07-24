下面的代码有问题吗？
```js
 arr.map(item=>{
    if(item.id === '123') {
        item = {};
    }
});
```

## 问题分析

这样赋值是无效的。`item`只是 `arr[index]` 的引用，当你改变item的属性的时候是有效地，因为指向的是同一个对象，但是直接修改item的值的话，不会影响到arr[index]的值。

## 解决方法
```js
 arr.map((item, index)=>{
    if(item.id === '123') {
        arr[index] = {};
    }
});
```


