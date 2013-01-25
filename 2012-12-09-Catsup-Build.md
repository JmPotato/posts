#Catsup部署教程

-tags:Work, Catsup

----

#关于Catsup
[whtsky](http://whouz.com/ "whtsky的博客")大牛写的一个轻量级博客，Python+Tornado。博客依靠[GitHub](https://github.com/PotatoBrother/_posts "我的博文")和[Markdown](http://baike.baidu.com/view/2311114.htm "Markdown简介")写作。
几个catsup博客：
http://messense.me/
http://linyis.tk/
http://whouz.com/
http://potatobox.org
GitHub项目地址：https://github.com/whtsky/catsup#readme
#准备工作
首先，要在[GitHub](https://github.com/ "创建项目")创建一个名为`_post`的项目，登录后点击右边栏`New Repository`创建新项目。
在进行下一步之前，你的服务器需要装有Nginx和Python，没有的请自行安装。
#配置Catsup
打开服务器的SSH，输入如下指令把catsup从GitHub克隆下来
```bash
git clone git://github.com/whtsky/catsup.git
```
完毕后依次输入如下指令
```bash
cd catsup
pip install -r requirements.txt
cp config-sample.py config.py
```
好了，想在打开服务器里的catsup文件夹，编辑`config.py`文件
```python
#coding=utf-8
site_title = 'catsup'                #博客名称，自己起一个
site_description = 'a blog'     #博客简介，自己写一个
site_url = 'https://github.com/whtsky/catsup'   #博客地址
disqus_shortname = 'catsup'  #不要改

#static_url = 'static'   #不要管
#下面的是主题，默认是sealscript，主题文件在themes，按个人喜好更改，记得去掉开头的井号
#theme_name = 'sealscript'
#google_analytics = ''    #谷歌统计

#gzip = True   #不管

twitter = 'whouz'   #你的Twitter名
github = 'whtsky'   #你的GitHub名

#feed = 'feed.xml'   #RSS，去掉开头的井号开启
#post_per_page = 3   #每页显示博文数量，默认3，去掉开头的井号后再更改

links = (
    ('whtsky', 'http://whouz.com', 'I write catsup'),
    ('catsup', 'https://github.com/whtsky/catsup', 'the source of this blog'),
)    #友链，自行添加
```
更改完毕以后，保存。
然后删除catsup文件夹里的`_post`文件，把你自己的克隆下来：
```bash
git clone git://github.com/你的用户名/_posts.git
```
然后输入如下命令，将Catsup部署为静态
```bash
python catsup.py deploy
```
catsup的配置大体完毕，接下来是Webhook的部署
#配置Webhook
在SSH里输入
```bash
nohup python catsup.py webhook --port=5555 &
```
然后打开你的`_post`的GitHub项目地址，点击右边的`Admin`，打开后在左侧栏里找到`Service Hooks`，点击`WebHook URLs`，填写Webhook地址
```
http://你的主机IP:5555/webhook
```
Webook配置完毕，这样一来，我们向GitHub提交了新博文时，Catsup就会自动更新
#Nginx配置
为了让你的域名能正常访问catsup，添加Nginx配置文件`catsup.conf`
```nginx
server { 
listen 80;
server_name 你的域名，不加http://; 
index index.html index.htm index.php; 
root /你的主机用户名/catsup/deploy/; 
access_log off;
}
```
注意设置catsup和deploy文件夹的权限，让Nginx可以正常访问。
重启Nginx：
```bash
ps -ef | grep nginx
```
在进程列表里 面找master进程，它的编号就是主进程号了，输入
```bash
kill -HUP 住进称号或进程号文件路径
```
#发表你的第一篇博文
推荐一个GitHub在线编辑器`Prose`
地址：http://prose.io/
用GitHub账号登录后，打开`_post`项目，点击右侧`New File`，然后按照如下格式，你就可以写作啦！

	#文章标题
	- tags: 标签1, 标签2, 标签3

	----

	正文，要插入的话代码，如下：
	```python
	print "hi,I'm coding."
	```

更多Markdown语法请参考：http://wowubuntu.com/markdown/
Ok，大功告成，你已经拥有了你自己的博客
