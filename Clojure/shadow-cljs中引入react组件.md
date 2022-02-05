shadow-cljs通过[reagent](https://github.com/reagent-project/reagent)可以使用react的组件
# 引入reagent
首先在`shadow-cljs.edn`中引入reagent，如下：
``` clojure
{:dependencies [reagent "0.8.1"]]}
```
然后在业务代码中require，如下：
```clojure
(ns app.views  
  (:require [reagent.core :as r]))
```
# 引入react的组件
这里以`@mui/material`组件为例
首先需要在package.json中引入react的组件，如下：
```json
{
	"dependencies": {  
	  "@mui/material": "^5.4.0",  
	  "@emotion/styled": "^11.6.0",  
	  "@emotion/react": "^11.7.1"  
	}
}
```
然后需要install这些依赖，如下：
```bash
yarn install
或者
npm install
```

然后在业务代码中使用
首先require，如下：
```clojure
(ns app.views  
  (:require ["@mui/material" :refer [Button]]))
```
然后定义使用的方法：
```clojure
(def btn (r/adapt-react-class Button))
```
然后使用：
```json
[btn {:color "error" :variant "contained"} "Click Me"]
```

#react    #shadow-cljs 