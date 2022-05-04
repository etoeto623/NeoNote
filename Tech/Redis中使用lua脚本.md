>听说过redis中可以使用lua脚本来实现分布式锁，这里就来了解下其中的原理
# Lua脚本
Lua是一门比较老的脚本语言了，语法较为简单，听说在魔兽世界中都可以使用lua脚本
Lua的官网是：[https://www.lua.org/](https://www.lua.org/)，lua的数据类型也比较简单，有如下几种：
- boolean
- number
- string
- table（类似于array和hashmap）
# Lua in redis
redis中内置了lua的解释器，传入lua脚本，redis会进行解释执行，使用的方式有如下几种：
## eval
redis有个eval的命令，可以执行脚本，格式如下
``` shell
eval script numkeys key [key ...] arg [arg ...]
```
比如
``` shell
eval 'redis.call("set", "neo", KEYS[1])' 1 long
```
上面的命令执行的是`set neo long`
EVAL的官方介绍：[https://redis.io/commands/eval/](https://redis.io/commands/eval/)
## redis-cli
使用redis-cli直接加载一个脚本文件执行，格式如下：
``` shell
redis-cli --eval /path/to/script key1 key2...keyM , arg1 arg2...argN
```
比如
``` lua
redis.call('set', KEYS[1], ARGV[1])
return redis.call('get', KEYS[1])
```
``` shell
redis-cli --eval test.lua neo , long
```
# Lua在redis中的原子性
Lua在redis中的原子性可以参考官方的说明
>Redis uses the same Lua interpreter to run all the commands. Also Redis guarantees that a script is executed in an atomic way: no other script or Redis command will be executed while a script is being executed. This semantic is similar to the one of [MULTI](https://link.juejin.cn?target=https%3A%2F%2Fredis.io%2Fcommands%2Fmulti "https://redis.io/commands/multi") / [EXEC](https://link.juejin.cn?target=https%3A%2F%2Fredis.io%2Fcommands%2Fexec "https://redis.io/commands/exec"). From the point of view of all the other clients the effects of a script are either still not visible or already completed.

即在redis中，命令是顺序执行的，在一个lua的script执行过程中，是不允许其他脚本或redis command执行的，所以能保证脚本执行过程中，数据不会被其他命令更改，如下：
![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/796a5523d1334bb9bbcd3ebc0572635c~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)


# 参考
- [官网-Running Lua Scripts](https://redis.io/docs/manual/cli/#running-lua-scripts)
- [简书-Redis万字长文Lua详解](https://juejin.cn/post/6904992773171216392)
- [cnblog-在redis中使用lua脚本让你的灵活性提高5个逼格](https://www.cnblogs.com/huangxincheng/p/6230129.html)
