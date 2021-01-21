
# 由来

今天在部署完，发布文章`hexo d`的时候，发布错误，没有发布成功，提示是这样的

`error: failed to push some refs to 'git@gitcafe.com:Daotin/daotin.git'`

导致我的 github 博客没有显示应该显示的文章。



# 解决办法

进入你的博客目录下，我的是 Daotin.github.io ，然后在文件夹查看选项里面，选择显示隐藏的文件，这时候就看见一个`.deploy_git`文件夹，将这个文件夹删除，然后再次部署发布就可以了。
