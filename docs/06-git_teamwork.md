

# github team work {#git_teamwork}

## 下载github desktop

下载安装并登陆.

## Fork repo

在网页操作

## Clone repo到本地

使用Rstudio操作.

打开Rstudio,点击new project, 点击version control,点击git,复制在你github主页的repo地址,然后复制到url栏中,修改地址.点击确定.

## 确认远程repo

```markdown
git remote -v
```

## 开始工作

每次修改之前,请确认本地repo和源repo保持一致.

## github desktop

可以使用github desktop进行.

## terminal

也可以在terminal中进行.

打开Rstudio,左下角选择'Terminal'.

然后

```markdown
git fetch upstream
git merge upstream/master
```

这时候你可以看到什么发生了改变,如果源repo进行了修改.

## 开始修改.

> 注意,每次只修改一小部分,就要开始上传.

## 再次确认源repo是否发生了改变.

为了防止在修改期间,源repo也发生了修改,请再次使用上面步骤,同步源repo.

## 提交

在github desktop中提交修改.

## 提交pull request

这些是在github网页完成.

打开自己的github中的该项目.

点击`Pull requests`

点击new pull request

点击creat pull request.

写下一些记录信息.

点击create pull request.

 
