---
title: 一些实用的vim技巧（尽量持续更新）
date: 2019-09-04
tags:
  - vim
  - useful tips
categories:
  - vim
---

## text object 的概念

text object 差不多是用 vim 来进行日常编辑时最为实用的一个部分了，这也是 vim 相比其他编辑器的强大之处。
对 text object 进行的操作大致可以总结成

```
d/y/c/v + i/a + [text object name]
```

text object 包括`` w/W/s/p/(/</{/[/`/"/t ``。其中 W 代表被空格所包围的整个 WORD，s 表示句子，p 代表有空行隔开的段落，t 在 html 代码中可以代表一个标签。

![text object](https://i.ibb.co/ZXbLgTV/Kapture-2019-09-04-at-13-15-52.gif)

## 熟悉一下`s`和`g`命令

要想了解更多，还是建议去<http://www.vimregex.com/>看看。

关于其基本用法，就不详细说了。

如果要替换一个单词最省心的办法就是`\<WORD\>`。

我觉得分组是一定要说一下的。可以利用`\(`和`\)`来进行分组的操作，具体和其他语言的正则表达式是一样的。但是在`:s`命令中可以使用`\1`，`\2`，`\3`等在替换字符串中指向对应的组。

例如，`:s/\(\w\+\)\(\s\+\)\(\w\+\)/\3\2\1/`可以用来交换两个单词的位置。在这里，`\1`表示第一个单词，`\2`表示中间的空白，`\3`表示第二个单词。

![subsitute with group](https://i.ibb.co/CVWV6kG/Kapture-2019-09-11-at-23-03-59.gif)

一些比较特殊的 anchor：`\zs,\ze`分别代表替换的开始和结束，`\{-}`表示非贪婪匹配（即尽量少的匹配），例如`:%s/ .\{-}\zs \ze//`可以用来清除每行第二个空格。

global 命令用来对每行进行一些操作。其最大的优点便是可以在后面接`normal`命令。比如下面的这个`:%g/".*"/norm ci"Vim Loves Me`。

![global command](https://i.ibb.co/t2KVzdq/Kapture-2019-09-12-at-0-09-41.gif)
![working with vim-sandwich](https://tva3.sinaimg.cn/large/005R6Otmgy1g6zi50n7rog30gj09balv.gif)

## 寄存器的使用

vim 中进行复制/剪切操作时，可以指定在哪个寄存器中保存文字片段。

使用方法也比较简单，`"{register}d/y`，可以指定拷贝到`{register}`中。

例如：`"ayy`可以将该行复制到`a`寄存器中，之后`"ap`即可将该行粘贴出来。

这样就可以同时复制多段不同的文本了。

如果要查看每个寄存器中的内容，`:reg`即可。

## 有用的`.`操作和`@:`

`.`操作可以重复上一个 normal/insert 模式的命令，在进行重复的操作时相当有用。

举个例子，比如想要删除下面几行，这种情况虽然只要输入`{n}dd`就好了，但是要数出要删多少行，还是需要一定的时间的。所以我们可以`dd`然后不停按`.`这样就可以不用数多少行直接来快速删除。

`@:`可以用来重复上一个 command，输入的命令比较长的话还是蛮有用的。当然如果是经常用的命令的话，完全值得为它写一个`map`。

## 读取 bash 命令的输出

我们知道`:!`后面接一个终端命令可以让 vim 运行一个终端命令。

那么`:r!`则可以读取终端命令的输出，并将其插入至目前所处的 buffer 中。

例如`:r !ls`可以在当前 buffer 中插入当前文件夹内所有的文件名。

![read command line output](https://i.ibb.co/qkTFhLN/Kapture-2019-09-12-at-0-15-37.gif)

## vim 内置的 `sort`

Vim 自带的 sort 是非常强大的文本排序工具，甚至可以用来处理一些比较复杂的排序。

详情可以见<http://vim.wikia.com/wiki/Sort_lines/>和`:help sort`。下面简单说一下 sort 的用法吧。

`:[range]sor[t][!] [b][f][i][n][o][r][u][x] [/{pattern}/]`

一些比较重要的选项：

- `!`：反向排序
- `n`: 会给碰到的第一个数字排序
- `i`: 大小写不敏感
- `f`: 会给碰到的第一个浮点数排序
- `-k 3`: sort on third occurence

特殊：

- `%sort u`: 去除所有多余的行
- `%!sort -M`: 依照月份的名字排序(January<Februay<...<December)
- `%sort /{pattern}/ r`: 给能匹配上这个正则表达式的 text 排序

## 编辑已经录制好的宏

有些时候明明录制好了一个很长的宏，可惜少按了一两个键，这种情况下肯定是不愿意重新录制这段宏的。

#### 复制进寄存器

- `"qp`可以将寄存器 q 中的内容粘贴到当前光标处。
- 直接编辑这段宏就好了。
- 完成之后`<Esc>`回到 Normal mode。
- `"qyy`把这一行的内容复制到你的寄存器中。
- `dd`删除该行。

#### 直接修改

- `:let @q='`打开 q 寄存器。
- `<C-r><C-r>q`可以把 q 寄存器的内容粘贴到当前的 buffer。
- 修改。
- `<CR>`结束修改。

## `Ex`，`Sex`和`Vex`

可以通过`:Ex`在当前的窗口打开文件浏览器，通过 hjkl 选择文件，`<CR>`来确认。

`:Sex`可以水平分屏打开，`:Vex`可以竖直分屏打开。

## `<C-i>`，`<C-o>`和 jumplist

Vim 可以记住你访问过的地点，这些地点会被记录在一个`jumplist`里。如果要查看这个 jumplist，可以通过`:jumps`来查看。

被当作 jump 的操作有搜索、替换和标记，只有 hjkl 的话是不会被记录到 jumplist 里的。

`<C-o>`可以跳回到前一个地点，`<C-i>`可以跳到下一个地点。
