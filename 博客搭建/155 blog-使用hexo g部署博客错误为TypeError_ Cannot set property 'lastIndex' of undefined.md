##  问题描述
使用`hexo g`部署博客的时候，出现以下错误：
```
FATAL Something's wrong. Maybe you can find the solution here: http://hexo.io/docs/troubleshooting.html
TypeError: Cannot set property 'lastIndex' of undefined
    at highlight (G:\Blog\Daotin.github.io\node_modules\highlight.js\lib\highlight.js:511:35)
    at G:\Blog\Daotin.github.io\node_modules\highlight.js\lib\highlight.js:561:21
    at Array.forEach (native)
    at Object.highlightAuto (G:\Blog\Daotin.github.io\node_modules\highlight.js\lib\highlight.js:560:40)
    at G:\Blog\Daotin.github.io\node_modules\hexo-util\lib\highlight.js:117:25
    at highlight (G:\Blog\Daotin.github.io\node_modules\hexo-util\lib\highlight.js:120:7)
    at highlightUtil (G:\Blog\Daotin.github.io\node_modules\hexo-util\lib\highlight.js:22:14)
    at G:\Blog\Daotin.github.io\node_modules\hexo\lib\plugins\filter\before_post_render\backtick_code_block.js:62:15
    at RegExp.[Symbol.replace] (native)
    at RegExp.[Symbol.replace] (native)
    at String.replace (native)
    at Hexo.backtickCodeBlock (G:\Blog\Daotin.github.io\node_modules\hexo\lib\plugins\filter\before_post_render\backtick_code_block.js:14:31)

```

## 解决办法

修改配置文件的 _config.yml，注意不是主题里面的配置文件！ 
把 `auto_detect` 设置为` false` 即可。
