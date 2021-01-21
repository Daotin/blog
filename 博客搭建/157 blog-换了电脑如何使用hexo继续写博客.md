# 前言

我们知道，使用 Github+hexo 搭建一个个人博客确实需要花不少时间的，我们搭好博客后使用的挺好，但是如果我们有一天电脑突然坏了，或者换了系统，那么我们怎么使用 hexo 再发布文章到个人博客呢？

如果我们还是按照之间我们总结的教程再次搭建一个博客，然后修改代码更换 hexo 主题等，各种配置特别繁琐，那么有没有一种方便的方法，直接使用我们之前搭建好的博客的源文件呢？

答案是肯定的，下面我们只需要简简单单几步就可以在新的电脑上继续轻松愉快的写文章，而不必在意环境搭建的繁琐了，废话不多说，开始干活！


# 操作步骤



## 一、安装必要软件

安装 Git 客户端

安装 node JS 

## 二、在 github 官网添加新电脑产生的密钥

参考我的另一篇文章：[http://fengdaoting.com/2018/02/27/技术综合/hexo上部署博客到Github失败/](http://fengdaoting.com/2018/02/27/技术综合/hexo上部署博客到Github失败/)



## 三、源文件拷贝

将你原来电脑上个人博客目录下必要文件拷到你的新电脑上（比如F:/Blog目录下），注意无需拷全部，只拷如下几个目录：

```
_config.yml
 package.json
 scaffolds/
 source/
 themes/
```



## 四、安装 hexo

在 cmd 下输入下面指令安装 hexo：

```
npm install hexo-cli -g
```



## 五、进入 F:/Blog 目录（你拷贝到新电脑的目录），输入下面指令安装相关模块

```
npm install
npm install hexo-deployer-git --save  // 文章部署到 git 的模块
（下面为选择安装）
npm install hexo-generator-feed --save  // 建立 RSS 订阅
npm install hexo-generator-sitemap --save // 建立站点地图
```



## 六、测试

这时候使用 `hexo s` 基本可以看到你新添加的文章了。



## 七、部署发布文章

```
hexo clean   // 清除缓存 网页正常情况下可以忽略此条命令
hexo g       // 生成静态网页
hexo d       // 开始部署
```



<u>***最后，如果帮助到你了，请帮忙点下赞，你的赞就是对我最大的鼓励！***</u>

原文链接：[http://localhost:4000/2018/03/16/技术综合/换了电脑如何使用hexo继续写博客/](http://localhost:4000/2018/03/16/技术综合/换了电脑如何使用hexo继续写博客/)

