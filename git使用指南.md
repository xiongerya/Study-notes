# git使用指南
## git 安装&初始化配置
git官网下载地址：<https://git-scm.com/>
**全局配置& ssh key**
本地配置用户名和邮箱
	config --global user.name "你的用户名"
	config --global user.email "你的邮箱"
生成ssh key: `ssh-keygen -t rsa -C "你的邮箱"`
复制ssh key: `clip < ~/.ssh/id_rsa.pub`
讲ssh key添加到github setting里的SSH and GPG keys
![ssh key](./images/git/ssh key.png)
测试是否添加成功: `ssh -T git@github.com`
**查看&删除配置**
查看配置: `git config --list`或`git config -l`
查看local配置: `git config --local  --list`
查看global配置: `git config --global  --list`
查看system配置: `git config --system --list`
删除local配置 : `git config --local --unset section.key`
删除global配置 : `git config --global --unset section.key`
删除system配置 : `git config --system --unset section.key`
___
## git本地仓库操作
### 文件添加&提交
初始化办本地仓库: `git init [project-name]`
查看文件状态: `git status`
![git-work](./images/git/status.png)
添加全部新文件至暂存区: `git add .`
添加某些新文件至暂存区: `git add file-name`
提交暂存区文件至仓库: `git commit -m "提交信息"`
![git-work](./images/git/git-work.png)
### 文件修改
查看工作区: `git diff`
## git本地操作&远程仓库关联
**本地&远程工作关联示意图**
![work steps](./images/git/work steps.png)

### 建立本地仓库并上传至远程
初始化本地仓库: `git init [project-name]`

### 多分支时（已有本地&远程仓库）
复制远程仓库地址url（http/ssh）
本地仓库关联远程: `git remote add origin url` 
从远程仓库pull文件: `git pull origin master`
（若远程仓库没有文件则跳过该步骤）
本地仓库上传远程: `git push origin master`
（若首次上传远程使用: `git push -u origin master`）
远程仓库关联完成，后续直接进行add-commit-push操作

### clone远程仓库（未建立本地仓库）
复制远程仓库地址url（http/ssh）
**clone自己/团队远程仓库**
克隆仓库到本地: `git clone url`
后续直接进行add-commit-push操作
**clone其他人的远程仓库**
克隆仓库到本地: `git clone url`

___
## git
