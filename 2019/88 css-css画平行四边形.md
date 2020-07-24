利用`伪元素`来操作。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .location{
            position: relative;
            top: 150px;
            left: 150px;
        }
         .button{
             width: 120px;
             height: 50px;
             line-height: 50px;
             text-align: center;
             color: white;
         }
        .button:before{
            content: '';
            position: absolute;
            top: 0; right: 0; bottom: 0; left: 0;
            background-color: #51bfff;
            transform: skewX(-45deg);
            z-index: -1;
        }
    </style>
</head>
<body>
    <div class="location button">click</div>
</body>
</html>
```

![image](https://user-images.githubusercontent.com/23518990/70420018-ae076b00-1aa1-11ea-82fa-fa46bd9a2b62.png)
