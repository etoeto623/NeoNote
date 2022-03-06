> 这里是一些elisp的code snippet
- **定义command**
``` lisp
(defun test (name age)
  (interactive "sinput:\nnage:")
  (message "%s is %d" name age))
```
备注：`interactive`表示这个function是一个交互式的command，参数中`\n`可以用来表示多个参数之间的分割。可以用`describe-function interactive`来看详细的用法
- **设置快捷键**
``` lisp
(global-set-key (kbd "C-M-m c") 'test)
```
- **加载其他文件**
可以使用`load`或`require`来加载其他文件
``` lisp
(load "~/.emacs.d/mine/diy.el")
```
