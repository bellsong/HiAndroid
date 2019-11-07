# 通过REPO管理多个git仓库

## Repo是什么

Repo是一款工具，可以轻松的管理多个Git仓库

## Repo使用详解

1. 服务器版本下载：

repo init -u git@xxx/manifest.git -b mybranch -m xxx.xml

repo sync

repo forall -c git checkout --track origin/xxx -b [你的本地分支]

或者：

git clone git@xxxx.git

git checkout --track origin/arm9_6120 -b [你的本地分支名]

 
2. 服务器新加仓库同步

3. 上传本地修改到服务器

repo forall -c git add .

repo forall -c git reset --hard

repo forall -c git pull --rebase        和服务器同步（要上传代码前，一般先进行此操作）

git add .    或git add 文件名            添加当前仓库修改的文件

git commit -m "..."                       在引号中添加你的修改记录

git push origin  本地分支名:froyo_almond       上传本地修改的代码

 

4. 设置一些默认的全局变量，对所有工程代码有效

git config --global user.name yourmail

git config --global user.email yourmail

git config --global push.default tracking     这样后续git push 后面不用带参数

 

5. 查看修改记录

git log                       本地仓库修改记录

repo forall -c git log --since="2011-04-19" --until="2011-04-21"   按条件查看工程所有仓库修改记录

repo status                查看工程中所有仓库的修改状态（包括文件位置）

git status                   查看仓库修改状态

 

6. 分支相关

git branch                 查看本地branch

git branch -r              查看远程branch

git branch -a              查看所有branch

git branch -D  (-d)  （branch name）    删除branch

cat .git/config      可以查看本地branch一些信息

 

7. 修改恢复相关

git checkout filename1  filename2  ...           取消本地修改，和服务器同步

git stash  

git stash apply          先stash本地修改，然后执行git pull --rebase同步，最后再APPLY恢复自己的修改

git reset --soft head_commit  恢复到最后一次commit,保持代码修改

git reset --hard commit    恢复到指定一次commit,放弃之前所有修改

#回退a.py这个文件的版本到上一个版本  

git reset HEAD^ a.py 

git reset commitNO filename

 

8. 本地某仓库出问题了，不好闹腾时，删除之，并重新同步跟踪

project_folder/vendor/qcom$ rm -rf proprietary/                         进到相应目录，删除之

project_folde$ repo sync platform/vendor/qcom/proprietary       重新repo sync,后面路径名称可查看：

                                                                                                 gedit .repo/manifest.xml

git branch -a    ----列举所有BRANCH

git branch -D 700_arm11_server

git branch -D 700_arm11_server_wifi  --删掉所有本地branch

git checkout --track origin/froyo_almond -b 700_arm11_server   然后track远程branch，重新创建本地分支

切换分支  git checkout another_branch   和创建分支就差-b参数


9. tag的使用

git tag [tag_name] [version]，在对应版本上（一般用change的SHA1），创建tag 

git tag -l 列出当前tag 

git tag -d [tag_name] 删除tag 

有了tag以后，可以使用git checkout [tag_name] -b [branch_name]来检出对应tag时刻的代码。也可以用tag name来实现diff等功能。 

 

10. patch的使用

git diff filename1 filename2 ...                  修改位置对比，查看源码

git diff > xxx.patch                                  将修改的地方打成一个patch

git apply xxx.patch                                  将patch打上

 

11. 后续有用到的命令继续添加

git revert 是撤销某次提交。git reset –hard，才是退回到以前的版本

git reset --soft commitNum      保存代码修改的reset，但这个时候无法使用git diff 进行比较修改的文件，必须：

git reset filename filename     这样就可以git diff查看

git diff ffd98b291e0caa6c33575c1ef465eae661ce40c9 b8e7b00c02b95b320f14b625663fdecf2d63e74c 查看某两个版本之间的差异

git diff ffd98b291e0caa6c33575c1ef465eae661ce40c9:filename b8e7b00c02b95b320f14b625663fdecf2d63e74c:filename 查看某两个版本的某个文件之间的差异

  Git 命令别名

$ git config –global alias.co checkout // co将会成为checkout的别名

$ git config –global alias.br branch

$ git config –global alias.ci commit

$ git config –global alias.st status

$ git config –global user.name “username”

$ git config –global user.email username@mail.com



12. gitk使用

 gitk & 可以让终端继续编写执行命令

gitk  文件名     可以查看某个文件的修改记录 

git show （commit）可以看代码差异，类似 gitk

gitk -50  ubuntu64位打开gitk会导致机子变卡，后面带数字可解决此问题



13. 合并冲突

git pull的原理，实际上git pull是分了两步走的，（1）从远程pull下origin/master分支（2）将远程的origin/master分支与本地master分支进行合并。

方法一：如果我们确定远程的分支正好是我们需要的，而本地的分支上的修改比较陈旧或者不正确，那么可以直接丢弃本地分支内容，运行如下命令

(看需要决定是否需要运行git fetch取得远程分支)：

                        $:git reset --hard origin/master

                或者$:git reset --hard ORIG_HEAD

解释：

 git-reset - Reset current HEAD to the specified state

--hard
               Resets the index and working tree. Any changes to tracked files
               in the working tree since <commit> are discarded.

方法二：我们不能丢弃本地修改，因为其中的某些内容的确是我们需要的，此时需要对unmerged的文件进行手动修改，删掉其中冲突的部分，然后运行如下命令

                $:git add filename

                $:git commit -m "message"



方法三：如果我们觉得合并以后的文件内容比价混乱，想要废弃这次合并，回到合并之前的状态，那么可以运行如下命令：

             $:git reset --hard HEAD

14 如何添加之前没有跟踪的文件

     比如kernel中要添加bin文件：打开这个库的.gitignore文件，然后删掉 bin 不跟踪，然后就可以git add bin

15. 出现repo sync 不能更新代码，但单个库 git pull 可以更新   --> 原因当前 manifest.xml 指向的xml 文件不对

      # cd .repo

      # ll    ---> 列出 manifest.xml -> manifests/default.xml

    repo init -m ***.xml  重新指向你想要的xml

## 参考

[Repo 使用详解](https://www.jianshu.com/p/9c57696165f3)