+++
title = "从原来的vim+markdown到现在的emacs+org-mode"
lastmod = 2019-10-19T00:36:28+08:00
tags = ["emacs", "org"]
categories = ["emacs"]
draft = false
author = "Sen Na"
+++

早在两年前我就对emacs有所耳闻，当时在macbook上下载了一个emacs下来，打开之后，面对着那个scratch buffer惊呆了，这个东西应该怎么用啊，我傻了啊。还记得当时误操作打开了好几个buffer，啊？buffer是什么意思啊。总之，在各种不适应之下，我最终放弃了这款“神之编辑器”，好后悔啊，如果当时就开始用emacs估计现在已经比较熟练了，就不用在大作业这么多的时期还忙着配置编辑器的事了。

在暑假的时候，我开始接触vim，在用了vim之后，我才明白像vim这样双手不离键盘的编辑方式有多么美妙，然后在暑假花了大量时间来配置vim。之前我一直以为像vim/emacs这样的编辑器都是很古老的东西，所以界面应该比较丑，然后自动补全也会比较蠢。不过在使用了
vim之后，我意识到这种想法真是大错特错，vim也可以很漂亮，也可以有像coc.nvim这样可以与vscode媲美的补全引擎。更重要的是，vim的定制度非常高（虽然跟emacs比起来有点弟弟的感觉，不过也十分强大了），一个编辑器可以完全按照自己的想法进行工作，并且面对各种对于其他普通编辑器来说很复杂、重复性工作很多的编辑场景，都能应对自如，这不正是我理想中的编辑器嘛。

我在使用vim的这几个月，还是十分眼馋emacs那妖娆的gui界面以及那个“伪装成操作系统的编辑器”的美誉。之后，国庆期间，我受到别人的诱惑，重新开始尝试emacs。在几番尝试之后，我选择了doom emacs这个配置，内置evil-mode，界面也很美观，作者也在很积极的更新。

一开始还是和初学vim时一样，各种不适应，但是我还是坚持下来了，用它写了几乎全部的计算机网络的大作业（写这篇文章的时候，我还在写这个大作业）（其实lsp-mode的配置也没有那么困难，ccls和lsp-mode配合提供的补全也让人很满意，还有semantic
highlight，可以说写c语言的体验很棒了，不过我到现在还不太会在emacs里面用gdb调试程序，不过也可以了）。

然后在这个过程中，我接触了emacs的org-mode。当然一开始我还不太会用，看了一个
org-mode的教学视频，然后开始用org-mode，尝试了用org-mode管理日程，写一个ppt，画
uml图，以及我现在在做的事情——写博客，文学编程、表格计算这种高阶技巧还没有尝试。现在感觉org-mode确实是emacs的一个大杀器，爱了爱了。感觉我以后大部分文本类型的工作都会在org-mode下进行了（这个模式真是太棒了！！）。

我是看了子龙山人的博客之后，知道了ox-hugo这个东西。可以把写出的org文本转成
markdown然后部署到博客上。于是我把我的博客从hexo换成了hugo，其实是个很简单的工作，网上一大堆教程。然后就可以以类似这样的方式写博客（这个截图用的是org-screenshot，可以直接在emacs里截图然后插入到org文档里，极度方便）
![](/ox-hugo/screenshot-01.png)

这里=hugo\_base\_dir=就是博客的根目录，ox-hugo就是通过这个知道博客的根目录的。这个东西的方便之处就在于，所有的博客文章都可以像这样，写在一个subtree里面，然后我只需要把这个TODO改成DONE，就可以直接利用ox-hugo来生成博客文章了。当然这样生成之后并不会自动进行部署，需要手动进入博客目录里用hugo来部署。这样肯定是有点麻烦啊，于是我写了一点特别简单的elisp（虽然我还完全不会elisp，可能写的有点垃圾。。）。

{{< highlight elisp >}}
(defun deploy-my-blog ()
  "Deploy my already generated markdown files to blog site."
  (interactive)
  (async-shell-command "~/Desktop/blog/deploy.sh"))
{{< /highlight >}}

这样可以直接~M-x deploy-my-blog~来把写好的文章部署到github上。这里的~deploy.sh~
就是一个很简单的git命令的脚本。

到现在为止，我应该正式使用emacs十几天了。虽然只有短短的十几天，但是我已经感受到了emacs的美妙之处。我还会好好学一下elisp，继续学习emacs。等过段时间，我再回来看看。
