# 135 vue-Vue如何下载文件？

## 1、a标签url下载

> exportUrl 是下载地址，点击之后直接在当前界面下载文件

```markup
<a :href="exportUrl" class="g-button simple">导出</a>
```

## 2、把地址传入浏览器地址中触发下载

```javascript
window.open(exportUrl, '_self'); // 本窗口打开下载，地址栏的地址并不会改变
window.open(exportUrl, '_blank'); // 新开窗口下载
```

## 3、form表单提交下载

> 不需要我们处理返回二进制流直接下载，非常方便。
>
> form的action设置为接口地址，method设置为post，Post到后台的数据设置为input的属性 name = key，value = value的形式，如果有多个key、value的值要传递，那么就设置多个input来分别储存单个的key、value；如果请求的接口可以不需要参数，那么input还是必须要一个，如果不要得话 会引起接口报错
>
> 原理：form的action相当于一个浏览器本页签/页面的一个请求，不会被后台，前台的路由拦截。所以能够提交成功。
>
> 注意点：如果设置method为get，在action中的uri添加了参数的话，想用这个参数替代input的key、value形式来提交到后台，这参数是没有效果的，后台拿不到这些参数，真正的参数还是以input的name、value的形式储存，在submit方法执行后传递到后台。 这样我们就是实现了文件下载，但是表单提交的数据一般是简单的键值对，如果传参比较复杂可以考虑将表单序列化（将多个key，value序列成json的格式）提交。

```javascript
$.download = function (url, params, method) {
        if (url && params) {
            var inputs = '', input, form = $('<form />').attr("action", url).attr("method", (method || "post"));
            $.each(params, function (k, v) {
                input = $("<input type='hidden' />");
                input.attr("name", k).attr("value", v);
                form.append(input);
            });
            form.appendTo('body').submit().remove();
        }
    };
```

使用方法：

```javascript
var url = "/app/lottery/awards/export", params = { FILETYPE: "excel07", FRMID: that.FRMID };
$.download(url, params, "post");
```

## 4、HTML5的a标签download属性下载

> 标签的download是HTML5标准新增的属性，作用是指示浏览器下载URL而不是导航到URL，因此将提示用户将其保存为本地文件。

```markup
<a href="http://ww2.sinaimg.cn/large/4b4d632fgw1f1hhza4495j20ku0rsjxs.jpg" download>下载</a>
```

> 注意：**此属性仅适用于同源 URL，跨域的资源无效。** 尽管 HTTP URL 需要位于同一源中，但是可以使用 blob: URL 和 data: URL ，以方便用户下载使用 JavaScript 生成的内容（例如使用在线绘图 Web 应用程序创建的照片）。

### 跨域资源下载

```javascript
// 下载一个文本文件
const downloadText = (text, filename = '') {
  const a = document.createElement('a')
  a.download = filename
  // 文本txt类型 
  const blob = new Blob([text], {type: 'text/plain'}) 
  // text指需要下载的文本或字符串内容
  a.href = window.URL.createObjectURL(blob) 
  // 会生成一个类似blob:http://localhost:8080/d3958f5c-0777-0845-9dcf-2cb28783acaf 这样的URL字符串
  document.body.appendChild(a)  
  a.click()
  a.remove()
}
```

