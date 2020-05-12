基本配置

设置用户名和邮箱地址，保证和githubs上的账户关联 

```shell
git config --global user.name "yourname"
git config --global user.mail "youremail"
```



上传本地项目到Github上

在指定目录中执行下面的命令

* 初始化本地git代码仓库

```shell
git init   #初始化git仓库
git status   # 查看当前代码仓库的状态
```

* 添加所要提交的文件

```shell
git add .   # .代表当前目录下的所有文件
```

* 提交

```shell
git commit -m "content"  # 引号中的是提交的描述
```

* 建立远程仓库的连接

```shell
git remote add origin https://github.com/dabric-coder/Algorithm.git
```

* push

```shell
git push origin master
```

创建分支

分支可以方便同时处理多个版本的代码。它是在创建分支的时间点上原始分支的精确副本，所以可以随意更改提交新的分支，直到所有的东西都准备好，就可和原始的分支进行合并。

在本地代码仓库执行下面命令

* 创建分支

```shell
git branch add-readme(branch name)
git branch   # 查看当前仓库分支
```

* 切换分支

```shell
git checkout add-readme
```

* 删除本地分支

```shell
git branch -d branchname
```

* 删除远程分支

```shell
git push origin :branchname
```

* 查看分支

```shell
git branch -a 
git branch -r   # 只查看远程分支
```



