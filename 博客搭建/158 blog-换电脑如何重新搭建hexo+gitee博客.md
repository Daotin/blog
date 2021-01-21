1、首先需要有原先博客的重要文件备份：

```
_config.yml
 package.json
 scaffolds/
 source/
 themes/
```



2、新电脑需要安装的工具：

```
Git
nodeJs
```



3、全局安装`hexo-cli`

```
npm install hexo-cli -g
```



4、在需要放博客的目录下（比如我的是D:盘根目录）执行下面指令初始化博客系统：

（命令会自动生成博客目录为D:/blog）

```
hexo init blog
cd blog
npm install
```



5、将第一步备份的文件替换掉自动生成的文件



6、启动测试`hexo s`



7、完。

