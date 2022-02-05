>shadow-cljs是一个ClojureScript的编译工具，能将cljs编译为js，使用它能用clojure开发前端应用，官网是：[github](https://github.com/thheller/shadow-cljs)
# 1、依赖项
shadow-cljs依赖项如下：
- nodejs(v6.0.0+)
- npm or yarn
- jdk(v8.0+，hotspot)
# 2、创建应用
使用npx命令可以创建应用，如下
``` bash
npx create-cljs-project neoapp
```
其中，`neoapp`为项目的名称
执行好以后，生成的文件如下：
``` text
.
├── node_modules/
├── package-lock.json
├── package.json
├── shadow-cljs.edn
└── src
    ├── main
    └── test
```
其中，`shadow-cljs.edn`是shadow-cljs的配置文件，`package.json`是npm的配置文件
# 3、一些操作
## 3、1、REPL
此时可以进行启动一个REPL了，命令如下：
``` bash
npx shadow-cljs browser-repl
或者
npx shadow-cljs node-repl
```
使用`browser-repl`会打开一个浏览器页面，repl中的输出信息会显示在网页上面
# 4、业务代码
业务代码放在`src/main`文件夹下，namespace参考java的规范，比如如果namespace是`neo.freontend.app`，则文件路径是`src/main/neo/frontend/app.cljs`
app.cljs文件内容如下：
``` clojure
(ns acme.frontend.app)

(defn init[]
	(println "hello world"))
```
同时修改shadow-cljs.edn配置文件，在`:builds`中增加如下配置：
``` clojure
{
:builds
 {:frontend
   {:target :browser
    :modules {:main {:init-fn acme.frontend.app/init}}}}}
```
其中，`:frontend`是编译id，即一个配置可以配置多个编译id
然后对项目进行watch，命令如下：
``` bash
npx shadow-cljs watch frontend
```
其中，`frontend`就是之前配置的编译id

编译完成后，在根目录下会创建`public`文件夹，里面有编译的js文件`public/js/main.js`，此时需要手动在public文件夹下创建一个`index.html`文件，如下：
``` html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>neo frontend</title>
  </head>
  <body>
    <div id="root"></div>
    <script src="/js/main.js"></script>
  </body>
</html>
```

然后还需要在`shadow-cljs.edn`中增加http server的配置，如下：
``` clojure
{
:dev-http {8080 "public"}}
```
此时，访问`http://localhost:8080`可以看到生成的页面
# 5、参考
- [shadow-cljs+reagent脚手架](https://github.com/jacekschae/shadow-reagent)

#shadow-cljs    #clojure