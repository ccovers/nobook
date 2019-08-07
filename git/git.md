# GIT

## git命令
Git is a version control system

## 公钥生成
- 生成用于连接`GitHub`的`SSH`的公私秘钥
ssh-keygen -t rsa -C "youremail@example.com"


### 基本操作
1. 查看系统信息
git config --list

2. 检出（危险命令，重写记录区）
git checkout .

3. 返回到最近一次`git commit`或`git add`时的状态
git checkout -- filename

4. 添加用户名到全局
git config --user.name ""

5. 添加用户邮件到全局
git config --user.email ""

6. 把所在目录变成Git可以管理的仓库
git init


### 分支操作
1. 查看当前分支
git branch

2. 切换分支
git checkout 'branch'

3. 备份当前分支到新的分支
git branch 'new branch'


### 文件操作
1. 复制git项目
git clone https://github.com/ccovers/nobook.git

2. 将更新提交到本地
git add .

3. 添加对提交的备注
git commit -m '提交备注'

4. 将本地提交推送到远端（分支）
git push
git push origin 'branch'

5. 删除远程分支
git push origin :'branch'

6. 从远端（分支）拉取最新版本
git pull
git pull origin 'branch'

7. 从远端拉取分支
git fetch origin '远程分支名':'本地分支名'
git fetch origin

8. 合并其它分支到当前分支（可能出现冲突`conflicts`，需要手动修改）
git merge <branch>

9. 将本地仓库关联到`GitHub`
git remote add origin git@github.com:ccovers/nobook.git

10. 删掉本地分支
git branch -d xxx

11. 删除远程分支
git push origin --delete xxx



### 查看状态、日志、对比差异、版本操作
1. 查看状态
git status

2. 查看提交的记录信息
git log

3. 查看提交的记录信息（一行显示）
git log --pretty=oneline

4. 查看版本号的变更
git reflog

5. 查看`filename`文件的是否修改，修改了哪个位置
git diff HEAD -- filename

6. 返回前一版本
git reset --hard HEAD^

7. 返回前2个版本
git reset --hard HEAD~2

9. 回滚当前分支到指定的版本
git reset --hard 'commit id'






