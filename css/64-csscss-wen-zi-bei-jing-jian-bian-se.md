# 64 css-css 文字背景渐变色

![&#x5FAE;&#x4FE1;&#x622A;&#x56FE;\_20191126175826](https://user-images.githubusercontent.com/23518990/69619372-6d0d6080-1076-11ea-93ff-fdd0bc09d6c8.png)

```markup
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width" />
        <title>css文字背景渐变色</title>
        <style type="text/css">
            section {
                text-align: center;
            }
            /* 背景渐变色 */
            .demo-1 {
                margin: 40px auto;
                width: 200px;
                height: 40px;
                border-radius: 20px;
                line-height: 40px;
                text-align: center;
                color: white;
                background-image: linear-gradient(90deg, #7e40ee 0, #63d9ee 100%);
            }

            /* 文字渐变色 */
            .demo-2 {
                display: inline-block;
                font-family: 'PingFangSC-semibold';
                font-size: 30px;
                text-align: center;
                color: transparent;
                /* 首先要设置文字透明 */
                -webkit-background-clip: text;
                /* 其次设置背景剪切为text */
                background-image: linear-gradient(90deg, #7e40ee 0, red 100%);
            }

            /* 文字背景为图案 */
            .demo-2.demo-2-1 {
                background-size: 200%;
                background-position: 10% 54%;
                background-image: url('http://img.hb.aicdn.com/24d85605d56d0d670bd7b5dc8f81625881eb43a242b9a-CMwyfa_fw658');
            }
        </style>
    </head>

    <body>
        <section>
            <div class="demo-1">
                按钮
            </div>
        </section>

        <section>
            <div class="demo-2">
                我是 SLOGAN
            </div>
        </section>

        <section>
            <div class="demo-2 demo-2-1">
                我是 SLOGAN
            </div>
        </section>
    </body>
</html>
```

