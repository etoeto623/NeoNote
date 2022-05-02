>emacs中要配置clojure的开发环境，主要需要用到**cider**，cider可以连接LSP(language server protocol)，给emacs提供clojure的运行和代码提示的支持。这里记录下配置的过程

# 需要用到的部件
- cider
- lsp-mode
- clojure-lsp
- company-mode
- use-package
# 步骤
## 安装use-package
``` lisp
(unless (package-installed-p 'use-package)
  (package-refresh-contents)
  (package-install 'use-package))
(eval-when-compile
  (require 'use-package))
```
## 安装相关包
``` elisp
(use-package rainbow-delimiters
  :ensure t)
;(use-package clj-refactor
;  :ensure t)
;(use-package company
;  :ensure t
;  :defer t
;  :config (global-company-mode))
```
## 安装相关mode
``` lisp
(setq package-selected-packages '(clojure-mode lsp-mode cider lsp-treemacs flycheck company))

(when (cl-find-if-not #'package-installed-p package-selected-packages)
  (package-refresh-contents)
  (mapc #'package-install package-selected-packages))

(add-hook 'clojure-mode-hook 'lsp)
(add-hook 'clojurescript-mode-hook 'lsp)
(add-hook 'clojurec-mode-hook 'lsp)
(add-hook 'clojure-mode-hook 'company-mode)

(setq gc-cons-threshold (* 100 1024 1024)
      read-process-output-max (* 1024 1024)
      treemacs-space-between-root-nodes nil
      company-minimum-prefix-length 2
      lsp-lens-enable t
      lsp-signature-auto-activate nil
      ; lsp-enable-indentation nil ; uncomment to use cider indentation instead of lsp
      ; lsp-enable-completion-at-point nil ; uncomment to use cider completion instead of lsp
      )

```
## 安装clojure-lsp
### 通过lsp-mode安装
首先，通过`M-x`来运行`lsp-install-server`命令，然后输入`clojure-lsp`进行安装
### 手动安装
``` shell
sudo bash < <(curl -s https://raw.githubusercontent.com/clojure-lsp/clojure-lsp/master/install)
```
# 使用
开启`clojure-mode`，然后`cider-jack-in`
## 连接babashka
首先启动babashka的repl server
``` shell
bb --nrepl-server
```
然后cider连接上
``` shell
cider-connect-clj
```

# 参考文章
- [一名 Clojure 粉的 Emacs 配置](https://toutiao.io/posts/7wagle/preview)
- [Configuring emacs as a clojure ide](https://emacs-lsp.github.io/lsp-mode/tutorials/clojure-guide/)
- [clojure-lsp-installation](https://clojure-lsp.io/installation/)
- [cider connect babashka](https://docs.cider.mx/cider/platforms/babashka.html)


#cider    #emacs    #ide   #lsp   #babashka

