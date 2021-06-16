# JavaScript 如何判断一个对象中是否有某个属性？



今天讲讲，**JavaScript 如何判断一个对象中是否有某个属性？**

我总结了5个方法：

**方法1：**

```javascript
if(Obj[a]) {}
```

缺点：对于参数值为 `undefined` 和 `0` ，`false`等无效。

**方法2：**

```javascript
if(a in Obj) {}
```

相比于方法1，如果你只是将一个属性的值赋值为 undefined 或者 0，而没有使用 delete 运算符删除它，则 in 运算仍然会返回true。

**方法3：**

```javascript
Object.keys(obj).includes('a')
```

**方法4：**

静态方法 `Reflect.has()` 作用与 in 操作符 相同。

```javascript
Reflect.has(obj, 'a')
```

参考链接：[Reflect-MDN](https://link.zhihu.com/?target=https%3A//developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect)

**方法5：** 使用对象的 hasOwnProperty 方法

```javascript
obj.hasOwnProperty('a')
```

> 注意： 
>
> 所有继承了 `Object` 的对象都会继承到 `hasOwnProperty` 方法。这个方法可以用来检测一个对象是否含有特定的自身属性；**和方法2的 `in` 运算符不同，该方法会忽略掉那些从原型链上继承到的属性**。
>
> 参考链接：[https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global\_Objects/Object/hasOwnProperty](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwnProperty)



