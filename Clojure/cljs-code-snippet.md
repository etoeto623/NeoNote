>收集一些cljs中的有用代码片段
- 操作js中的对象
``` clojure
;; 访问location的href属性值
(.-href js/location)
;; 设置location的href属性值
(set! (.-href js/location) "new url")
```
参考博客：[cnblog](https://www.cnblogs.com/fsjohnhuang/p/7040661.html)
