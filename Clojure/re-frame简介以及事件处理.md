> re-frame是一个clojure-script的前端框架，可以整合react
> 官网地址是：[re-frame](http://day8.github.io/re-frame/) 
# re-frame简介
re-frame中的几个重要概念是：
- event dispatch
- event handling
- effect handling
- query
- view
- dom
## event dispatch
事件是一个重要概念，用户点击是事件、网络请求是事件...
正是事件驱动了re-frame中的数据流动
## event handling
对事件做出响应，一个事件可以有多个handler
handler会产生`side effects`
event handling只是一个处理数据的function
## effect handling




## re-frame工作流程图解
![](https://day8.github.io/re-frame/images/event-dispatch.png)
![](https://day8.github.io/re-frame/images/handling-one-event.png)

#re-frame
