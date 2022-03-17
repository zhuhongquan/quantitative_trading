# Git

> 一个比较基础的教程：[https://www.liaoxuefeng.com/wiki/896043488029600/896067008724000](https://www.liaoxuefeng.com/wiki/896043488029600/896067008724000)

## 版本控制的概念

* 仓库（Repository）：项目文件版本信息的存储地&#x20;
* 工作区：供人编辑的项目文件&#x20;
* 提交/版本（Commit）：项目文件的一个**快照**
* 差分（Diff）：两个版本之间的差异部分&#x20;
* 分支（Branch）：**并行**的版本串

## Git简介

开源分布式版本控制系统\
跨平台\
Linus 为了管理 Linux 内核用了 10 天写出来的

## Git安装

* Windows：[官网安装程序](https://git-scm.com/downloads)
* Linux（Debian 系）：`sudo apt install git`
* macOS：`brew install git`
* 验证安装： `git --version` 之后可能需要一些配置，使用时根据提示操作就好

![](<../.gitbook/assets/image (1) (1).png>)

## 创建仓库

* 在想要做仓库的目录下执行 `git init` 会生成一个 `.git` 文件夹，即仓库
* 不要尝试修改 `.git` 文件夹下的内容
* 仓库以外可编辑的文件，被称为工作区，一般情况下是你的代码

## 文件的几种状态

* 未跟踪（Untracked）
* 未修改（Unmodified）
* 已修改（Modified）
* 已暂存（Staged）
* git status 查看状态

![git status 查看状态](<../.gitbook/assets/image (2) (1).png>)

![文件状态的转换](<../.gitbook/assets/image (7) (1) (1).png>)

## 常用命令

* git status 查看工作区文件状态
* git add 添加被追踪文件/暂存修改
* git commit 提交所有修改，并说明提交日志
* git log 查看当前分支提交历史
* git diff 查看版本差分
* git reset --hard 将仓库和工作区切换到某个版本的状态
* git checkout 将工作区中的某些文件切换到某个版本的状态
* 具体用法：--help/官方文档

## .gitignore

* 用于告知 git 忽略某些文件
  * 典型地：编译后的文件、可以通过网络安装的依赖等
* 场景
  * git add --all
  * git log

## 其它

Git 主要用于管理纯文本文件，对于二进制文件，`git diff`会失效，这也是很多文档使用 Markdown 而不是 docx 来写的原因。

## 小结

* 讲完了“冰山一角”：Git 的工作流程与概念
* 需要在实践中学习
* 参考：[《Pro Git》](https://git-scm.com/book/zh/v2)
