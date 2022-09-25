### 彩色输出
终端中的色彩其实是通过一些特殊的字符来控制的，比如如下代码可以输出红色字：
``` go
fmt.Printf("\033[1;31;40m%s\033[0m\n","Red.")
```
参考：[go终端 彩色输出](https://zhuanlan.zhihu.com/p/76751728)
### 文字退格的实现
在终端中先显示一段文字，然后想覆盖掉这段文字，重新输出新的文字，类似退格键的操作，可以打印特殊字符`\b`来实现，如下：
``` go
fmt.Print("text")
fmt.Print(strings.Repeat("\b", 4))
fmt.Print("new text")
```

#go #cli