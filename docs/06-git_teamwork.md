

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

 
 
 
## 来自chatgpt4的文档

使用 GitHub 进行协同工作是现代软件开发的常见做法。下面是一个简单的步骤来指导您如何使用 GitHub 进行团队合作：

1. **创建或加入一个仓库**:
   - 如果您是项目的发起者，可以创建一个新的 GitHub 仓库。
   - 如果您是被邀请参与的成员，您应该接受项目负责人发给您的邀请链接，或者直接通过 GitHub 界面搜索并加入该仓库。

2. **克隆仓库**:
   使用 `git clone [仓库链接]` 克隆仓库到本地。

3. **创建新的分支**:
   在进行任何更改之前，为您的任务/特性创建一个新的分支：`git checkout -b [新分支名]`

4. **进行更改**:
   在您的分支上进行所需的更改。

5. **提交更改**:
   - 使用 `git add .` 将更改添加到暂存区。
   - 使用 `git commit -m "提交信息"` 提交您的更改。

6. **推送到 GitHub**:
   使用 `git push origin [您的分支名]` 将您的分支推送到 GitHub。

7. **创建 Pull Request (PR)**:
   在 GitHub 的仓库界面，点击“New pull request”创建一个新的 PR。选择您的分支，并写下您所做的更改的描述。

8. **代码审查**:
   团队成员可以在 PR 上进行评论，提出建议或请求更改。

9. **合并 PR**:
   一旦代码审查通过，项目的主人或管理员可以合并您的分支到主分支。

10. **拉取最新更改**:
   在开始新任务前，确保您的本地仓库是最新的。使用 `git pull origin [主分支名]` 拉取最新的更改。

11. **重复 3-10 步**:
   对于每个新任务或特性，都重复上述步骤。

记住，为了确保协同工作的顺利进行，团队成员应该经常与其他人沟通，并确保遵循团队约定的最佳实践和代码风格。

另外，使用 `.gitignore` 文件可以帮助您忽略不需要跟踪的文件，例如编译产生的文件或敏感配置文件。
