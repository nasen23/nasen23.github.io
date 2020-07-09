---
title: mac上的窗口管理工具yabai以及配置自己的neovim
date: 2019-08-18
categories:
  - macos
  - neovim
tags:
  - vim
  - window manager
---

先上效果图
![preview](https://tva4.sinaimg.cn/large/007DFXDhgy1g63t611lppj31c00u0hdy.jpg)

# 安装并使用 yabai

yabai 实际上是一段注入 Dock.app 的程序，所以为了使用它，我们必须关闭 macOS 的 System Integrity Protection，至于具体怎么关可以百度一下。当然，这样做也是存在一定的风险的，如果不喜欢这样做的话，可以考虑使用 chunkwm（这个和 yabai 都是一个人做的）。不过 yabai 确实有一些很棒的 feature，你甚至可以自己写一个状态栏（不过我还是懒得搞了）。

这是 yabai 作者给出的示例图片，自己权衡一下吧。
![yabai example](https://user-images.githubusercontent.com/6175959/59977681-9d873500-95d4-11e9-91e7-7be05cb11d06.png)

安装需要 xcode-10 command line tools。

**使用 homebrew 安装**

```zsh
# clone tap
brew tap koekeishiya/formulae

# install latest stable version
brew install yabai

# install from git master branch
brew install --HEAD yabai
```

**直接把 git 仓库复制下来安装**

```zsh
# clone repo and build binary
git clone https://github.com/koekeishiya/yabai
make install      # release version
make              # debug version

# symlink binary to a location in $PATH
ln -s $PWD/bin/yabai /usr/local/bin/yabai

# symlink manpage
ln -s $PWD/doc/yabai.1 /usr/local/share/man/man1/yabai.1
```

完成上面这些后，应该可以在终端里运行 yabai 了。但是还没有将脚本注入到 dock.app 中。

**安装注入脚本**

```zsh
sudo yabai --install-sa
```

到这里就完成 yabai 的安装了，我们直接利用 homebrew 启动 yabai。

```zsh
brew services start yabai
```

然后清空桌面，启动终端。yabai 现在应该开始生效了。

好像还少了点什么？在 linux 上用 i3wm 可以直接 super+cr 打开终端，为了在 mac 上达到类似的体验，我们还需要一个快捷键工具 skhd（作者同 yabai）。

具体安装和配置见 https://github.com/koekeishiya/skhd

如果你懒得配置各种快捷键，可以直接将 yabai 里的 example 中的 skhdrc 和 yabairc 作为你自己的配置文件。

# neovim 配置推荐

把 neovim 当做 rust 的 ide。
![rust ide](https://tva3.sinaimg.cn/large/007DFXDhgy1g63urp7sv9j31c00u0qv9.jpg)

我个人还是比较懒得折腾太多，直接用了一个现成的配置 ThinkVim。
可以见 https://github.com/hardcoreplayers/ThinkVim

