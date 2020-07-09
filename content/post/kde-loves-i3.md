+++
title = "KDE ❤ i3"
lastmod = 2020-03-03T22:46:37+08:00
tags = ["kde", "i3wm"]
categories = ["wmde"]
draft = false
author = "Sen Na"
+++

我来整活啦。

今天参照网络上的一些教程，尝试了在 kde 中，把 i3 作为其 wm，结果发现效果居然还挺不错。

我一直很喜欢 tiling window manager，可以充分利用屏幕的空间，由于其相对简单的
窗口排列逻辑，仅仅利用键盘就可以实现在窗口与 workspace 之间的快速切换。Who needs
a mouse anymore?

尽管 tiling window manager 有不少优点，但是它在有些情况下确实有些麻烦了。比如在
i3wm 下，想要调节屏幕亮度的话，只能利用 xbacklight 或者一些其他的命令行工具 ，当
然你也可以给它一个快捷键，只不过对于我这个懒人来说，实在是有些麻烦了，况且还有屏
幕显示，音量，电池管理等种种，这些当然理论上是可以设置的，不过我实在是懒得搞
了（当然不排除还是有许多人比较喜欢 i3 的可定制化度的，只是 i3 要定制起来是比较麻
烦的）。所以可不可以保留比如 kde 的调节亮度，设置 wifi 连接 的 app tray，然后在窗口管
理上还是 i3 那一套呢？

在这之前，我已经用了很久的某个 KWin 的关于 tiling window management 的插件，这个
插件做的还是相当不错的，但是可惜体验还是不够完美，实在是没有 i3 内味。

今天我偶然搜索到了关于将 kde 和 i3 结合起来的博客内容，于是我跟着做了一下。

先创建一个新的 desktop session，在 /usr/share/xsession 中新建一个
plasma-i3.desktop，内容如下。

{{< highlight desktop >}}
[Desktop Entry]
Type=XSession
Exec=env KDEWM=/usr/bin/i3 /usr/bin/startplasma-x11
DesktopNames=KDE
Name=Plasma with i3
Comment=Plasma with i3
{{< /highlight >}}

然后打开 i3 的 config，针对 kde 做一些改动，把下面的内容加到 i3 的配置里。

{{< highlight config >}}
for_window [class="yakuake"] floating enable
for_window [class="systemsettings"] floating enable
for_window [class="plasmashell"] floating enable;
for_window [class="Plasma"] floating enable; border none
for_window [title="plasma-desktop"] floating enable; border none
for_window [title="win7"] floating enable; border none
for_window [class="krunner"] floating enable; border none
for_window [class="Kmix"] floating enable; border none
for_window [class="Klipper"] floating enable; border none
for_window [class="Plasmoidviewer"] floating enable; border none
for_window [class="(?i)*nextcloud*"] floating disable
for_window [class="plasmashell" window_type="notification"] floating enable, border none, move position 30px 40px, no_focus
{{< /highlight >}}

为了防止一些本该浮动的窗口占满屏幕。这里我把 notification 的位置设置了一下，因为如果不
设置的话 notification 会默认出现在屏幕中间，有点烦的。其实也可以干脆不用
kde 的通知，直接用 dunst。

{{< highlight config >}}
for_window [title="Desktop — Plasma"] kill; floating enable; border none
{{< /highlight >}}

如果是英文版本的 kde 的话，上面这个是没有问题的。但是如果是其他语言的版本，可能会
有所不同，可以用 wmctrl -l 来查看。

为了方便以后登出，可以覆盖掉这个键位（或者不改应该问题也不大，我觉得）。

{{< highlight config >}}
bindsym $mod+Shift+e exec --no-startup-id qdbus org.kde.ksmserver /KSMServer org.kde.KSMServerInterface.logout -1 -1 -1
{{< /highlight >}}

这样差不多就可以了。登出之后，可以看到桌面的选项中多了一个 Plasma with i3，选
择这个 session，登录进来即可，然后就可以愉快地使用啦。

还有一些问题，kde 的菜单栏并不会默认显示 i3 的 workspace，可以添加一个叫 Pager
的 widget，这样就可以正常显示 i3 的 workspace 了。由于没有了 KDesktop，你可以使用
一些别的 window compositor ，比如 compton。

其实好像还挺简单的。

{{< figure src="/ox-hugo/screenshot.png" >}}
