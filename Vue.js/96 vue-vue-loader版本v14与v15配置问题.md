## 问题描述

vue-loader版本`v14`和`v15`在`webpack.config.js`配置上是有区别的。

在`v14`版本，我们只需要配置vue的rules即可：

```js
module: {
    rules: [
        { test: /\.vue$/, loader: "vue-loader" },
    ]
},
```

## 解决办法
需要在`webpack.config.js`头部加上：
```js
const VueLoaderPlugin = require('vue-loader/lib/plugin');
```
然后在`plugin`中加上：
```js
plugins: [
    new VueLoaderPlugin()
]
```

问题即可解决。

