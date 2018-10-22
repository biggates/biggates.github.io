---
title: 将 SVN 仓库迁移到 git
layout: post
---

# 将 SVN 仓库迁移到 git

0. 准备工作

需要准备的内容有：

* 需要迁移的 SVN 地址和具有权限的账号
* 足够的硬盘空间
* 命令行中具有 git 和 git-svn 工具

```
# sudo yum install git git-svn
```

```
# sudo apt-get install git git-svn
```

* 如果需要的话，还要准备一个 authors-transform.txt 文件

```
# svn log -q | awk -F '|' '/^r/ {sub("^ ", "", $2); sub(" $", "", $2); print $2" = "$2" <"$2">"}' | sort -u > authors-transform.txt
```

然后手动修改其中的作者信息，如将

```
jwilkins = jwilkins <jwilkins>
```

修改为:

```
jwilkins = John Albin Wilkins <johnalbin@example.com>
```

1. 使用 git svn 初始化仓库

```
# git svn clone [SVN 地址] -A authors-transform.txt
```

2. 配置 git 仓库地址

```
# git remote add origin [git 仓库地址]
```

3. push 到 git 仓库

```
# git push origin --all
```

## 参考

* [Migrating to Git](https://git-scm.com/book/en/v2/Git-and-Other-Systems-Migrating-to-Git)
* [Converting a Subversion repository to Git](https://john.albin.net/git/convert-subversion-to-git) (较旧)
