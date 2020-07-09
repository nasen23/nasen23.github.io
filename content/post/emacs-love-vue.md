+++
title = "在emacs配置vue的开发环境"
lastmod = 2019-12-13T00:17:36+08:00
tags = ["emacs", "vue", "前端"]
categories = ["emacs"]
draft = false
author = "Sen Na"
+++

主要针对doom emacs，其他的emacs配置应该也差不多。

首先确保安装好了company-mode和lsp-mode还有web-mode，在init.el中取消相应的注释并执行doom refresh即可。

安装vue-language-server。

{{< highlight sh >}}
npm install -g vue-language-server
{{< /highlight >}}

设置在.vue文件中使用web-mode，然后给web-mode添加一个hook，启动web-mode之后就再去启动lsp-mode就可以了（这一步doom emacs的话好像就不用自己去添加了），然后
lsp-mode应该会根据文件类型自己尝试去启动vue-language-server。

{{< highlight elisp >}}
(def-package! web-mode
  :mode ("\\.vue\\'" "\\.wxml\\'")
  :config
  ;; 这里就是设置一下缩进为2个空格
  (setq web-mode-indent-level 2)
  (setq web-mode-markup-indent-offset 2)
  (setq web-mode-css-indent-offset 2)
  (setq web-mode-code-indent-offset 2))

;; 如果需要手动加hook的话大概就是这样
(def-package! lsp-mode
  :hook ((css-mode js-mode web-mode) . lsp-mode))
{{< /highlight >}}
