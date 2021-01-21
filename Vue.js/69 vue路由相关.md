## router-link与a标签跳转区别

router-link只会**按需加载**需要改变的页面；但是a标签会**刷新整个页面**（包括不需要刷新的地方，比如左边目录结构不变，只改变右侧显示的内容）





## 获取地址栏路径

```js
this.$route.path ：//获取当前路由
this.$route.fullPath：//获取当前路由（包含路由参数）

this.$router.push('/list'); //路由跳转
```



## 监听路由变化

watch监听路由变化：

```js
watch:{
    $route(to,from) {
        
    }
}
```

监听**路由参数**变化：（组件守卫）

```js
// 在当前路径下,当路由的参数发生变化时，才会触发该路由守卫
beforeRouteUpdate(to, from, next) {
    console.log(to.params.path);
}
```









