# 36 浏览器工作原理-11 this：从JavaScript执行上下文的视角讲清楚this

先看下下面的代码：

```javascript
var myObj = {
  name : " 极客时间 ", 
  showThis: function(){
    console.log(this)   // this指向 myObj
    function bar(){console.log(this)}  // this 指向 window
    bar()
  }
}
myObj.showThis()
```

_为什么第二个this指向window呢？_

答：this 没有作用域的限制，这点和变量不一样，所以**嵌套函数不会从调用它的函数中继承 this**，而普通函数中改的this就是指向的window。

> tips：如果使用了严格模式，普通函数的this为undefined。
>
> ```javascript
> 'use strict'
> function a() {
>     console.log(this);
> }
> a();
> ```

