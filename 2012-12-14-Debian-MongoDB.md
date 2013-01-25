#Debian安装MongoDB

-tags:Work

----

#安装
首先，添加key，在命令行里输入：
```bash
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv 7F0CEB10
```
然后在`/etc/apt/sources.list.d/`路径下创建一个名为`10gen.list`的文件，内容为：
```bash
deb http://downloads-distro.mongodb.org/repo/debian-sysvinit dist 10gen
```
保存后，在命令行里输入如下命令重新载入存储库。：
```bash
sudo apt-get update
```
接着，安装MongoDB：
```bash
sudo apt-get install mongodb-10gen
```
#运行
启动，停止，重启的命令分别为：
```bash
sudo /etc/init.d/mongodb start
sudo /etc/init.d/mongodb stop
sudo /etc/init.d/mongodb restart
```
--------------------------------
以上内容参考自：http://cn.docs.mongodb.org/manual/