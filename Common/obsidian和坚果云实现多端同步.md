# 准备工作
这里主要介绍PC端+Android端的同步方式
要实现PC+Android的同步，需要用到的工具如下：
- PC端：obsidian、坚果云客户端
- Android端：obsidian、FolderSync
# 同步原理
实现同步主要依靠如下几点：
- 坚果云支持webdav协议
- FolderSync支持通过webdav协议来同步文件
# 同步设置
具体设置步骤如下
- 在坚果云网页端的[安全选项页面](https://www.jianguoyun.com/#/safety)设置第三方应用管理，开启webdav，同时需要设置一个设备密码
- 在FolderSync这个app中，添加账号，设置好和坚果云的webdav连接，然后添加同步文件夹对，将坚果云的文件夹和一个本地文件夹关联，然后选择同步到本地文件夹即可
``` ad-note
要确保FolderSync有收集的存储设备写入权限，否则无法同步成功
```

参考文章：[Obsidian移动端同步（Android +Win ）](https://www.bilibili.com/read/cv13339751)

#obsidian    #设备同步