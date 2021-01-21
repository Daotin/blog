# 144 github-国内加速访问Github的办法，超级简单！（转）

一、打开 [http://IPAddress.com](http://IPAddress.com) 网站，查询下面3个网址对应的IP地址

```text
http://github.com 
http://assets-cdn.github.com 
http://github.global.ssl.fastly.net
```

二、修改本地电脑系统hosts文件

```text
# ipaddress.com 加快github访问速度

140.82.114.3 github.com
199.232.5.194 github.global.ssl.fastly.net
185.199.108.153 assets-cdn.github.com
```

三、刷新系统dns缓存 用`WIN+R`快捷键打开运行窗口，输入命令：cmd并回车进入命令行窗口。 接着输入命令：`ipconfig /flushdns`回车后执行刷新本地dns缓存数据即可。

![image](https://user-images.githubusercontent.com/23518990/70410454-15b1bc00-1a8a-11ea-9ead-637bd0cf0e77.png)

参考链接：[https://zhuanlan.zhihu.com/p/75994966](https://zhuanlan.zhihu.com/p/75994966)

