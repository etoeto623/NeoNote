```toc 
style: bullet
min_depth: 1 
```
# jdk 9
## 模块化
模块化是在package之上的一个封装，主要有如下几个重要的作用：
- 模块化加强了安全性，如果没有export，即使是public的类，在其他模块也无法使用
- 优化了内存开销，按需引用，不需要引用整个jdk
参考稳定：[csdn-java9新特性：模块化](https://blog.csdn.net/54powerman/article/details/78091989)
## jshell
jshell是java的repl，在repl中可以快速的验证java的功能。
使用方式就是执行命令`jshell`
## try优化
在jdk8中，可以使用try对资源进行自动关闭，但要求资源在try子句中进行初始化，jdk9的try则可以对已经初始化好的资源进行自动关闭，如下：
``` java
InputStream is = new InputStream();
try(is){
	// some code here
}catch(Exception e){
}
```
## InputStream的transferTo方法
`transferTo`方法可以将inputstream快速的转换为outputstream，而不用像jdk8中那样读取然后写入outputstream
``` java
try(InputStream is = new FileInputStream("test.txt")){
	OutputStream os = new FileOutputStream("test_copy.txt");
	is.transferTo(os);
}catch(Exception e){
	e.printStackTrace();
}
```
# java10
## 类型推断
定义变量时，可以使用`var`关键字，对变量进行类型推断
## 集合类增加创建不可变类型的方法
集合类增加了`of`方法，可以创建不可变的集合变量，如下：
``` java
var lst = List.of(1, 3, 5);
lst.add(7);  // this will generate exception
```


#java #jdk9