clamp() 函数的作用是把一个值限制在一个上限和下限之间，当这个值超过最小值和最大值的范围时，在最小值和最大值之间选择一个值使用。

## 语法
clamp() 函数接收三个用逗号分隔的表达式作为参数，按最小值、首选值、最大值的顺序排列。

```css
{
    font-size: clamp(20px, 18px, 40px); 
    width: clamp(100px, 100%, 200px);
}
```

- 当首选值比最小值要小时，则使用最小值。
- 当首选值介于最小值和最大值之间时，用首选值。
- 当首选值比最大值要大时，则使用最大值。



## 兼容性
https://caniuse.com/?search=clamp
从 caniuse 网站可以看出，**不支持IE11**。

![image](https://user-images.githubusercontent.com/23518990/95972278-d770f000-0e44-11eb-8edb-30e3f9ca4a22.png)


