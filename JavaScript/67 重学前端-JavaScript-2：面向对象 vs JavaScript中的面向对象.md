
![微信截图_20191203183905](https://user-images.githubusercontent.com/23518990/70043974-634aa680-15fc-11ea-8729-bd47bf7899cc.png)


（题图：梵高-开花的杏树）

---

对象的特征如下：

- 对象具有唯一标识性：即使完全相同的两个对象，也并非同一个对象。
- 对象有状态：对象具有状态，同一对象可能处于不同状态之下。
- 对象具有行为：即对象的状态，可能因为它的行为产生变迁。



JavaScript 中对象独有的特色是：**对象具有高度的动态性，这是因为 JavaScript 赋予了使用者在运行时为对象添改状态和行为的能力。** 如果你用过 Java 或者其它别的语言，肯定会产生跟我一样的感受。 



##  JavaScript 对象的两类属性 

 对 JavaScript 来说，属性并非只是简单的名称和值，JavaScript 用一组特征（attribute）来描述属性（property）。 



属性又分为：**数据属性和访问器(getter/setter)属性**。



### 数据属性

数据属性具有4个特征：

- `value`：就是属性的值。
- `writable`：决定属性能否被赋值。
- `enumerable`：决定 for in 能否枚举该属性。
- `configurable`：决定该属性能否被删除或者改变特征值。 

 在大多数情况下，我们只关心数据属性的值即可。 



### 访问器(getter/setter)属性

访问器(getter/setter)属性也具有4个特征：

- `getter`：函数或 undefined，在取属性值时被调用。
- `setter`：函数或 undefined，在设置属性值时被调用。
- `enumerable`：决定 for in 能否枚举该属性。
- `configurable`：决定该属性能否被删除或者改变特征值。 

 访问器属性使得属性在读和写时执行代码，它允许使用者在写和读属性时，得到完全不同的值，它可以视为一种函数的语法糖。 



 我们通常用于定义属性的代码会产生数据属性，其中的 writable、enumerable、configurable 都默认为 true。

我们可以使用内置函数 `Object.getOwnPropertyDescripter` 来查看，如以下代码所示： 

```js
var o = { a: 1 };
o.b = 2;
//a 和 b 皆为数据属性
Object.getOwnPropertyDescriptor(o,"a") // {value: 1, writable: true, enumerable: true, configurable: true}
Object.getOwnPropertyDescriptor(o,"b") // {value: 2, writable: true, enumerable: true, configurable: true}
```



 如果我们要想改变属性的特征，或者定义访问器属性，我们可以使用 `Object.defineProperty`，示例如下： 

```js
var o = { a: 1 };
Object.defineProperty(o, "b", {value: 2, writable: false, enumerable: false, configurable: true});
//a和b都是数据属性，但特征值变化了
Object.getOwnPropertyDescriptor(o,"a"); // {value: 1, writable: true, enumerable: true, configurable: true}
Object.getOwnPropertyDescriptor(o,"b"); // {value: 2, writable: false, enumerable: false, configurable: true}
o.b = 3;
console.log(o.b); // 2
```



 在创建对象时，也可以使用 `get` 和 `set` 关键字来创建访问器属性，代码如下所示： 

```js
var o = { 
    get a() {
        console.log('getter');
        return a;
    },
    set a(val) {
        console.log('setter');
        a = val;
    }
};

o.a = 123;
console.log(o.a); // setter getter 123
```



 实际上 JavaScript 对象的运行时是一个“属性的集合”，属性以**字符串或者 Symbol** 为 key，以**数据属性特征值或者访问器属性特征值**为 value。 

比如上面 a 为对象o的key，访问器属性或者` {writable:true,value:1,configurable:true,enumerable:true} ` 为value。


到这里，可以理解有人得出 “JavaScript 不是面向对象”  的说法， 但是 JavaScript 提供了完全运行时的对象系统，这使得它可以模仿多数面向对象编程范式，所以它也是正统的面向对象语言。  

JavaScript 语言标准也已经明确说明，JavaScript 是一门面向对象的语言，我想标准中能这样说，正是因为 JavaScript 的高度动态性的对象系统。 