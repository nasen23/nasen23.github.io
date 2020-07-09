+++
title = "Tsinghua sslvpn"
lastmod = 2020-03-26T13:02:19+08:00
tags = ["network", "linux"]
draft = false
author = "Sen Na"
+++

一篇水文。

考虑到清华官方提供的 sslvpn 客户端 pulse secure，界面并不好看也不好用，提供一些
其他的连接方式。


## 手动连接 {#手动连接}

{{< highlight bash >}}
sudo openconnect --protocol nc sslvpn.tsinghua.edu.cn --servercert sha256:398c6bccf414f7d71b6dc8d59b8e3b16f6d410f305aed7e30ce911c3a4064b31
{{< /highlight >}}


## network manager {#network-manager}

把这个 vpn 添加到 network manager 里面。

{{< highlight bash >}}
nmcli con add type vpn con-name "Tsinghua SSL VPN" ifname "*" vpn-type openconnect -- vpn.data "gateway=sslvpn.tsinghua.edu.cn,protocol=nc"
{{< /highlight >}}

这样就可以直接在 network manager 里面进行连接了。

{{< figure src="/ox-hugo/20200326_130059_y6WpLf.png" caption="Figure 1: like this..." >}}
