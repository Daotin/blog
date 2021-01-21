## 关于类型，有哪些你不知道的细节？

根据最新的语言标准，JavaScript有7种语言类型：

1.  Undefined；
2.  Null；
3.  Boolean；
4.  String；
5.  Number；
6.  Symbol（ES6新加入）；
7.  Object。



### 为什么有的编程规范要求用 void 0 代替 undefined？

Undefined类型的值只有一个 undefined。

任何一个变量未赋值前值都为 undefined，一般我们使用全局变量 undefined 表示，但是由于 undefined 是个变量，是可以被修改的，所以我们一般使用 `void 0` 的返回值（**void 运算符** 可以对给定的表达式进行求值，然后返回 `undefined`）来代替 undefined 变量。

> PS：根据ES5的规范，在现代浏览器中，全局的undefined的值将不可修改。但是在局部环境是可以被赋值的！！



Null类型的值只有一个 null，表示一个变量定义了但是为空。

所以，在实际编程时，我们一般不会把变量赋值为 undefined，这样可以保证所有值为 undefined 的变量，都是从未赋值的自然状态。

还有null是关键字，而不是变量，可以放心使用。





### 字符串是否有最大长度？

String 有最大长度是 `2^53 - 1`，但是这个所谓最大长度，并不完全是你理解中的字符数。

因为 JavaScript 中的String类型存储的方式是 `UTF16`格式的，所以 String 的最大长度指的是保存为UTF16格式时候的最大程度，转化成 字符数的话肯定不到一半长度。。但是肯定够日常使用了。



### 为什么在 JavaScript 中，0.1+0.2 不能 =0.3？

答：计算机中用二进制来存储小数，而大部分小数转成二进制之后都是无限循环的值，因此存在取舍问题，也就是精度丢失，当这些丢失精度的二进制转换成十进制就造成上面的错误了。

> 另外，值得注意的是，JavaScript 中有 +0 和 -0，在加法类运算中它们没有区别，但是除法的场合则需要特别留意区分，“忘记检测除以 -0，而得到负无穷大”的情况经常会导致错误，而区分 +0 和 -0 的方式，正是检测 1/x 是 Infinity 还是 -Infinity。

**浮点数的比较**

```js
  console.log( 0.1 + 0.2 == 0.3); // false
```

上面的代码输出的结果是 false，说明两边不相等的。



这里输出的结果是 false，说明两边不相等的，这是浮点运算的特点，也是很多同学疑惑的来源，浮点数运算的精度问题导致等式左右的结果并不是严格相等，而是相差了个微小的值。

所以实际上，这里错误的不是结论，而是比较的方法，正确的比较方法是使用 JavaScript 提供的最小精度值：

```js
  console.log( Math.abs(0.1 + 0.2 - 0.3) <= Number.EPSILON); // true
```

检查等式左右两边差的绝对值是否小于最小精度，才是正确的比较浮点数的方法。



### 什么是Symbol类型？

ES5 的对象属性名都是字符串，这容易造成属性名的冲突。比如，你使用了一个他人提供的对象，但又想为这个对象添加新的方法（mixin 模式），新方法的名字就有可能与现有方法产生冲突。如果有一种机制，保证每个属性的名字都是独一无二的就好了，这样就从根本上防止属性名的冲突。这就是 ES6 引入Symbol的原因。

```js
let s = Symbol();
```

上面代码中，变量s就是一个独一无二的值，像一个独一无二的字符串，但是又不是一个字符串类型。

typeof运算符的结果，表明变量s是 Symbol 数据类型，而不是字符串之类的其他类型。

> 注意：Symbol函数前不能使用new命令，否则会报错。这是因为生成的 Symbol 是一个原始类型的值，不是对象。也就是说，由于 Symbol 值不是对象，所以不能添加属性。基本上，它是一种类似于字符串的数据类型。

Symbol函数可以接受一个字符串作为参数，表示**对 Symbol 实例的描述**，主要是为了在控制台显示，或者转为字符串时，比较容易区分。

```js
let s1 = Symbol('foo');
let s2 = Symbol('bar');

s1 // Symbol(foo)
s2 // Symbol(bar)

s1.toString() // "Symbol(foo)"
s2.toString() // "Symbol(bar)"
```

上面代码中，s1和s2是两个 Symbol 值。如果不加参数，它们在控制台的输出都是Symbol()，不利于区分。有了参数以后，就等于为它们加上了描述，输出的时候就能够分清，到底是哪一个值。

如果 Symbol 的参数是一个对象，就会调用该对象的toString方法，将其转为字符串，然后才生成一个 Symbol 值。

```js
const obj = {
  toString() {
    return 'abc';
  }
};
const sym = Symbol(obj);
sym // Symbol(abc)
```

> **注意**，`Symbol`函数的参数只是表示对当前 Symbol 值的描述，因此相同参数的`Symbol`函数的返回值是不相等的（虽然描述看起来是相同的）。

```js
let s = Symbol('aaa');
let s1 = Symbol('aaa');
console.log(s,s1,s==s1); // Symbol(aaa) Symbol(aaa) false
```

