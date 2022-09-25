>收集一些cljs中的有用代码片段
- 操作js中的对象
``` clojure
;; 访问location的href属性值
(.-href js/location)
;; 设置location的href属性值
(set! (.-href js/location) "new url")
```
参考博客：[cnblog](https://www.cnblogs.com/fsjohnhuang/p/7040661.html)
- clojurescript中嵌套map的值修改: `assoc-in`
``` clojure
(def nested-map {:a 1 :b {:c 2 :d 3}})
;; if i want to modify :c to 6, i can do this
(assoc-in nested-map [:b :c] 6)
```
参考文章: [stackoverflow](https://stackoverflow.com/questions/11577601/clojure-nested-map-change-value)  [clojuredoc](https://clojuredocs.org/clojure.core/assoc-in)
