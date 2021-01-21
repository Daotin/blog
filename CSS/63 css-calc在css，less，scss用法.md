在 **css** 使用方式：

```css
width:calc(100% - 10px);
```




在 **less** 使用方式：

```less
width:("calc(100% - 10px)"); 
// 或者
width:calc(~"100% - 10px");  // 更倾向于这种。

// 加变量的使用
div {
    @diff: 30px;
    width: calc(~"100% - @{diff}");
}
```



在 **scss** 使用方式：

```scss
width:calc(100% - 10px);  // 直接使用

// 加变量的使用
$w: 10px;
width: calc(100% - #{$w});
```

