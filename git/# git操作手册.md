### Git操作手册







##### 常用命令



设置全局账号

```shell
git config --global user.name "yourname"

git config --global user.email "your_email@youremail.com"
```



```shell
git init
```

连接远程仓库

```shell
git remote add origin git@github.com:yourName/repositoryname.git

git remote add origin https://github.com/yourName/repositoryname.git
```

pull远程仓库

```shell
git pull origin master
```

push到远程仓库

```shell
git status　　　　　　　　　　查看工作目录的状态

git add <file>　　　　　　　　将文件添加到暂存区

git commit -m "commnet" 　　提交更改,添加备注信息(此时将暂存区的信息提交到本地仓库)

git push origin master 　　 将本地仓库的文件push到远程仓库
```





分支

```shell
git fetch origin  # 获取远程分支信息到本地
git branch -a  # 查看分支信息
git branch  # 查看分支
git branch <name>   # 创建分支：
git checkout <name>   # 切换分支：
git checkout -b <name>   # 创建+切换分支：
git merge <name>  # 合并某分支到当前分支：
git branch -d <name>   # 删除分支：

git push origin [branch name]  # 将分支送到GitHub
git push origin :[branch name]    #  删除GitHub远程分支，:代表删除
```



把GitHub源码clone到本地仓库

```shell
git clone [gitsite  git远程网址]
```





##### 创建ssh key

```shell
ssh-keygen -t rsa -C "your_email@example.com"
```

