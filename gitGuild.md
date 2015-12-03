# git 简单入门指南
如何把一个工程从无到有,放到github上.

## 1 初始化
在工程目录下,初始化git: `git init`
> Create an empty Git repository in the specified directory.Usage:
```shell
git init <directory>
```

## 2 自定义配置**github**的地址 (*可缺省*)
### 本工程配置方式
只在这个工程生效,适合一台机器有多个github,比如需要同时想gitlab, github, bitbucket上上传代码,记得采用这种方式.
```shell
git config -- user.name <name>
git config -- user.email <email>
```

### 全局方式
```shell
git config --global user.name <name>
git config --global user.email <email>
```
## 3 添加分支,并关联上github
首先,现在github上创建一个empty的git工程,比如`maven_git_guide.git`,然后依次执行下面的命令.
```shell
git remote add origin https://github.com/HQebupt/maven_git_guide.git
git remote -v
git pull origin master
```

然后,提交一次commit,才可以push到远程的master分支.
```shell
git add README.md
git commit -m "初始化工程成功"
git push -u origin master
```

## 常用命令
- clone: `git pull <remote>`
- 切换分支:`git checkout <master>`
- 文件放弃修改:`git checkout -- <fileName>`
- 删除本地仓库标签 `git tag -d [tag]`
- 打标签(带有注释的) `git tag -a v1.0 -m 'my version message'`
- 推送标签 `git push origin [tagname]` 
- 推送所有标签 `git push origin --tags`
