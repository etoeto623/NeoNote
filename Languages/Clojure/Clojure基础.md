# 数据结构
| 数据结构类型 | 示例               | 操作  | 说明                                          |
|:------------ | ------------------ | ----- | --------------------------------------------- |
| set          | `#{:a :b :c}`      |       | 可以通过`hash-set`、`sorted-set`、`set`等构建 |
| vector       | `[1 2 3]`          |       | `vector`、`vec`                               |
| map          | `{:b "str", :a 1}` | `get` ,`(:a imap)`| `hash-map`、`sorted-map`                      |
| list         | `(1 2 3)`          |       | `list`                                        |
| symbol       | `` `abc ``         |       | ``` ` ```                                     |
| function     | `#(* 2 %1)`        |       | `(#(2 %1) 3)`=> 6                                              |
# 关键字
| 关键字 | 说明         | 示例                               |
| ------ | ------------ | ---------------------------------- |
| def    | 定义变量     | `(def a 1)`                        |
| defn   | 定义函数     | `(defn hello [] (println "hello")` |
| fn     | 定义匿名函数 | `(fn [x] (* x x))`                 | 

这里贴一个cljs的语法页面：[clojure script syntax](http://cljs.github.io/api/syntax/)

#clojure 