以上参考链接：https://www.cnblogs.com/sunshineForFuture/p/10432440.html



### 为什么给对象添加的方法能用在基本类型上？

JavaScript 中的几个基本类型，都在对象类型中有一个“亲戚”（你看他们长得一模一样）。它们是：

*   Number；
*   String；
*   Boolean；
*   Symbol。

所以，我们必须认识到 3 与 new Number(3) 是完全不同的值，它们一个是 Number 类型， 一个是对象类型。

Number、String 和 Boolean，三个构造器是两用的，当跟 new 搭配时，它们产生对象，当直接调用时，它们表示强制类型转换。

Symbol 函数比较特殊，直接用 new 调用它会抛出错误，但它仍然是 Symbol 对象的构造器。

回答上面的问题，为什么给对象添加的方法能用在基本类型上？

答案就是，**运算符提供了装箱操作，它会根据基础类型构造一个临时对象，使得我们能在基础类型上调用对应对象的方法。**

```js
let num = 123;
console.log(num.toString()); //  num被悄悄转换成临时对象 new Number(123)
console.log("abc".charAt(0)); // 'abc' 字符串被悄悄转换成临时对象 new String('abc')
```



### 装箱转换和拆箱转换

每一种基本类型 Number、String、Boolean、Symbol 在对象中都有对应的类，所谓装箱转换，正是把基本类型转换为对应的对象。

前文提到，全局的 Symbol 函数无法使用 new 来调用，但我们仍可以利用装箱机制来得到一个 Symbol 对象，我们可以利用一个函数的 call 方法来强迫产生装箱。

```js
    var symbolObject = (function(){ return this; }).call(Symbol("a"));

    console.log(typeof symbolObject); //object
    console.log(symbolObject instanceof Symbol); //true
    console.log(symbolObject.constructor == Symbol); //true

```

我们定义一个函数，函数里面只有 return this，然后我们调用函数的 call 方法到一个 Symbol 类型的值上，这样就会产生一个 symbolObject。

我们可以用 console.log 看一下这个东西的 type of，它的值是 object，我们使用 symbolObject instanceof 可以看到，它是 Symbol 这个类的实例，我们找它的 constructor 也是等于 Symbol 的，所以我们无论从哪个角度看，它都是 Symbol 装箱过的对象。

装箱机制会频繁产生临时对象，在一些对性能要求较高的场景下，我们应该尽量避免对基本类型做装箱转换。

使用内置的 Object 函数，我们可以在 JavaScript 代码中显式调用装箱能力。

```js
    var symbolObject = Object(Symbol("a"));

    console.log(typeof symbolObject); //object
    console.log(symbolObject instanceof Symbol); //true
    console.log(symbolObject.constructor == Symbol); //true

```

每一类装箱对象皆有私有的 Class 属性，这些属性可以用 Object.prototype.toString 获取：

```js
    var symbolObject = Object(Symbol("a"));

    console.log(Object.prototype.toString.call(symbolObject)); //[object Symbol]

```

在 JavaScript 中，没有任何方法可以更改私有的 Class 属性，因此 Object.prototype.toString 是可以准确识别对象对应的基本类型的方法，它比 instanceof 更加准确。

**拆箱转换**，就是把对象变成基本类型，再从基本类型转换为对应的 String 或者 Number。

拆箱转换会尝试调用 `valueOf` 和` toString` 来获得拆箱后的基本类型。如果 valueOf 和 toString 都不存在，或者没有返回基本类型，则会产生类型错误 TypeError。

如下面的例子：

```js
var o = {
    valueOf : () => {console.log("valueOf"); return {}},
    toString : () => {console.log("toString"); return {}}
}

o * 2
// valueOf
// toString
// TypeError
```
我们定义了一个对象 o，o 有 valueOf 和 toString 两个方法，这两个方法都返回一个对象，然后我们进行 o*2 这个运算的时候（一个对象乘以2，那么js会想把o转换成Number基本类型，会调用valueOf，并期望返回一个基本类型，但是返回的却是一个对象。。），你会看见先执行了 valueOf，接下来是 toString，最后抛出了一个 TypeError，这就说明了这个拆箱转换失败了。

如果将valueOf的返回值改为任意基本类型就会拆箱成功，如：

```js
var o = {
    valueOf : () => {console.log("valueOf"); return undefined},
    toString : () => {console.log("toString"); return {}}
}
```

> valueOf，和toString的执行顺序默认是valueOf先，然后是toString。



### typeof 类型判断准确吗？

事实上，“类型”在 JavaScript 中是一个有争议的概念。一方面，标准中规定了运行时数据类型； 另一方面，JavaScript 语言中提供了 typeof 这样的运算，用来返回操作数的类型，但 typeof 的运算结果，与运行时类型的规定有很多不一致的地方。我们可以看下表来对照一下。

![](https://raw.githubusercontent.com/Daotin/pic/master/img/20190923152315.png)



在表格中，多数项是对应的，但是请注意 object——Null 和 function——Object 是特例，我们理解类型的时候需要特别注意这个区别。