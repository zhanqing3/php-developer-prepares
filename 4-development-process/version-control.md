# 版本管理

> 眼下最流行的"版本管理系统"，非 Git 莫属！
> 
> 关于是否使用 SVN？ 个人的建议是如果团队有权限管理、代码安全控制等考虑的话，那么考虑 SVN，否则建议使用 Git/Mercurial，  
> 关于 Mercurial 还是 Git，因为 GitHub 这个大社区的缘故，因此如果是程序员个人搞开源的话，还是建议优先 Git，如果 Git、Mercurial 您都想试试，那么这里打个广告：强烈建议您试试 [BitBucket](https://bitbucket.org/)，Mercurial 和 Git 都支持，重要的是支持无限个私有库（业界良心），用来搞私人项目真心好，1024 个赞！Google Code 在 2008 年曾经有一个关于 Git or Mercurial 的分析文章（[DVCSAnalysis : Analysis of Git and Mercurial](https://code.google.com/p/support/wiki/DVCSAnalysis)），不过时间很久了，仅供参考。Google Code 也即将被大扫除掉了~ :-(

## Git

### Git Flow

* [Blog: A successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/) - 基本上是目前为止最靠谱的 Git Flow 指南了，也可以参考阮一峰老师的解读 - [Blog: Git 分支管理策略](http://www.ruanyifeng.com/blog/2012/07/git.html)
* [Atlassian Git Tutorials](https://www.atlassian.com/git/tutorials) - Atlassian 关于 Git 的教程（**推荐**）。 Comparing Workflows 这个章节介绍了几种常见的 Git 协作形式，[Comparing Workflows 中文翻译：Git 工作流指南](http://blog.jobbole.com/76843/)


### Github Flow

* [Tutorial: Understanding the GitHub Flow](https://guides.github.com/introduction/flow/index.html) - Github 官方指南 
* [Blog: Github flow](http://scottchacon.com/2011/08/31/github-flow.html) - [Scott Chacon](http://scottchacon.com/about.html) 在 Github 工作，更是 [《Pro Git Book》](http://git-scm.com/book/en/v2) 作者


### 扩展阅读

* Book: Pro Git Book - 关于 Git 这一本书够了，入门先看前面几章了解基本概念和日常使用
    * [Pro Git 第一版 V1](http://git-scm.com/book/zh/v1)
    * [Pro Git 第二版 V2](http://git-scm.com/book/en/v2)
* [GitHub Cheatsheet](http://git.io/sheet) - 这个项目整理了许多关于 Github & Git 的小技巧，强烈建议过一下


## Subversion/SVN


### SVN 版本库布局 - trunk, branches, tags

SVN  Flow? :-)  

SVN 版本库只是一个包含目录和文件的文件系统，而且它的文件系统是版本化的，并且实现了” 廉价”拷贝，让它的这种操作比传统文件系统廉价很多，但是版本库本身还是像一个文件系统。

至于版本库中目录的名称，只是一种习惯，他们在 SVN 中没有特别含义。我们在一些著名开源项目的版本库中，通常可以看到 trunk、branches、tags 等三个目录。

由于 SVN 固有的特点，目录在SVN 中并没有特别的意义，但是这三个目录却在大多数开源项目中存在，这是因为这三个目录反映了软件开发的通常模式。
* trunk 是主分支，可以认为是项目的开发主线，你可以称之为 main、master 或任何你喜欢的名字。
* branches 是分支。一些阶段性的 release 版本，这些版本是可以继续进行开发和维护的，则放在 branches 目录中。人们因各种目的使用分支，你或许希望通过特性分支或客户修改分支来隔离你的发布或维护分支等。
* tags 目录一般是只读的，这里存储阶段性的发布版本，对作为里程碑的版本进行存档。通常情况下 tags 与分支的区别就是 tags 一旦创建不能修改，你也可以将标签目录叫做 releases 或任何你喜欢的名字
> 

开源社区采纳了多种常用布局作为最佳实践，我们应该遵循推荐的布局方式，按照这些布局约定，大家可以方便的访问版本库找到所需要的信息。 

有两种常见的版本库布局： 

第一种布局是版本库包含一个项目或一组紧密联系项目的最佳选择。

    Project
     ├─trunk
     ├─branches
     └─tags

这个布局非常好用，因为分支与标签整个项目或一组项目会非常简单，只需要一个简单的命令： 

```sh
svn copy url://repos/trunk url://repos/tags/tagname -m “Create tagname” 
```

这可能是最常用的版本库布局，被许多开源项目采用，就像 Subversion 本身和 Subclipse，这是大多数主机站点，如Tigris.org、SourceForge.net 和 Google Code 遵循的方法，这些站点的每个项目有自己的版本库。 

另一种布局是针对一个版本库包含不相关项目的最佳选择。

    ProjectA
     ├─trunk
     ├─branches
     └─tags
    ProjectB
     ├─trunk
     ├─branches
     └─tags

在这种布局里，每个项目会存在顶级目录里，然后该目录之下创建 trunk/branches/tags，其中与第一种布局相同，这只是将项目放到自己版本库方式的替换，他们都在一个版本库中。 Apache 软件基金会使用这种布局方式来存放他们的所有项目在一个版本库。 

通过这种布局，每个项目都有自己的分支和标签，可以很容易使用一个命令创建分支和标签，就像前面展示的： 

```sh
svn copy url://repos/ProjectA/trunk url://repos/ProjectA/tags/tagname -m “Create tagname” 
```

这种布局可以简单的创建同时包含 ProjectA 和 ProjectB 的标签，你可以这样做，但是需要多个命令，你也要决定是否创建一个特别的目录存放这种分支和标签，如果你需要经常这样做，你或许应该考虑第一种布局。 


### 扩展阅读

* [Book: Subversion 与版本控制](http://svnbook.red-bean.com/index.zh.html)


## **Mercurial/hg**

### 扩展阅读

* [Book: Mercurial: The Definitive Guide](http://hgbook.red-bean.com/)
* [Learning Mercurial in Workflows](http://mercurial.selenic.com/guide)
* [Blog: An Introduction to Hgflow](https://andy.mehalick.com/2011/12/24/an-introduction-to-hgflow)

## 相关工具

* [TortoiseSVN](http://tortoisesvn.net/) - Subversion client for Windows
* [TortoiseGit](https://tortoisegit.org) - Git client for Windows
* [TortoiseHg]() - Mercurial client for Windows, Mac OS and Linux
* [SmartSVN](http://www.smartsvn.com/) - Subversion client for Mac OS, Windows and Linux
* [RabbitVCS](http://rabbitvcs.org/) - Subversion, Git & Mercurial client for Linux
* [Tower](http://www.git-tower.com/) - Git client for Mac OS
* [Cornerstone](https://www.zennaware.com/cornerstone/) - Subversion client for Mac OS
* [Versions](http://versionsapp.com/) - Subversion client for Mac OS
* [SourceTree](http://sourcetreeapp.com/) - Git & Mercurial client for Windows or Mac
* [More GUI client from git-scm.com](http://git-scm.com/download/gui/linux)