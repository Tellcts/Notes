# **Git常用命令**

---

## **1.仓库与系统配置**

~~~bash
git config --global user.name  "xxxx"                # 配置新 commit 的用户名

git config --global user.email "xxxx@example.com"    # 配置新 commit 的邮箱

git config --global credential.helper store          # 以文本的方式保存 http 凭证

git config -l                                        # 查看所有配置信息

git config --global --unset <config>                 # 删除配置

git remote set-url origin git@xxxxxxx.git       	 # 设置 origin 地址（修改已存在远程仓库的url）

git remote add origin git@xxxxxxx.git				 # 设置 origin 地址（添加一个新的远程仓库的url）
~~~



## **2.仓库初始化与代码拉取**

~~~bash
git init                       # 初始化本地版本库

git clone <url>                # 克隆仓库

git fetch origin               # 拉取远端分支（不影响当前分支）

git pull origin                # 拉取远端分支，并 merge 到当前分支

git pull origin --rebase       # 拉取远端分支，将当前分支 rebase 为远端分支
~~~



## **3.文件操作**

~~~bash
git add .                        # 跟踪所有改动过的文件

git add <file>                   # 跟踪指定的文件

git rm <file>                    # 删除文件

git checkout <file> <commit>     # 将文件或者目录检出（覆盖）为指定版本。commit 可不传，默认表示当前版本

git checkout --force/-f <file>   # **高危** 效果同上，--force 或者 -f 代表强制操作

git clean                        # **高危** 清理工作区中的 untracked 文件，需要配合参数使用：-f 清理文件，-d 清理文件夹，-x 清理原因 .gitignore 被忽略的目录
~~~



## **4.提交操作**

~~~bash
git commit –m "描述信息"         # 提交代码到本地仓库

git commit --amend              # 修改最后一次改动

git merge <branch_name>         # 将指定分支合并到当前分支

git rebase <branch_name>        # 将当前分支变基到指定分支上

git cherry-pick <commit>        # 拣选指定 commit

git reset --hard <commit>       # **高危** 将当前分支的所有的文件和提交历史都还原到指定 commit

git revert <commit>             # 在当前基础上涂改掉某个历史版本
~~~



## **5.查看仓库情况**

~~~bash
git status                     # 查看文件提交状态

git log                        # 查看历史

git diff                       # 查看变更内容

git blame <file>               # 逐行查看文件最新行的变更历史

git reflog                     # 查看本地最近的几次版本变更操作

git remote –v                  # 查看当前远程仓库地址
~~~



## **6. ref 操作**

- **对于 Git 来说，分支、tag统称为ref。**

~~~bash
git checkout –b <branch_name>                    # 创建并切换分支

git checkout <name>                              # 切换指定分支（ref、tag 或者 commit）

git branch <branch_name>                         # 创建分支

git branch -d <branch_name>                      # **高危** 删除指定分支

git tag                                          # 查看本地的 tag

git tag <tag_name> <commit_id>                  # 创建轻量型 tag

git tag <tag_name> <commit_id> -n "备注信息"      # 创建附注型 tag

git show-ref                                     # 显示本地所有的 ref

git update-ref -d <ref_name>                    # **高危** 删除指定 ref
~~~



## **7.代码推送**

~~~bash
git push origin <local>:<ref_name>              # 将本地的版本（分支名、commit、tag）推动到远端 ref

git push origin :<ref_name>                     # **高危** 删除远端 ref

git push origin <ref_name> -d                   # **高危** 删除远端 ref

git push origin <ref_name> -f/--force           # **高危** 强制覆盖远端 ref

git push origin <ref_name> --force-with-lease   # **高危** 效果同上，但更安全一些：推送之前会先检查远端的目标分支commit跟本地下载的是否一致，不一致则push失败。这时需要执行 git fetch 更新本地版本，fetch后最好人工确认新版本要不要继续执行强制覆盖																

git push --mirror                               # **高危** 将比较本地和远端的差异，推送成功后，远端版本库将与本地保持一致，使用不当可能导致大量分支历史丢失，只建议bare版本库同步时使用
~~~



## **8.复杂场景**

#### **问题排查与仓库维护相关**

~~~bash
git fsck --full                                        # 仓库完整性检查

ssh -T -v git@xxxxx.git                                # 测试 ssh 连接，并打印交互信息（-v）

GIT_CURL_VERBOSE=1 git ls-remote https://xxxxxxx.git   # 查看远端分支，并打印 http 日志（GIT_CURL_VERBOSE=1）

cat ~/.gitconfig                                       # 查看本地配置

cat .git/config                                        # 查看当前仓库配置

git gc                                                 # **高危** 回收未被 ref 引用到的对象

git remote prune origin                                # 对比远端分支，删除掉本地旧的、有冲突的分支
~~~



#### **3种分支合并方式**

~~~bash
# 普通合并
git checkout target_branch
git merge --no-ff source_branch

# 压缩合并
git checkout target_branch
git merge --squash source_branch
git commit

# 变基合并
git checkout source_branch --detach
git rebase target_branch
git branch target_branch HEAD -f
git checkout target_branch
~~~



#### **初始化并关联远程仓库**

~~~bash
cd existing_folder
git init
git remote add origin <url>
git add .
git commit -m "init"
git push -u origin
~~~

---

