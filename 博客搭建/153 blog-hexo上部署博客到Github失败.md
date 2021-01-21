## fatal: could not read Username for 'https://github.com': No error

今天在上传博客到搭建到 Github 的个人博客上的时候，已经使用 `hexo s` 预览成功的。但是在`hexo d`部署到个人博客的时候出现了一面的问题：

![](https://raw.githubusercontent.com/Daotin/pic/master/img/1.png)



于是我就使用关键字搜索，找到下面的解决方式：

```
把_config.yml文件中repository: https://github.com/Daotin/Daotin.github.io.git 这个地址改为  git@github.com:Daotin/Daotin.github.io.git
```

改完之后再次部署 `hexo d`:

还是错误，但是错误已经不同了：

![](https://raw.githubusercontent.com/Daotin/pic/master/img/2.png)



于是我再次查找原因，得到的结果说**没有在 Github 上添加公钥**。



这就奇观了，我之前一直使用的好好地，怎么突然就坏了呢？

我使用下面代码去测试下：

```
ssh -T git@github.com
```

![](https://raw.githubusercontent.com/Daotin/pic/master/img/3.png)



如上， `Permission denied(publickey)`这就表示缺少公钥。



好吧，缺少公钥就加一个呗。

---

# Github 添加 SSH Keys

首先在本地创建 `SSH Keys`:

```
ssh-keygen -t rsa -C "17607171931@163.com"
```

后面的邮箱即为 github 注册邮箱，也是你登录 Github 的邮箱，之后会要求确认路径和输入密码，一路回车就行。

成功的话会在 `~/ `下生成 `.ssh`文件夹，进去，打开 `id_rsa.pub`，复制里面的`key`即可。



![](https://raw.githubusercontent.com/Daotin/pic/master/img/4.png)





然后我们再次测试下公钥有没有添加成功：`ssh -T git@github.com`

![](https://raw.githubusercontent.com/Daotin/pic/master/img/5.png)

成功了。



之后我们再次部署我们的博客网站：`hexo d`

![](https://raw.githubusercontent.com/Daotin/pic/master/img/6.png)



成功。我的个人博客网站也正常显示：

![](https://raw.githubusercontent.com/Daotin/pic/master/img/7.png)

---