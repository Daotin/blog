最近新发现了一个特别简介的主题`maupassant`，非常符合我的审美观念。

[github项目地址](https://github.com/tufu9441/maupassant-hexo)

[Demo](https://www.haomwei.com/technology/maupassant-hexo.html)

具体的操作步骤其实 Demo 里面描写的非常清楚了，这里我只描写我踩过的坑，也帮助大家少踩坑。



1、主题部分文字显示问题

主题安装好了，`hexo s` 查看的时候，部分提示显示乱码。如下图：

![](https://raw.githubusercontent.com/Daotin/pic/master/img/dd1.png)




问题1： hexo主题下 language 设置那里 未设置语言为 zh-CN 

修改之后再次刷新，还是未解决。

问题2： hexo-renderer-sass 插件未安装好。再次安装（注意：国内环境可能安装失败，需要代理，你懂得），再次安装好后，重新 `hexo s` ，成功显示中文。



