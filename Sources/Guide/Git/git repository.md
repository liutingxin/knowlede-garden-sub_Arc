---
date created: 2024-08-03
date modified: 2024-08-03
aliases:
  - 仓库
---

## 空仓库: 向一个空仓库中添加文件

在网页端新建好一个空仓库后，接下来在待上传的目录下输入以下命令即可：

``` shell
echo "Message......" >> ./README.md
git init
git add . && git commit -m "first commit" 
# 也可以使用git commit -a -m "firestorm commit"， 直接上传，不必add .
git branch -M main
git remote add origin git@xxx.com......
git push -u origin main
```

## 已在本地存在的仓库

``` shell
git remote add origin git@github.com:xxx.git
git branch -M main
git push -u origin main
```


当我们在推送仓库文件时，需要保证本地仓库的提交记录和远程仓库的提交记录相同，否则就会出现<mark style="background: #FF5582A6;">提交冲突</mark>的问题。

考虑下面一种情况：

我们在已经存在一个远程仓库，并且这个仓库中，存在一些已经提交的文件和提交记录。

然而我们在本地已经有一个仓库，这个仓库中也有一个文件，但是这个本地仓库的文件和提交记录与远程仓库的都不相同。

如果我们想要把本地仓库的合并到远程仓库中，即在远程仓库的提交文件和提交记录的基础上，再提交我们本地的文件和提交记录，那么我们需要使用到以下命令：

``` Shell
git pull --rebase original main
# rebase: 在另一个分支上重新应用提交
```
