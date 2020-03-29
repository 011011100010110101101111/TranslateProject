[#]: collector: (lujun9972)
[#]: translator: (MjSeven)
[#]: reviewer: ( )
[#]: publisher: ( )
[#]: url: ( )
[#]: subject: (Run your blog on GitHub Pages with Python)
[#]: via: (https://opensource.com/article/19/5/run-your-blog-github-pages-python)
[#]: author: (Erik O'Shaughnessy https://opensource.com/users/jnyjny/users/jasperzanjani/users/jasperzanjani/users/jasperzanjani/users/jnyjny/users/jasperzanjani)

在 GitHub 上使用 Python 运行你的博客
======
使用 Pelican（一个基于 Python 的平台）在 GitHub 上创建博客，看起来很不错。

![Raspberry Pi and Python][1]

[GitHub][2] 是一个非常流行的用于源代码控制的 Web 服务，它使用 [Git][3] 同步本地文件和 GitHub 服务器上保留的副本，这样你就可以轻松地共享和备份你的工作。

除了为代码仓库提供用户界面之外，GitHub 还运允许用户直接从仓库[发布网页][4]。GitHub 推荐的网站生成软件包是 [Jekll][5]，使用 Ruby 编写。因为我是 [Python][6] 的忠实粉丝，所以我更喜欢 [Pelican][7]，这是一个基于 Python 的博客平台，可与 GitHub 很好地协同工作。

Pelican 和 Jekll 都可以将 [Markdown][8] 或 [reStructuredText][9] 中编写的内容转换为 HTML 以生成静态网站，并且两个生成器都支持定制的主题。 

在本文中，我将介绍如何安装 Pelican、设置 GitHub 仓库、运行快速入门帮助、编写一些 Markdown 文件以及发布第一篇博客。我假设你有一个 [GitHub 账户][10]，熟悉[基础的 Git 命令][11]，并且想使用 Pelican 发布博客。

### 安装 Pelican 并创建仓库

首先，你必须在本地计算机上安装 Pelican 和 **ghp-import**。使用 Python 软件包安装工具 [pip][12]（你有，对吧？），这非常容易：

```
$ pip install pelican ghp-import Markdown
```

然后，打开浏览器并在 GitHub 上为你的新博客创建一个新的仓库，命名如下（在此处以及整个教程中，用 GitHub 用户名替换 <username>）：

```
https://GitHub.com/username/username.github.io
```

让它保持空白，稍后我们用引人注目的博客内容来填充它。

使用命令行（确保正确），将空的 Git 仓库克隆到本地计算机：

```
$ git clone <https://GitHub.com/username/username.github.io> blog
$ cd blog
```

### 奇怪的把戏...

在 GitHub 上发布 Web 内容有一个不太明显的技巧，对于用户页面（托管在名为 _username.github.io_ 的仓库的页面上），其内容来自 **master** 分支。

我强烈建议不要将所有 Pelican 配置文件和原始 Markdown 文件都保留在 **master** 中，**master** 中只保留 Web 内容中。因此，我将 Pelican 配置和原始内容保留在一个我喜欢称为 **content** 的单独分支中。（你可以随意创建一个分支，但以下内容沿用 **content**。）我喜欢这种结构，因为我可以放弃掉 **master** 中的所有文件，然后用 **content** 分支重新填充它。

```
$ git checkout -b content
Switched to a new branch 'content'
```

### 配置 Pelican

现在该进行内容配置了。Pelican 提供了一个很棒的初始化工具 **pelican-quickstart**，它会询问你有关博客的一系列问题。

```
$ pelican-quickstart
Welcome to pelican-quickstart v3.7.1.

This script will help you create a new Pelican-based website.

Please answer the following questions so this script can generate the files
needed by Pelican.

> Where do you want to create your new web site? [.]
> What will be the title of this web site? Super blog
> Who will be the author of this web site? username
> What will be the default language of this web site? [en]
> Do you want to specify a URL prefix? e.g., <http://example.com> (Y/n) n
> Do you want to enable article pagination? (Y/n)
> How many articles per page do you want? [10]
> What is your time zone? [Europe/Paris] US/Central
> Do you want to generate a Fabfile/Makefile to automate generation and publishing? (Y/n) y
> Do you want an auto-reload & simpleHTTP script to assist with theme and site development? (Y/n) y
> Do you want to upload your website using FTP? (y/N) n
> Do you want to upload your website using SSH? (y/N) n
> Do you want to upload your website using Dropbox? (y/N) n
> Do you want to upload your website using S3? (y/N) n
> Do you want to upload your website using Rackspace Cloud Files? (y/N) n
> Do you want to upload your website using GitHub Pages? (y/N) y
> Is this your personal page (username.github.io)? (y/N) y
Done. Your new project is available at /Users/username/blog
```

你可以对每个问题都采用默认值，但除了以下这些问题：

  * 网站标题，应该唯一且特殊
  * 网站作者，可以是个人用户名或你的全名
  * 时区，可能你不在巴黎
  * 上传到 GitHub 页面，我们选择 "y"


回答完所有问题后，Pelican 会在当前目录中保留以下内容：

```
$ ls
Makefile      content/     develop_server.sh*
fabfile.py    output/      pelicanconf.py
publishconf.py
```

你可以查看 [Pelican 文档][13]来了解如何使用这些文件，但 _现在_ 我们要做的是完成手头的工作。说实话，我也没有阅读文档。

### 继续

将所有 Pelican 生成的文件添加到本地 Git 仓库的 **content** 分支，提交更改，然后将本地更改推送到 Github 上托管的远程仓库：

```
$ git add .
$ git commit -m 'initial pelican commit to content'
$ git push origin content
```

这件事情并不是特别令人兴奋，但是如果我们需要将内容恢复到这些文件的时候，这将非常方便。

### 终于

终于，现在你得到一个博客了！你所有的博客文章、照片、图像、PDF 等都将位于 **content** 目录中，它最初是空的。要开始创建第一篇博客和关于页面，输入：

```
$ cd content
$ mkdir pages images
$ cp /Users/username/SecretStash/HotPhotoOfMe.jpg images
$ touch first-post.md
$ touch pages/about.md
```

接下来，在你喜欢的文本编辑器中打开 **first-post.md**，并添加以下内容：

```
title: First Post on My Sweet New Blog
date: <today's date>
author: Your Name Here

# I am On My Way To Internet Fame and Fortune!

This is my first post on my new blog. While not super informative it
should convey my sense of excitement and eagerness to engage with you,
the reader!
```

前三行是 Pelican 用于组织内容的元数据。有很多不同的元数据可供你选择。再说一次，文档是你了解更多选项的最佳选择。

现在，打开空白文件 **pages/about.md** 并添加以下文本：

```
title: About
date: <today's date>

![So Schmexy][my_sweet_photo]

Hi, I am <username> and I wrote this epic collection of Interweb
wisdom. In days of yore, much of this would have been deemed sorcery
and I would probably have been burned at the stake.

😆

[my_sweet_photo]: {filename}/images/HotPhotoOfMe.jpg
```

现在，content 目录中将包含三个新的 Web 内容，在 content 分支中还有很多内容。

### 发布

不要急，马上要见到成果了！

剩下要做的就是：

  * 运行 Pelican 以在 **output** 中生成静态 HTML 文件：
  ```
    `$ pelican content -o output -s publishconf.py`
  ```

  * 使用 **ghp-import** 将 **output** 目录的内容添加到 **master** 分支中：
  ```
  $ ghp-import -m "Generate Pelican site" --no-jekyll -b master output`
  ```

  * 将本地 master 分支推送到远程仓库：
  ```
  $ git push origin master
  ```

  * 提交新内容并将其推送到 **content** 分支
  ```
  $ git add content
  $ git commit -m 'added a first post, a photo and an about page'
  $ git push origin content
  ```

### OMG，我成功了

现在最激动的时候到了，当你想要看到你发布给大家的博客内容时，打开浏览器输入：

```
https://username.github.io
```

恭喜你可以在 GitHub 上发布自己的博客了！当你想添加更多页面或文章时，都可以按照上面的步骤来。希望你可以愉快地发布博客。

--------------------------------------------------------------------------------

via: https://opensource.com/article/19/5/run-your-blog-github-pages-python

作者：[Erik O'Shaughnessy][a]
选题：[lujun9972][b]
译者：[MjSeven](https://github.com/MjSeven)
校对：[校对者ID](https://github.com/校对者ID)

本文由 [LCTT](https://github.com/LCTT/TranslateProject) 原创编译，[Linux中国](https://linux.cn/) 荣誉推出

[a]: https://opensource.com/users/jnyjny/users/jasperzanjani/users/jasperzanjani/users/jasperzanjani/users/jnyjny/users/jasperzanjani
[b]: https://github.com/lujun9972
[1]: https://opensource.com/sites/default/files/styles/image-full-size/public/lead-images/getting_started_with_python.png?itok=MFEKm3gl (Raspberry Pi and Python)
[2]: https://github.com/
[3]: https://git-scm.com
[4]: https://help.github.com/en/categories/github-pages-basics
[5]: https://jekyllrb.com
[6]: https://python.org
[7]: https://blog.getpelican.com
[8]: https://guides.github.com/features/mastering-markdown
[9]: http://docutils.sourceforge.net/docs/user/rst/quickref.html
[10]: https://github.com/join?source=header-home
[11]: https://git-scm.com/docs
[12]: https://pip.pypa.io/en/stable/
[13]: https://docs.getpelican.com
