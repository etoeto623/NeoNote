# pods
babashka提供了一些pods，相当于第三方扩展，而sqlite就是一个pods，配置方式如下：
``` clojure
{:paths ["wid"]
 :pods {org.babashka/go-sqlite3 {:version "0.1.0"}}}

```
在配置babashka的配置文件`bb.edn`中配置
# 使用
``` clojure
(require '[babashka.pods :as pods])
(pods/load-pod 'org.babashka/go-sqlite3 "0.1.0")
(require '[pod.babashka.go-sqlite3 :as sqlite])

(sqlite/execute! "/Users/longhai/babashka.db"
                 ["create table if not exists task (name text, description text)"])

(sqlite/execute! "/Users/longhai/babashka.db"
                 ["insert into task(name, description) values (?,?)" "neolong" "awesome"])

(sqlite/query "/Users/longhai/babashka.db"
              ["select * from task"])
```


# 参考文档
- [babashka-go-sqlite3](https://github.com/babashka/pod-babashka-go-sqlite3)
- [honey-sql](https://github.com/seancorfield/honeysql)

#babashka #sqlite