#Nginx用法总结

-tags: Nginx,

----

搞了好几天博客，终于稳定下来了，在此写一下Nginx各种命令和用法
首先是Nginx的启动，重启，关闭命令
#停止操作
停止操作是通过向nginx进程发送信号来进行的
首先，在SSH里输入：
```bash
ps -ef | grep nginx
```
在进程列表里 面找到nginx的master进程，它的编号就是nginx的主进程号
从容停止Nginx：
`kill -QUIT`+主进程号
快速停止Nginx：
`kill -TERM`+主进程号
强制停止Nginx：
`pkill -9 nginx`
平滑重启
如果更改了配置就要重启Nginx，要先关闭Nginx再打开？不是的，可以向Nginx 发送信号，平滑重启。
平滑重启命令：
`kill -HUP`+住进称号或进程号文件路径

上面就是Nginx的一些基本的操作，希望以后Nginx能有更好的方法来处理这些操作， 最好是Nginx的命令而不是向Nginx进程发送系统信号。

***
以上内容部分参考http://www.cnblogs.com/derekchen/archive/2011/02/17/1957209.html