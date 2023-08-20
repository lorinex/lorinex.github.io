---
categories:
  - 编程语言
  - GIT
---
## --gitee创建仓库

* 在gitee新建一个仓库
* 设为开源仓库
* 点http 复制
* 在codes文件夹右键 点git bush
* 输入git clone 右键paste
* 会自动生成一个文件夹
* `git config --global user.name "谢欣栩"`
* `git config --global user.email "1187601935@qq.com"`
* 就可以add commit push
* git add .
* git commit -m message
* git push origin master

## 日常工作流程

本地工作区（尚未暂存） ---> add . 到暂存区 ---> commit 到本地仓库 ---> pull拉取关联远程仓库分支合并到本地的分支---> push 到远程分支

## 分支

### 分支管理

　　**作用**：假设你准备开发一个新功能，但需要两周才能完成，第一周写了60%，如果提交，由于代码还没写完，不完整的代码库会导致别人不能干活,如果等代码全部写完在一次提交，又会存在丢失每天进度的风险。有了分支，可以避免上述问题，创建一个属于自己的分支，别人看不到，还继续在原来的分支上正常工作，而我们在自己的分支上干活，想提交就提交，直到开发完毕后，在一次性合并到原来的分支上，这样，即安全又不影响别人工作。

​        **特点**：Git分支是与众不同的，无论创建、切换、和删除分支，Git在非常短的时间内就能完成，无论版本库是1个文件还是1万个文件。

​    　**master主分支**：在版本回退中，每次提交，Git都把它们串成一条时间线，在git里，这个分支叫主分支,即master分支，HEAD严格来说不是指向提交，而是指向master，master才是指向提交的，所以，HEAD指向的就是当前分支。每次提交，master分支都会向前移动一步，这样，随着不断提交，master分支的线也越来越长。

![img](https://img2018.cnblogs.com/i-beta/1213900/201911/1213900-20191126143130871-1058818644.png)

## git合并分支

合并步骤：
1、进入要合并的分支（如开发分支合并到master，则进入master目录）
git checkout master
git pull

2、查看所有分支是否都pull下来了
git branch -a

3、使用merge合并开发分支
git merge 分支名

4、查看合并之后的状态
git status

5、有冲突的话，通过IDE解决冲突；

6、解决冲突之后，将冲突文件提交暂存区
git add 冲突文件

7、提交merge之后的结果
git commit

如果不是使用git commit -m “备注” ，那么git会自动将合并的结果作为备注，提交本地仓库；

8、本地仓库代码提交远程仓库
git push

git将分支合并到分支，将master合并到分支的操作步骤是一样的。

## git 回退版本

在主分支：`git reset HEAD`

非主分支：`git add .` 再`git reset --hard`



